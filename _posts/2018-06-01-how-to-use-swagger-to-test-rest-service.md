---
layout: post
title: "How to use swagger to test Rest Api web service in .Net 2017"
date: 2018-06-01
---

In last blog we created Rest API Service but now we are going to test it. The most popular way now days is to use Swagger so we are going to do that in this blog. Here is a link to my previous project: https://github.com/elsame/RestfulService

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
        // GET: api/EmployeeModels
        public IQueryable<EmployeeModel> GetEmployees()
        {
            return db.Employees;
        }

        /// <summary>
        /// Retrieves one value from the list of employees
        /// </summary>
        /// <param name="id">The id of the item to be retrieved</param>
        /// <returns></returns>
        // GET: api/EmployeeModels/5
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
        // PUT: api/EmployeeModels/5
        [ResponseType(typeof(void))]
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
        // POST: api/EmployeeModels
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

        /// <summary>
        /// Delete an item from the list
        /// </summary>
        /// <param name="id">id of the item to be deleted</param>
        // DELETE: api/EmployeeModels/55
        [ResponseType(typeof(EmployeeModel))]
        public IHttpActionResult DeleteEmployeeModel(int id)
        {
            EmployeeModel employeeModel = db.Employees.Find(id);
            if (employeeModel == null)
            {
                return NotFound();
            }

            db.Employees.Remove(employeeModel);
            db.SaveChanges();

            return Ok(employeeModel);
        }
        ```
        Then open 

