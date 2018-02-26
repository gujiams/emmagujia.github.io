---
title: OAuth 2.0 Note 1 - Authorization
date: 2018-01-01 18:00:00
categories: oauth 
tags:
---
This note is about OAuth 2.0 protocol, for authorization.
<!-- more -->
### What is OAuth
OAuth 2.0 is the industry-standard protocol for authorization. It enables applications to obtain limited access to user accounts on an HTTP service, such as Facebook, GitHub. It works by delegating user authentication to the service that hosts the user account, and authorizing third-party applications to access the user account. 
#### An example for better understanding
##### Before knowing OAuth
Emma wants to watch Games of Thrones on Youku.com but she does not want to register an account. There's an other option that she can login in using QQ account. Apparently she does not want to give her QQ account and password to Youku.com, because there's a lot of secret she did not want anyone else to know. So she decides to waste her life for 5 mins to get an account.
##### After knowing basic knowledge of OAuth
[Standard memo of OAuth](https://tools.ietf.org/html/rfc6749).<br>
First of all, YouKu.com don't have a chance to get Emma's QQ account password, login page will redirect to QQ login page which host in Tencent's server.

     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+
In above example:
- Resource Owner：Emma, person who is giving access to some portion of their account；
- Resource Server：QQ, API server used to access the user's information；
- Client：Youku.com, application that wants to access the user's account. Before it may do so, it must be authorized by the user, and the authorization must be validated by the API；
- Authorization Server ：Server that presents the interface where the user approves or denies the request. In smaller implementations, this may be the same server as the API server, but larger scale deployments will often build this as a separate component.
