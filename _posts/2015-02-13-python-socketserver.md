---
layout: post
title: Using Python SocketServer
author: Patrick Shriwise
category: upcoming
tags: python server 
---


## Attending

- 


## Patrick Shriwise

Graduate student of Paul's at UW-Madison.

## Using Python's SocketServer

Today's presenation will be a brief demo of how to setup a local server, handling get and post requests using the server, and using the server to access classes or modules within python from the server instance. We will be using code from [this repo][repo].

###Python modules needed to follow along

       -python-socketserver (for server)
       -python-simplehttpserver (for request handler, ususally comes with python)

##Setting up the server

1. Import the needed modules

2. Create a handler for this server

3. Make a new threaded server class

4. Starup the server

---
```
#1
import SimpleHTTPServer, SocketServer

#2
class Handler(SimpleHTTPServer.SimpleHTTPRequestHandler):

    def do_GET(self):
    	SimpleHTTPServer.SimpleHTTPRequestHandler.do_GET(self)

#3
PORT=4000
httpd = SocketServer.ThreadingTCPServer(("", PORT), Handler) # Can also use ForkingTCPServer

#4
print "serving at port", PORT
httpd.serve_forever()
```
---

You can now access this server by navigating to <http://localhost:4000>, and what you'll see is pretty standard server behavior. There will be a listing of all the files in your directory as well as some navigation options for moving through your file tree. By having this in our own python instance however, we can do more... 




[repo]: https://github.com/Pshriwise/thw_server_demo
[code]: {{ site.github.repository_url }}/tree/master/topic
