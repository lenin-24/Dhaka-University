### Here we will try to modify function even if we dont have access by changing request

like if i as a user want to change a video/image in dashboard and clickbutton to change name than in burp suite we select that request and right click "Send to repeater" 

than click send to generate the response

now with PUT method we can change/update a function than we can use DELETE to delete the post so 
instead of PUT we write DELETE and press send but if the person is of user role and doesnt have access or authorization to delete than in that request instead of user we rewrite it as admin to 
check if it really works.
