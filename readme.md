Installing the (OCI8) oracle instant client extension to connect the Oracle Database with Windows Azure Web App
===
OCI drivers are already installed on Azure Web Apps under `D:\Program Files (x86)\PHP\v5.6\ext\php_oci8_12c.dll` but we need to setup Oracle instant client to connect to the Oracle Database.
---

Download the .dll:
---

* Get the latest php_oci8.dll from here **https://pecl.php.net/package/oci8/2.0.12/windows**
* Make sure that the extensions are compatible with ***default version of PHP and are VC9 and non-thread-safe (nts) compatible***.
* Unzip the folder and Copy the `php_oci8.dll` to `d:\home\site\ext\`

Download the Orcale Instant Client:
---

* Download `32-bit Oracle Instant Client 12`c (Instant Client Package – Basic **Version 12.2.0.1.0**) from here **https://www.oracle.com/technetwork/topics/winsoft-085727.html**
* It should download a file `instantclient-basic-nt-12.2.0.1.0.zip`
* Drag and drop the `instantclient-basic-nt-12.2.0.1.0.zip` to `d:\home\site` from kudu debug console `https://sitename.scm.azurewebsites.net/DebugConsole`
* This will unzip the contents and create `instantclient_12_2` folder
* Create new `applicationHost.xdt` file inside the `d:\home\site` folder and copy the below configuration
	
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
   <system.webServer>
      <runtime xdt:Transform="InsertIfMissing">
         <environmentVariables xdt:Transform="InsertIfMissing">
            <add name="PATH" value="%PATH%d:\home\site\instantclient_12_2\;" xdt:Locator="Match(name)" xdt:Transform="InsertIfMissing" />
         </environmentVariables>
      </runtime>
   </system.webServer>
</configuration>
```


* Also, attached the applicationHost.xdt in this repro

Application Settings:
---

* Add an App Setting with the name `PHP_EXTENSIONS` and set the value to `d:\home\site\ext\php_oci8.dll`

**Stop and start the Web App and check the phpinfo page. It should return oci8 module section.**
	
Refrence
---
**https://blogs.msdn.microsoft.com/azureossds/2016/02/23/access-oracle-databases-from-azure-web-apps-using-oci8-drivers-with-php/**
	