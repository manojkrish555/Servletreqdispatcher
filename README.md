## mvn generate in single line
```
mvn archetype:generate 
-DgroupId=com.kg.webapp1  
-DartifactId=webapp1 
-DarchetypeArtifactId=maven-archetype-webapp 
-DinteractiveMode=false
```

## remove web.xml from your project

## java 8 and failonmissing web.xml
```
  <properties>
    <failOnMissingWebXml>false</failOnMissingWebXml>
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.source>1.8</maven.compiler.source>
  </properties>
```

## add dependencies
```
<dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
        <version>2.3.1</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>jstl</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
```

## add plugins

```
 <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <port>9090</port>
          <path>/</path>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.1.0</version>
        <configuration>
          <!-- put your configurations here -->
        </configuration>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
```
## index.jsp 
```
<a href="/user.jsp">List All Users</a>
```

## user.jsp
```
<form action="" method="GET">
        <label>Username :</label>
        <input type="text" name="username" id="username">
        <input type="submit">
    </form>
```
## create servlets
```
package com.kg.webapp1;

import java.io.IOException;
import java.util.ArrayList;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.annotation.WebServlet;
import javax.servlet.RequestDispatcher;

/**
 * UserServlet
 */

@WebServlet("/userservlet")
public class UserServlet extends HttpServlet {

    ArrayList<String> users = new ArrayList<String>();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("doGet called");

        String username = req.getParameter("username");
        System.out.println(username);

        users.add(username);

        System.out.println(users);

        req.setAttribute("username", username);
        req.setAttribute("users", users);
        RequestDispatcher dispatcher = req.getRequestDispatcher("user.jsp");
        dispatcher.forward(req, resp);
    }
}
```
## user.jsp
```
<!DOCTYPE html>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

        <html lang="en">

        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <meta http-equiv="X-UA-Compatible" content="ie=edge">
            <title>Document</title>
        </head>

        <body>
            <form action="userservlet" method="GET">
                <label>Username :</label>
                <input type="text" name="username" id="username">
                <input type="submit">
            </form>
            <hr> ${username}

            <h1>All Users</h1>
            ${users}
            <hr>

            <div align="center">
                <c:if test="${users != null}">
                    <table border="1" cellpadding="5">
                        <caption>
                            <h2>List of Users</h2>
                        </caption>
                        <tr>
                            <th>username</th>
                            <th>Actions</th>
                        </tr>

                        <c:forEach var="user" items="${users}">
                            <tr>
                                <td>
                                    <c:out value="${user}" />
                                </td>
                                <td>
                                    <a href="/edit?id=<c:out value='${book.id}' />">Edit</a>
                                    &nbsp;&nbsp;&nbsp;&nbsp;
                                    <a href="/delete?id=<c:out value='${book.id}' />">Delete</a>
                                </td>
                            </tr>
                        </c:forEach>
                </c:if>
                </table>
            </div>
        </body>

        </html>
```
# Servletreqdispatcher
