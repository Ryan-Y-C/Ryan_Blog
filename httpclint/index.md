# HttpClient


<!--more-->
# HttpClients使用
## 1 创建Maven依赖
```
<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.11</version>
</dependency>
```
## 2 创建Get请求
教程：http://hc.apache.org/httpcomponents-client-ga/index.html
```
//创建HttpClient实例
CloseableHttpClient httpclient = HttpClients.createDefault();
//创建Get请求实例
HttpGet httpGet = new HttpGet("http://targethost/homepage");
//添加Header
//所在主机
httpGet.addHeader("User-Agent","Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.116 Safari/537.36");
//想要的数据类型
httpGet.addHeader("Content-Type","text/html; charset=utf-8");
//执行请求
CloseableHttpResponse response1 = httpclient.execute(httpGet);
// The underlying HTTP connection is still held by the response object
// to allow the response content to be streamed directly from the network socket.
// In order to ensure correct deallocation of system resources
// the user MUST call CloseableHttpResponse#close() from a finally clause.
// Please note that if response content is not fully consumed the underlying
// connection cannot be safely re-used and will be shut down and discarded
// by the connection manager. 
try {
    System.out.println(response1.getStatusLine());
    HttpEntity entity1 = response1.getEntity();
    // do something useful with the response body
    // and ensure it is fully consumed
    EntityUtils.consume(entity1);
} finally {
    response1.close();
}

//创建Post请求
HttpPost httpPost = new HttpPost("http://targethost/login");
List <NameValuePair> nvps = new ArrayList <NameValuePair>();
nvps.add(new BasicNameValuePair("username", "vip"));
nvps.add(new BasicNameValuePair("password", "secret"));
httpPost.setEntity(new UrlEncodedFormEntity(nvps));
CloseableHttpResponse response2 = httpclient.execute(httpPost);

try {
    System.out.println(response2.getStatusLine());
    HttpEntity entity2 = response2.getEntity();
    // do something useful with the response body
    // and ensure it is fully consumed
    EntityUtils.consume(entity2);
} finally {
    response2.close();
}
```
## 3 HTML解析器Jsoup
Maven依赖
```
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.11.2</version>
</dependency>
```
解析String类型的html
```
Document document = Jsoup.parse(html);  //  将字符串解析成Document对象
```
Jsoup可以使用Jsoup中提供的一些静态方法从网络获取html文档并进行解析成Document对象
```
public static void main(String[] args) throws Exception{
        Document document = Jsoup.connect("http://blog.beifengtz.com/").get();
        System.out.println(document);
    }
```
// 爬取GitHub的Pull request并存储为CSV文件
```java
package com.github.hcsp.io;

import org.apache.commons.io.FileUtils;

import org.apache.http.HttpStatus;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;

import java.io.File;

import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;


public class Crawler {
    // 给定一个仓库名，例如"golang/go"，或者"gradle/gradle"，读取前n个Pull request并保存至csvFile指定的文件中，格式如下：
    // number,author,title
    // 12345,blindpirate,这是一个标题
    // 12345,FrankFang,这是第二个标题
    public static void savePullRequestsToCSV(String repo, int n, File csvFile) throws IOException {
        String html = "";
        CloseableHttpClient httpclient = HttpClients.createDefault();
        HttpGet httpGet = new HttpGet("http://GitHub.com/" + repo + "/pulls");
        CloseableHttpResponse response = httpclient.execute(httpGet);
        try {
            if (response.getStatusLine().getStatusCode() == HttpStatus.SC_OK) {
                html = EntityUtils.toString(response.getEntity(), "UTF-8");
            }
            Document document = Jsoup.parse(html);
            ArrayList<Element> elements = document.select(".js-issue-row");
            FileUtils.writeLines(csvFile, Collections.singleton("number,author,title"), true);
            if ((elements.size() >= n) && (n >= 0)) {
                for (int i = 0; i < n; i++) {
                    String number = elements.get(i).attr("id").split("_")[1];
                    String title = elements.get(i).select("[data-hovercard-type=pull_request]").text();
                    String author = elements.get(i).select("[data-hovercard-type=user]").text();
                    StringBuffer pr = new StringBuffer().append(number).append(",").append(author).append(",").append(title);
                    FileUtils.writeLines(csvFile, Collections.singleton(pr), true);
                }
            }

        } finally {
            response.close();
        }
    }

}

```

配置Maven
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>hcsp</groupId>
    <artifactId>save-pull-requests-to-csv</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <repositories>
        <repository>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        </repository>
    </repositories>
    <dependencies>
        <dependency>
            <groupId>org.kohsuke</groupId>
            <artifactId>github-api</artifactId>
            <version>1.95</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.opencsv</groupId>
            <artifactId>opencsv</artifactId>
            <version>4.6</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.6.0</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.6.0</version>
            <scope>test</scope>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.11</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.jsoup/jsoup -->
        <dependency>
            <groupId>org.jsoup</groupId>
            <artifactId>jsoup</artifactId>
            <version>1.12.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.6</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.1</version>
                <configuration>
                    <argLine>-Dfile.encoding=UTF-8</argLine>
                </configuration>
            </plugin>
                <plugin>
                    <artifactId>maven-checkstyle-plugin</artifactId>
                    <version>3.1.0</version>
                    <configuration>
                        <configLocation>${basedir}/.circleci/checkstyle.xml</configLocation>
                        <includeTestSourceDirectory>true</includeTestSourceDirectory>
                        <enableRulesSummary>false</enableRulesSummary>
                    </configuration>
                    <executions>
                        <execution>
                            <id>compile</id>
                            <phase>compile</phase>
                            <goals>
                                <goal>check</goal>
                            </goals>
                        </execution>
                    </executions>
                    <dependencies>
                        <dependency>
                            <groupId>com.puppycrawl.tools</groupId>
                            <artifactId>checkstyle</artifactId>
                            <version>8.29</version>
                        </dependency>
                    </dependencies>
                </plugin>
        </plugins>
    </build>
</project>
```
