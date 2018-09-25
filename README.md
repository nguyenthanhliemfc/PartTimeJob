# PartTimeJob
Sample using WebService in Xamarin Forms
For NET STANDARD: When install RestClient will not create RestClient class you can create your self content below:
public class RestClient<T>
    {
        //private const string WebServiceUrl = "http://taskmodel.azurewebsites.net/api/TaskModels/";
        //private const string WebServiceUrl = "http://localhost:51227/";

        public async Task<List<T>> GetAsync(string WebServiceUrl)
        {
            try
            {
                var httpClient = new HttpClient();

                var json = await httpClient.GetStringAsync(WebServiceUrl);

                var taskModels = JsonConvert.DeserializeObject<List<T>>(json);

                return taskModels;
            }
            catch (Exception ex)
            {
                Debug.WriteLine("GetAsync Error: " + ex.Message);
                return null;
            }
            
        }

        public async Task<bool> PostAsync(T t, string WebServiceUrl)
        {
            var httpClient = new HttpClient();

            var json = JsonConvert.SerializeObject(t);

            HttpContent httpContent = new StringContent(json);

            httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            var result = await httpClient.PostAsync(WebServiceUrl, httpContent);

            return result.IsSuccessStatusCode;
        }

        public async Task<bool> PutAsync(int id, T t, string WebServiceUrl)
        {
            var httpClient = new HttpClient();

            var json = JsonConvert.SerializeObject(t);

            HttpContent httpContent = new StringContent(json);

            httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

            var result = await httpClient.PutAsync(WebServiceUrl + "/" + id, httpContent);

            return result.IsSuccessStatusCode;
        }

        public async Task<bool> DeleteAsync(int id, string WebServiceUrl)
        {
            var httpClient = new HttpClient();

            var response = await httpClient.DeleteAsync(WebServiceUrl + "/" + id);

            return response.IsSuccessStatusCode;
        }
    }
