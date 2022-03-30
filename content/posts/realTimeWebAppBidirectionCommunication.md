+++
author = "c4ndy"
title = "Real Time Web App #1 Bi-directional Communication: Server, The CandyMan"
date = "2022-03-30"
description = "Real Time web App"
tags = [
    "http",
    "web developement",
    "web-security",
    "polling",
    "websockets",
    
]
categories = [
    "web development",
    "real time",
    "django",
    "python",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
+++
# Real Time Web App #1 Bi-directional Communication: Server, The CandyMan

#### How do websites usually work?

You click something: a button, a link, a hyperlink, a form button i.e you request for a resource from a web-server, in return the website gives you some text, an image, or some other media, more specifically a constructed web document, and a 2xx status code i.e the web-server responds to your request with some resource. Sometimes if that resource is not available it the web server will send a 404 not found response, or if the resource has been moved it will send s 3xx response

This is achieved through a protocol called HTTP(Hyper text Transfer Protol)

HTTP is a protocol for fetching resources such as HTML documents. It is the foundation of any data exchange on the Web and it is a client-server protocol, which means requests are initiated by the recipient, usually the Web browser. A complete document is reconstructed from the different sub-documents fetched, for instance, text, layout description, images, videos, scripts, and more.

#### A usual HTTP session looks like this:

1.  The client establishes a TCP connection (or the appropriate connection if the transport layer is not TCP).
2.  The client sends its request, and waits for the answer.
3.  The server processes the request, sending back its answer, providing a status code and appropriate data.

It would have become evident that HTTP is a client-server protocol i.e A client(usually a web browser) requests for a resource and a web server responds with it. A web server *cannot* just arbitrarity send out resources to client.

#### But wait, we are making a real time web-app, that implies some *things*(anythings, we dont have to define things rn) change for one client based on other clients doing other *things* i.e user interactions. User interactions usually mean **Server Side Events**. What about them?

example: a T aims at your head(the exact pixels that make your head at a given time *t*), shoots their AK, and one tap kills you. Sad life for you :(, but, iff, the server logs you as dead, and then actually tells you "hey you got killed, sad life you you :(" 

This is an arbitrary response that the server needs to send it, you cannot request for it, neither can you deterministically predict when to ask for it. It's like server needs to be candyman handing out candies(well, death candy in this case)

There are 2 broad ways to go about it:
 # 1. **Polling:** 

In the standard HTTP model, a server cannot initiate a connection with a client nor send an unrequested HTTP response to a client; thus, the server cannot push asynchronous events to clients. Therefore, in order to receive asynchronous events as soon as possible, the client needs to poll the server periodically for new content.

 The two most common server-push mechanisms are HTTP long polling and HTTP streaming:

 -  HTTP Long Polling:  The server attempts to "hold open" (not
      immediately reply to) each HTTP request, responding only when
      there are events to deliver.  In this way, there is always a
      pending request to which the server can reply for the purpose of
      delivering events as they occur, thereby minimizing the latency in
      message delivery.

  - HTTP Streaming:  The server keeps a request open indefinitely; that
      is, it never terminates the request or closes the connection, even
      after it pushes data to the client.
	  
(This is framework agnostic, but the following examples will be django-python.)

There are again, various ways to implement polling in Django, the quickest and cleanest I've found is this magical library called HTMX(i cannot stress enough of my love for you), other than that, this also let's you get away without writing a shit ton of Javascript (more server side ftw), and let's you add implemt JavaScript functionalities throgh pure HTML attributes. Do check it out!

Step 1. Include the script source in your HTML template: 
```
<script src="https://unpkg.com/htmx.org@1.7.0" integrity="sha384-EzBXYPt0/T6gxNp0nuPtLkmRpmDBbjg6WmCUZRLXBBwYYmwAUxzlSGej0ARHX0Bo" crossorigin="anonymous"></script>
```



Step 2: 
Add the htmx attribute, point it the URL you want to poll, mention the http method you want to make requests with, put in the poll frequency.
```
<div hx-get="/pollingEndpoint/" hx-trigger="every 2s">
```


Step 3: make the end point. (views.py in Django)
```
print(randint(1, 100))
def pollingEndpoint(request):
    if request.method == "GET": 
		rand  = randint(1, 100)
    return HttpResponse(rand)
```

Voila!

# **2. Web Sockets**

So as you can see, creating web applications that need bidirectional communication between a client and a server (e.g., instant messaging and gaming applications) has required an abuse of HTTP to poll the server for updates while sending upstream notifications as distinct HTTP calls [RFC6202]

Websockets allow you to use a single TCP connection along with a Websocket API to send traffic in both directions. it provides an alternative to HTTP polling for two-way communication from a web page to a remote server. It is designed to work over HTTP ports 80 and 443 as well as to support HTTP proxies and intermediaries, even if this implies some complexity specific to the current environment. 

- Websockets can work alongside HTTP. Ideally all synchronous stuff should be over HTTP and asyncronous/user-interactions/server sent events over WebSockets. And they work together!
- The WebSocket Protocol is an independent TCP-based protocol. Its only relationship to HTTP is that its handshake is interpreted by HTTP servers as an Upgrade request. By default, the WebSocket Protocol uses port 80 for regular WebSocket connections and port 443 for WebSocket connections tunneled over
Transport Layer Security (TLS) [RFC2818].

The protocol has two parts: a handshake and the data transfer
	 
The handshake from the client looks as follows:
```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Origin: http://example.com
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
```


The handshake from the server looks as follows:
```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
Sec-WebSocket-Protocol: chat
```

To use websockets in you django app, you can use channels: https://channels.readthedocs.io/en/stable/
It might take a few minutes to an hour to once get started with, once you get it working working with it is essentially the same as working with the MVC paradigm.

- Instead of **view.py**s you have **consumers.py** and you now recieve the power up to write async code iin them.
- For routing we have a **routing.py** instead or **urls.py**

A good resource to check out how to integrate channels is : https://www.youtube.com/watch?v=R4-XRK6NqMA

# Thoughts:
If you reached here and read through both the examples in this writeup both websockets and polling examples, you would have realised they are doing the same thing: sending a random number to the client. They are just doing it differently.

This was a simple example(you could argue this this did not require any server side code at all, and you'd be be absolutely right in that, but this is an example, replace the random number with some something server generated and that's where the use case lies)   

I personally like websockets. As complexity of your app, and load on the app, increases, polling starts to feel a bit t0o crowd-y.

Before deciding on what to use take into account pros and cons of both the protocols itselt, but also:
- complexity of your app
- load on the app
- framework (yes. you know why)

 ---
###### Further Readings:
- A typical HTTP session: https://developer.mozilla.org/en-US/docs/Web/HTTP/Session
- An Overview of HTTP: https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview
- Known Issues and Best Practices
    for the Use of Long Polling and Streaming in Bidirectional HTTP: https://datatracker.ietf.org/doc/html/rfc6202
- 