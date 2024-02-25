# CozyHosting Write-up

<img src="https://labs.hackthebox.com/storage/avatars/eaed7cd01e84ef5c6ec7d949d1d61110.png" width="200" height="200">

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
