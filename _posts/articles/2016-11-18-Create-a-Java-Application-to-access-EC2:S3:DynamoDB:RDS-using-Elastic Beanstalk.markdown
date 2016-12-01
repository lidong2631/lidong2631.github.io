---
layout: article
date: 2016-11-18
project-date: Nov 2016
category: articles
title: Create a Java Application to access EC2/S3/DynamoDB/RDS using Elastic Beanstalk
---

**AWS Elastic Beanstalk** lets you quickly deploy and manage applications in the AWS cloud without worrying about the infrastructure that runs those applications. You simply upload your application and EB automatically handles the details of capacity provisioning, load balancing, scaling and application health monitoring.

To use Elastic Beanstalk, you create an application, upload an application version in the form of an application source bundle (for example, a java .war file) to Elastic Beanstalk. EB automatically launches an environment and creates and configures the AWS resources needed to run your code. After your environment is launched, you can then manage your environment and deploy new application versions.

Elastic Beanstalk supports several platform configurations for Java applications, including multiple versions of Java with the Tomcat application server and Java-only configurations for applications that do not use Tomcat.

You can use [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/) to use other AWS services from within your Java application. The AWS SDK for Java is a set of libraries that allow you to use AWS APIs from your application code without writing the raw HTTP calls from scratch.

If you use Eclipse IDE to develop your Java application, you can get the [AWS Toolkit for Eclipse](http://docs.aws.amazon.com/toolkit-for-eclipse/v1/user-guide/setup-install.html). The AWS Toolkit for Eclipse is an open source plug-in that lets you manage AWS resources, including Elastic Beanstalk applications and environments, from within the Eclipse IDE.

If the command line is more your style, install the [Elastic Beanstalk Command Line Interface](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html) (EB CLI) and use it to create, monitor, and manage your Elastic Beanstalk environments from the command line. If you run multiple environments for your application, the EB CLI integrates with Git to let you associate each of your environments with a different Git branch.

To demonstrate, I installed AWS Toolkit for Eclipse plug-in and after that, in Eclipse, click File->New->Project, under AWS folder, click "AWS Java Web Project". Give it a name and click Finish. 

The project would be looked like below:

![ProjectStructure](/img/articles/20161118/ProjectStructure.png)

Put the following codes in index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8" pageEncoding="utf-8" %>
<%@ page import="com.amazonaws.*" %>
<%@ page import="com.amazonaws.auth.*" %>
<%@ page import="com.amazonaws.auth.profile.*" %>
<%@ page import="com.amazonaws.services.ec2.*" %>
<%@ page import="com.amazonaws.services.ec2.model.*" %>
<%@ page import="com.amazonaws.services.s3.*" %>
<%@ page import="com.amazonaws.services.s3.model.*" %>
<%@ page import="com.amazonaws.services.dynamodbv2.*" %>
<%@ page import="com.amazonaws.services.dynamodbv2.model.*" %>
<%@ page import="com.amazonaws.services.rds.*" %>
<%@ page import="com.amazonaws.services.rds.model.*" %>
<%@ page import="java.sql.*" %>

<%! // Share the client objects across threads to
    // avoid creating new clients for each web request
    private AmazonEC2         ec2;
    private AmazonS3           s3;
    private AmazonDynamoDB dynamo;
    private AmazonRDS 		  rds;
 %>

<%
    /*
     * AWS Elastic Beanstalk checks your application's health by periodically
     * sending an HTTP HEAD request to a resource in your application. By
     * default, this is the root or default resource in your application,
     * but can be configured for each environment.
     *
     * Here, we report success as long as the app server is up, but skip
     * generating the whole page since this is a HEAD request only. You
     * can employ more sophisticated health checks in your application.
     */
    if (request.getMethod().equals("HEAD")) return;
%>

<%
    if (ec2 == null) {
        AWSCredentialsProviderChain credentialsProvider = new AWSCredentialsProviderChain(
            new InstanceProfileCredentialsProvider(),
            new ProfileCredentialsProvider("default"));

        ec2    = new AmazonEC2Client(credentialsProvider);
        s3     = new AmazonS3Client(credentialsProvider);
        dynamo = new AmazonDynamoDBClient(credentialsProvider);
        rds    = new AmazonRDSClient(credentialsProvider);
    }
%>

<%
	// Read RDS connection information from environment
	String dbName = System.getProperty("RDS_DB_NAME");
	String userName = System.getProperty("RDS_USERNAME");
	String password = System.getProperty("RDS_PASSWORD");
	String hostname = System.getProperty("RDS_HOSTNAME");
	String port = System.getProperty("RDS_PORT");
	String jdbcUrl = "jdbc:mysql://" + hostname + ":" +
		port + "/" + dbName + "?user=" + userName + "&password=" + password;
	
	// Load JDBC driver
	try {
		System.out.println("Loading driver...");
		Class.forName("com.mysql.jdbc.Driver");
		System.out.println("Driver loaded!");
	} catch (ClassNotFoundException e) {
		throw new RuntimeException("Cannot find the driver in the classpath!", e);
	}
	
	Connection conn = null;
	Statement readStatement = null;
	ResultSet resultSet = null;
	String results = "";
	int numresults = 0;
	String statement = null;
		
	try {
		conn = DriverManager.getConnection(jdbcUrl);
		
		readStatement = conn.createStatement();
		resultSet = readStatement.executeQuery("SELECT * FROM YOUR_TABLE;");
		
		resultSet.next();
		results = resultSet.getString(RESOURCE_NAME);
		 
		resultSet.close();
		readStatement.close();
		conn.close();
		
	} catch (SQLException ex) {
		// Handle any errors
		System.out.println("SQLException: " + ex.getMessage());
		System.out.println("SQLState: " + ex.getSQLState());
		System.out.println("VendorError: " + ex.getErrorCode());
	} finally {
		System.out.println("Closing the connection.");
		if (conn != null) try { conn.close(); } catch (SQLException ignore) {}
	}
%>

<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-type" content="text/html; charset=utf-8">
    <title>Hello AWS Web World!</title>
    <link rel="stylesheet" href="styles/styles.css" type="text/css" media="screen">
</head>
<body>
    
    <div id="content" class="container">
        
        <div class="section s3">
            <h2>Amazon S3 Buckets:</h2>
            <ul>
            <% for (Bucket bucket : s3.listBuckets()) { %>
               <li> <%= bucket.getName() %> </li>
            <% } %>
            </ul>
        </div>

        <div class="section sdb">
            <h2>Amazon DynamoDB Tables:</h2>
            <ul>
            <% for (String tableName : dynamo.listTables().getTableNames()) { %>
               <li> <%= tableName %></li>
            <% } %>
            </ul>
        </div>

        <div class="section ec2">
            <h2>Amazon EC2 Instances:</h2>
            <ul>
            <% for (Reservation reservation : ec2.describeInstances().getReservations()) { %>
                <% for (Instance instance : reservation.getInstances()) { %>
                   <li> <%= instance.getInstanceId() %></li>
                <% } %>
            <% } %>
            </ul>
        </div>
        
        <div class="section">
        	<h2>Amazon RDS Tables:</h2>
        	<p>Established connection to RDS. Read first row: <%= results %></p>
        </div>
    </div>
</body>
</html>
```

<br>
This simple jsp file would access to your EC2, S3, DynamoDB and print the corresponding resource name. It also execute a simple query using mysql JDBC driver to fetch data from your RDS database. (You have to make sure you have permissions to access those services!)

Here is my result for the jsp page:

![jspResult](/img/articles/20161118/jspResult.png)