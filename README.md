# userbase_growth_graph
A java CLI application used to visualise the growth of a user base by graph.
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;

public class Main {
    private static HttpURLConnection connection;
    public static void main(String[] args) {

        BufferedReader reader;
        String line;

        //extract data from the API with the userbase
        String json;
        try {
            URL url = new URL("http://sam-user-activity.eu-west-1.elasticbeanstalk.com/");
            connection = (HttpURLConnection) url.openConnection();

            //Request setup
            connection.setRequestMethod("GET");
            connection.setConnectTimeout(5000);
            connection.setReadTimeout(5000);

            int status = connection.getResponseCode();
            StringBuffer responseContent = new StringBuffer();
            json = new String();
            if (status > 299) {
                reader = new BufferedReader(new InputStreamReader(connection.getErrorStream()));

            } else {
                reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            }

            while ((line = reader.readLine()) != null) {
                json = json + String.valueOf(reader.readLine()) + "\n";
            }

            System.out.println(json);

        } catch (MalformedURLException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            connection.disconnect();
        }


    }


}


