---
title: 'How to send HTTP GET / POST request in Java '
media_order: http.png
date: '09-02-2017 19:26'
taxonomy:
    tag:
        - Java
header_image_file: http.png
header_image_width: 128
header_image_height: 64
---

This post describes how to make HTTP GET and POST request on server. I will use httpbin server for testing. It returns you everything you send there : query params, post data, headers, etc.

===

## HTTP GET request

```
package httpclientexample;

import java.io.BufferedReader;
import java.io.Closeable;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
//import javax.net.ssl.HttpsURLConnection;

public class HttpClientExample {

    public static void main(String[] args) {
        try {
            HttpClientExample hce = new HttpClientExample();
            String body = hce.get("http://httpbin.org/get");
            System.out.println(body);
        } catch(IOException ioe) {
            ioe.printStackTrace();
        }
    }

    public String get(String getUrl) throws IOException {
        URL url = new URL(getUrl);
        HttpURLConnection con = (HttpURLConnection) url.openConnection();
        con.setRequestMethod("GET");

        return this.read(con.getInputStream());
    }

    private String read(InputStream is) throws IOException {
        BufferedReader in = null;
        String inputLine;
        StringBuilder body;
        try {
            in = new BufferedReader(new InputStreamReader(is));

            body = new StringBuilder();

            while ((inputLine = in.readLine()) != null) {
                body.append(inputLine);
            }
            in.close();

            return body.toString();
        } catch(IOException ioe) {
            throw ioe;
        } finally {
            this.closeQuietly(in);
        }
    }

    protected void closeQuietly(Closeable closeable) {
        try {
            if( closeable != null ) {
                closeable.close();
            }
        } catch(IOException ex) {

        }
    }
}
```

## HTTP POST request

```
package httpclientexample;

import java.io.BufferedReader;
import java.io.Closeable;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
//import javax.net.ssl.HttpsURLConnection;

public class HttpClientExample {

    public static void main(String[] args) {
        try {
            HttpClientExample hce = new HttpClientExample();
            String body = hce.post("http://httpbin.org/post", "data=test data");
            System.out.println(body);
        } catch(IOException ioe) {
            ioe.printStackTrace();
        }
    }

    public String post(String postUrl, String data) throws IOException {
        URL url = new URL(postUrl);
        HttpURLConnection con = (HttpURLConnection) url.openConnection();
        con.setRequestMethod("POST");

        con.setDoOutput(true);

        this.sendData(con, data);

        return this.read(con.getInputStream());
    }

    protected void sendData(HttpURLConnection con, String data) throws IOException {
        DataOutputStream wr = null;
        try {
            wr = new DataOutputStream(con.getOutputStream());
            wr.writeBytes(data);
            wr.flush();
            wr.close();
        } catch(IOException exception) {
            throw exception;
        } finally {
            this.closeQuietly(wr);
        }
    }

    private String read(InputStream is) throws IOException {
        BufferedReader in = null;
        String inputLine;
        StringBuilder body;
        try {
            in = new BufferedReader(new InputStreamReader(is));

            body = new StringBuilder();

            while ((inputLine = in.readLine()) != null) {
                body.append(inputLine);
            }
            in.close();

            return body.toString();
        } catch(IOException ioe) {
            throw ioe;
        } finally {
            this.closeQuietly(in);
        }
    }

    protected void closeQuietly(Closeable closeable) {
        try {
            if( closeable != null ) {
                closeable.close();
            }
        } catch(IOException ex) {

        }
    }
}
```

## Conclusion

If you need to send additional headers, for example when you want to upload JSON data, just use `setRequestProperty`. It takes header field name and it’s value as parameters.

```
...
con.setRequestProperty("Content-Type", "application/json");
...
```

#### References

[httpbin : HTTPS Request & Response](http://httpbin.org/) page for testing HTTP client
