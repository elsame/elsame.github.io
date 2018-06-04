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

```
public class EmployeeModel
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string City { get; set; }
}
```
## Step 5 ##

Next I need to create Controller class so I right click on the Controllers folder and select Add->Controller and I choose MVC 5 Controller-Empty

![My helpful screenshot]({{ "/assets/article3_pic5.jpg" | absolute_url }})

And then click Add. Call it HomeController and wait.

## Step 6 ##

Now replace the code inside HomeController with this code:

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;

using System.Threading.Tasks;
using System.Net.Http;
using RestfulService.Models;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace WebClientForRestService.Controllers
{
    public class HomeController : Controller
    {
        // GET: Default
        //Hosted web API REST Service base url  
        string Baseurl = "http://localhost:52651";

        private void setClientSettings(HttpClient client)
        {
            //Passing service base url  
            client.BaseAddress = new Uri(Baseurl);

            client.DefaultRequestHeaders.Clear();
            //Define request data format  
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

        }
        public async Task<ActionResult> Index()
        {
            List<EmployeeModel> EmpInfo = new List<EmployeeModel>();

            using (var client = new HttpClient())
            {
                setClientSettings(client);
                //Sending request to find web api REST service resource GetAllEmployees using HttpClient  
                HttpResponseMessage Res = await client.GetAsync("api/EmployeeModels");

                //Checking the response is successful or not which is sent using HttpClient  
                if (Res.IsSuccessStatusCode)
                {
                    //Storing the response details recieved from web api   
                    var EmpResponse = Res.Content.ReadAsStringAsync().Result;

                    //Deserializing the response recieved from web api and storing into the Employee list  
                    EmpInfo = JsonConvert.DeserializeObject<List<EmployeeModel>>(EmpResponse);

                }
                //returning the employee list to view  
                return View(EmpInfo);
            }
        }

        public async Task<ActionResult> Details(int id)
        {
            EmployeeModel EmpInfo = new EmployeeModel();

            using (var client = new HttpClient())
            {
                setClientSettings(client);
                //Sending request to find web api REST service resource GetAllEmployees using HttpClient  
                HttpResponseMessage Res = await client.GetAsync("api/EmployeeModels/" + id);

                //Checking the response is successful or not which is sent using HttpClient  
                if (Res.IsSuccessStatusCode)
                {
                    //Storing the response details recieved from web api   
                    var EmpResponse = Res.Content.ReadAsStringAsync().Result;

                    //Deserializing the response recieved from web api and storing into the Employee list  
                    EmpInfo = JsonConvert.DeserializeObject<EmployeeModel>(EmpResponse);

                }
                //returning the employee list to view  
                return View(EmpInfo);
            }
        }

        public async Task<ActionResult> Edit(int id)
        {
            EmployeeModel EmpInfo = new EmployeeModel();

            using (var client = new HttpClient())
            {
                setClientSettings(client);
                //Sending request to find web api REST service resource GetAllEmployees using HttpClient  
                HttpResponseMessage Res = await client.GetAsync("api/EmployeeModels/" + id);

                //Checking the response is successful or not which is sent using HttpClient  
                if (Res.IsSuccessStatusCode)
                {
                    //Storing the response details recieved from web api   
                    var EmpResponse = Res.Content.ReadAsStringAsync().Result;

                    //Deserializing the response recieved from web api and storing into the Employee list  
                    EmpInfo = JsonConvert.DeserializeObject<EmployeeModel>(EmpResponse);

                }
                //returning the employee list to view  
                return View(EmpInfo);
            }
        }

        // POST: EmpModel/Edit/5
        [HttpPost]
        public async Task<ActionResult> Edit(int id, EmployeeModel empModel)
        {
            try
            {
                // TODO: Add update logic here
                using (var client = new HttpClient())
                {
                    setClientSettings(client);
                    //serialize object to Json and create the HttpContent
                    HttpContent content = new StringContent(JsonConvert.SerializeObject(empModel));
                    content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("application/json");

                    //Sending request to find web api REST service resource GetAllEmployees using HttpClient  
                    HttpResponseMessage Res = await client.PutAsync("api/EmployeeModels/" + id, content);

                    //Checking the response is successful or not which is sent using HttpClient  
                    if (Res.IsSuccessStatusCode)
                    {
                        //Storing the response details recieved from web api   
                        var EmpResponse = Res.Content.ReadAsStringAsync().Result;

                    }
                }

                return RedirectToAction("Index");
            }
            catch
            {
                return View();
            }
        }

        public async Task<ActionResult> Delete(int id)
        {
            try
            {
                // TODO: Add update logic here
                using (var client = new HttpClient())
                {
                    setClientSettings(client);
                    //Sending request to find web api REST service resource GetAllEmployees using HttpClient  
                    HttpResponseMessage Res = await client.DeleteAsync("api/EmployeeModels/" + id);

                    //Checking the response is successful or not which is sent using HttpClient  
                    if (Res.IsSuccessStatusCode)
                    {
                        //Storing the response details recieved from web api   
                        var EmpResponse = Res.Content.ReadAsStringAsync().Result;

                    }
                }

                return RedirectToAction("Index");
            }
            catch
            {
                return View();
            }
        }

        public ActionResult Create()
        {
            EmployeeModel emp = new EmployeeModel();
            return View(emp);
        }

        [HttpPost]
        public async Task<ActionResult> Create(EmployeeModel empModel)
        {
            try
            {
                // TODO: Add update logic here
                using (var client = new HttpClient())
                {
setClientSettings(client);
                    //serialize object to Json and create the HttpContent
                    HttpContent content = new StringContent(JsonConvert.SerializeObject(empModel));
                    content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("application/json");

                    //Sending request to find web api REST service resource GetAllEmployees using HttpClient  
                    HttpResponseMessage Res = await client.PostAsync("api/EmployeeModels/", content);

                    //Checking the response is successful or not which is sent using HttpClient  
                    if (Res.IsSuccessStatusCode)
                    {
                        //Storing the response details recieved from web api   
                        var EmpResponse = Res.Content.ReadAsStringAsync().Result;

                    }
                }

                return RedirectToAction("Index");
            }
            catch
            {
                return View();
            }
        }
    }
}
```

## Step 7 ##

Now I have to have my RestfulService I created in my last blog up and running like this:

![My helpful screenshot]({{ "/assets/article3_pic6.jpg" | absolute_url }})

## Step 8 ##

Right click on Views folder and select Add->New Folder named Home.
Right Click on the Home folder and select Add->view

![My helpful screenshot]({{ "/assets/article3_pic7.jpg" | absolute_url }})

This is our first view, but we are going to add view for each action so we continue this.
And select again Add->view a fiew times more:

![My helpful screenshot]({{ "/assets/article3_pic8.jpg" | absolute_url }})

![My helpful screenshot]({{ "/assets/article3_pic9.jpg" | absolute_url }})

![My helpful screenshot]({{ "/assets/article3_pic10.jpg" | absolute_url }})

![My helpful screenshot]({{ "/assets/article3_pic11.jpg" | absolute_url }})

When this is done we compile and run. And it should look like this:
(http://localhost:59320/Home/Index)

![My helpful screenshot]({{ "/assets/article3_pic11.jpg" | absolute_url }})

And it should be fully function Web Client where you can get a list of all the employees, create new, edit, see detail and delete employee. So that is it.



