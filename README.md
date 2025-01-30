**I. Workflow of the Spring Boot Application**

   The Spring Boot project is a RESTful API that manages employees, allowing CRUD operations (Create, Read, Update, Delete). It also includes auditing features to track when and by whom records were created or updated.
   1️⃣ Key Features
✅ CRUD Operations for Employee
✅ Database Integration (JPA & Hibernate)
✅ Exception Handling (ResourceNotFoundException)
✅ Logging (SLF4J with Logback)
✅ Auditing (CreatedBy, CreatedAt, UpdatedBy, UpdatedAt)

  📌 2. EmployeeAudit (Auditing Fields)
This class enables auditing (tracks creation and updates).
🔹 Purpose:
Tracks when (createdAt, updatedAt) and who (createdBy, updatedBy) made changes.
Benefit: Helps in auditing employee records.

  📌 3. EmployeeRepository (Database Access)
This interface provides CRUD operations on Employee using Spring Data JPA.

   API Endpoints in EmployeeController
HTTP Method	Endpoint	Description
POST	/api/employees	Create a new employee
GET	/api/employees	Retrieve all employees
GET	/api/employees/{id}	Retrieve a specific employee
PUT	/api/employees/{id}	Update an employee
DELETE	/api/employees/{id}	Delete an employee

📌 What Happens Here?

The API receives an HTTP POST request with employee details in JSON.
The employeeRepository.save(employee) method saves it to the database.
A log message is recorded (LOGGER.info).

  📌 5. AuditorAwareImpl (Who Made Changes?)
This class provides the username of the person making database changes.

📌 Why?

This is useful for auditing who created or updated records.
Right now, it returns "SYSTEM_USER", but it can be modified to get real users dynamically.

  3️⃣ How the Program Works (Flow of Execution)
User sends an API request (POST /api/employees with { "name": "John Doe" }).
Spring Boot receives the request and calls the controller (EmployeeController).
Controller validates the request and calls EmployeeRepository.save().
JPA saves the record in the database.
Auditing is triggered (createdAt, updatedBy fields are set).
Spring Boot returns a response (201 Created with the employee details).
Logs are recorded for debugging.


**II. Logging method used?**

  1️⃣ Logging Framework Used
SLF4J (Simple Logging Facade for Java)
Logback (Default Logging Implementation in Spring Boot)
Why SLF4J?

SLF4J is a logging facade that allows you to switch between logging frameworks (Logback, Log4j, etc.) without changing your code.
Logback is the default logging framework in Spring Boot, offering high performance and flexibility.


**III. Docker Setup**

  This project includes a **`docker-compose.yml`** file that defines two main services:
 
### 1. PostgreSQL Database (`postgres-db`)
- Runs a **PostgreSQL** container.
- Stores blog data persistently using Docker **volumes**.
- Exposes **port 5432** (default PostgreSQL port).
- Configured with environment variables for credentials.
 
### 2. Spring Boot Application (`employee-app`)
- Runs the **Spring Boot Blog API**.
- Connects to the PostgreSQL database.
- Exposes APIs on **port 8080**.
- Uses **environment variables** for database configuration.


**IV. API's exposed and details of the APIs.**

  Create Employee (POST /api/employees)

This API adds a new employee to the database.
The request body contains employee details in JSON format.
The response returns the created employee object with an ID.
Get All Employees (GET /api/employees)

This API fetches a list of all employees from the database.
The response is a JSON array of employee objects.
Get Employee by ID (GET /api/employees/{id})

This API retrieves a specific employee by their ID.
If the employee exists, it returns the employee details.
If not found, it throws a ResourceNotFoundException.
Update Employee (PUT /api/employees/{id})

This API updates an existing employee’s details.
The request body contains updated employee information.
If the employee exists, it updates and returns the modified employee.
If not found, it throws a ResourceNotFoundException.
Delete Employee (DELETE /api/employees/{id})

This API deletes an employee from the database.
If the employee exists, it deletes the record.
If not found, it throws a ResourceNotFoundException.
Each API logs relevant actions using the SLF4J logger.
