using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

public class ApiTokenFetcher
{
    private static readonly HttpClient client = new HttpClient();

    public static async Task<string> FetchApiTokenAsync(string apiUrl, string apiKey)
    {
        // Prepare the request
        var requestMessage = new HttpRequestMessage(HttpMethod.Post, apiUrl);
        requestMessage.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        
        // If the API expects the key in the headers
        requestMessage.Headers.Add("x-api-key", apiKey);

        // If the API expects the key in the body (often in OAuth flows)
        var content = new StringContent("{ \"apiKey\": \"" + apiKey + "\" }", System.Text.Encoding.UTF8, "application/json");
        requestMessage.Content = content;

        // Send the request
        HttpResponseMessage response = await client.SendAsync(requestMessage);

        // Ensure the response indicates success
        response.EnsureSuccessStatusCode();

        // Parse the response body as a JSON object
        var responseBody = await response.Content.ReadAsStringAsync();
        var jsonResponse = JObject.Parse(responseBody);

        // Extract the token from the response
        string token = jsonResponse["access_token"]?.ToString();

        if (string.IsNullOrEmpty(token))
        {
            throw new Exception("Token not found in the response.");
        }

        return token;
    }
}



public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
        FetchAndUseApiToken();
    }

    private async void FetchAndUseApiToken()
    {
        string apiUrl = "https://example.com/token";
        string apiKey = "your-api-key-here";

        try
        {
            string token = await ApiTokenFetcher.FetchApiTokenAsync(apiUrl, apiKey);

            // Use the token, for example, setting it to a TextBlock or using it for further API calls
            Console.WriteLine("Fetched Token: " + token);
        }
        catch (Exception ex)
        {
            Console.WriteLine("Error fetching token: " + ex.Message);
        }
    }
}

