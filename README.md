<?xml version="1.0" encoding="utf-8" ?>
<ApiConfiguration>
  <ApiUrl>https://example.com/token</ApiUrl>
  <ApiKey>your-api-key-here</ApiKey>
</ApiConfiguration>

using System;
using System.Linq;
using System.Threading.Tasks;
using System.Xml.Linq;

public async Task UseApiTokenAsync()
{
    // Specify the path to your XML configuration file
    string xmlFilePath = "ApiConfig.xml";  // Adjust the path as needed

    try
    {
        // Load the XML file
        XElement config = XElement.Load(xmlFilePath);

        // Fetch the ApiUrl and ApiKey values from the XML
        string apiUrl = config.Descendants("ApiUrl").FirstOrDefault()?.Value;
        string apiKey = config.Descendants("ApiKey").FirstOrDefault()?.Value;

        // Check if ApiUrl or ApiKey are missing
        if (string.IsNullOrEmpty(apiUrl) || string.IsNullOrEmpty(apiKey))
        {
            throw new Exception("API URL or API Key is missing in the configuration file.");
        }

        // Wait for 3 seconds before proceeding
        await Task.Delay(3000);

        // Fetch the token using the URL and key from the XML
        string token = await ApiTokenFetcher.FetchApiTokenAsync(apiUrl, apiKey);

        // Use the token, for example, store it or use it in subsequent API calls
        Console.WriteLine("Fetched Token: " + token);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error: " + ex.Message);
    }
}
