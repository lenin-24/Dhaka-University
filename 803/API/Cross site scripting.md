go to http://43.230.120.10:5000/

in this url http://43.230.120.10:5000/1.php?status=hello

instead of hello we can put this script

<script> alert(1)</script>
<script> confirm(1)</script>

### How to Exploit?

```
Find parameter = status is param
Find Input field= that page didnt had input field
Check for injection= in url give <>/()hello to see output also shows that present
Check the context= after giving <>/()hello go to right click -> page souce 
Attempt to exploit=give this payload <script> confirm(1)</script> to see response
```


another task 

go to http://43.230.120.10:8000/
and try every urland see if xploit is possible with this <script> confirm(1)</script>


than go to 
http://43.230.120.10:8000/post.php?pid=3

open burp request and send to intruder mark number of pid "1" with Add $


than right click it to "scan defined insertion points" -> scan config -> custom 
unmark all scan and search in filter and mark all related to sql injection maybe 3 are there 
than mark all related to cross site scripting first 5 of them





