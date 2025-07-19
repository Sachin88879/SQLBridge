#SQLBridge
SQLBridge - Student Learning Project
Welcome to SQLBridge, a beginner-friendly middleware project built with .NET 8 and ASP.NET Core MVC to connect web applications to a SQL Server database. This project is designed for students to learn key concepts like MVC architecture, Entity Framework Core, SQL Server, and RESTful APIs in a simple, hands-on way. SQLBridge lets you execute SQL queries through a web interface or API, making it a great way to practice building real-world applications.
What You’ll Learn

How to set up a .NET 8 MVC project.
Connecting to SQL Server using Entity Framework Core.
Creating and consuming RESTful APIs.
Building a simple web interface with MVC views.
Basic database operations (CRUD: Create, Read, Update, Delete).
Securely handling database connections.

Table of Contents

Features
Prerequisites
Installation
Configuration
Usage
API Endpoints
Learning Tips
Contributing
License

Features

Simple Web Interface: Run SQL queries through a user-friendly webpage.
RESTful API: Execute queries programmatically using HTTP requests.
SQL Server Integration: Connects to SQL Server for data storage and retrieval.
Entity Framework Core: Simplifies database operations with code-first migrations.
Secure Connections: Uses trusted connections for easy setup in learning environments.
Lightweight: Minimal dependencies to keep things simple for beginners.

Prerequisites
Before starting, make sure you have:

.NET 8 SDK (free, install the latest version).
SQL Server Express 2022 (free version for learning).
SQL Server Management Studio (SSMS) (optional, for managing your database).
A code editor like Visual Studio 2022 Community (free) or VS Code.
Git (optional, for cloning the project).

Installation
Follow these steps to set up the project on your computer:

Clone or Download the Project:

If using Git, run:git clone https://github.com/your-org/sqlbridge.git
cd sqlbridge


Or download the ZIP file from the repository and extract it.


Open the Project:

In Visual Studio: Open SQLBridge.sln.
In VS Code: Open the sqlbridge folder.


Restore Dependencies:Run this command in the terminal (inside the project folder):
dotnet restore


Set Up SQL Server:

Open SSMS (or any SQL client) and connect to your SQL Server instance (usually localhost or . for Express).
Create a new database called SQLBridgeDB:CREATE DATABASE SQLBridgeDB;




Build the Project:
dotnet build


Configure the Project (see Configuration).

Run the Project:
dotnet run --project src/SQLBridge.Web



Configuration
The project uses a file called appsettings.json to store settings. Open src/SQLBridge.Web/appsettings.json and update it as needed:
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=SQLBridgeDB;Trusted_Connection=True;TrustServerCertificate=True;"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  }
}


Server: Use localhost for SQL Server Express or the name of your SQL Server instance.
Trusted_Connection=True: Uses your Windows login for authentication (no username/password needed).
TrustServerCertificate=True: Simplifies setup for learning by bypassing certificate validation.

If you’re using SQL authentication (username and password), update the connection string like this:
"DefaultConnection": "Server=localhost;Database=SQLBridgeDB;User Id=your_username;Password=your_password;TrustServerCertificate=True;"

Database Setup
The project uses Entity Framework Core to manage the database. To create the database tables:

Run these commands in the terminal (from the project folder):dotnet ef migrations add InitialCreate --project src/SQLBridge.Data --startup-project src/SQLBridge.Web
dotnet ef database update --project src/SQLBridge.Data --startup-project src/SQLBridge.Web


This creates a sample Users table in SQLBridgeDB for testing.

Usage
Running the Application
Start the project:
dotnet run --project src/SQLBridge.Web

Open a browser and go to https://localhost:5001 (or the port shown in the terminal).
Using the Web Interface

Navigate to https://localhost:5001/Query.
Enter a SQL query, e.g., SELECT * FROM Users.
Click "Run Query" to see results in a table.
Try inserting data:INSERT INTO Users (Name, IsActive) VALUES ('John Doe', 1);



Using the API
You can send queries programmatically using tools like Postman or code. Example in C#:
using System.Net.Http.Json;
using System.Text.Json;

var client = new HttpClient { BaseAddress = new Uri("https://localhost:5001") };

var queryRequest = new
{
    Query = "SELECT * FROM Users WHERE IsActive = @p0",
    Parameters = new object[] { true }
};

var response = await client.PostAsJsonAsync("/api/query疗

System: query", queryRequest);
var result = await response.Content.ReadFromJsonAsync<JsonElement>();
Console.WriteLine(result);

Sample Data
To test the project, you can add sample data to the Users table using SSMS:
INSERT INTO Users (Name, IsActive) VALUES ('Alice', 1);
INSERT INTO Users (Name, IsActive) VALUES ('Bob', 0);

Then query it via the web interface or API:
SELECT * FROM Users;

API Endpoints

POST /api/query: Run a SQL query.

Request Body:{
    "query": "SELECT * FROM Users WHERE IsActive = @p0",
    "parameters": [true]
}


Response: JSON array of results, e.g.:[{ "Id": 1, "Name": "Alice", "IsActive": true }]




GET /api/health: Check if the middleware is running.

Response:{ "status": "healthy", "database": "connected" }





Learning Tips

Explore the Code: Open src/SQLBridge.Web/Controllers/QueryController.cs to see how the API handles queries.
Try CRUD Operations:
Create: INSERT INTO Users (Name, IsActive) VALUES ('New User', 1);
Read: SELECT * FROM Users;
Update: UPDATE Users SET IsActive = 0 WHERE Name = 'Alice';
Delete: DELETE FROM Users WHERE Name = 'Bob';


Debugging: Use Visual Studio’s debugger or add Console.WriteLine in the code to understand how data flows.
Extend the Project:
Add authentication (e.g., JWT) for secure access.
Create new tables and queries to practice.
Add styling to the web interface using CSS or Bootstrap.



Check docs/learn.md for more learning resources and exercises.
Contributing
Want to improve SQLBridge? Here’s how:

Fork the repository.
Create a branch:git checkout -b feature/your-feature


Make changes and commit:git commit -m "Add your feature"


Push to your fork:git push origin feature/your-feature


Open a pull request.

Ideas for contributions:

Add new API endpoints (e.g., for specific reports).
Improve the web UI with better styling.
Add error handling for invalid queries.

License
This project is licensed under the MIT License. See the LICENSE file for details.
