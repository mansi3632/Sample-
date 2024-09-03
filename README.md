# Replace with your actual API URL and token
$apiUrl = "https://api.example.com/data"
$token = "your_token"

# Create a WebRequest object
$request = [System.Net.WebRequest]::Create($apiUrl)

# Set the method to GET
$request.Method = "GET"

# Set the authorization header with the token
$request.Headers.Add("Authorization", "Bearer $token")

# Get the response
$response = $request.GetResponse()

# Read the response stream
$reader = New-Object System.IO.StreamReader($response.GetResponseStream())

# Read the response content
$responseText = $reader.ReadToEnd()

# Close the reader and response
$reader.Close()
$response.Close()

# Process the response text as needed
Write-Host "Response text:"
Write-Host $responseText
