---
layout: post
title: "How to create Web Client using Rest Service in .Net 2017"
date: 2018-06-04
---

I´m showing how easy it is to create Web client using Rest service in Visual Studio 2017.
So here you can find the finish project of the Web Client : <https://github.com/elsame/WebClientForRestService> and it uses the Rest Service I created in my earlier blog: <https://github.com/elsame/RestfulService>

This blog is based on the article <https://www.c-sharpcorner.com/article/consuming-asp-net-web-api-rest-service-in-asp-net-mvc-using-http-client/>

So you can also fallow that article. The only thing here that is different is that I add the code for all the view, not just the employee list and I´m using my Rest service that uses Entity Framework and Local Database.

## Step 1 ##

Open Visual Studio 2017. Select from Menu File->New->Project in Visual Studio 2017 select ASP.Net Web Application (.Net framework) and call it WebClientForRestService

![My helpful screenshot]({{ "/assets/article3_pic1.jpg" | absolute_url }})
In the next screenshot we select Empty and MVC:

![My helpful screenshot]({{ "/assets/article3_pic2.jpg" | absolute_url }})

Click "Ok" button and wait.

## Step 2 ##

We are going to use HttpClient to consume the Web API REST Service, so we need to install this library from NuGet Package Manager .
Right click on the project and select Manage NuGet packages.
HttpClient is base class which is responsible to send HTTP request and receive HTTP response resources i.e from REST services.
 
To install HttpClient, right click on Solution Explorer of created application and search for HttpClient, as shown in the following image.

Browse for system.net.http and install it.
 
![My helpful screenshot]({{ "/assets/article3_pic3.jpg" | absolute_url }})

## Step 3 ##

Install WebAPI.Client library from NuGet.
 
This package is used for formatting and content negotiation which provides support for System.Net.Http. To install, right click on Solution Explorer of created application and search for WebAPI.Client, as shown in following image.

![My helpful screenshot]({{ "/assets/article3_pic4.jpg" | absolute_url }})

## Step 4 ##

Right click on Models map and select Add->class  and name it EmployeeModel.cs and add this code to it: 
