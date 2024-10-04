
DATE: 2024-08-05
TIME: 02:19
STATE: #AddOn #
TAGS:  [[Articles]] [[Code]] [[C]] | Reference => [[Block-11-Assignment]] | [[Code]] | [[4 - Indexes/Containers/Container Elements/_Excerpt]] 

In my previous article I discuss about _[create CRUD operation using Scaffolding with ASP.NET MVC 5](https://www.techstrikers.com/Articles/crud-operation-using-scaffold-template-with-entity-framework5.php) and [database first approach using ASP.NET MVC 5 and Entity Framework 6](https://www.techstrikers.com/Articles/database-first-approach-in-entity-framework6-with-asp.net-mvc5.php) and [model first approach using ASP.NET MVC 5 and Entity Framework 6](https://www.techstrikers.com/Articles/model-first-approach-in-entity-framework6-with-asp.net-mvc5.php)_

In this article I am going to discuss how to create web application using code first approach using entity framework 6 with ASP.NET MVC 5. Code-First is mainly focus on Domain Driven Design. This article mainly focuses on how to connect our ASP.NET MVC 5 application with a database using the code first approach. In my previous article [model first approach using ASP.NET MVC 5 and Entity Framework 6](https://www.techstrikers.com/Articles/model-first-approach-in-entity-framework6-with-asp.net-mvc5.php) you learn how to create model entity using designer called "EDMX" and generate database script (DDL).

# NOTE


On the other hand, to supporting a designer-based development, Entity Framework enables more code centric option which we call "code first development". The code first approach gives us an opportunity to mainly concentrate on domain design rather than database design. With code first approach we focus on creating entities/POCO classes as per our model requirement.

It enables you to:

- Start development without having designer or using "EDMX" mapping file.
- Allows you to define your model objects by writing "POCO" classes and relation between classes.
- This approach enables database persistence without explicitly configuring anything.

If you are not expert in database design, this approach is gift for you if you are a good C# developer. You need to focus on your domain class creation. The Entity Framework will take care of creating and managing databases for you.

Using code first approach I will start writing application domain classes and context classes to generate database using Entity Framework 6. This article is using ASP.NET MVC 5 and Entity Framework 6 which will create database and table when application runs first time. Also we will perform complete CRUD operation with code first approach.

So let's open Visual Studio and start creating new MVC 5 project. In this article I am using Visual Studio Express 2013 for Web but you can use Visual Studio 2013 as well.

Now start creating ASP.NET MVC 5 project in Visual Studio 2013.

**Step 1:** Start Visual studio 2013, click on "New" Project from "File" menu and select "Web" and then "ASP.NET web application" from "New Project". I am selecting .Net framework 4.5 to build this sample application. All right, now give proper name as "MVCCodeFirst" to your application and click "Ok" button as below screen shot illustrate:

![](https://www.techstrikers.com/Articles/Images/mvc/create-new-project-4-5.png)

  

**Step 2:** Once done, you will see another window which asked to select "Template". Select "Empty" and checked "MVC" then click on "Ok" button as bellow screen shot illustrate:

![](https://www.techstrikers.com/Articles/Images/mvc/select-empty.jpg)

  

**Step 3:** In order to use Entity Framework 6 in your project, you need to and add Entity Framework 6(If you have not installed on your system) from "Manage NuGet Packages...". To precede this, right click on your project and select "Manage NuGet Packages..." from menu.

![](https://www.techstrikers.com/Articles/Images/mvc/start-nugut-package-mgr.png)

  

**Step 4:** From "Manage NuGet Packages" dialog box select "Online" option and then type "Entity Framework 6" from search text box and click on "Install" button after search result display. Now search for ASP.NET MVC, it will search MVC 5 version now click "Install" button. This will installed all the require DLL references to the project. Click "Close" button. This will install all require DLL references in your project

![](https://www.techstrikers.com/Articles/Images/mvc/select-entity-framework6.png)

  

![](https://www.techstrikers.com/Articles/Images/mvc/add-nuget-asp-net5.png)

  

**Step 5:** All right, In order to start, right click on "Models" folder and select "Add" and then "Class" from popup. Now give proper name to entity class here I am creating entity for "Student.cs".

![](https://www.techstrikers.com/Articles/Images/mvc/code-first-add-entity-class.png)

  

**Step 6:** Now, add bellow code to "Student.cs" class. Notice that I have added "Key" attribute for "Id" property of "Student" class. This attribute tell Entity Framework to create "Id" as "Primary" key for "Student" database table. Also you can see here "DataType" attribute for "DOB" property, which include data type and date formatting string that Entity Framework will consider while creating database and table.

```
using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.Linq;
using System.Web;

namespace MVCCodeFirst.Models
{
    public class Student
    {
        [Key]
        public int Id { get; set; }
        public string Name { get; set; }
        public string Department { get; set; }
        public string EmailId { get; set; }
        public string Genter { get; set; }
        [DataType(DataType.Date),DisplayFormat(DataFormatString = "{0:MM/dd/yyyy}",ApplyFormatInEditMode = true)]
        public DateTime? DOB { get; set; }
        public string Address { get; set; }
        public string City { get; set; }
        public string State { get; set; }
    }
}
```

**Step 7:** I have done with the initial domain class for our sample ASP.NET MVC 5 application. In code first approach, also requires context class which should be derived from DbContext. So let's create our DBContext class next.

**Step 8:** Now again, right click on "Models" folder and select "Add" and then "Class" from popup. Now give proper name to context class, here I am creating "StudentContext.cs" class.

![](https://www.techstrikers.com/Articles/Images/mvc/code-first-add-dbcontext-class.png)

  

**Step 9:** The DBContext class exposes DbSet property for the type that we want to expose from the model, in our example "Students". You can see here DbSet property is a collection of entity type (e.g. "Student"). This DbSet property will be used to perform CRUD operations against a specific type from the model. Now add below code to "StudentContext.cs" class:

```
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Web;

namespace MVCCodeFirst.Models
{
    public class StudentContext: DbContext
    {
        public StudentContext()
            : base("StudentDbContext")
    {
    }
        public DbSet Students { get; set; }
    }
}
```

**Step 10:** Notice in above code, the constructor is used to pass connection string "StudentDbContext" to the base class which will be used to connect to SQL Server to create database and table. By default, the name of the DbContext class will be the name of our database that will be created automatically. You remember one thing here is that the connection string "StudentDbContext" name should be similar to connection string that you configure in "Web.Config" file.

**Step 11:** Now, define a connection string in the web.config file. Notice that in below connection string, I specified "DBStudent" as database name that will be created. The new connection string will be same as following screen shot:

![](https://www.techstrikers.com/Articles/Images/mvc/code-first-config.png)

  

**Step 12:** All right, right click on project and build the application. Now right click on Controller folder and select Add and then "Controller".

![](https://www.techstrikers.com/Articles/Images/mvc/model-first-add-controllser.png)

  

**Step 13:** Now, select scaffold controller template from "Add Scaffold" dialog box and click "Add". It will ask for controller name, give proper name to controller e.g. "StudentController" in this sample application.

![](https://www.techstrikers.com/Articles/Images/mvc/model-first-select-scaffold-controller.png)

  

**Step 14:** In the above step as we selected Scaffold template, it will automatically generate below code to the "StudentController.cs" class file. Notice that the attribute "ValidateAntiForgeryToken" added automatically by ASP.NET MVC 5 code generator, I will discuss more about "ValidateAntiForgeryToken in another Article.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Entity;
using System.Linq;
using System.Net;
using System.Web;
using System.Web.Mvc;
using MVCCodeFirst.Models;

namespace MVCCodeFirst.Controllers
{
public class StudentController : Controller
{
	private StudentContext db = new StudentContext();

	// GET: /Student/
	public ActionResult Index()
	{
		return View(db.Students.ToList());
	}
	// GET: /Student/Details/5
	public ActionResult Details(int? id)
	{
		if (id == null)
		{
			return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
		}
		Student student = db.Students.Find(id);
		if (student == null)
		{
			return HttpNotFound();
		}
		return View(student);
	}
	// GET: /Student/Create
	public ActionResult Create()
	{
		return View();
	}
	[HttpPost]
	[ValidateAntiForgeryToken]
	public ActionResult Create([Bind(Include="Id,Name,Department,
	EmailId,Genter,DOB,Address,City,State")] Student student)
	{
		if (ModelState.IsValid)
		{
			db.Students.Add(student);
			db.SaveChanges();
			return RedirectToAction("Index");
		}
		return View(student);
	}
	// GET: /Student/Edit/5
	public ActionResult Edit(int? id)
	{
		if (id == null)
		{
			return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
		}
		Student student = db.Students.Find(id);
		if (student == null)
		{
			return HttpNotFound();
		}
		return View(student);
	}
	[HttpPost]
	[ValidateAntiForgeryToken]
	public ActionResult Edit([Bind(Include="Id,Name,Department,
	EmailId,Genter,DOB,Address,City,State")] Student student)
	{
		if (ModelState.IsValid)
		{
			db.Entry(student).State = EntityState.Modified;
			db.SaveChanges();
			return RedirectToAction("Index");
		}
		return View(student);
	}
	// GET: /Student/Delete/5
	public ActionResult Delete(int? id)
	{
		if (id == null)
		{
			return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
		}
		Student student = db.Students.Find(id);
		if (student == null)
		{
			return HttpNotFound();
		}
		return View(student);
	}
	// POST: /Student/Delete/5
	[HttpPost, ActionName("Delete")]
	[ValidateAntiForgeryToken]
	public ActionResult DeleteConfirmed(int id)
	{
		Student student = db.Students.Find(id);
		db.Students.Remove(student);
		db.SaveChanges();
		return RedirectToAction("Index");
	}
	protected override void Dispose(bool disposing)
	{
		if (disposing)
		{
			db.Dispose();
		}
		base.Dispose(disposing);
	}
}
}
```

**Step 15:** Notice that, because we selected Scaffold controller, the ASP.NET MVC 5 entity framework will also generate "Views" for us automatically you don't need to add view manually for CRUD operations. See below screen shot:

![](https://www.techstrikers.com/Articles/Images/mvc/code-first-solution.png)

  

**Step 16:** All right, our sample ASP.NET MVC 5 model first application is ready to fly. Run the application, open SQL Server and see "DBStudent" database has been created. This is the beauty of Entity Framework. Now we can perform all the CRUD operations on this database using our sample application.

![](https://www.techstrikers.com/Articles/Images/mvc/code-first-index.png)

  

![](https://www.techstrikers.com/Articles/Images/mvc/code-first-db-create.png)

  

**Step 17:** Our database is ready, now click on "Create New" link on page and insert all fields value and "Save". It will redirect you to the index page as below screen shot:

![](https://www.techstrikers.com/Articles/Images/mvc/code-first-display-list.png)

  

**Step 18:** Now go to SQL Server Management Studio and check "Students" table that was created via code first approach sample application. You can see the inserted record into the "Student" table.

![](https://www.techstrikers.com/Articles/Images/mvc/code-first-database-list.png)

  

## Appreciate your valuable feedback:

I hope this article is useful for you. I look forward for your comments and feedback. So please provide your valuable feedback so that I can make this blog better. You can also share this article by hitting below button.  
**Happy learning...**

  

[Download Code ⇩](https://github.com/techstrikers/Code-first-approach-in-entity-framework6-with-asp.net-mvc5)

[AngularJS Code Examples](https://www.techstrikers.com/AngularJS/Code/angularjs-code-examples.php)

[AngularJS Live Demo](https://www.techstrikers.com/Articles/code-first-approach-in-entity-framework6-with-asp.net-mvc5.php#)

[ASP.NET Code Examples](https://www.techstrikers.com/ASP.NET/Code/aspnet-code-examples.php)

[Javascript Code Examples](https://www.techstrikers.com/Articles/code-first-approach-in-entity-framework6-with-asp.net-mvc5.php#)

[Interview Q & A](https://www.techstrikers.com/Interview-QA/index.php)

[Google MAP API Live Demo](https://www.techstrikers.com/Articles/code-first-approach-in-entity-framework6-with-asp.net-mvc5.php#)

[Google MAP API Code Examples](https://www.techstrikers.com/GoogleMap/Code/google-map-api-code-examples.php)

[Utility Tools](https://www.techstrikers.com/Tools/developers-utility-tools.php)

[Articles](https://www.techstrikers.com/Articles/article-category.php)

#### Recent Articles

- ![...](https://www.techstrikers.com/Articles/Images/asp/aspnet-img-front.png)[Getting Started With ASP.NET Core MVC Application](https://www.techstrikers.com/Articles/asp.net-core-first-mvc-application.php)

- ![...](https://www.techstrikers.com/Articles/Images/asp/aspnet-core-setup-steps.png)[Setup and Installation ASP.NET Core 1.0](https://www.techstrikers.com/Articles/setup-and-installation-aspdotnet-core-1.0.php)

- ![...](https://www.techstrikers.com/Articles/Images/asp/aspnet-core10-front.png)[Understanding ASP.NET Core 1.0](https://www.techstrikers.com/Articles/understanding-asp.net-core1.0.php)

- ![...](https://www.techstrikers.com/Articles/Images/identity/mvc-identity.png)[Customize User Profile Info in MVC With ASP.NET Identity](https://www.techstrikers.com/Articles/customize-user-profile-info-in-mvc-with-asp.net-identity.php)

- ![...](https://www.techstrikers.com/Articles/Images/identity/security-img-edit.jpg)[How to extend the properties of Identity with additional custom properties in ASP.NET](https://www.techstrikers.com/Articles/customize-user-profile-in-asp.net-identity-system.php)

- ![...](https://www.techstrikers.com/Articles/Images/identity/authentication-identity.png)[ASP.NET Identity Authentication - User Login and Registration Form](https://www.techstrikers.com/Articles/asp.net-identity-authentication-user-login-and-registration-form.php)

- ![...](https://www.techstrikers.com/Articles/Images/identity/security-img2.jpg)[Understanding ASP.NET Identity](https://www.techstrikers.com/Articles/understanding-asp.net-identity.php)

- ![...](https://www.techstrikers.com/Articles/Images/identity/log-img-edit.jpg)[Implement custom user registration and login page with the help of Entity Framework using AES encryption in ASP.NET MVC application](https://www.techstrikers.com/Articles/custom-user-registration-and-login-page-with-entity-framework.php)

- ![...](https://www.techstrikers.com/Articles/Images/asp/display-csv-option-front.png)[Export Report as CSV or XLSX or XLS from ReportViewer in ASP.NET](https://www.techstrikers.com/Articles/export-report-as-csv-or-xlsx-or-xls-from-reportviewer-in-asp.net.php)

- ![...](https://www.techstrikers.com/Articles/Images/asp/jqgrid-https-handler-result-list-front.png)[JQGrid Server-Side processing using HttpHandler in ASP.NET](https://www.techstrikers.com/Articles/jqgrid-server-side-processing-using-httpshandler-asp.net.php)

##### TOP TUTORIALS

---

##### [HTML Tutorial](https://www.techstrikers.com/HTML/index.php)

##### [HTML5 Tutorial](https://www.techstrikers.com/HTML5/index.php)

##### [Bootstrap3 Tutorial](https://www.techstrikers.com/Bootstrap/index.php)

##### [Javascript Tutorial](https://www.techstrikers.com/Javascript/index.php)

##### [TypeScript Tutorial](https://www.techstrikers.com/TypeScript/index.php)

##### [AngularJS Tutorial](https://www.techstrikers.com/AngularJS/index.php)

##### [CSharp Tutorial](https://www.techstrikers.com/CSharp/index.php)

##### [.NET Tutorial](https://www.techstrikers.com/DotNet/index.php)

##### [PHP Tutorial](https://www.techstrikers.com/PHP/index.php)

##### [MySQL Tutorial](https://www.techstrikers.com/MySQL/index.php)

##### [PLSQL Tutorial](https://www.techstrikers.com/PLSQL/index.php)

##### INTERVIEW Q & A

---

##### [ASP.NET Q & A](https://www.techstrikers.com/Interview-QA/asp.net-interview-questions-answers.php?lid=2)

##### [WEB API Q & A](https://www.techstrikers.com/Interview-QA/web.api-interview-questions-answers.php?lid=14)

##### [WCF Q & A](https://www.techstrikers.com/Interview-QA/wcf-interview-questions-answers.php?lid=18)

##### [JQuery Q & A](https://www.techstrikers.com/Interview-QA/jquery-interview-questions-answers.php?lid=5)

##### [MVC Q & A](https://www.techstrikers.com/Interview-QA/mvc-interview-questions-answers.php?lid=11)

##### [Bootstrap Q & A](https://www.techstrikers.com/Interview-QA/bootstrap-interview-questions-answers.php?lid=21)

##### [LINQ Q & A](https://www.techstrikers.com/Interview-QA/linq-interview-questions-answers.php?lid=16)

##### [AJAX Q & A](https://www.techstrikers.com/Interview-QA/ajax-interview-questions-answers.php?lid=13)

##### [SQL Server Q & A](https://www.techstrikers.com/Interview-QA/sqlserver-interview-questions-answers.php?lid=8)

##### [C# Q & A](https://www.techstrikers.com/Interview-QA/csharp-interview-questions-answers.php?lid=3)

##### [OOPS Q & A](https://www.techstrikers.com/Interview-QA/oops-interview-questions-answers.php?lid=3)

##### CODE EXAMPLES

---

##### [AngularJS](https://www.techstrikers.com/AngularJS/Code/angularjs-code-examples.php)

##### [Google MAP API V3](https://www.techstrikers.com/GoogleMap/Code/google-map-api-code-examples.php)

##### [ASP.NET](https://www.techstrikers.com/ASP.NET/Code/aspnet-code-examples.php)

##### TOP LIVE DEMO

---

##### [Javascript](https://www.techstrikers.com/Articles/code-first-approach-in-entity-framework6-with-asp.net-mvc5.php#)

##### [AngularJS](https://www.techstrikers.com/Articles/code-first-approach-in-entity-framework6-with-asp.net-mvc5.php#)

##### [Google MAP API V3](https://www.techstrikers.com/Articles/code-first-approach-in-entity-framework6-with-asp.net-mvc5.php#)

##### DEVELOPER TOOLS

---

##### [Html Encode](https://www.techstrikers.com/Tools/html-encode.php)

##### [Html Decode](https://www.techstrikers.com/Tools/html-decode.php)

##### [URL Decode](https://www.techstrikers.com/Tools/url-encode.php)

##### [URL Encode](https://www.techstrikers.com/Tools/url-decode.php)

##### [Base64 Encode](https://www.techstrikers.com/Tools/base64-encode.php)

##### [Base64 Decode](https://www.techstrikers.com/Tools/base64-decode.php)

##### [JSON Beautifier](https://www.techstrikers.com/Tools/json-beautifier.php)

##### LINKS

---

##### [Contact Us](https://www.techstrikers.com/contact-us.php)

##### [Advertise with Us](https://www.techstrikers.com/advertise-with-us.php)

##### [Privacy Policy](https://www.techstrikers.com/privacy-policy.php)

##### [Disclaimer](https://www.techstrikers.com/disclaimer.php)

Copyright ©2024 [www.techstrikers.com](http://www.techstrikers.com/) All rights reserved.  
Unauthorized reproduction/replication of any part of this site is prohibited.

×

#### **Get Our Free Newsletter!**

Email:

NOTE: Your email address will not be shared, and we won't send you any spam.

Close