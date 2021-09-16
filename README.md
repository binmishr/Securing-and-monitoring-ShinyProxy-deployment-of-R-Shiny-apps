# Securing-and-monitoring-ShinyProxy-deployment-of-R-Shiny-apps

The details of the codeset and plots are included in the attached Microsoft Word Document (.docx) file in this repository. 
You need to view the file in "Read Mode" to see the contents properly after downloading the same.


Introduction
================

A while ago I have seen a tutorial on deploying R Shiny apps using ShinyProxy. Following it, you should be able to deploy a functional Shiny app on AWS EC2 instance that can serve multiple users simultaneously. However, the app is not really production quality due to a couple of flaws. First, the authentication method of ShinyProxy was set to ‘Simple’, meaning that the username and password were stored as plain text in the application.yml. Second, the communication between users and the AWS instance is not secured as we didn’t set up the HTTPS. Third, we were using the default AWS domain, which is hardly user friendly. Finally, we need a proper function to log usage statistics and system metrics in order to maintain the apps in the long-term. In this tutorial, I will address these four issues by extending the current deployment framework utilising a couple of easy-to-set-up services.

In nutshell, we will use:

1.Certbot(Let’s Encrypt) together with a valid domain name to set up the HTTPS site and use
2.Nginx as a proxy to route HTTP traffic to HTTPS. For authentication, we will use
3.AWS Cognito, a service provided by AWS that uses OpenID Connect protocol, which ShinyProxy also supports. For logging and monitoring, we will use
4.InfluxDB, an open-source time-series database, to store the usage statistics from ShinyProxy. We will use
5.Telegraf to collect other metrics about the host machine and Docker containers and feed those to InfluxDB. Finally, we will use
6.Grafana, a well-established database analytics and monitoring platform to visualise the metrics. The graph below illustrates the framework.

![image](https://user-images.githubusercontent.com/26252963/133567578-4abec46e-b8be-4b43-b968-b1eb65989710.png)
