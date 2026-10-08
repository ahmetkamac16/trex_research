# .NET Backend Development - My Research Notes

In this study, I summarized the basic concepts used in backend development in terms of what they do and how they relate to one another. For the examples, I mostly had a simple product and order application in mind. What matters to me is being able to explain the problem a concept solves, in addition to knowing its definition.

## 1. Modern Software Development Practices

### Git and GitHub
#### 1. What Is Git?
Git is a distributed version control system used to track changes made in software projects. It stores the history of a project's files and allows reverting to previous versions when needed.

Git allows multiple developers to work on the same project. Each developer keeps a copy of the project on their own computer and records their changes. This makes it possible to track who made a change, in which file, when, and what kind of change it was.

The main advantages of Git are as follows: 
- It keeps a record of changes made to files.
- It allows reverting to previous versions.
- It makes it easier for multiple developers to work on the same project.
- It allows new features to be developed without affecting the main project.
- It helps identify faulty changes.
- It makes managing different versions of the project easier.

Git is mostly used through terminal commands. In addition, graphical Git tools are available in IDEs such as Visual Studio, Visual Studio Code, and JetBrains Rider (PyCharm, IntelliJ IDEA, etc.).

#### 2. What Is GitHub? 
GitHub is a cloud-based platform that allows projects managed with Git to be stored online and shared with team members.

Git is a version control system, while GitHub is a service that hosts Git repositories and supports teamwork. Git can be used on local computers without an internet connection. GitHub is used to store a project on a remote server, share it, and develop it together with team members.

Some of the features GitHub offers are as follows:
- Storing projects in remote repositories.
- Sharing project code with team members.
- Reviewing code changes through a Pull Request. (Pull Request: A formal request to merge changes made to software into the main code, sent to project managers and teammates.)
- Tracking tasks and bugs through the Issue system. (Issue: A built-in task management system used to track bugs, shortcomings, new feature requests, or tasks to be completed in a software project.)
- Maintaining project documentation.
- Creating CI/CD processes with GitHub Actions. (GitHub Actions: A system that automatically runs tasks in a project when specific events occur.)
- Making it easier to contribute to open-source projects.

Besides GitHub, there are alternative Git-based platforms such as GitLab, Bitbucket, and Azure Repos.

#### 3. Git and GitHub in Summary

Git is a distributed version control system that keeps the history of changes to files. GitHub is a platform that hosts Git repositories and provides teamwork features such as pull requests, code reviews, and work tracking. 

I can use Git on my computer without internet access; GitHub makes it easier for me to share this work.

### Basic Git Commands

| Command | What do I use it for? |
| --- | --- |
| `git init` | I start tracking with Git in my current folder. |
| `git clone <repo-url>` | I download an existing repository to my computer along with its history. |
| `git add README.md` | I stage the changes in the file for the next commit. |
| `git commit -m "Git notes added"` | I save the changes I have staged to the local history. |
| `git push origin main` | I send local commits to the remote repository. |
| `git pull` | I fetch the changes from the remote branch and integrate them into my current branch; a merge or rebase may be performed depending on the configuration. |
| `git branch arastirma` | I create a new branch; this command alone does not switch to that branch. |
| `git merge arastirma` | I merge the specified branch into the branch I am currently on. |
| `git checkout -b arastirma` | A new branch is created and switched to. It is also used to switch between branches. |

For example, I can switch to the research branch with `git switch arastirma` and work there. `add`, `commit`, and `push` are not the same operation: they are the steps of staging, saving locally, and sharing.

### Merge Conflict

A Merge Conflict is a conflict that occurs when Git cannot automatically merge two different changes.

For example, if two developers change the same line of the same file in different ways, Git does not decide on its own which change should be kept. In this case, the developer needs to examine the file and choose the correct code.

In this situation, I use `git status` to find the conflicting files; I read the section containing the `<<<<<<<`, `=======`, and `>>>>>>>` markers and edit it to contain the correct content. I then remove the markers, check the result, and use `git add` and `git commit` to complete the merge. I need to understand the purpose of both changes instead of just choosing my own change.

### CI/CD and Pipeline Examples

**CI (Continuous Integration)** is the frequent integration of developers' changes into a shared codebase and the verification of these changes through automated builds and tests. 
**Continuous Delivery** means keeping the verified version ready for deployment; going live may require approval. 
**Continuous Deployment** is the automatic deployment of successful changes to production.

**Benefits of CI/CD**
- Repetitive operations are automated.
- Build and test errors are detected earlier.
- Errors caused by manual operations are reduced.
- New versions are released faster.
- Every change is checked against the same standards.
- Software quality and trust within the team increase.
- The likelihood of faulty code being sent to the production environment is reduced.
- The deployment process is recorded and becomes traceable.

In a .NET project, I can set up the workflow (pipeline) as `restore → build → test → publish → deploy`. `dotnet publish` prepares the deployment files; it does not install the application on the server by itself.

**Commands commonly used in a .NET project:**
- `dotnet restore`: Downloads the NuGet packages required by the project.
- `dotnet build --no-restore`: Builds the project and checks for build errors.
- `dotnet test --no-build`: Runs the project's automated tests.
- `dotnet publish`: Creates the application's deployment-ready files.


**GitHub Actions:** The following example is a CI workflow that can be used in `.github/workflows/dotnet.yml`.

```yaml
name: .NET CI
on: [push, pull_request]
permissions:
  contents: read
jobs:
  kontrol:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '10.0.x'
      - run: dotnet restore
      - run: dotnet build -c Release --no-restore
      - run: dotnet test -c Release --no-build
```

**Azure DevOps:** I can define the same checks in `azure-pipelines.yml`.

```yaml
trigger:
  - main
pool:
  vmImage: ubuntu-latest
steps:
  - task: UseDotNet@2
    inputs:
      packageType: sdk
      version: '10.0.x'
  - script: dotnet restore
  - script: dotnet build -c Release --no-restore
  - script: dotnet test -c Release --no-build
```

These examples assume that an appropriate .NET solution/project and a test project exist in the root directory; they are not workflows to be run in this research repository, which contains only a README. For CD, I add a stage that packages the API project with `dotnet publish` after successful tests and deploys it to the target environment. I do not write production access credentials into YAML; I use a secret or a service connection.


### SDLC (Software Development Life Cycle), Agile, Scrum, and Kanban

SDLC is the life cycle of software from the initial idea through maintenance:

1. **Planning:** The goal, scope, and priorities are determined.
2. **Analysis:** The user's needs and acceptance criteria are clarified.
3. **Design and development:** The data model, architecture, and code are created.
4. **Testing:** Expected behaviors and error scenarios are checked.
5. **Deployment:** The verified version is made available to the user.
6. **Maintenance:** Bugs are fixed, and the product is improved through feedback.

I do not see the developer's role as only writing code. Questioning requirements, writing tests, and investigating issues in production are also part of the job.

- **Agile Methodology**
Agile is a flexible approach to working based on dividing the software development process into small, manageable parts. In traditional methods, all the project's requirements may be determined at the beginning, and the product may be delivered at the end of a long development process. In the Agile approach, working software is delivered to the user in small pieces at frequent intervals.

- **Scrum Methodology**
Scrum is a lightweight framework compatible with the Agile approach. The project is divided into short periods of work called sprints. A sprint is a fixed-length period of work lasting one month or less. The aim is to produce a usable product increment at the end of the sprint; the team reviews its progress regularly.

- **Kanban Methodology** 
Kanban is a method that aims to track work visually and continuously improve the workflow. Tasks are usually displayed on a Kanban board. 

  A simple board might consist of the following columns: 

  To Do → In Progress → In Testing → Done

  The developer updates the status of the task they are working on on the board. This allows team members to easily see which tasks are waiting, in progress, or completed. 

  As a developer, in addition to keeping the board up to date, I pay attention to limiting work in progress (WIP), making bottlenecks visible, and finishing the work I start.

Scrum seems to me a more suitable starting point for planned product development, and Kanban for ongoing maintenance requests; the choice depends on how the team works.

## 2. The .NET Ecosystem

### What Is .NET, and Why Is It Used?

.NET is a software platform consisting of a runtime, libraries, and development tools for developing and running different types of applications. In this structure, developed by Microsoft and the open-source community, C# is the programming language used to write projects, while .NET is the ecosystem that enables this code to be compiled, executed, and managed. Its base class libraries (BCL), automatic memory management (Garbage Collector), and modern frameworks such as ASP.NET Core make it easier to develop cross-platform, secure, and high-performance backend systems.

The platform began with .NET Framework in 2002 and moved in a cross-platform direction with .NET Core in 2016. In 2020, “Core” was removed from the product's name with .NET 5. This progression continued with .NET 6, 7, 8, 9, and 10.

### .NET Framework, .NET Core, and Modern .NET

| Feature | .NET Framework | .NET Core | .NET 7/8 and later |
| --- | --- | --- | --- |
| Position | Common in older Windows applications | The name of modern .NET during the 1.x–3.x era | The continuation of .NET Core |
| Platform | Windows | Windows, Linux, macOS | Windows, Linux, macOS |
| Web | Classic ASP.NET | ASP.NET Core | ASP.NET Core |
| When to choose it | Maintaining existing Framework dependencies | Understanding older Core projects | Choosing a supported version for a new project |

Platform support does not mean that every application type will run everywhere. For example, WPF/Windows Forms depends on Windows, while an ASP.NET Core API can also run on Linux with suitable dependencies.

### `dotnet --info` Output and Interpretation

This command provides information about the .NET installation and the computer environment. Version and path information vary by computer.

An abbreviated version of the actual output obtained in the working environment:

```text
.NET SDK:
 Version: 10.0.401

Runtime Environment:
 OS Name: Windows
 RID: win-x64

.NET SDKs installed:
 10.0.301
 10.0.401

.NET runtimes installed:
 Microsoft.AspNetCore.App 10.0.9
 Microsoft.AspNetCore.App 10.0.12
 Microsoft.NETCore.App 10.0.9
 Microsoft.NETCore.App 10.0.12
 Microsoft.WindowsDesktop.App 10.0.9
 Microsoft.WindowsDesktop.App 10.0.12

global.json file:
 Not found
```

The SDK shows the development and build tools, while the runtime shows the components that run the application. `win-x64` indicates that the environment is Windows and 64-bit. Multiple versions can be installed side by side. Since there is no `global.json` in this location, no project-specific SDK selection has been made; the SDK selected by the command is `10.0.401`.

### Synchronous and Asynchronous Programming

#### Synchronous Programming
In a synchronous operation, the calling code does not proceed to the next step until the called operation is complete. For example, when a file is to be read:
- File reading begins.
- The calling thread waits until the operation is complete.
- The next line runs after the file has been read.

This is an understandable approach for short and simple tasks. If long-running operations are performed on the user interface thread, they can cause the interface to respond slowly (freeze).

#### Asynchronous Programming
In the asynchronous approach, the calling thread is not blocked while waiting for the result of an unfinished operation. For example, in a desktop application waiting for data from a server, the user interface can continue to respond to interactions during this time. 

Asynchrony does not mean that the operation will automatically finish faster or necessarily run on a separate thread. While a thread is blocked waiting for the result of a synchronous call, in asynchronous I/O the thread is released during the wait and returns to the pool to handle other requests. This structure does not shorten the query time; it allows system resources to manage the waiting period much more efficiently. It is particularly useful for input/output (I/O) operations such as network, file, and database operations.

```csharp
// Inside a method using EF Core:
var urunler = db.Urunler.ToList();             // Synchronous
var urunlerAsync = await db.Urunler.ToListAsync(); // Asynchronous
```

- `async` allows `await` to be used in a method; it does not start a new thread by itself.
- `Task` represents the completion of an operation; `Task<T>` also represents its result.
- `await` waits for an operation to complete without blocking the thread if it has not finished yet.
- `ConfigureAwait(false)` indicates that I do not want the continuation to return to the captured context. It may be useful in general-purpose libraries; since ASP.NET Core does not have a custom `SynchronizationContext` by default, it is not a performance rule that must be added to every line.

A typical example for me is waiting asynchronously for the database response while saving an order. An intensive computation, on the other hand, is CPU work; simply writing `async` does not make it parallel. 

### The `=>` Expression in C#

Although it resembles the “arrow function” expression in JavaScript, in C#, `=>` is used for lambdas and expression-bodied members; it does not imply asynchrony.

```csharp
Func<int, int> ikiKat = sayi => sayi * 2; // Lambda
decimal KdvEkle(decimal fiyat) => fiyat * 1.20m; // Short method body
```
### Using async with `=>`

=> does not provide asynchrony by itself. To make a lambda or a short method body asynchronous, the async keyword is added at the beginning, and the return type becomes Task / Task<T>. await can be used in the body.

```csharp
// Asynchronous lambda
Func<int, Task<int>> ikiKatAsync = async sayi =>
{
    await Task.Delay(100); // Example delay
    return sayi * 2;
};

// Asynchronous short method body
async Task<string> SayfaGetirAsync(string url) =>
    await httpClient.GetStringAsync(url);

// Usage
int sonuc = await ikiKatAsync(5);          // 10
string html = await SayfaGetirAsync("https://ornek.com");
```

In these examples, => shortens the syntax by connecting the parameters on the left to the body on the right; it is the async keyword added at the beginning that provides asynchrony. In the ikiKatAsync lambda, the await Task.Delay(100) line waits for 100 ms without blocking the thread and then returns the result of sayi * 2 as a Task<int>. In the SayfaGetirAsync method, => defines the body on a single line without writing braces or return; the method allows the rest of the program to run while downloading the page content. The await expressions in the usage section wait for the tasks to complete and assign the actual values inside the Tasks to the variables; without await, a Task object that has not yet finished would be assigned to the variables instead of the result.

## 3. Fundamentals of Backend Development

### Backend, frontend, web server, and API

#### What Is a Backend?

The backend is the part of an application that runs on the server side. It processes requests from users, applies the application's rules, communicates with the database, and sends the necessary results to the user.

For example, in a shopping application, the backend performs the following operations:
- Checks the user's login credentials.
- Retrieves products and stock information from the database.
- Creates orders.
- Checks which operations the user is allowed to perform.

#### What Is a Frontend?

The frontend is the part that the user sees and interacts with. Page design, menus, buttons, and forms are part of the frontend.

We can explain the difference using a login screen:
- The user enters their email and password into the form on the frontend.
- The frontend sends this information to the backend.
- The backend checks the information and returns the login result.
- The frontend displays a message to the user or opens the relevant page based on the result.

> Technologies such as HTML, CSS, and JavaScript are used in frontend development; JavaScript/Node.js, Python, Java, C#, and PHP are used in backend development.

#### What Is a Web Server?

A web server is software that receives and responds to HTTP requests from clients (the term also describes the server hardware on which this software runs). In the ASP.NET Core architecture, Kestrel is the main built-in server that receives and processes requests.

The client is usually a browser or mobile application. When a user opens a web address, the client sends a request to the server. The server returns a response such as an HTML page, an image, a file, or JSON data.

Nginx, Apache, and IIS are common examples of web servers.

#### What Is an API? 

An API (Application Programming Interface), known in Turkish as Uygulama Programlama Arayüzü, enables software systems to communicate with one another through defined rules.

For example, a weather application can retrieve temperature data by sending city information to another system's API.

An address that an API provides for a specific operation is called an endpoint, meaning an end point.
>https://api.ornek.com/urunler
This address can be used to access operations related to products.

API Types by Access Level: 
- Public API: Made available to external developers. It may still require an API key or paid access.
- Private API: Used by an organization's own applications or teams.
- Partner API: Provides access to specific companies or business partners.

API Types by Communication Approach:
- REST: An architectural approach that focuses on resources.
- SOAP: A communication protocol that works with XML-based messages.
- GraphQL: A query language and execution system that allows the client to specify the data fields it wants.

The concept of an API is not limited to web services; operating systems and software libraries can also provide APIs.

### HTTP and basic methods

HTTP - Hypertext Transfer Protocol is the request/response protocol between a client and a server. HTTPS protects this communication with TLS.

| Method | Example | Purpose |
| --- | --- | --- |
| GET | `/api/urunler/5` | Read product information |
| POST | `/api/urunler` | Create a product using the submitted information |
| PUT | `/api/urunler/5` | Replace the product's updatable representation in its entirety |
| DELETE | `/api/urunler/5` | Delete the product |

Repeating GET, PUT, and DELETE preserves the same intended server state; this is called **idempotency**. Response codes do not have to be the same. With POST, repeating a request may create a new record. PATCH can be used for partial updates. 

#### GET - Reading Data
Used to retrieve a resource or a list of resources.

**GET /urunler/15** (This request asks for the information of the product with the ID 15.)

Example response:

```json
{
  "id": 15,
  "ad": "Keyboard",
  "fiyat": 1200
}
```

#### POST - Sending Data and Creating Resources
Generally used to create a new resource or initiate an operation on the server.

**POST /urunler**

```json
{
  "ad": "Mouse",
  "fiyat": 650
}
```

When the request is processed successfully, a product named Mouse with a price of 650 can be created.

#### PUT - PATCH Usage and Difference 
When we want to set the product's price to 1500, PUT and PATCH are used as follows:

**PUT /urunler/15**

```json
{
  "ad": "Mechanical Keyboard",
  "fiyat": 1500,
  "stok": 20
}
```

**PATCH /urunler/15**

```json
{
  "fiyat": 1500
}
```
Revise it from scratch with PUT, and change only the desired data with PATCH. The PATCH body here is meaningful if the API supports this format.


#### DELETE - Deleting Resources
Used to delete a specific resource.

**DELETE /urunler/15** (This request asks to delete the product with the ID 15.)

#### Common HTTP Status Codes

- 200: The request was processed successfully.
- 201: A new resource was created.
- 204: The operation was successful; there is no response body.
- 400: The request is malformed or invalid.
- 401: Authentication is required.
- 403: The client is not authorized to perform the operation.
- 404: The requested resource was not found.
- 500: An unexpected error occurred on the server.

### The Logic of RESTful Services

In REST, I identify resources such as products with URLs and specify the operation with an HTTP method. Requests must be understandable on their own; the server does not need to rely on client session context from a previous request. This **stateless** rule does not mean that the server cannot have a database. Resources, standard methods, and appropriate status codes make the API easier to understand.

### What Is OpenAPI?

OpenAPI defines how an API is used. It describes, in YAML or JSON format, which addresses requests can be sent to, what information is required, and how responses are received.

### What Is Swagger?

Swagger is a family of tools used to create, view, and test OpenAPI definitions. Common Swagger tools include:
- Swagger Editor: Helps you write the OpenAPI file and helps us see errors in the definition.
- Swagger UI: Reads the definition and displays endpoints, parameters, and example responses in the browser.
- Swagger Codegen: Used to generate client code and a server skeleton from the API definition.

### JSON

JSON (JavaScript Object Notation) is a text-based format used for data exchange:

```json
{
  "id": 5,
  "ad": "Keyboard",
  "fiyat": 850.50,
  "stoktaVar": true,
  "etiketler": ["computer", "accessory"]
}
```

Here, the object contains a number, text, a boolean, and an array. Field names are written in double quotes. I think of JSON as a shared data representation between the frontend and backend; it is not an API or a database on its own.

### Comparison of REST, SOAP, and GraphQL

| Approach | Main feature | Example use case |
| --- | --- | --- |
| REST | An architectural style; resources and HTTP methods are central, and JSON is generally used. | A standard product/order API |
| SOAP | A protocol that uses XML messages; it can be used with WSDL and WS-* standards. | Enterprise integration requiring a SOAP contract |
| GraphQL | A query language and execution model; the client selects the fields it wants. | Screens needing different combinations of data |

I would start with REST in a simple CRUD application. GraphQL can reduce unnecessary data transfer, but query cost and authorization still require attention.

## 4. ASP.NET

### ASP.NET
Classic ASP.NET is a Windows-focused web development technology built on .NET Framework. It enables the development of websites, administration panels, and APIs using C#.

"Classic ASP.NET" generally refers to ASP.NET 4.x running on .NET Framework. This includes technologies such as Web Forms, ASP.NET MVC 5, and Web API 2. 

### ASP.NET Core
ASP.NET Core is a cross-platform, modular web framework that runs on modern .NET. Built-in DI, its middleware structure, and API development support make my work easier. It can be used on Windows, Linux, and macOS.

### Key Advantages
- Platform Independence: Runs on different operating systems.
- Performance: Uses the high-performance Kestrel server.
- Modular Structure: The HTTP pipeline can be configured according to the application's needs.
- Built-in DI: Provides ready-to-use infrastructure for managing dependencies.
- Testability: Makes it easier to test components individually.
- Cloud and Container Compatibility: Can be used in different deployment environments.

### Key Differences
- Runtime Environment: Classic ASP.NET is based on .NET Framework and Windows; ASP.NET Core runs on modern .NET across different platforms.
- Request Processing: HTTP modules and handlers are common in classic ASP.NET. ASP.NET Core uses a middleware pipeline.
- Dependency Management: ASP.NET Core provides a built-in DI container.
- Web Development Model: In classic ASP.NET, MVC and Web API were separate frameworks; in Core, controller-based web pages and APIs share the same MVC infrastructure.

### MVC

MVC separates responsibilities into **Model**, **View**, and **Controller**. The Model represents data and behavior, the View produces the interface, and the Controller receives the request and initiates the appropriate flow. For example, the Controller has the product list prepared, and the View displays it. In a Web API, JSON can be returned instead of an HTML View; I try to avoid letting business rules grow inside the Controller.

### Middleware and ordering

Middleware is a component that takes part in processing an HTTP request. It can pass the request to the next component or complete the response itself. The following order is for an application in which the relevant services have been registered:

```csharp
app.UseExceptionHandler("/hata"); // An error endpoint handling this address is required.
app.UseHttpsRedirection();
app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
```

When the exception handler comes first, it can catch errors from subsequent components. Routing determines the endpoint; authentication establishes the identity, and authorization checks access. This is why authentication must come before authorization. The response travels back through the call chain. In modern projects, these can be found in `Program.cs`, and in older layouts, in `Startup.Configure`.

### Background Worker 

A background worker is a helper process that performs work in the background of an application. It carries out time-consuming tasks while the user continues to use the application.
For example, when you press the “Generate report” button on a website:
1. The application adds the report request to a job queue.
2. The background worker takes this job and prepares the report.
3. When the job is complete, the report becomes available for download or a notification is sent to you.
It is used for tasks such as sending emails, processing videos, and converting files. This allows the application to respond without waiting for long-running jobs to finish.

### Background Job

A background job is a task that runs in the background when triggered by an event, a user action, or a scheduled time. It performs work such as sending emails, preparing reports, or creating backups without preventing the user from using the application.
Its relationship to the previous concept is as follows:
- Background job: The work to be done.
- Background worker: The process that does that work.
For example, “Send a welcome email to this user” is a job. The process that takes this task from the queue and sends the email is the worker.

### Dependency Injection (DI)

With DI, a class receives its dependency from outside instead of creating it internally with `new`. This makes it easier to replace the real service with a fake service in a test.

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddScoped<IFiyatService, FiyatService>();
var app = builder.Build();

app.MapGet("/api/fiyat", (IFiyatService servis) => servis.KdvEkle(100m));
app.Run();

public interface IFiyatService
{
    decimal KdvEkle(decimal fiyat);
}

public class FiyatService : IFiyatService
{
    public decimal KdvEkle(decimal fiyat) => fiyat * 1.20m;
}
```

The endpoint does not create the concrete class; the DI container provides the required object. `Transient` creates an instance on every resolution, `Scoped` creates one per scope (usually per request on the web), and `Singleton` creates one throughout the lifetime of the application's container. I do not choose a lifetime just for speed; in particular, I do not keep a scoped object such as `DbContext` inside a singleton.

A typical call flow in a layered application:

```text
HTTP request → Controller   → Service       → Repository / DbContext → Database
               presentation   business rule   data access
```

### ABP Framework

ABP Framework is an open-source software infrastructure built on C# and ASP.NET Core that makes it easier to develop enterprise web applications in particular. By providing common technical tasks out of the box, it allows the developer to focus on the application's core functions.

Its main offerings:
- User and permission management: Login, roles, and permissions.
- Modular structure: Dividing the application into parts that can be developed independently.
- Multi-tenant structure (multi-tenancy): Allowing multiple customer companies to use the same application.
- Common technical services: Operation logs, background tasks, caching, and data access.
- Architectural organization: Support for layered architecture and Domain-Driven Design (DDD) to manage complex business rules.

## 5. Database and ORM

### SQL and database types

SQL is the language used to query and manipulate data in relational databases. Relational databases organize data with tables, keys, and relationships; SQL Server and PostgreSQL are examples. NoSQL systems use different models such as documents, key-value pairs, or graphs; MongoDB and Redis are examples. I do not think of NoSQL as “having no relationships or schema at all”; the data model and consistency characteristics vary by product.

In an application where the relationships between products, customers, and orders are clear, a relational database is an understandable starting point for me.

### ORM, Entity Framework Core, and DbContext

ORM (Object-Relational Mapping) provides mapping between objects in the application and database records. EF Core is one of the tools that does this for .NET. `DbContext` is the center of querying, change tracking, and save operations. `DbSet<Urun>` lets me query products; `SaveChangesAsync()` writes the tracked changes to the database.

```csharp
using Microsoft.EntityFrameworkCore;

public class MagazaDbContext(DbContextOptions<MagazaDbContext> options)
    : DbContext(options)
{
    public DbSet<Urun> Urunler => Set<Urun>();
}

public class Urun
{
    public int Id { get; set; }
    public string Ad { get; set; } = "";
    public decimal Fiyat { get; set; }
}
```

In addition to these classes, a database provider and connection configuration are required. For example, for SQL Server, `AddDbContext<MagazaDbContext>(o => o.UseSqlServer(connectionString))` is registered. I do not run concurrent operations on the same `DbContext`; the context is not thread-safe.

#### How is DbContext used?


```csharp

// DbContext is injected into the service through the constructor via DI.
public class UrunServisi(MagazaDbContext db)
{
    // Insertion: Add starts tracking the object, and SaveChangesAsync writes it to the database.
    public async Task UrunEkleAsync(string ad, decimal fiyat)
    {
        db.Urunler.Add(new Urun { Ad = ad, Fiyat = fiyat }); // The change is tracked
        await db.SaveChangesAsync();                          // INSERT executes
    }

    // Update: A change made to a tracked object is written with SaveChangesAsync.
    public async Task FiyatGuncelleAsync(int id, decimal yeniFiyat)
    {
        var urun = await db.Urunler.FindAsync(id);            // The record is found and tracked
        if (urun is null) return;

        urun.Fiyat = yeniFiyat;                               // The change is tracked
        await db.SaveChangesAsync();                          // UPDATE executes
    }

    // Reading: A query is written through DbSet.
    public Task<List<Urun>> HepsiniGetirAsync()
        => db.Urunler.ToListAsync();                          // SELECT executes
}
```


### LINQ and SQL equivalents

LINQ is a construct that lets me query data within C#. It can also be used with in-memory collections; in EF Core, supported expressions are translated into SQL by the provider.

```csharp
var urunler = await db.Urunler
    .Where(u => u.Fiyat > 500)
    .OrderBy(u => u.Fiyat)
    .Select(u => new { u.Ad, u.Fiyat })
    .ToListAsync();
```

Logical SQL equivalent:

```sql
SELECT Ad, Fiyat
FROM Urunler
WHERE Fiyat > 500
ORDER BY Fiyat;
```

`Where` filters, `Select` selects fields, and `OrderBy` sorts. `ToListAsync` retrieves the result; the preceding steps build the query. The details of the generated SQL may vary by provider.

| LINQ example | Meaning in SQL |
| --- | --- |
| `CountAsync()` | Counting with `COUNT(*)` |
| `AnyAsync(u => u.Fiyat > 500)` | Checking whether a record exists using `EXISTS` logic |
| `FirstOrDefaultAsync(u => u.Id == 5)` | Filtering and retrieving at most one record; the default value if none exists |
| `OrderBy(u => u.Id).Skip(10).Take(10)` | Pagination in an ordered result with `OFFSET/FETCH` or `LIMIT/OFFSET` |
| `GroupBy(u => u.Ad).Select(g => new { Ad = g.Key, Adet = g.Count() })` | Grouping with `GROUP BY Ad` and `COUNT(*)` |

### Code-First and Database-First

| Approach | Starting point | When would I consider it? |
| --- | --- | --- |
| Code-First | C# models and configurations; schema changes through migrations | In a new project where I develop the data model alongside the application |
| Database-First | Generating the model and context from an existing database (scaffolding) | With an existing database or one managed by another team |

In Code-First, I also review the generated migration; an incorrect schema change can cause data loss. In Database-First, I consider the risk of overwriting when adding custom code to files that will be regenerated.

### Four basic SQL queries

```sql
SELECT Id, Ad, Fiyat FROM Urunler WHERE Fiyat > 500;
INSERT INTO Urunler (Ad, Fiyat) VALUES ('Keyboard', 850.50);
UPDATE Urunler SET Fiyat = 900 WHERE Id = 5;
DELETE FROM Urunler WHERE Id = 5;
```

These perform reading, insertion, updating, and deletion, respectively. I assumed that the `Id` field is generated automatically. If `WHERE` is omitted from `UPDATE` and `DELETE`, all rows may be affected. I use parameterized queries instead of concatenating user-provided values into SQL text.

## 6. Security and Performance

### Authentication and authorization

**Authentication** verifies who the user is. **Authorization** determines which action that user can perform (User permissions). Preventing a logged-in customer from viewing another customer's order is an authorization matter; checking login status alone is not sufficient.

### JWT (JSON Web Token)

JWT is a token format that carries information called claims. The common signed JWT format is `header.payload.signature`. The header provides information such as the algorithm, the payload provides claims such as user/role/expiration, and the signature provides integrity and source verification.

The user logs in. The server verifies the information, signs the token, and gives it to the client. The client sends the access token in the `Authorization: Bearer ...` header. The API validates the signature, expiration, issuer, and audience values, then checks permissions. Encoding the payload with Base64URL is not encryption; I do not put passwords in it. Not every access token has to be a JWT.

### OAuth, OAuth 2.0, OpenID, and OpenIddict
OAuth is an authorization protocol developed to allow an application to access another service on behalf of a user. The main purpose here is to allow users to grant limited access without sharing their passwords with that application.

OAuth 1.0 and OAuth 2.0 are two different generations of this protocol. OAuth 2.0 is not backward compatible with 1.0. While 1.0 requires each request to be signed, 2.0 uses HTTPS and bearer tokens instead, and also defines different flows for different scenarios. This also needs to be stated: OAuth 2.0 on its own is not a user login protocol; it only handles authorization.

OpenID Connect (OIDC) handles authentication. OIDC adds an authentication layer on top of OAuth 2.0 and uses an ID token for this. I do not think of the old OpenID (OpenID 2.0) and OIDC as the same thing. The old OpenID was an older authentication protocol independent of OAuth, and today it has largely been replaced by OIDC.

OpenIddict, on the other hand, is not a protocol but a set of libraries written for .NET. It provides server, client, and token validation components for OAuth 2.0 and OIDC.

In a scenario such as "Sign in with another account", I associate the user's identity with OIDC and access to the API with the access token. I do not use the ID token in place of an access token to access the API.

In short, their relationship is as follows: OAuth 2.0 provides authorization, OIDC adds authentication on top of it, and OpenIddict is the library that allows us to implement both in .NET.

### Techniques I can use for performance

- **AsNoTracking:** Disables change tracking in EF Core queries that will only be read; reduces tracking overhead.
- **Field selection and pagination:** Instead of retrieving the entire table, I fetch the required data with `Select`, `Skip`, and `Take`; I also examine indexes appropriate for the query.
- **IAsyncEnumerable:** Allows data to be consumed asynchronously in chunks. Avoiding loading all results into a list can reduce memory usage; the data source and provider may still perform buffering.
- **Caching / Redis:** I keep frequently read data that rarely changes in the cache. Redis can be a shared cache for multiple application instances; an expiration and invalidation strategy for data changes is required.
- **Profiling:** I first measure whether the slowness comes from the query, the CPU, or an external service. Adding a cache without measuring may optimize the wrong area.

### OWASP Top 10

Here, I used the **2025 list** as the basis. These are risk categories rather than names of individual vulnerabilities.

| Risk | My brief explanation |
| --- | --- |
| A01 - Broken Access Control | A user accessing a record or action beyond their permissions. Order ownership must be checked on the server. |
| A02 - Security Misconfiguration | Incorrect permissions or enabled debug settings. Production environment configuration must be checked. |
| A03 - Software Supply Chain Failures | A compromise of the security of the package, tool, or deployment chain. Dependencies and their sources must be audited. |
| A04 - Cryptographic Failures | Inadequate protection of sensitive data. HTTPS and appropriate password hashing mechanisms must be used. |
| A05 - Injection | Input being interpreted as a command. SQL injection and XSS are considered within this scope. |
| A06 - Insecure Design | Security being missing from the design. Abuse scenarios for the workflow must also be considered. |
| A07 - Authentication Failures | Weaknesses in login and session mechanisms. MFA and attempt limits reduce the risk. |
| A08 - Software or Data Integrity Failures | Trusting the correctness of software or data without verification. The integrity of updates and incoming data must be verified. |
| A09 - Security Logging and Alerting Failures | Signs of an attack not being logged or noticed. Meaningful alerts must be generated for security events. |
| A10 - Mishandling of Exceptional Conditions | Unsafe handling of unexpected conditions. Access control must not be skipped when an error occurs. |

**SQL injection** is input changing the structure of a query; I prevent it with parameterized queries. **XSS** is attacker content executing as code in the browser; I use output encoding and do not render untrusted content with `Html.Raw`. If HTML is accepted, an appropriate sanitizer is required.

**CSRF** is causing requests to be made on behalf of a user using credentials such as cookies that the browser sends automatically. I use antiforgery validation in cookie-based operations that change state. **Broken authentication** is weak login/session management; it is mitigated through secure password storage, MFA, and session duration checks.

In ASP.NET Core, I validate field formats/limits with DTOs and model validation, and check permissions with an authorization policy. Model validation alone does not provide SQL injection or XSS protection; each risk has its own preventive measures.

## 7. Logging and Error Handling

### Logging and log levels

Logs allow me to understand what happened in the application afterward. They are used to debug errors, monitor the application in production, and track security events. In ASP.NET Core, `ILogger<T>` is the common interface; providers such as console determine where the entries are written.

A log level indicates the severity of an entry. By setting a minimum level in `appsettings.json`, I can filter which entries are written; for example, I can disable entries below `Information` in production.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  }
}
```

| Level | Usage |
| --- | --- |
| Trace | Very detailed flow tracking |
| Debug | Diagnostic information during development |
| Information | A normal, meaningful event such as an order being created |
| Warning | A situation that requires attention even though the application continues |
| Error | A failed operation |
| Critical | An error that seriously affects the application's operation |
| None | A filter value that disables logging |

For example, I use `logger.LogInformation("Order created: {SiparisId}", id)`. Named fields make searching easier. I do not log information such as passwords and access tokens.

### Global exception handling and UseExceptionHandler

Instead of adding the same `try/catch` block to every method, I can handle unexpected HTTP processing errors in a central place. The following example is a standalone `Program.cs` flow:

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.UseExceptionHandler(handler => handler.Run(async context =>
{
    // UseExceptionHandler also records the error in the framework logs.
    await Results.Problem(
        statusCode: 500,
        title: "An unexpected error occurred during the operation.")
        .ExecuteAsync(context);
}));

app.MapGet("/api/durum", (ILogger<Program> logger) =>
{
    logger.LogInformation("Status check performed.");
    return Results.Ok(new { durum = "running" });
});

app.MapGet("/api/hata", IResult () =>
{
    throw new InvalidOperationException("Test error");
    // UseExceptionHandler catches this error and returns a 500 problem response to the client.
  
});

app.Run();
```

`UseExceptionHandler` handles unhandled errors from subsequent HTTP components. For example, the error thrown in a request to `/api/hata` reaches this middleware because it is not caught in the endpoint. I return a standard problem response to the client instead of a stack trace; I examine the technical details in the server logs. I handle expected outcomes directly, such as returning `400` for invalid input or `404` for a product that is not found. This middleware does not cover errors in separately running workers.

## 8. Software Development Principles

### SOLID

| Principle | My understanding and brief example |
| --- | --- |
| **S - Single Responsibility** | A class should have one primary reason to change. `SiparisService` manages order rules, and `EmailService` manages sending emails. |
| **O - Open/Closed** | I should not have to keep changing working code when adding new behavior. For a new discount type, I can add an `IIndirim` implementation instead of expanding the existing calculator. |
| **L - Liskov Substitution** | A subtype should not violate the expected contract of its base type. I should not bind a read-only repository to an interface that promises `Kaydet` and then throw an error when it is called. |
| **I - Interface Segregation** | A class should not depend on operations it does not use. I can separate reading and writing needs with `IUrunOkuyucu` and `IUrunYazici`. |
| **D - Dependency Inversion** | Business rules should depend on abstractions rather than concrete infrastructure. The order service can accept `IBildirimGonderici` instead of `SmtpEmailService`. |

DI makes it easier for me to apply the last principle; however, using DI alone does not mean the design is automatically SOLID.

### Singleton, Repository, and Factory

- **Singleton:** Shares a single instance. If there is mutable shared state, I need to consider concurrent access; I do not store user-specific data in a singleton service.
- **Repository:** Encapsulates data access behind specific operations. For example, it can abstract the `SiparisleriMusteriyeGoreGetir` query. Since EF Core already provides similar responsibilities, I do not consider a layer that merely repeats every `DbSet` method mandatory.
- **Factory:** Centralizes the object creation decision. Selecting the appropriate `IOdemeSaglayici` implementation based on the payment method is an example of this.

### Clean Code

I see Clean Code as allowing the next person who reads the code to easily understand its intent. Meaningful names, small methods, reducing unnecessary repetition, and using early returns help achieve this.

```csharp
// The meaning of the condition is unclear:
if (s > 0 && a) { /* ... */ }

// The same intent is clearer:
bool satinAlinabilir = stokAdedi > 0 && urunAktif;
if (!satinAlinabilir)
    return;
```

In comments, I explain the reason behind the business rule instead of repeating what the code already says. Short code and understandable code are not always the same thing.

### Software architectures

| Architecture | Basic approach | When would I consider it? |
| --- | --- | --- |
| Layered | Separates presentation, business rules, and data access into layers. | A small/medium-sized, straightforward business application |
| Clean Architecture | Keeps business rules at the center and directs dependencies inward. | Projects with extensive business rules and a long lifespan |
| Microservices | Divides business capabilities into independently deployable services. | When teams and components need to be scaled/deployed independently |
| Event-Driven | Components communicate by publishing and consuming events. | In independent processes such as invoicing/notification after an order |
| Hexagonal | Separates the application core from external systems through ports and adapters. | To make it easier to change/test database, payment, or messaging infrastructure |
| Onion Architecture | Aims to keep the application's core logic independent of the database, interface, and external services. | When business rules are complex and the application is expected to be developed over a long period |
| Monolith | Different functions are developed and run as a single whole. | I prefer it when starting a new project and the team is small |

### Layered architecture: Service and Repository

**Presentation** manages the HTTP request and response. **Business** applies rules such as whether an order can be placed. **Data Access** carries out database operations. Service coordinates the workflow; Repository abstracts data access. For example, the stock check and order creation flow can reside in Service, while access to the order record can reside in Repository.

### Clean Architecture and the direction of dependencies

**Domain** contains the core entities and business rules; **Application** contains the use cases and required interfaces; **Infrastructure** contains technical implementations such as EF Core/email; **API** contains the HTTP entry point.

```text
Source code dependencies:

API ─────────────→ Application ─────────────→ Domain
Infrastructure ──→ Application / Domain
```

The arrow here means “depends on.” In Clean Architecture, dependencies point **from outer layers to inner layers**. Although the assignment said “flowing outward,” I treated this principle as pointing inward. Application does not know the concrete class in Infrastructure; it uses the interface it defines itself. Infrastructure implements this interface, and it is mapped through DI registrations on the API side. This way, business rules do not depend on technical details. Here, it is necessary to distinguish the runtime call flow from the direction of source code dependencies.

For me, the purpose of these separations is to narrow the area affected by a change rather than increase the number of layers. Not having to rewrite order rules when the database or email service changes is the tangible benefit of the design.


