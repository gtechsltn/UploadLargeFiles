# Upload Large Files
```
Article 1/ How to implement low-cost CI/CD for .Net projects
https://medium.com/@kagamimendoza/how-to-implement-low-cost-ci-cd-for-net-projects-7aeaa1e5fdd6
Using Azure DevOps with hosting on SmarterASP.NET
Artículo en español: https://bit.ly/3WI5qBV
 
Welcome, this guide aims to teach you everything you need to know to implement CI/CD in Azure DevOps with hosting on Smarter ASP for .Net projects, particularly with APIs, in just 29 steps.
Requirements:
•	Basic knowledge of Visual Studio .NET 2022.
•	Visual Studio 2022 Version 17.5.4.
•	Dotnet version 6.0.16
Why CI/CD?
Continuous integration and continuous delivery/deployment (CI/CD) with Azure DevOps are important because they offer a fast and reliable way to develop, test, and deploy software applications.
CI/CD allows software development teams to deliver software updates frequently, ensuring higher quality and reducing costs.
Context
Are you aware of the option to create CI/CD for smaller development teams using Azure DevOps with SmarterASP.NET hosting? It is a cost-effective way to improve collaboration, streamline development processes, and provide the scalability and security needed to host .NET applications for startups, small and medium-sized businesses, as well as personal projects.
Not everyone can afford the high cost of implementing CI/CD in the Azure or AWS cloud. That’s why I offer this good alternative based on my past experiences.
Start of the guide
I will show you the concise guide on how to carry out this process in the following PDF. In some steps, I will provide you with links to videos that can help you meet certain prerequisites before starting the actual guide.
Step 0: Create an account, organization, project in Azure DevOps.
https://www.youtube.com/watch?v=A91difri0BQ
https://www.youtube.com/watch?v=705enJzR2cY
https://www.youtube.com/watch?v=Ii70yZ7pmJs
Step 1: Create an account in SmarterASP.NET hosting
Step 2: How to create a MSSQL database in SmarterASP.NET (optional)

How can I create MSSQL database from hosting control panel?
When you need a sql server database for your site, you should create it from your hosting control panel first, please…
www.smarterasp.net

Step 3: For this implementation, we will need a .Net API project. You can use an existing one that you have created before or learn how to create one and obtain additional information in the following article I have created: [ In it, you will find detailed instructions on how to create a .Net API project and other interesting related concepts.
https://bit.ly/42XOucN For the rest of the article, I will use the repository in the end of the link article.
Step 3.1: Quick verification
Step 3.2: Please verify if you have the following two code line in your .Net api project
 
Step 3.3: Please verify if your swagger documentation is available for all environment.
 
Ready, all was verified.
Step 4: We need to create a new website in our SmarterASP hosting.
 
 
Step 5: We need to turn on the website create before in the step 4.
 
 
 
Step 5.1: Write this data because you will need them in the CD pipeline.
ServiceUrl your web deploy Service URL
SiteName your web deploy site name
UserName your web deploy user name
Password your web deploy password
Step 5.2: I recommend for security reasons to change the password to one that you can always remember because normally is the same as the login.
Step 6: We need to create an Azure repository to upload our .Net api project.
 
 
We choose the project
 
 
click on create button.
 
result.
Step 7: Now we need to add our code to the new Azure repository created.
 
copy the url
Step 7.1: Now you can use your favorite tool to do the following steps, in my case is git bash.

Downloads
Git comes with built-in GUI tools ( git-gui, gitk), but there are several third-party tools for users looking for a…
git-scm.com

Step 7.2: Clone the repository to your local machine using Git command-line tools.
 
git clone https://your-project-url the url copied in the step 7
Remember your login credential to azure portal here.
Step 7.3: Change your working directory to the local repository.
 
cd YourRepoName
Step 7.4: Add your code files to the repository using the “git add” command.
 
git add .
Step 7.5: Commit your changes to the repository using the “git commit” command.
 
git commit -m “Initial api version 2”
Step 7.6: Push your changes to the Azure DevOps repository using the “git push” command.
 
git push origin main
 
result.
Step 8: We need to establishing pull requests as the way to merge changes into the main branch in Azure Repositories (or any Git repository) is important for several reasons for example: Code review, Collaboration, Control over changes, version control.
 
click in more options.
 
That is the basic configuration to do it
Building Continuous Integration — CI
Step 9: We need to create our first pipeline, navigate to Pipelines at the left nav menu bar, then click Create Pipeline button.
 
Step 10: Select the classic editor to create your first pipeline.
 
Step 11: We need to select a source of our repository, in our case, we use Azure Repos Git.
 
Step 12: We need to choose a template that builds your kind of app,in our case its a ASP.NET Core API.
 
Step 13: Do a similar configuration in the following fields and parameters:
 
Step 13.1: Name* : DON’T leave any white space in the name, to prevent any unexpected problems.
 
use a name related to your project.
Step 13.2: Agent Specification* : please choose “windows” since we are going to build and publish the project to a Windows platform.
 
Step 13.3: Project(s) to restore and build, please be specific.
 
find where is yout .csproj
 
result.
Step 14: We need to open the “Publish” pane, untick Zip published projects.
 
 
Step 15: If you need to add,update or delete variables you can use Variables tab page, you can update the configurations as you need, the default settings are enough to publish a standard asp.net core app.
 
Step 16: We need to go to the triggers page and tick the Enable continuous integration, this will build your project pipeline whenever a change is committed to your main branch repository.
 
Step 17: We need to Save & queue the pipeline
 
Step 18: After of save and run, the page will navigate to the Summary pane automatically. You can check the job status and our published results.
 
 
Step 19: We need to verify the artifact about the published project produced files, write down the path “drop\src”, we will need to use it later in the CD pipeline.
 
 
SourcePath this will be your pipeline published path, default is “drop\src”.
Building Continuous Delivery — CD
Step 20: We need to create our second pipeline, navigate to the releases tab page, and create a new release pipeline.
 
 
Step 21: We need to select a template of “Empty job”.
 
We need to write the name of our first environment, in my case will be “Development”.
Step 22: We need to add our artifact, created before.
 
Step 23: We need to choose the source(build pipeline) NOT update the Source alias * field.
 
Step 24: We need to enable the continuous deployment trigger, this will execute the subsequent stage jobs when a pipeline is built.
 
Step 25: We need to add a task for the deployment.
 
Step 26: We need to do the following configurations.
Step 26.1: Open the Agent job pane, in the Agent pool, choose Hosted Windows 2019 with VS2019.
 
Step 26.2: Make sure the Artifact download is configured with the matched build pipeline.
 
Step 26.3: Add a PowerShell task.
 
Step 26.4: We need open powerShell script task:
- Change Type to Inline
- Copy and paste the following script to Script* field.
You don't need to update anything in the script.
 
$sourcePath = Join-Path -Path "$($Env:AGENT_RELEASEDIRECTORY)\$($Env:RELEASE_PRIMAR
YARTIFACTSOURCEALIAS)" -ChildPath $Env:SourcePath
# Deploy project using Web Deploy
& "C:\Program Files\IIS\Microsoft Web Deploy V3\msdeploy.exe" -verb:sync -source:co
ntentPath=$sourcePath -dest:contentPath=$Env:SiteName,computerName=$Env:ServiceUrl,
userName=$Env:UserName,password=$Env:Password,authtype="Basic",includeAcls="False"
-allowUntrusted -disableLink:AppPoolExtension -disableLink:ContentExtension -disabl
eLink:CertificateExtension -enableRule:AppOffline -verbose
Or you can use the following with retries, this is my favorite to be honest.
$sourcePath = Join-Path -Path "$($Env:AGENT_RELEASEDIRECTORY)\$($Env:RE
LEASE_PRIMARYARTIFACTSOURCEALIAS)" -ChildPath $Env:SourcePath
$maxRetries = 5
$retryCount = 0
while ($retryCount -lt $maxRetries) {
 & "C:\Program Files\IIS\Microsoft Web Deploy V3\msdeploy.exe" -verb:sync -sourc
e:contentPath=$sourcePath -dest:contentPath=$Env:SiteName,computerName=$Env
:ServiceUrl,userName=$Env:UserName,password=$Env:Password,authtype="Basic",
includeAcls="False" -allowUntrusted -disableLink:AppPoolExtension -disableLink:Con
tentExtension -disableLink:CertificateExtension -enableRule:AppOffline -verbose
 # Check if the deployment succeeded
 if ($LASTEXITCODE -eq 0) {
 Write-Output "Deployment succeeded after $($retryCount + 1) retries"
 break
 }
 # Wait for a few seconds before retrying
 Start-Sleep -Seconds 10
pg. 23 Ing. Javier Eduardo Mendoza Blandón
cvmendozacr.com
 $retryCount++
}
if ($retryCount -eq $maxRetries) {
 Write-Output "Deployment failed after $maxRetries retries"
}
Step 26.5: Expand the “Environment Variables” tab and add the following information:
SourcePath
ServiceUrl
SiteName
UserName
Password
 
Remember you can get this data from Step 5, Step 19.
Step 27: We need to do click in the following marked images to complete the pipeline.
 
click on save button.
 
click on OK button , after create release button.
 
 
click on release 1 created.
 
click on deploy.
 
result.
 
Cheers we have completed all.
Step 28: We need to verify if your .Net Api project was deployed successfully at SmarterASP side.
https://www.smarterasp.net/
https://member5-3.smarterasp.net/account/loginform
 
End of implementation.
After to complete the implementation, this will be your frequently step.
Step 29: Let do another test folks, only to be sure if all this effort to create this CI/CD flow is working as expected.
A- Let create another git branch with the name that you prefer.
 
git checkout -b myTestBrach
B- Open your .net api project with VS and do any change in your swagger ui
 
C- Let save,add, push the change to the azure repository.
git add .
 
git commit -m “Initial api version 3”
 
git push origin myTestBrach: myTestBrach
D- We’ll need to create the pull request in the azure repository
 
 
E- Automatically after completing the pull request the CI pipeline will be triggered.
 
F- Automatically after complete the CI pipeline , CD pipeline will be triggered.
 
 
That is everything folks, I will wait for your feedback, ideas, or advice, don’t hesitate to do it please, thank you.
Article 2/ Handling large file uploads
https://medium.com/@craftingcode/comprehensive-guide-to-handle-large-file-uploads-in-asp-net-web-api-36bd78c86c5f
 
The main repository for all the Code Maze guides
https://github.com/gtechsltn/CodeMazeGuides
Uploading Large Files in ASP.NET Core
https://github.com/gtechsltn/CodeMazeGuides/tree/main/files-csharp/UploadingLargeFiles
Uploading Large Files in ASP.NET Core
https://code-maze.com/aspnetcore-upload-large-files/
Upload files in ASP.NET Core
https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/mvc/models/file-uploads.md
Upload files in ASP.NET Core
https://learn.microsoft.com/en-us/aspnet/core/mvc/models/file-uploads?view=aspnetcore-7.0#uploading-large-files-with-streaming
The sample project of large files upload
https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/mvc/models/file-uploads/samples/5.x/LargeFilesSample
Upload large files with .net core
https://medium.com/@agustinafassina_92108/upload-large-files-with-net-core-4170acc02f93
https://github.com/gtechsltn/UploadLargeFiles
Dealing with large files in ASP.NET Web API
https://www.strathweb.com/2012/09/dealing-with-large-files-in-asp-net-web-api/
Article 3/ Most popular .NET Libraries every developer should know
https://medium.com/dotnet-hub/most-popular-c-net-core-libraries-every-developer-should-know-5e38c1a63830
Best .NET libraries — Top useful libraries every .NET developers use in their projects.
 
.NET Hub: Most popular .NET Libraries
AutoMapper
AutoMapper is a convention-based object-object mapper in .NET. It is a simple little library built to solve a deceptively complex problem — getting rid of code that mapped one object to another. This type of code is rather dreary and boring to write.
GitHub ⟷ NuGet ⟷ Website
FluentValidation
FluentValidation is a popular .NET validation library for building strongly-typed validation rules. It is a small validation library for .NET that uses a fluent interface and lambda expressions for building validation rules.
GitHub ⟷ NuGet ⟷ Website
MediatR
Simple, unambitious mediator implementation in .NET. In-process messaging with no dependencies. Supports request/response, commands, queries, notifications and events, synchronous and async with intelligent dispatching via C# generic variance.
GitHub ⟷ NuGet ⟷ Website
Autofac
Autofac is an IoC container for Microsoft .NET. It manages the dependencies between classes so that applications stay easy to change as they grow in size and complexity. This is achieved by treating regular .NET classes as components.
GitHub ⟷ NuGet ⟷ Website
Hangfire
An easy way to perform background job processing in your .NET and .NET Core applications. No Windows Service or separate process required. Incredibly easy way to perform fire-and-forget, delayed and recurring jobs inside ASP.NET applications. CPU and I/O intensive, long-running and short-running jobs are supported. No Windows Service / Task Scheduler required. Backed by Redis, SQL Server, SQL Azure and MSMQ.
GitHub ⟷ NuGet ⟷ Website
Swashbuckle (Swagger — OpenAPI)
Simplify API development for users, teams, and enterprises with the Swagger open source and professional toolset. Swagger tooling for API’s built with ASP.NET Core. Generate beautiful API documentation, including a UI to explore and test operations, directly from your routes, controllers and models. In addition to its Swagger 2.0 and OpenAPI 3.0 generator, Swashbuckle also provides an embedded version of the awesome swagger-ui that’s powered by the generated Swagger JSON. This means you can complement your API with living documentation that’s always in sync with the latest code. Best of all, it requires minimal coding and maintenance, allowing you to focus on building an awesome API.
GitHub ⟷ NuGet ⟷ Website
Serilog
Simple .NET logging with fully-structured events. Serilog is a diagnostic logging library for .NET applications. It is easy to set up, has a clean API, and runs on all recent .NET platforms. While it’s useful even in the simplest applications, Serilog’s support for structured logging shines when instrumenting complex, distributed, and asynchronous applications and systems.
GitHub ⟷ NuGet ⟷ Website
xUnit
xUnit.net is a free, open source, community-focused unit testing tool for the .NET Framework. Written by the original inventor of NUnit v2, xUnit.net is the latest technology for unit testing C#, F#, VB.NET and other .NET languages. xUnit.net works with ReSharper, CodeRush, TestDriven.NET and Xamarin.GitHub ⟷ NuGet ⟷ Website
Moq
The most popular and friendly mocking library for .NET. Moq (pronounced “Mock-you” or just “Mock”) is the only mocking library for .NET developed from scratch to take full advantage of .NET Linq expression trees and lambda expressions, which makes it the most productive, type-safe and refactoring-friendly mocking library available. And it supports mocking interfaces as well as classes. Its API is extremely simple and straightforward, and doesn’t require any prior knowledge or experience with mocking concepts.
GitHub ⟷ NuGet ⟷ Website
FluentAssertions
Fluent Assertions is a set of .NET extension methods that allow you to more naturally specify the expected outcome of a TDD or BDD-style unit test. Write assertions that keep you and your fellow developers out of the debugger hell and clearly communicate what they are trying to accomplish.
GitHub ⟷ NuGet ⟷ Website
AutoFixture
AutoFixture is an open source library for .NET designed to minimize the ‘Arrange’ phase of your unit tests in order to maximize maintainability. Its primary goal is to allow developers to focus on what is being tested rather than how to setup the test scenario, by making it easier to create object graphs containing test data. Write maintainable unit tests, faster. AutoFixture makes it easier for developers to do Test-Driven Development by automating non-relevant Test Fixture Setup, allowing the Test Developer to focus on the essentials of each test case.
GitHub ⟷ NuGet ⟷ Website
Bogus
A simple fake data generator for C#, F#, and VB.NET. Bogus is fundamentally a C# port of faker.js and inspired by FluentValidation's syntax sugar. Bogus will help you load databases, UI and apps with fake data for your testing needs.
GitHub ⟷ NuGet ⟷ Website
Polly
Polly is a .NET resilience and transient-fault-handling library that allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.
GitHub ⟷ NuGet ⟷ Website
RestSharp
Simple REST and HTTP API Client for .NET. RestSharp is probably the most popular HTTP client library for .NET. Featuring automatic serialization and deserialization, request and response type detection, variety of authentications and other useful features, it is being used by hundreds of thousands of projects.
GitHub ⟷ NuGet ⟷ Website
Refit
The automatic type-safe REST library for .NET Core, Xamarin and .NET. Heavily inspired by Square’s Retrofit library, Refit turns your REST API into a live interface.
GitHub ⟷ NuGet ⟷ Website
Humanizer
Humanizer meets all your .NET needs for manipulating and displaying strings, enums, dates, times, timespans, numbers and quantities. It is part of the .NET Foundation.
GitHub ⟷ NuGet ⟷ Website
NodaTime
Noda Time is an alternative date and time API for .NET. It helps you to think about your data more clearly, and express operations on that data more precisely.
GitHub ⟷ NuGet ⟷ Website
BenchmarkDotNet
BenchmarkDotNet helps you to transform methods into benchmarks, track their performance, and share reproducible measurement experiments. It’s no harder than writing unit tests! Under the hood, it performs a lot of magic that guarantees reliable and precise results thanks to the perfolizer statistical engine. BenchmarkDotNet protects you from popular benchmarking mistakes and warns you if something is wrong with your benchmark design or obtained measurements. The results are presented in a user-friendly form that highlights all the important facts about your experiment. The library is adopted by 6800+ projects including .NET Runtime and supported by the .NET Foundation.
GitHub ⟷ NuGet ⟷ Website
GenFu
GenFu is a library you can use to generate realistic test data. It is composed of several property fillers that can populate commonly named properties through reflection using an internal database of values or randomly created data. You can override any of the fillers, give GenFu hints on how to fill them. GenFu is all about smartly building up objects to use for test and prototype data. It will walk your object graph and fill in the properties on your type with realistic looking data.
Use GenFu’s static methods to new up new objects for testing, design-time data or seeding a database.
GitHub ⟷ NuGet ⟷ Website
MiniProfiler
MiniProfiler is a library and UI for profiling your application. By letting you see where your time is spent, which queries are run, and any other custom timings you want to add, MiniProfiler helps you debug issues and optimize performance. A simple but effective mini-profiler for ASP.NET (and Core) websites.
GitHub ⟷ NuGet ⟷ Website
EPPlus
Create advanced Excel spreadsheets using .NET, without the need of interop. It had the basic functionality needed to read and write a spreadsheet. EPPlus is a .NET library that reads and writes Excel files using the Office Open XML format (xlsx). EPPlus has no dependencies other than .NET.
GitHub ⟷ NuGet ⟷ Website
iText
iText 7 for .NET is the .NET version of the iText 7 library, formerly known as iTextSharp, which it replaces. iText 7 represents the next level of SDKs for developers that want to take advantage of the benefits PDF can bring. Equipped with a better document engine, high and low-level programming capabilities and the ability to create, edit, etc.
GitHub ⟷ NuGet ⟷ Website

Article 4/ Top .NET (ASP.NET Core) Open-Source Projects
https://medium.com/dotnet-hub/top-dotnet-asp-net-core-open-source-projects-most-popular-aspnetcore-best-github-repos-6261569bdb06
Awesome useful .NET Projects on GitHub — Most popular .NET Projects to get practical knowledge.
 
.NET Hub: Top .NET (ASP.NET Core) Open-Source Projects
E-Commerce
nopCommerce
nopCommerce is the most popular and one of the best open-source eCommerce shopping cart solution based on ASP.NET Core that contains both a catalog frontend and an administration tool backend.
Using the latest Microsoft technologies, it provides high performance, stability, and security. It is also fully compatible with Azure and web farms.
nopCommerce architecture follows well-known software patterns and the best security practices. Pluggable and clear architecture makes it easy to develop custom functionality and follow any business requirements.
GitHub ⟷ Website ⟷ Docs
GrandNode
GrandNode is the most advanced free, open source, fast, headless, multi-tenant eCommerce platform built with the newest version of .NET Core, MongoDB, AWS DocumentDB, Azure CosmosDB, Vue.js.
It is an intuitive e-Commerce platform that enables the flexible development of online stores. It supports various business models such as B2B, B2C, Multi-Store, Multi-Vendor, Multi-Tenant, Multi-Language, Multi-Currency.
Advanced personalisation, scalability, speed, and high efficiency — these are features that effectively support your e-business management.
GitHub ⟷ Website ⟷ Docs
SimplCommerce
A simple, cross platform, modularized ecommerce system built on .NET Core.
GitHub ⟷ Website ⟷ Docs
VirtoCommerce
Virto Commerce is an open-source platform for an extensible e-commerce applications.
Complex digital commerce solutions for B2B, B2C or B2B2C businesses, marketplaces and derived SaaS commerce platforms.
Virto Commerce architecture was designed on principles: microservices, API-first, cloud-native, headless, and advanced extensibility.
GitHub ⟷ Website ⟷ Docs
Blog Website
Miniblog.Core
A full-featured yet simple blog engine built on ASP.NET Core.
GitHub ⟷ Website
Blogifier
Blogifier is an open-source publishing platform Written in .NET and Blazor WebAssembly. With Blogifier you can make a multi-author or a personal blog and a powerful website.
Right now it can be used as a personal or group blog, and more features are under development.
GitHub ⟷ Website ⟷ Docs
Architecture Styles and Patterns
eShopOnContainers
Cross-platform .NET sample microservices and container based application that runs on Linux, Windows, and macOS. Powered by .NET 6, Docker Containers, Azure Kubernetes Services and it supports Visual Studio, VS for Mac and CLI based environments.
The architecture proposes a microservice oriented architecture implementation with multiple autonomous microservices (each one owning its own data/db) and implementing different approaches within each microservice (simple CRUD vs. DDD/CQRS patterns) using HTTP as the communication protocol between the client apps and the microservices and supports asynchronous communication for data updates propagation across multiple services based on Integration Events and an Event Bus (a light message broker, to choose between RabbitMQ or Azure Service Bus, underneath) plus other features defined at the roadmap.
GitHub
CleanArchitecture (ardalis)
Clean Architecture Solution Template: A starting point for Clean Architecture with ASP.NET Core.
Clean Architecture is just the latest in a series of names for the same loosely-coupled, dependency-inverted architecture. You will also find it named hexagonal, ports-and-adapters, or onion architecture.
GitHub
CleanArchitecture (jasontaylordev)
Clean Architecture solution template for .NET 6.
This is a solution template for creating a Single Page App (SPA) with Angular and ASP.NET Core following the principles of Clean Architecture. Create a new project based on this template by installing and running the associated NuGet package.
GitHub ⟷ Docs
dotnet-webapi-boilerplate
.NET Web API Boilerplate is a Clean Architecture Template for .NET 6.0 WebApi built with Multi-tenancy Support.
The goal of this repository is to provide a complete and feature-rich starting point for any .NET developer / team to kick-start their next major project using .NET 6 Web API. This also serves the purpose of learning advanced concepts and implementations such as Multitenancy, CQRS, Onion Architecture, Clean Coding standards and so on.
GitHub
Architecture (rafaelfgx)
.NET 6, ASP.NET Core 6, Entity Framework Core 6, C# 10, Angular 13, Clean Code, SOLID and DDD.
This project is an example of architecture using new technologies and best practices. The goal is to share knowledge and use it as reference for new projects.
GitHub
Abp Framework
ABP Framework is an Open Source web application framework and it offers an opinionated architecture to build enterprise software solutions with best practices on top of the .NET and the ASP.NET Core platforms.
It provides the fundamental infrastructure, production-ready startup templates, modules, themes, tooling, guides and documentation to implement that architecture properly and automate the details and repetitive works as much as possible.
GitHub ⟷ Website ⟷ Docs
modular-monolith-with-ddd
Full modular Monolith .NET application with Domain-Driven Design approach.
GitHub
sample-dotnet-core-cqrs-api
Sample .NET Core REST API CQRS implementation with raw SQL and DDD using Clean Architecture.
GitHub
University-Microservices
Implementing an “University” microservices application with .Net Core, Docker, RabbitMQ, CAP, gRPC, Ocelot API Gateway, Redis, PostgreSQL, SqlServer, Entity Framework Core, CQRS , DDD and Clean Architecture implementation. Also includes Cross-Cutting concerns like Implementing Centralised Distributed Logging with Seq.
GitHub
.NET Samples-Examples
Practical-aspnetcore
Practical samples of ASP.NET Core 2.1, 2.2, 3.1, 5.0 and 6.0 projects you can use.
The goal of this project is to enable .NET programmers to learn the new ASP.NET Core stack from the ground up directly from code. There is so much power in the underlying ASP.NET Core stack.
GitHub
aspnetcore-realworld-example-app
ASP.NET Core codebase was created to demonstrate a fully fledged fullstack application built with ASP.NET Core (with Feature orientation) including CRUD operations, authentication, routing, pagination, and more that adheres to the RealWorld spec and API.
GitHub ⟷ Website ⟷ Docs
EquinoxProject
The Equinox Project is an open-source project written in .NET Core with DDD, CQRS and Event Sourcing concepts.
The goal of this project is to implement the most common used technologies and share with the technical community the best way to develop great applications with .NET.
GitHub ⟷ Docs
.NET libraries, tools, frameworks and roadmap
awesome-dotnet-core
A collection of awesome .NET core libraries, tools, frameworks and software.
GitHub
awesome-dotnet
A collection of awesome .NET libraries, tools, frameworks and software.
GitHub
AspNetCore-Developer-Roadmap
Roadmap to becoming an ASP.NET Core developer in 2022.
You can find a chart demonstrating the paths that you can take and the libraries that you would want to learn to become an ASP.NET Core developer.
GitHub
.NET-Backend-Developer-Roadmap
A roadmap for .NET developers that want to focus on backend and specifically work with microservices.
GitHub
Awesome-Microservices-DotNet
A collection of awesome training series, articles, videos, books, courses, sample projects, and tools for microservices in .NET.
GitHub
clean-code-dotnet
Clean Code concepts and tools adapted for .NET.
GitHub
```
