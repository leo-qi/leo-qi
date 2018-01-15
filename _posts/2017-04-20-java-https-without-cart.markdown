---
layout : post
title : Java实现无证书的HTTPS访问
author : "Leo Qi"
Date : 2017-03-16
catalog: true
tags:
    - Java
    - HTTPS
    - Socket
    - Java网络编程
---

1. 实现自己的证书信任管理器类，使之信任所有证书
```java
class MyX509TrustManager implements javax.net.ssl.X509TrustManager {
	@Override
	public void checkClientTrusted(X509Certificate[] arg0, String arg1)
					throws CertificateException {
	}

	@Override
	public void checkServerTrusted(X509Certificate[] arg0, String arg1)
					throws CertificateException {
	}

	@Override
	public X509Certificate[] getAcceptedIssuers() {
		return null;
	}	    	
}
```

2. 创建SSLContext对象，并使用自己的信任管理器初始化
```java
  TrustManager[] tm = {new MyX509TrustManager ()};
  SSLContext sslContext = SSLContext.getInstance("SSL","SunJSSE");
  sslContext.init(null, tm, new java.security.SecureRandom());
```

3. 从上述SSLContext对象中得到SSLSocketFactory对象
```java
SSLSocketFactory sslSocketfactory = sslContext.getSocketFactory();
```

4. 建立连接
  - 建立Socket连接
  ```java
    SSLSocketsocket ssf= (SSLSocket)sslSocketfactory.createSocket(hostAddress,httpsPort);
  ```
  - 建立HttpsURLConnection连接
  ```java
    // 信任所有证书域名
    static HostnameVerifier hv = new HostnameVerifier() {  
        @Override  
        public boolean verify(String hostname, SSLSession session) {  
            return true;  
        }  
      };
  ```
  ```java
			URL url = new URL("https://"+host+serverPath+parameters);
      HttpsURLConnection.setDefaultSSLSocketFactory(sslContext.getSocketFactory());
      HttpsURLConnection.setDefaultHostnameVerifier(hnv);
      HttpsURLConnection httpsConn = (HttpsURLConnection)url.openConnection();
      // 此处可以不写
      httpsConn.setSSLSocketFactory(sslSocketfactory);
  ```
5. 其他
  - 有时运行时会报出错误：java.net.SocketTimeoutException: connect timed out，在建立连接之前增加如下代码：
  ```java
    System.setProperty("https.protocols", "TLSv1");
  ```
  - 可以将信任证书设计成公共方法，在建立连接之前调用trustAllHttpsCertificates();就可以使用了。
  ```java
    // 信任所有证书域名
    static HostnameVerifier hv = new HostnameVerifier() {   
        @Override  
        public boolean verify(String hostname, SSLSession session) {  
            return true;  
        }  
    };
    // 信任所有证书
    private static void trustAllHttpsCertificates() {  
        try {  
            TrustManager[] tm = {new MyX509TrustManager()};
            SSLContext sc = SSLContext.getInstance("SSL");
            sc.init(null, tm, null);  
            HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());
            HttpsURLConnection.setDefaultHostnameVerifier(hv);
        } catch (NoSuchAlgorithmException e) {  
            e.printStackTrace();  
        } catch (KeyManagementException e) {  
            e.printStackTrace();  
        }  
    }
  ```
