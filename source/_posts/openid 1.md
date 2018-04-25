---
title: OpenID
date: 2018-04-24 22:15:38
categories: openid 
tags:
---
This post is about OpenID connect.
<!-- more -->
### What is OpenID
OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the End-User in an interoperable and REST-like manner.
OpenID Connect allows clients of all types, including Web-based, mobile, and JavaScript clients, to request and receive information about authenticated sessions and end-users. The specification suite is extensible, allowing participants to use optional features such as encryption of identity data, discovery of OpenID Providers, and session management, when it makes sense for them.

#### [OpenID protocol suite](http://openid.net/connect/)
1. Core – Defines the core OpenID Connect functionality: authentication built on top of OAuth 2.0 and the use of Claims to communicate information about the End-User
2. Discovery – (Optional) Defines how Clients dynamically discover information about OpenID Providers
3. Dynamic Registration – (Optional) Defines how clients dynamically register with OpenID Providers
4. OAuth 2.0 Multiple Response Types – Defines several specific new OAuth 2.0 response types
5. OAuth 2.0 Form Post Response Mode – (Optional) Defines how to return OAuth 2.0 Authorization Response parameters (including OpenID Connect Authentication Response parameters) using HTML form values that are auto-submitted by the User Agent using HTTP POST
6. Session Management – (Optional) Defines how to manage OpenID Connect sessions, including postMessage-based logout functionality
7. Front-Channel Logout – (Optional) Defines a front-channel logout mechanism that does not use an OP iframe on RP pages
8. Back-Channel Logout – (Optional) Defines a logout mechanism that uses direct back-channel communication between the OP and RPs being logged out

#### Overview
The OpenID Connect protocol, in abstract, follows the following steps.
(A)The RP (Client) sends a request to the OpenID Provider (OP).
(B)The OP authenticates the End-User and obtains authorization.
(C)The OP responds with an ID Token and usually an Access Token.
(D)The RP can send a request with the Access Token to the UserInfo Endpoint.
(E)The UserInfo Endpoint returns Claims about the End-User.
(F)These steps are illustrated in the following diagram:
```
+--------+                                   +--------+
|        |                                   |        |
|        |---------(A) AuthN Request-------->|        |
|        |                                   |        |
|        |  +--------+                       |        |
|        |  |        |                       |        |
|        |  |  End-  |<--(B) AuthN & AuthZ-->|        |
|        |  |  User  |                       |        |
|   RP   |  |        |                       |   OP   |
|        |  +--------+                       |        |
|        |                                   |        |
|        |<--------(C) AuthN Response--------|        |
|        |                                   |        |
|        |---------(D) UserInfo Request----->|        |
|        |                                   |        |
|        |<--------(E) UserInfo Response-----|        |
|        |                                   |        |
+--------+                                   +--------+
```
