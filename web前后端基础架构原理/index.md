# Web前后端基础架构原理


<!--more-->
# Web前后端基础架构原理
## 1 计算机网络是如何工作的

在打开网页的时候发生了什么

- 主机
- 域名与DNS
- 端口：HTTPS协议的默认端口是443，HTTP协议默认的端口是80
TCP协议
HTTP协议

### 2 使用Java代码访问GitHub的issues
选择一个合适的客户端
- 如何快速上手使用自己从没用过的库？
设置正确的HTTP header
发送请求，等待响应
解析拿到的响应

### 3 同步加载与异步加载

1 发送请求
2 接受响应 html
3 html转换成Document
4 筛选值
```
    // 给定一个仓库名，例如"golang/go"，或者"gradle/gradle"，返回第一页的Pull request信息
    public static List<GitHubPullRequest> getFirstPageOfPullRequests(String repo) throws IOException {
        List<GitHubPullRequest> pr = new ArrayList<>();
        CloseableHttpClient httpclient = HttpClients.createDefault();
        HttpGet httpGet = new HttpGet("https://github.com/" + repo + "/pulls");
        CloseableHttpResponse response1 = httpclient.execute(httpGet);
        try {
            HttpEntity entity1 = response1.getEntity();
            InputStream is = entity1.getContent();
            String html = IOUtils.toString(is, "UTF-8");
            Document document = Jsoup.parse(html);
            ArrayList<Element> elements = document.select(".js-issue-row");
            for (Element e : elements) {
                try {
                    pr.add(
                            new GitHubPullRequest(Integer.parseInt(e.child(0).child(1).child(0).attr("href").substring(20)),
                                    e.child(0).child(1).child(0).text(),
                                    e.child(0).child(1).child(2).child(0).child(1).text())
                    );
                } catch (RuntimeException e1) {
                    pr.add(
                            new GitHubPullRequest(Integer.parseInt(e.child(0).child(1).child(0).attr("href").substring(20)),
                                    e.child(0).child(1).child(0).text(),
                                    e.child(0).child(1).child(3).child(0).child(1).text()));
                }
            }
        } finally {
            response1.close();
        }
        return pr;
    }
```
### 4 反爬虫
1. 返回错误的状态码

- states 401/403

2. 返回错误的数据 

### 5 HTTP method（方法）
- GET 获取 
- POST 发送
    - Request Payload(请求有效负载)
    - 发送Json数据
    - 放在http的body里面
- PUT
- DELETE

### 6 HTTP status(状态码)
301 永久移动，将地址移动另一个位置
4... 客户端错误
### 7 HTTP 请求 header
重要的header
- Response Header
- Request Header
请求
    - Accept*
    - Cookie 登录凭证
    - User-Agent:区分浏览器
    - Referer：当前页面跳转过来的页面
响应
Content-Type:当前http响应类型
- application/zip可以支持下载
- img /jpeg 图片请求
Transfer-Encoding:传输编码
### 8 HTTP body
HTTP request body
- 表单
- K-V对
HTTP response body
- JSON
- HTML/XML
- 二进制（图片/下载文件）
### 9 HTTP协议是无状态的

```java
package com.github.hcsp.http;


import com.alibaba.fastjson.JSON;

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

public class Crawler {	public class Crawler {
    public static String loginAndGetResponse(String username, String password) {}	    public static String loginAndGetResponse(String username, String password) throws IOException {
        CloseableHttpClient httpclient = HttpClients.createDefault();

        HttpPost httpPost = new HttpPost("http://47.91.156.35:8000/auth/login");
        httpPost.addHeader("Content-Type", "application/json;charset=utf-8");
        httpPost.addHeader("User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.100 Safari/537.36");

        Map<String, String> map = new HashMap<>();
        map.put("username", "xdml");
        map.put("password", "xdml");
        String user = JSON.toJSONString(map);

        httpPost.setEntity(new StringEntity(user, "utf-8"));
        CloseableHttpResponse resp = httpclient.execute(httpPost);
        String cookie = resp.getFirstHeader("Set-Cookie").getValue().split(";")[0];

        HttpGet httpGet = new HttpGet("http://47.91.156.35:8000/auth");
        httpGet.addHeader("Cookie", cookie);
        CloseableHttpResponse response1 = httpclient.execute(httpGet);

        HttpEntity entity1 = response1.getEntity();
        String body = EntityUtils.toString(entity1, "utf-8");
        return body;
    }
}	}
```
