### code

```java
public class HttpClientUtils
{


    private static final Logger logger = LoggerFactory.getLogger(HttpClientUtils.class);
    private static final String HTTP = "http";
    private static final String HTTPS = "https";
    private static SSLConnectionSocketFactory sslConnectionSocketFactory;
    private static PoolingHttpClientConnectionManager poolingHttpClientConnectionManager;//连接池管理类
    private static SSLContextBuilder sslContextBuilder;//管理Https连接的上下文类

    static
    {
        try
        {
            sslContextBuilder = new SSLContextBuilder().loadTrustMaterial(null, new TrustStrategy()
            {
                @Override
                public boolean isTrusted(X509Certificate[] x509Certificates, String s) throws CertificateException
                {
                    // 信任所有站点 直接返回true
                    return true;
                }
            });
            sslConnectionSocketFactory = new SSLConnectionSocketFactory(sslContextBuilder.build(), new String[]{"SSLv2Hello", "SSLv3", "TLSv1", "TLSv1.2"}, null, NoopHostnameVerifier.INSTANCE);
            Registry<ConnectionSocketFactory> registryBuilder = RegistryBuilder.<ConnectionSocketFactory>create()
                    .register(HTTP, new PlainConnectionSocketFactory())
                    .register(HTTPS, sslConnectionSocketFactory)
                    .build();
            poolingHttpClientConnectionManager = new PoolingHttpClientConnectionManager(registryBuilder);
            poolingHttpClientConnectionManager.setMaxTotal(200);
        }
        catch (Exception e)
        {
            throw new RuntimeException(e);
        }
    }

    /**
     * 获取连接
     */
    private static CloseableHttpClient getHttpClient() throws Exception
    {
        CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(sslConnectionSocketFactory).setConnectionManager(poolingHttpClientConnectionManager).setConnectionManagerShared(true).build();
        return httpClient;
    }


    /**
     * 封装请求头
     */
    private static void warpHeader(Map<String, String> params, HttpRequestBase httpMethod)
    {
        if (MapUtils.isNotEmpty(params))
        {
            for (Map.Entry<String, String> entry : params.entrySet())
            {
                httpMethod.setHeader(entry.getKey(), entry.getValue());
            }
        }

    }

    /**
     * 拼接url
     */
    private static String buildUrl(String url, Map<String, String> paraMap) throws Exception
    {
        if (null == url || url.isEmpty())
        {
            return null;
        }
        int index = url.lastIndexOf("/");
        String str = url.substring(index + 1);
        url = url.substring(0, index + 1) + URLEncoder.encode(str, "utf-8");
        if (MapUtils.isEmpty(paraMap))
        {
            return url;
        }
        StringBuffer stringBuffer = new StringBuffer("?");
        for (Map.Entry<String, String> stringStringEntry : paraMap.entrySet())
        {
            stringBuffer.append(stringStringEntry.getKey()).append("=").append(stringStringEntry.getValue()).append("&");
        }
        String s = stringBuffer.toString();
        if (null != s)
        {
            s = s.substring(0, s.length() - 1);//去掉结尾的&连接符
        }
        return url + s;
    }


    /**
     * 关掉连接释放资源
     */
    private static void closeConnection(CloseableHttpClient httpClient, CloseableHttpResponse httpResponse)
    {
        if (httpClient != null)
        {
            try
            {
                httpClient.close();
            }
            catch (IOException e)
            {
                logger.error(e.getLocalizedMessage(), e);
            }
        }
        if (httpResponse != null)
        {
            try
            {
                httpResponse.close();
            }
            catch (IOException e)
            {
                logger.error(e.getLocalizedMessage(), e);
            }
        }
    }

    /**
     * 解析响应数据
     */
    private static String getHttpClientResult(String url, CloseableHttpResponse httpResponse) throws Exception
    {

        if (HttpStatus.SC_OK == httpResponse.getStatusLine().getStatusCode())
        {
            httpResponse.getEntity().getContent();
            return EntityUtils.toString(httpResponse.getEntity(), Consts.UTF_8);
        }
        else
        {
            StringBuffer stringBuffer = new StringBuffer();
            HeaderIterator headerIterator = httpResponse.headerIterator();
            while (headerIterator.hasNext())
            {
                stringBuffer.append("\t" + headerIterator.next());
            }
            logger.error("http请求异常:请求地址:{},响应状态:{},请求返回结果:{}", url, httpResponse.getStatusLine().getStatusCode(), stringBuffer.toString());
        }
        return null;
    }


    /**
     * post提交表单
     * Content-type:application/x-www-form-urlencoded
     *
     * @param url    请求地址
     * @param header httpHeader参数
     * @param params 表单参数
     * @return 响应内容
     */
    public static String postForm(String url, Map<String, String> header, Map<String, String> params)
    {
        if (Strings.isNullOrEmpty(url))
        {
            return null;
        }
        CloseableHttpClient httpClient = null;
        CloseableHttpResponse httpResponse = null;
        try
        {
            httpClient = getHttpClient();
            HttpPost httpPost = new HttpPost(url);
            warpHeader(header, httpPost);
            RequestConfig requestConfig = RequestConfig.custom().setConnectTimeout(5000)//请求超时
                    .setConnectionRequestTimeout(5000)//获取连接超时
                    .setSocketTimeout(5000)//请求获取数据超时
                    .build();
            httpPost.setConfig(requestConfig);
            //请求参数信息
            if (MapUtils.isNotEmpty(params))
            {
                List<NameValuePair> paramList = new ArrayList<>();
                for (Map.Entry<String, String> stringStringEntry : params.entrySet())
                {
                    paramList.add(new BasicNameValuePair(stringStringEntry.getKey(), stringStringEntry.getValue()));
                }
                UrlEncodedFormEntity urlEncodedFormEntity = new UrlEncodedFormEntity(paramList, Consts.UTF_8);
                httpPost.setEntity(urlEncodedFormEntity);
            }

            //发起请求
            httpResponse = httpClient.execute(httpPost);
            return getHttpClientResult(url, httpResponse);
        }
        catch (Exception e)
        {
            logger.error(e.getLocalizedMessage(), e);
        }
        finally
        {
            closeConnection(httpClient, httpResponse);
        }
        return null;
    }

    /**
     * post提交json
     * Content-type:application/json
     *
     * @param url       请求地址
     * @param header    httpHeader参数
     * @param jsonParam body参数,json字符串
     * @return 响应内容
     */
    public static String postJson(String url, Map<String, String> header, String jsonParam)
    {
        if (Strings.isNullOrEmpty(url))
        {
            return null;
        }
        CloseableHttpClient httpClient = null;
        CloseableHttpResponse httpResponse = null;
        try
        {
            httpClient = getHttpClient();
            HttpPost httpPost = new HttpPost(url);
            warpHeader(header, httpPost);
            RequestConfig requestConfig = RequestConfig.custom().setConnectTimeout(5000)//请求超时
                    .setConnectionRequestTimeout(5000)//获取连接超时
                    .setSocketTimeout(5000)//请求获取数据超时
                    .build();
            httpPost.setConfig(requestConfig);
            if (null != jsonParam)
            {
                // 解决中文乱码问题
                StringEntity entity = new StringEntity(jsonParam, "UTF-8");
                entity.setContentEncoding("UTF-8");
                entity.setContentType("application/json");
                httpPost.setEntity(entity);
            }
            //发起请求
            httpResponse = httpClient.execute(httpPost);
            return getHttpClientResult(url, httpResponse);

        }
        catch (Exception e)
        {
            logger.error(e.getLocalizedMessage(), e);
        }
        finally
        {
            closeConnection(httpClient, httpResponse);
        }
        return null;
    }

    /**
     * get请求
     *
     * @param url    请求地址
     * @param header httpHeader参数
     * @param params url路径参数
     * @return 响应内容
     */
    public static String get(String url, Map<String, String> header, Map<String, String> params)
    {
        if (Strings.isNullOrEmpty(url))
        {
            return null;
        }
        CloseableHttpClient httpClient = null;
        CloseableHttpResponse httpResponse = null;
        try
        {
            httpClient = getHttpClient();
            //请求参数信息
            url = buildUrl(url, params);
            HttpGet httpGet = new HttpGet(url);
            RequestConfig requestConfig = RequestConfig.custom().setConnectTimeout(5000)//请求超时
                    .setConnectionRequestTimeout(5000)//获取连接超时
                    .setSocketTimeout(5000)//请求获取数据超时
                    .setRedirectsEnabled(true).build();//允许重定向
            httpGet.setConfig(requestConfig);
            if (MapUtils.isNotEmpty(header))
            {
                for (Map.Entry<String, String> stringStringEntry : header.entrySet())
                {
                    httpGet.setHeader(stringStringEntry.getKey(), stringStringEntry.getValue());
                }
            }
            //发起请求
            httpResponse = httpClient.execute(httpGet);
            return getHttpClientResult(url, httpResponse);

        }
        catch (Exception e)
        {
            logger.error(e.getLocalizedMessage(), e);
        }
        finally
        {
            closeConnection(httpClient, httpResponse);
        }
        return null;

    }


    /**
     * get请求
     *
     * @param url    请求地址
     * @param header httpHeader参数
     * @param params url路径参数
     * @return 字节数组
     */
    public static byte[] getBytes(String url, Map<String, String> header, Map<String, String> params)
    {

        if (Strings.isNullOrEmpty(url))
        {
            return null;
        }
        CloseableHttpClient httpClient = null;
        CloseableHttpResponse httpResponse = null;
        try
        {
            httpClient = getHttpClient();
            //请求参数信息
            url = buildUrl(url, params);
            HttpGet httpGet = new HttpGet(url);
            RequestConfig requestConfig = RequestConfig.custom().setConnectTimeout(5000)//请求超时
                    .setConnectionRequestTimeout(5000)//获取连接超时
                    .setSocketTimeout(5000)//请求获取数据超时
                    .setRedirectsEnabled(true).build();//允许重定向
            httpGet.setConfig(requestConfig);
            if (MapUtils.isNotEmpty(header))
            {
                for (Map.Entry<String, String> stringStringEntry : header.entrySet())
                {
                    httpGet.setHeader(stringStringEntry.getKey(), stringStringEntry.getValue());
                }
            }
            //发起请求
            httpResponse = httpClient.execute(httpGet);
            if (HttpStatus.SC_OK == httpResponse.getStatusLine().getStatusCode())
            {
                ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
                HttpEntity resEntity = httpResponse.getEntity();
                resEntity.writeTo(outputStream);
                byte[] data = outputStream.toByteArray();
                EntityUtils.consume(resEntity);
                return data;
            }
            else
            {
                StringBuffer stringBuffer = new StringBuffer();
                HeaderIterator headerIterator = httpResponse.headerIterator();
                while (headerIterator.hasNext())
                {
                    stringBuffer.append("\t" + headerIterator.next());
                }
                logger.error("http请求异常:请求地址:{},响应状态:{},请求返回结果:{}", url, httpResponse.getStatusLine().getStatusCode(), stringBuffer.toString());
            }

        }
        catch (Exception e)
        {
            logger.error(e.getLocalizedMessage(), e);
        }
        finally
        {
            closeConnection(httpClient, httpResponse);
        }
        return null;

    }


    /**
     * post提交json
     * Content-type:application/json
     *
     * @param url       请求地址
     * @param header    httpHeader参数
     * @param jsonParam body参数,json字符串
     * @return 响应内容
     */
    public static String postJson(String url, Map<String, String> header, String jsonParam,int connetTime,int requestTime,int socketTime)
    {
        if (Strings.isNullOrEmpty(url))
        {
            return null;
        }
        CloseableHttpClient httpClient = null;
        CloseableHttpResponse httpResponse = null;
        try
        {
            httpClient = getHttpClient();
            HttpPost httpPost = new HttpPost(url);
            warpHeader(header, httpPost);
            RequestConfig requestConfig = RequestConfig.custom().setConnectTimeout(connetTime)//请求超时
                    .setConnectionRequestTimeout(requestTime)//获取连接超时
                    .setSocketTimeout(socketTime)//请求获取数据超时
                    .build();
            httpPost.setConfig(requestConfig);
            if (null != jsonParam)
            {
                // 解决中文乱码问题
                StringEntity entity = new StringEntity(jsonParam, "UTF-8");
                entity.setContentEncoding("UTF-8");
                entity.setContentType("application/json;charset=utf-8");
                httpPost.setEntity(entity);
            }
            //发起请求
            httpResponse = httpClient.execute(httpPost);
            return getHttpClientResult(url, httpResponse);

        }
        catch (Exception e)
        {
            logger.error(e.getLocalizedMessage(), e);
        }
        finally
        {
            closeConnection(httpClient, httpResponse);
        }
        return null;
    }

```

