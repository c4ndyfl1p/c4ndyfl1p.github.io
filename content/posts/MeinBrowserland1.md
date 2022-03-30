+++
author = "c4ndy"
title = "Mein Browserland #1: The web, cookies, authentication"
date = "2022-03-02"
description = "Web Security"
tags = [
    "http",
    "cookies",
    "web-security",
    
]
categories = [
    "web development",
    "web security",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]

+++
### The web is a tangled mess; 

And securing it, no matter what we try, appears to be a hard task, if not impossible. Before talking about HOW to go about securing it, or writing and deploying secure web applications, let's talk about why the web is a tangled mess.

The web, as we know it, is ever-evolving and came about by decades of working on existing features. If a feature was ever released over the web, it was permanent and you could not just get rid of it. Somehow developers are sorta forced to treat "a bug" as a feature because there is no way about it, *you have to maintain backwards compatibility*.
The extremely challenging task of a browser is: to run untrusted code securely, *while* maintaining strict backwards compatibility, *while* protecting the user from malicious untrusted code, *while* running untrusted code from MULTIPLE sources and while providing *the right* isolation between these various sources. Heck, the browser needs almost an operating system level isolation. The modern-day browser kinda acts like an operating system itself! Something that was dreamt of being just a simple document viewer, over the years, has become this gigantic application platform. And it's everywhere. It's used by almost every person on this Earth
Then of course securing this complex infrastructure or even reaching a conses and mutually agreeing on policies about how to go about securing it and governing it, is a gargantuan task. One because of such complexity, two because of the number of stakeholders involved.

For the past few months, I have been spending significant hours of my life developing web applications. By some fortune or misfortune, I, a second-year undergrad, was given the ambitious and scary task of developing a 2-factor authentication system. To develop this I learnt a lot of things, not just about web development but about Browserland(noun), its laws, and its flaws. In these posts, I'd assimilate and share some of my learnings and note down stuff and terms for my future benefit and well as anyone who accidentally reads these posts.

Authentication vs Authorisation 
	-  **Authentication:** Verify the user is who they say they are
	- **Authorization:** Decide if a user has permission to access a resource
	
HTTP the Procol that the web works on is a stateless protocol(by design). 

> The Hypertext Transfer Protocol (HTTP) is an application-level
protocol for distributed, collaborative, hypermedia information
systems. It is a generic, stateless, protocol which can be used for
many tasks beyond its use for hypertext, such as name servers and
distributed object management systems, through extension of its
request methods, error codes and headers [47]. A feature of HTTP is
the typing and negotiation of data representation, allowing systems
to be built independently of the data being transferred.(RFC 2616)

It's a neat lightweight protocol capable of wonderful things.
- Works on the client-server model. A client requests for resources, a server replies with the resource
- It is human-readable
- Extensible: you can do wonderful things by adding more headers
- Stateless(we'll talk about this in more detail soon)
- transport protocol agnostic i.e it doesn't care much for the transport protocol, it just has to exist and be reliable for HTTP to work

The HTTP protocol is a request/response protocol. 

A client sends a request to the server in the form of a **request** method, URI, and
protocol version :
>`GET / HTTP/1.1` 

(followed by a MIME-like message containing a request
modifiers, client information, and possible body content over a connection with a server)

The server **responds** with a status line, including the message's protocol version and a success or error code:
>` HTTP/1.1 200 OK` 

(followed by a MIME-like message containing server information, entity metainformation, and possible entity-body content)


For our purposes let's rephrase the **stateless property**(I have had a few tech bro friends ignorantly calling out something as "it's too stateful", I fear, without actually having a single clue as to what stateful means. I hope this writeup serves useful to you). One can rephrase it as: no 2 HTTP requests need to be aware of each other, or requests before them, or requests after them.

Then how on Earth in this stateless model that the web works on, do we go about establishing a state and verifying this state? How can we make the server remember people or clients or who requested the same resource before, so on and so forth?

There are many ways: Cookies, IP checking(TIP: look into this if you're attending uni remotely like I am, want to access university library resources, and have a friend on campus who can open a VPN for you, not that I'm doing this), Built-in HTTP authentication, client certificates. But here we are just gonna talk about one of them.

**Headers!**: We can extend the functionality of basic HTTP protocol by using headers without requiring protocol level changes. Headers let client and server pass additional information with an HTTP request or response in form of key-value pairs. 

In this particular case, we use a header with a lot of historical and emotional baggage: *Set-Cookie* header

What's the baggage?

1. Well, the web is BIG. We kept trying a lot of workings of this header, but could not unanimously agree on one(much like a gazillion JavaScript frameworks).
Before RFC 6269 thee were at least three descriptions of cookies: the so-called "Netscape cookie specification" [Netscape], RFC 2109 [RFC2109], and RFC 2965 [RFC2965]

2. Browser laws which are supposed to work in tandem with these cookies, also took time for us to agree on and implement the same.

So here is a very short bird's eye authentication 101 example: 

1. A sever sends a Set-Cookie:<actual cookie, maybe a unique pseudo-random number server saves to each user in its database>. Let's call it 
**Set-Cookie: "cndkwoh48390cbnxr3tx1?,eo1=]gk"**
The server stored it against c4ndy. like **c4ndy: cndkwoh48390cbnxr3tx1?,eo1=]gk**

2. Browser saves this value, and after receiving it once, send this header to EACH SUBSEQUENT REQUEST.

3. Server looks at each subsequent request, extracts the set-cookie header, matches the string against its user's table. If match found: server knows who it is. And authenticates based on the user's permissions. If not match found, someone is tricking it, sad life.

This way HTTP cookies act as ambient authority over the web. 

**Law of the browser land:** I said the browser is a tangled mess. So it needs laws to govern it. The fundamental security model is the **Same Origin Policy** which lays down rules of how 2 pages from different sources can or cannot interfere with each other's resources. 

"Basic Rule: Given 2 separate JavaScript execution contexts, one should only be able to access the other if the protocols, hostnames, and port numbers associated with those documents match perfectly. This "protocol-host-port tuple is called an **origin**" (Prof Feross)

**FUN IMP FACTOID: Cookies predate this Same Origin Policy so do not obey laws of the BrowserLand** 

There is again a lot to talk about browser security, same-origin policy and its exceptions, attacks, but in this particular writeup, I will primarily talk about how to set safe-ish cookies for authentication(well, to the best of my knowledge). IMHO(I am NOT a security expert. Please do your research, in general, do not make custom authentication, and invest in security audits) an authorisation cookie set like this is kinda, sorta, safe-ish(I write kinda sorta to emphasise that I do not possess credentials, yet,  to complete that sentence without the kinda sorta. ):

`Set-Cookie: key=value; Secure; HttpOnly; Path=/; SameSite=Lax`

Let's talk about each attribute and why.

1. Secure: The Secure tag prevents your cookie from being transmitted over unencrypted HTTP connections. Sending HTTP cookies which are used to determine ambient authority over unencrypted HTTP is a very bad idea (TM). If anyone sees your cookie, they can, for all purposes, pretend to be you!

Example: The server sends Bob an authorisation cookie unique to Bob over plain HTTP. Mallory is listening to Bob's traffic. Mallory sees Bob's cookie. Mallory can send the same cookie to the server. The server receives it and lets Mallory access resources that belong to Bob!

2. HttpOnly: This one can be a cause of confusion sometimes. This tag prevents the cookie from being read by JavaScript.  Without this, a malicious site can potentially iframe your site(if you are allowing framing) where the user is authenticated, do a simple **document.cookie**, and VOILA. The malicious website has your cookie. 

3. Path = / . The scope of each cookie is limited to a set of paths, controlled by the Path attribute. So one might think "hey if I just set the path to a specific URI the browser won't attach my cookies to other domains and my website will be more secure". Although seemingly useful for isolating cookies between different paths within a given host, the Path attribute cannot be relied upon for security. WHY you might ask?
Cookies can be accessed by equal or more-specific domains. Mallory can create a malicious site running with a subdomain of your site and access your user's cookies.

   Example: **myVerySecureSite.com** is your site and you set that as the path
A malicious site by the same of **lulz.myVerySecureSite.com** can access your cookies. **lulz.myVerySecureSite.com** can also set cookies for you myVerySecureSite.com!

> That is why it is a good idea to set the login URL as login.myVerySecureSite.com and NOT myVerySecureSite.com/login

   So setting the Path = / acts as a formal way of declaring that you are NOT relying on the Path attribute to protect your cookies.
   
   Thanks for reading! I hope this clears up a bit about establishing stateful properties like authentication using a stateless protocol like HTTP. In the next post, I'll talk about extending this idea to a 2 Factor Authentication System and talk more about authentication specific terms. You'll also realise why googling something like "jwt VS COOKIES" is a cardinal sin.
