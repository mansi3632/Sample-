Currently I am putting most of my efforts in Amelia side of things. Trying to develop an automation which takes the information from sharepoint or gsow and create issues in gitlab. Earlier I managed to create a workflow which does the same thing but as sharepoint integration is not present in amelia , we are going to use powershell script and see if we can achieve our goal. Then login enterprise activities are in progress, right now I am creating script for onedrive application.









?xml version="1.0" encoding="utf-8" ?>
<ApiConfiguration>

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
