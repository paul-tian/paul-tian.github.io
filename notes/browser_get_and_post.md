---
layout: default
---

# Use Browser to add data into Database

## Step 0 Polish the Database Display Webpage

At first, the  ```test.jsp``` which created in [This Note](/notes/tomcat_and_mysql)(Section 3.2.3)
has evolved to a better situation, and renamed as ```display.jsp```:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%request.setCharacterEncoding("UTF-8"); %>

<html>
<meta http-equiv="Refresh" content="3;url=display.jsp">
<!-- auto refresh every 3 seconds-->

<sql:query var="data" dataSource="jdbc/demo">
    select * from user
</sql:query>

<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Database Test</title>
</head>

<body>
    <table border="1" width="450" align="center">
        <!--use table for better display-->
        <tr>
            <th align="center">id</th>
            <th align="center">name</th>
            <th align="center">password</th>
        </tr>
        <c:forEach var="row" items="${data.rows}">
            <tr>
                <td align="center">
                    <c:out value="${row.id}" />
                </td>
                <td align="center">
                    <c:out value="${row.name}" />
                </td>
                <td align="center">
                    <c:out value="${row.password}" />
                </td>
            </tr>
        </c:forEach>
    </table>

    <br>
    <a href="main.jsp">back</a>

</body>

</html>
```

Now this webpage can display the data from database much easier to read and will no longer need to refresh it manually.

## Step 1 JSP Operation

To update database, the most important step is to let the server know your update command, for example:

```sql
insert into user (id, name, password) values(X, Y, Z);
```

Here we also use JSP to recieve the request send form users(the sending method could be found in Step 2).

```op.jsp```:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@page import="java.sql.*"%>

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%request.setCharacterEncoding("UTF-8"); %>
<%-- enable UTF-8 --%>

<%
int id = Integer.parseInt(request.getParameter("getid"));
String name = request.getParameter("getname");
String password = request.getParameter("getpassword");
%>
<%-- get user input --%>

<%
Class.forName("com.mysql.jdbc.Driver");
String dburl = "jdbc:mysql://localhost:3306/demo";
String dbname = "root";
String dbpassword = "rootroot";
%>
<%-- database info --%>

<%
Connection conn = DriverManager.getConnection(dburl, dbname, dbpassword);
String sql = "insert into user (id, name, password) values(?, ?, ?)";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setInt(1, id);
ps.setString(2, name);
ps.setString(3, password);
ps.executeUpdate();
ps.close();
conn.close();
%>
<%-- insert data and close connection --%>
```

These codes could get the input data and insert them into database, try  
```http://localhost:8080/test/op.jsp?getid=255&getname=Browser&getpassword=MySQL``` in a browser if can't wait to see what this jsp page can achieve.

If add the following codes at the bottom of the ```op.jsp```, it will automatically redirect to the ```display,jsp```, which make it easier to see the result:

```html

<html>
<meta http-equiv="Refresh" content="3;url=display.jsp">

<head>
    <title>Redirecting</title>
</head>

<body>
    <h1 align="center">dispiay page will show in 3 seconds</h1>
</body>

</html>
```

## Step 2 About HTTP Method

The way to insert a new data in Step 1 is

```html
http://localhost:8080/test/op.jsp?getid=255&getname=Browser&getpassword=MySQL
```

Which actually used the [HTTP GET method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) to send the data that the ```op.jsp``` needs. As it shows, the variables need to send to ```op.jsp``` are plain text in the URL, which makes it unsafe on the network.

An alternative method is [HTTP POST method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST), and is not hard to implement:  

```main.jsp```

```jsp
<%request.setCharacterEncoding("UTF-8"); %>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<html>

<meta http-equiv="Content-Type" content="text/html; charset=utf-8">

<script type="text/javascript">
    function show() {
        location.href = 'display.jsp';
    }
</script>

<body>
    <form action="op-redir.jsp" method="POST">
        <!-- the method here can change to "GET" to compare -->
        <table align="center">
            <tr>
                <td align="center" colspan="2">
                    <h2>Add User info</h2>
                    <hr>
                </td>
            </tr>

            <tr>
                <td align="right">id</td>
                <th><input type="text" name="getid" /></th>
            </tr>

            <tr>
                <td align="right">name</td>
                <th><input type="text" name="getname" /></th>
            </tr>

            <tr>
                <td align="right">password</td>
                <th><input type="text" name="getpassword" /></th>
            </tr>

            <tr>
                <td align="right"><input id="submit" type="button" onClick="show();" value="show"></td>
                <th align="center"><input id="submit" type="submit" value="add"></th>
            </tr>

        </table>
    </form>

</body>

</html>
```

After input the data and click ```add``` button, the info will send to ```op.jsp``` using post method. Also the form action method could be changed to ```GET```, so they can be compared directly.

[homepage](/)
