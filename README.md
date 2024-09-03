using System;
using System.Net.Http;
using System.Threading.Tasks;
using System.Windows;

namespace YourNamespace
{
    public partial class MainWindow : Window
    {
        private string apiUrl = "https://your-api-endpoint";
        private string accessToken = "your-access-token";

        public string ApiData { get; set; }

        public MainWindow()
        {
            InitializeComponent();
            DataContext = this;
        }

        private async void FetchDataButton_Click(object sender, RoutedEventArgs e)
        {
            try
            {
                using (HttpClient client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", accessToken);
                    HttpResponseMessage response = await client.GetAsync(apiUrl);

                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        ApiData = content;
                    }
                    else
                    {
                        MessageBox.Show("Error fetching data: " + response.StatusCode);
                    }
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show("An error occurred: " + ex.Message);
            }
        }
    }
}



<Window x:Class="YourNamespace.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:YourNamespace"
        Title="MainWindow" Height="450" Width="800"
        Loaded="Window_Loaded">
    <Grid>
        <TextBlock x:Name="textBlock" Text="{Binding ApiData}" />
    </Grid>
</Window>

using System;
using System.Net.Http;
using System.Threading.Tasks;
using System.Windows;

namespace YourNamespace
{
    public partial class MainWindow : Window
    {
        private string apiUrl = "https://your-api-endpoint";
        private string accessToken = "your-access-token";

        public string ApiData { get; set; }

        public MainWindow()
        {
            InitializeComponent();
            DataContext = this;
            Loaded += Window_Loaded;
        }

        private async void Window_Loaded(object sender, RoutedEventArgs e)
        {
            try
            {
                // ... (rest of the code from the previous response)
            }
            catch (Exception ex)
            {
                // ... (rest of the code from the previous response)
            }
        }
    }
}
