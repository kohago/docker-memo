```
cat << EOF  > HttpURLConnectionExample.java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import javax.net.ssl.HttpsURLConnection;
import java.io.DataOutputStream;

public class HttpURLConnectionExample {

    public static void main(String[] args) throws Exception {
        HttpURLConnectionExample http = new HttpURLConnectionExample();
        switch (args[0]){
            case "token":
                http.getToken();
                break;
            case "encrypt":
                http.encrypt(args[1],args[2]);
                break;
            case "decrypt":
                http.decrypt(args[1],args[2]);
                break;
        }
    }

    //HTTP GET request
    private String getToken()  throws Exception {

        String url = "http://169.254.169.254/metadata/identity/oauth2/token?resource=https://vault.azure.net";

        URL obj = new URL(url);
        HttpURLConnection con = (HttpURLConnection) obj.openConnection();

        //optional default is GET
        con.setRequestMethod("GET");

        //add request header
        con.setRequestProperty("Metadata", "true");

        int responseCode = con.getResponseCode();
        System.out.println("\nSending 'GET' request to URL : " + url);
        System.out.println("Response Code : " + responseCode);

        BufferedReader in = new BufferedReader(
                new InputStreamReader(con.getInputStream()));
        String inputLine;
        StringBuffer response = new StringBuffer();

        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }
        in.close();

        String res=response.toString();
        System.out.println(res);
        return res;
    }

    //HTTP POST request
    private void encrypt(String token,String target) throws Exception {

        String url = "https://akv-name.vault.azure.net/keys/key-name/key-version/encrypt?api-version=7.1";
        URL obj = new URL(url);
        HttpsURLConnection con = (HttpsURLConnection) obj.openConnection();

        //add reuqest header
        con.setRequestMethod("POST");
        con.setRequestProperty("Content-Type", "application/json");
        con.setRequestProperty("Authorization", "Bearer "+token);

        //Send post request
        con.setDoOutput(true);
        DataOutputStream wr = new DataOutputStream(con.getOutputStream());
        String body = "{\"alg\":\"RSA-OAEP\",\"value\":\""+ target+"\"}";
        wr.writeBytes(body);
        wr.flush();
        wr.close();

        int responseCode = con.getResponseCode();
        System.out.println("\nSending 'POST' request to URL : " + url);
        System.out.println("Response Code : " + responseCode);

        BufferedReader in = new BufferedReader(
                new InputStreamReader(con.getInputStream()));
        String inputLine;
        StringBuffer response = new StringBuffer();

        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }
        in.close();

        //print result
        System.out.println(response.toString());
    }

    //HTTP POST request
    private void decrypt(String token,String target) throws Exception {

        String url = "https://akv-name.vault.azure.net/keys/key-name/key-version/decrypt?api-version=7.1";
        URL obj = new URL(url);
        HttpsURLConnection con = (HttpsURLConnection) obj.openConnection();

        //add reuqest header
        con.setRequestMethod("POST");
        con.setRequestProperty("Content-Type", "application/json");
        con.setRequestProperty("Authorization", "Bearer "+token);

        //Send post request
        con.setDoOutput(true);
        DataOutputStream wr = new DataOutputStream(con.getOutputStream());
        String body = "{\"alg\":\"RSA-OAEP\",\"value\":\""+ target+"\"}";
        wr.writeBytes(body);
        wr.flush();
        wr.close();

        int responseCode = con.getResponseCode();
        System.out.println("\nSending 'POST' request to URL : " + url);
        System.out.println("Response Code : " + responseCode);

        BufferedReader in = new BufferedReader(
                new InputStreamReader(con.getInputStream()));
        String inputLine;
        StringBuffer response = new StringBuffer();

        while ((inputLine = in.readLine()) != null) {
            response.append(inputLine);
        }
        in.close();

        //print result
        System.out.println(response.toString());
    }
}
EOF

javac HttpURLConnectionExample.java
java -cp . HttpURLConnectionExample token
java -cp . HttpURLConnectionExample encrypt eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImppYk5ia0ZTU2JteFBZck45Q0ZxUms0SzRndyIsImtpZCI6ImppYk5ia0ZTU2JteFBZck45Q0ZxUms0SzRndyJ9.eyJhdWQiOiJodHRwczovL3ZhdWx0LmF6dXJlLm5ldCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0L2QwODA5MDI3LTE1OGUtNDQwMC1hMmJiLTEwOWI2MTM5Y2FmNy8iLCJpYXQiOjE1OTg3ODQ3NjIsIm5iZiI6MTU5ODc4NDc2MiwiZXhwIjoxNTk4ODcxNDYyLCJhaW8iOiJFMkJnWU1pSTRUL2h6Q3Y4ZGxWNTErUlVIZlpNQUE9PSIsImFwcGlkIjoiYzQ3NTJjYWYtOTQwNC00M2JiLWI4MzEtY2U3MjQ4ZjJjYmNlIiwiYXBwaWRhY3IiOiIyIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZDA4MDkwMjctMTU4ZS00NDAwLWEyYmItMTA5YjYxMzljYWY3LyIsIm9pZCI6IjNiN2U2NDA0LWI4NzItNGRmYy1iMTBiLTI2YjIxYzVjZGU1NCIsInJoIjoiMC5BQUFBSjVDQTBJNFZBRVNpdXhDYllUbks5NjhzZGNRRWxMdER1REhPY2tqeXk4NUpBQUEuIiwic3ViIjoiM2I3ZTY0MDQtYjg3Mi00ZGZjLWIxMGItMjZiMjFjNWNkZTU0IiwidGlkIjoiZDA4MDkwMjctMTU4ZS00NDAwLWEyYmItMTA5YjYxMzljYWY3IiwidXRpIjoibG54amJUX2dQMGV6VDdqNXU0Y0JBQSIsInZlciI6IjEuMCIsInhtc19taXJpZCI6Ii9zdWJzY3JpcHRpb25zL2VlNjhmN2VmLWJkMTQtNDExZS1hYzQzLWVjMDBhZjBiMGRkOS9yZXNvdXJjZWdyb3Vwcy9NQ19yZy1zdGFnZS10YmxvY2tzaWduLWFrc19ha3MtdGVzdC10YmxvY2tzaWduX2phcGFuZWFzdC9wcm92aWRlcnMvTWljcm9zb2Z0Lk1hbmFnZWRJZGVudGl0eS91c2VyQXNzaWduZWRJZGVudGl0aWVzL3RibG9ja3NpZ24tcG9kLWlkZW50aXR5In0.nZ7_L8cuUSGvbnkqab1a9OQiVOTK4qNA3623UT9BVr8ZUNkydc6pGHLHqa3rDvfHyMZ6SPQQF-7Tburpo2boU-CW5dqq61nVafapEBIxNSrOSr3BmGDgXCxrHvV3e_5OfijKSIckLEqnkJTHziVZmEiH8jbMkquM7steUUMdFrlEPOaLOpWxJhtoCI0T0YEhzY9sf_JDuvoAb5_F6zJdlLFtE6nkF1HYzvZA049bGvwQgJZWZwLpTX51bd6rejn2q9XnBFzmbJn3yTV8ht8YZ5OB27LvhSirmXynexKzuXKTG9N0p6Ci8j_rSDuQHYQdRdNh_3rJW_ZzqNf3HuzUXQ c3VudG9yeeWNl+OCouODq+ODl+OCueOBruWkqeeEtuawtAo=

java -cp . HttpURLConnectionExample decrypt eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImppYk5ia0ZTU2JteFBZck45Q0ZxUms0SzRndyIsImtpZCI6ImppYk5ia0ZTU2JteFBZck45Q0ZxUms0SzRndyJ9.eyJhdWQiOiJodHRwczovL3ZhdWx0LmF6dXJlLm5ldCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0L2QwODA5MDI3LTE1OGUtNDQwMC1hMmJiLTEwOWI2MTM5Y2FmNy8iLCJpYXQiOjE1OTg3ODQ3NjIsIm5iZiI6MTU5ODc4NDc2MiwiZXhwIjoxNTk4ODcxNDYyLCJhaW8iOiJFMkJnWU1pSTRUL2h6Q3Y4ZGxWNTErUlVIZlpNQUE9PSIsImFwcGlkIjoiYzQ3NTJjYWYtOTQwNC00M2JiLWI4MzEtY2U3MjQ4ZjJjYmNlIiwiYXBwaWRhY3IiOiIyIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZDA4MDkwMjctMTU4ZS00NDAwLWEyYmItMTA5YjYxMzljYWY3LyIsIm9pZCI6IjNiN2U2NDA0LWI4NzItNGRmYy1iMTBiLTI2YjIxYzVjZGU1NCIsInJoIjoiMC5BQUFBSjVDQTBJNFZBRVNpdXhDYllUbks5NjhzZGNRRWxMdER1REhPY2tqeXk4NUpBQUEuIiwic3ViIjoiM2I3ZTY0MDQtYjg3Mi00ZGZjLWIxMGItMjZiMjFjNWNkZTU0IiwidGlkIjoiZDA4MDkwMjctMTU4ZS00NDAwLWEyYmItMTA5YjYxMzljYWY3IiwidXRpIjoibG54amJUX2dQMGV6VDdqNXU0Y0JBQSIsInZlciI6IjEuMCIsInhtc19taXJpZCI6Ii9zdWJzY3JpcHRpb25zL2VlNjhmN2VmLWJkMTQtNDExZS1hYzQzLWVjMDBhZjBiMGRkOS9yZXNvdXJjZWdyb3Vwcy9NQ19yZy1zdGFnZS10YmxvY2tzaWduLWFrc19ha3MtdGVzdC10YmxvY2tzaWduX2phcGFuZWFzdC9wcm92aWRlcnMvTWljcm9zb2Z0Lk1hbmFnZWRJZGVudGl0eS91c2VyQXNzaWduZWRJZGVudGl0aWVzL3RibG9ja3NpZ24tcG9kLWlkZW50aXR5In0.nZ7_L8cuUSGvbnkqab1a9OQiVOTK4qNA3623UT9BVr8ZUNkydc6pGHLHqa3rDvfHyMZ6SPQQF-7Tburpo2boU-CW5dqq61nVafapEBIxNSrOSr3BmGDgXCxrHvV3e_5OfijKSIckLEqnkJTHziVZmEiH8jbMkquM7steUUMdFrlEPOaLOpWxJhtoCI0T0YEhzY9sf_JDuvoAb5_F6zJdlLFtE6nkF1HYzvZA049bGvwQgJZWZwLpTX51bd6rejn2q9XnBFzmbJn3yTV8ht8YZ5OB27LvhSirmXynexKzuXKTG9N0p6Ci8j_rSDuQHYQdRdNh_3rJW_ZzqNf3HuzUXQ aGTN3LmNB5Kr5pz7V5cFvWeyZ55Cwi6ax6lmZWiofyeAOMLwCsGzwXD14NsMmRuUsOoxwss9MnUD4kg9cuLae1Ef8gQviRF3ZvzRK892TwiqpWAZutH3XENvMKyb_PbJ8s1mYLdrKhDpREcD_e2DbIaQEjXZgYo3ZGCOCIyQnSwVDD0xKKNZvz4isYXe3vP35QLMK42gaiCwLNJ3ndXunx58pm8tmoLb3yx8yvn2b4Ckbee7_Lu5hEP-SSFI-OA5DpAip1xcZhqJQQTN80WtQ8rRQNfEVJd5gvUMlo4bicSPjg-TknUr5e31KcAAcjj4FuGwjy9QMlfpbD6aOJiRag
```


