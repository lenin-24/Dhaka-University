# SQL Injection with Burp Suite CO2 and SQLMAP

### 1st we need to install CO2 extension from Burp Suite Extensions → BApp Store

Now we need to install sqlmap, For kali linux we can simply use caommand sudo apt
install sqlmap

But for this tutorial, we will use sqlmap in windows. To install in windows simply
download sqlmap from here →

https://github.com/sqlmapproject/sqlmap/archive/refs/tags/1.8.zip


Extract this and go to the extracted folder.

Go to the extracted folder. And open command prompt from here by running cmd
in the address bar.

Command Prompt will open here. Now we can run sqlmap by running command
python3 sqlmap.py

or 

python sqlmap.py

as some windpws have problem with python3

Next go to https://security-vault.security-pedia.com/26.php

in log in give

username= test'
pass=1234

after enter it will show an error

than we go to view request and see if a params tick mark is present 

than right click to send to repeater

than in there we delete the sign (') in username in the request tab and click send

than right click inside the tab and go to extensions->CO2-> Send to SQLMapper

there in options tab 

options-> injection->testable Parameters write username

than from top copy the SQLMap Command

than go to the earlier CMD and write

python3 sqlmap.py (paster the command) 

than ques [Do you want to reduce number of image than you give input N]


another one

go to http://testphp.vulnweb.com/listproducts.php?cat=1

put (') in cat=1
we get error
than open in request earlier and do smae steps  but now in 
options-> injection->testable Parameters write cat

than from top copy the SQLMap Command

than go to the earlier CMD and write

python3 sqlmap.py (paster the command) 

than ques [Do you want to skip test payloads ? give Y 

