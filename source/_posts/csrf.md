---
title: Cross-Site Request Forgeries Study
date: 2018-02-20 20:00:00
categories: security
tags:
---
Recently, I notice there're some CSRF configuration in Spring Security. So it's a good chance to learn more about it. This post introduces CSRF, pronounced "sea surf", and provides a few simple steps to help prevent these types of attacks in our own Spring applications.
<!-- more -->
### Example of Attack
#### GET attack
Emma wants to transfer 8000 RMB to her friend online, the vulnerable bank application sends a state changing request as a plain text without any encryption.
`https://bank.example.com/transfer?amount=8000&destinationAccount=emmaFriendAccount`
Now the hacker constructs a request that transfers money from the victim's account to the attacker's account by embedding the request in an image that is stored on various sites under the attacker's control and wait for emma to bite the hook.<br>
```html 
<img src = "https://bank.example.com/transfer?amount=8000&destinationAccount=attackersAccount" 
    width = "0" height = "0" />
```
The vulnerable application - bank, notice Emma already login(browser will bring cookie) and receive the request that on step 2, then the transfer done, money will transfer to the hacker.
#### What if we change request to POST
Still can sightly open a `iframe` 
```html
 <iframe style="display:none" name="csrf-frame"></iframe>
 <form method='POST' action='hhttps://bank.example.com/transfer' target="csrf-frame" id="csrf-form">
   <input type='hidden' name='destinationAccount' value='attackersAccount'>
   <input type='submit' value='submit'>
 </form>
 <script>document.getElementById("csrf-form").submit()</script>
 ```
 #### What if app only accept Json type
 Still can put separated request together according to [spring doc](https://docs.spring.io/spring-security/site/docs/current/reference/html/csrf.html)
 ```html
<form action="https://bank.example.com/transfer" method="post" enctype="text/plain">
<input name='{"amount":8000,"destinationAccount":"attackersAccount", "ignore_me":"' value='test"}' type='hidden'>
<input type="submit"
	value="Click me!"/>
</form>
```
This will produce the following JSON structure.
```json
{ 
  "amount": 8000,
  "account": "attackersAccount",
  "ignore_me": "=test"
}
```
If an application were not validating the Content-Type, then it would be exposed to this exploit. Depending on the setup, a Spring MVC application that validates the Content-Type could still be exploited by updating the URL suffix to end with ".json" as shown below:
 ```html
<form action="https://bank.example.com/transfer.json" method="post" enctype="text/plain">
<input name='{"amount":8000,"destinationAccount":"attackersAccount", "ignore_me":"' value='test"}' type='hidden'>
<input type="submit"
	value="Click me!"/>
</form>
```
### Approaches to Defend
There are some existing method such as validate referer in request header, but it rely on browser which is not secure, referer can be changed due to some browser's bug. 
Also there is good and simple way called [Same-site cookies](https://www.chromestatus.com/feature/4672634709082112) that by asserting that a particular cookie should only be sent with requests initiated from the same registrable domain.
`Set-Cookie: session_id=ewfewjf23o1; SameSite`
I'll mainly talk about how to defend in Spring application.
#### Use proper HTTP verbs
HTTP GET can cause the information to be leaked. See [RFC 2616 Section 15.1.3](https://www.w3.org/Protocols/rfc2616/rfc2616-sec15.html#sec15.1.3) Encoding Sensitive Information in URI’s for general guidance on using POST instead of GET for sensitive information.
(remark: Books named RESTful Web APIs says GET is secure method because it only get the resource representation, will not change the state)
#### Configure CSRF Protection in Spring Security
Spring Security implements CSRF protection with a synchronizer token. State-changing
requests (for example, any request that is not GET, HEAD, OPTIONS, or TRACE)
will be intercepted and checked for a CSRF token. If the request doesn't carry a CSRF
token, or if the token doesn't match the token on the server, the request will fail with
a CsrfException.
CSRF protection is enabled by default with XML configuration. If you would like to disable CSRF protection, the corresponding XML configuration can be seen below:
```xml
<http>
	<!-- ... -->
	<csrf disabled="true"/>
</http>
```
Or java config:
```java
@Override
protected void configure(HttpSecurity http) throws Exception {
http
...
.csrf()
.disable();
}
```
If you’re using Thymeleaf for your page template,you’ll get the hidden `_csrf` field automatically, as long as the `<form>` tag’s action attribute is prefixed to come from the Thymeleaf namespace:
```html
<form method="POST" th:action="@{/transfer}">
...
</form>
```
If you’re using JSP for page templates, you can do something very similar:
```jsp
<input type="hidden"
name="${_csrf.parameterName}"
value="${_csrf.token}" />
```
Even better, if you’re using Spring’s form-binding tag library, the `<sf:form>` tag will automatically add the hidden CSRF token tag for you.
