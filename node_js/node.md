# Node_js
* Node markets itself as an asynchronous, event-driven framework built on top of Chrome's JavaScript engine and designed for creating scalable network applications. 
* Node is single-threaded and uses a concurrency model based on an event loop.
* Node is ideal for I/O bound applications (or those that wait on user events), but not so great for CPU-heavy applications. Good examples include data-intensive realtime applications (DIRT), single page applications, JSON APIs, and data-streaming applications.
* From a developer's point of view Node.js is single-threaded - but under the hood libuv handles threading, file system events, implements the event loop, features thread pooling and so on. In most cases you won't interact with it directly.

## Http module
* Node provide us with the inbuilt `http` module which we can use to create the server. 
* Get the access to the module by using `require`
* Create a server using the `createServer()` method. `createServer()` method takes a callback function as arguments. This callback is executed each time a request is received.
  * The two arguments of the callback function are : 
    1. request : which contains all the information related to client's request such as URL, custom headers , client info , etc. 
    2. response : which is used to return the data back to the client.
