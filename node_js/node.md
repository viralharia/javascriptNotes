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
  
  * Important methods of response object:
    1. `response.writeHead` is an inbuilt method which is used to send the status code and the MIME type
    2. `response.write()` is the inbuilt method which is used to send the response.
    3. `response.end()` is an inbuilt function which is used to tell the server that the request has been fulfilled.Along with that we can also send one response using this
    
## FS module
#### Read file operation using nodejs
 1. `fs.readFile()` : Read file in asynchronous way.
 2. `fs.readFileSync()` : Read file in synchronous way.
#### Write file operation using nodejs
 1. `fs.writeFile()` : Write file in asynchronous way.
 2. `fs.writeFileSync()` : Write file in synchronous way.
#### Append file operation using nodejs
 1. `fs.appendFile()` : Append file in asynchronous way.
 2. `fs.appendFileSync()` : Append file in synchronous way.
#### Rename file operation using nodejs
 1. `fs.rename()` : Rename file name in asynchronous way.
 2. `fs.renameSync()` : Rename file name in synchronous way.
#### Delete (unlink) file operation using nodejs
 1. `fs.unlink()` : Delete file in asynchronous way.
 2. `fs.unlinkSync()` : Delete file in synchronous way
