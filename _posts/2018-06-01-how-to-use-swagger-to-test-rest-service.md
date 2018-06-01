---
layout: post
title: "How to use swagger to test Rest Api web service in .Net 2017"
date: 2018-06-01
---

In last blog we created Rest API Service but now we are going to test it. The most popular way now days is to use Swagger so we are going to do that in this blog. Here is a link to my previous project https://github.com/elsame/RestfulService .

We are going to implement Swagger using SwashBuckle. Open the project that we created in my last blog. Right click on the project and select Manage NuGet packages and in the browse search window we look for SwashBuckle and install it.

![My helpful screenshot]({{ "/assets/article2_pic1.jpg" | absolute_url }})

When finished installing right click on the project and select Properties.
In the Build page select XML Documentation file.

![My helpful screenshot]({{ "/assets/article2_pic2.jpg" | absolute_url }})

Open EmployeeModelsController.cs file. 
We need to add xml comment for each action in the class. So the code should look like this:

```
/// <summary>
/// Retrieves the list of employees
/// </summary>
/// <returns></returns>
public IQueryable<EmployeeModel> GetEmployees()
{
    return db.Employees;
}

/ <summary>
/// Retrieves one value from the list of employees
/// </summary>
/// <param name="id">The id of the item to be retrieved</param>
/// <returns></returns>
 [ResponseType(typeof(EmployeeModel))]
        public IHttpActionResult GetEmployeeModel(int id)
        {
            EmployeeModel employeeModel = db.Employees.Find(id);
            if (employeeModel == null)
            {
                return NotFound();
            }

            return Ok(employeeModel);
        }
        
/// <summary>
/// Change a single value in the list
/// </summary>
/// <param name="id">The id of the value to be changed</param>
/// <param name="employeeModel">The new value</param>
ResponseType(typeof(void))]
public IHttpActionResult PutEmployeeModel(int id, EmployeeModel employeeModel)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    if (id != employeeModel.Id)
    {
        return BadRequest();
    }

    db.Entry(employeeModel).State = EntityState.Modified;

    try
    {
        db.SaveChanges();
    }
    catch (DbUpdateConcurrencyException)
    {
        if (!EmployeeModelExists(id))
        {
            return NotFound();
        }
        else
        {
            throw;
        }
    }

    return StatusCode(HttpStatusCode.NoContent);
}
        
/// <summary>
/// Insert a new value in the list
/// </summary>
/// <param name="employeeModel">New value to be inserted</param>
[ResponseType(typeof(EmployeeModel))]
        public IHttpActionResult PostEmployeeModel(EmployeeModel employeeModel)
        {
            if (!ModelState.IsValid)
            {
                return BadRequest(ModelState);
            }

            db.Employees.Add(employeeModel);
            db.SaveChanges();

            return CreatedAtRoute("DefaultApi", new { id = employeeModel.Id }, employeeModel);
        }
```
Then open SwaggerConfig.cs class and add this method to it:
        
```
protected static string GetXmlCommentsPath()
  {
      return System.String.Format(@"{0}\bin\RestfulService.XML", 
          System.AppDomain.CurrentDomain.BaseDirectory);
  }
```
Uncomment the following block of code inside the “Register” method:

```
c.IncludeXmlComments(GetXmlCommentsPath());
```
        
Now run the application. My application runs at http://localhost:52651 so I add /Swagger/ui/index to the url and then I can select EmployeeModels and it looks like this:

![My helpful screenshot]({{ "/assets/article2_pic3.jpg" | absolute_url }})

So now I can easily test my Rest API Service. I can select Post method and insert values for example:


![My helpful screenshot]({{ "/assets/article2_pic4.jpg" | absolute_url }})

And hit the "Try it out" button and get an answer looking like this:

![My helpful screenshot]({{ "/assets/article2_pic5.jpg" | absolute_url }})

So now I want to get the list of Employees and now I should be an Employee at this company.

![My helpful screenshot]({{ "/assets/article2_pic6.jpg" | absolute_url }})

So I hit the Try out button. And I get this as a answer:

![My helpful screenshot]({{ "/assets/article2_pic7.jpg" | absolute_url }})

So yes there is an Employee at the company now that has the name Elsa and lives in Hafnarfjörður and has Id 1.
You can play further with this and you see that it is pretty easy to create a Rest API service connected to database.  
You can then use the Swagger xml file to create tests and use for automated testing.
So next up I we just need to create web client to use this rest API service. I will do it in my next blog.

