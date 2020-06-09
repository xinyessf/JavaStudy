### code

```java
public class OKHttpUtil {

	private static OkHttpClient okHttpClient=new OkHttpClient().newBuilder()
			.retryOnConnectionFailure(true)
			.connectionPool(new ConnectionPool(500, 5, TimeUnit.MINUTES))
            .connectTimeout(5, TimeUnit.SECONDS)
            .readTimeout(5, TimeUnit.SECONDS)
            .writeTimeout(5, TimeUnit.SECONDS)
            .sslSocketFactory(getTrustedSSLSocketFactory(),x509TrustManager())
            .hostnameVerifier(new HostnameVerifier() {
                @Override
                public boolean verify(String hostname, SSLSession session) {
                    return true;
                }
            }).build();
	
	private static X509TrustManager x509TrustManager(){
        return new X509TrustManager() {
			@Override
			public X509Certificate[] getAcceptedIssuers() {
				X509Certificate[] x509Certificates = new X509Certificate[0];
                return x509Certificates;
			}
			
			@Override
			public void checkServerTrusted(X509Certificate[] chain, String authType) throws CertificateException {
				// TODO Auto-generated method stub
			}
			
			@Override
			public void checkClientTrusted(X509Certificate[] chain, String authType) throws CertificateException {
				// TODO Auto-generated method stub
			}
		};
    }

    private static SSLSocketFactory getTrustedSSLSocketFactory() {
        try {
            SSLContext sc = SSLContext.getInstance("TLS");
            sc.init(null, new TrustManager[]{x509TrustManager()}, new java.security.SecureRandom());
            return sc.getSocketFactory();
        } catch (KeyManagementException | NoSuchAlgorithmException e) {
            e.printStackTrace();
            return null;
        }
    }
	
	
	/**
     * get
     * 
     * @param url 请求的url
     * @param queries 请求的参数，在浏览器？后面的数据，没有可以传null
     * @return
     */
    public static String get(String url, Map<String, String> queries) throws Exception {
        String responseBody = "";
        StringBuffer sb = new StringBuffer(url);
        if (MapUtils.isNotEmpty(queries)) {
            boolean firstFlag = true;
            Iterator<Map.Entry<String, String>> iterator = queries.entrySet().iterator();
            while (iterator.hasNext()) {
                Map.Entry<String, String> entry = iterator.next();
                if (firstFlag) {
                    sb.append("?" + entry.getKey() + "=" + entry.getValue());
                    firstFlag = false;
                } else {
                    sb.append("&" + entry.getKey() + "=" + entry.getValue());
                }
            }
        }
        Request request = new Request.Builder().url(sb.toString()).build();
        Response response = null;
        try {
            response = okHttpClient.newCall(request).execute();
            //int status = response.code();
            if (response.isSuccessful()) {
                return response.body().string();
            }
        } catch (Exception e) {
            throw e;
        } finally {
            if (response != null) {
                response.close();
            }
        }
        return responseBody;
    }
    
    /**
     * 获取请求原始字节数组
     */
    public static byte[] getBytes(String url, Map<String, String> queries) throws Exception {
    	byte[] responseBody = null;
        StringBuffer sb = new StringBuffer(url);
        if (MapUtils.isNotEmpty(queries)) {
            boolean firstFlag = true;
            Iterator<Map.Entry<String, String>> iterator = queries.entrySet().iterator();
            while (iterator.hasNext()) {
                Map.Entry<String, String> entry = iterator.next();
                if (firstFlag) {
                    sb.append("?" + entry.getKey() + "=" + entry.getValue());
                    firstFlag = false;
                } else {
                    sb.append("&" + entry.getKey() + "=" + entry.getValue());
                }
            }
        }
        Request request = new Request.Builder().url(sb.toString()).build();
        Response response = null;
        try {
        	
            response = okHttpClient.newCall(request).execute();
            //int status = response.code();
            if (response.isSuccessful()) {
                return response.body().bytes();
            }
        } catch (Exception e) {
            throw e;
        } finally {
            if (response != null) {
                response.close();
            }
        }
        return responseBody;
    }

    /**
     * Post请求发送JSON数据....{"name":"zhangsan","pwd":"123456"} 参数一：请求Url 参数二：请求的JSON
     * 参数三：请求回调
     */
    public static String postJsonParams(String url, String jsonParams) throws Exception {
        String responseBody = "";
        RequestBody requestBody = RequestBody.create(MediaType.parse("application/json; charset=utf-8"), jsonParams);
        Request request = new Request.Builder().url(url).post(requestBody).build();
        Response response = null;
        try {
            response = okHttpClient.newCall(request).execute();
            //int status = response.code();
            if (response.isSuccessful()) {
                return response.body().string();
            }
        } catch (Exception e) {
            throw e;
        } finally {
            if (response != null) {
                response.close();
            }
        }
        return responseBody;
    }
    
    /**
     * get
     * 
     * @param url 请求的url
     * @param queries 请求的参数，在浏览器？后面的数据，没有可以传null
     * @return
     */
    public static String post(String url, Map<String, String> params) throws Exception {
        String responseBody = "";
        StringBuffer sb = new StringBuffer();
        RequestBody requestBody = null;
        if (MapUtils.isNotEmpty(params)) {
        	
            boolean firstFlag = true;
            Iterator<Map.Entry<String, String>> iterator = params.entrySet().iterator();
            while (iterator.hasNext()) {
                Map.Entry<String, String> entry = iterator.next();
                if (firstFlag) {
                    sb.append(entry.getKey() + "=" + entry.getValue());
                    firstFlag = false;
                } else {
                    sb.append("&" + entry.getKey() + "=" + entry.getValue());
                }
            }
            if(sb.length()!=0) {
            	requestBody = RequestBody.create(MediaType.parse("application/xml; charset=utf-8"), sb.toString());
            }
        }
        Request request = new Request.Builder().url(url).post(requestBody).build();
        Response response = null;
        try {
            response = okHttpClient.newCall(request).execute();
            if (response.isSuccessful()) {
                return response.body().string();
            }
        } catch (Exception e) {
            throw e;
        } finally {
            if (response != null) {
                response.close();
            }
        }
        return responseBody;
    }
    
    public static void main(String[] args) {
    	try {
			System.out.println(get("https://www.baidu.com",null));
		} catch (Exception e) {
			e.printStackTrace();
		}
    }
```

