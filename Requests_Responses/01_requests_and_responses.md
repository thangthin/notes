# Tools
- python3
- git
- Nmap
  - You'll also need to install ```ncat```, which is part of the **Nmap** network testing toolkit. We'll be using ```ncat``` to investigate how web servers and browsers talk to each other.

    - Windows: Download and run [https://nmap.org/dist/nmap-7.30-setup.exe](https://nmap.org/dist/nmap-7.30-setup.exe)
    - Mac (with Homebrew): In the terminal, run ```brew install nmap```
    - Mac (without Homebrew): Download and install [https://nmap.org/dist/nmap-7.30.dmg](https://nmap.org/dist/nmap-7.30.dmg)
    - Debian/Ubuntu/Mint: In the terminal, run ```sudo apt-get install nmap```

To check whether ```ncat``` is installed and working, open up two terminals. In one of them, run ```ncat -l 9999``` then in the other, ```ncat localhost 9999```. Then type something into each terminal and press Enter. You should see the message on the opposite terminal:

What's going on here? Well, one of the ```ncat``` programs is acting as a very simple network server, and the other is acting as a client.

To exit the ```ncat``` program, type ```Control-C``` in the terminal. If you exit the server side first, the client should automatically exit. This happens because the server ends the connection when it shuts down.

# Your first web server
An HTTP transaction always involves a client and a server. You're using an HTTP client right now — your web browser. Your browser sends HTTP requests to web servers, and servers send responses back to your browser. Displaying a simple web page can involve dozens of requests — for the HTML page itself, for images or other media, and for additional data that the page needs.

HTTP was originally created to serve hypertext documents, but today is used for much more. As a user of the web, you're using HTTP all the time.

python3 has a build in web server
```
python3 -m http.server 8000
```
## What's a server anyway?
A server is just a program that accepts connections from other programs on the network.

When you start a server program, it waits for clients to connect to it — like the demo server waiting for your web browser to ask it for a page. Then when a connection comes in, the server runs a piece of code — like calling a function — to handle each incoming connection. A connection in this sense is like a phone call: it's a channel through which the client and server can talk to each other. Web clients send requests over these connections, and servers send responses back.

# URI
### Parts of URI
A web address is also called a **URI** for *Uniform Resource Identifier*. You've seen plenty of these before. From a web user's view, a **URI** is a piece of text that you put into your web browser that tells it what page to go to. From a web developer's view, it's a little bit more complicated.

You've probably also seen the term **URL** or *Uniform Resource Locator*. These are pretty close to the same thing; specifically, a URL is a URI for a resource on the network. Since URI is slightly more precise, we'll use that term in this course. But don't worry too much about the distinction.
A URI is a name for a resource — such as this lesson page, or a Wikipedia article, or a data source like the Google Maps API. URIs are made out of several different parts, each of which has its own syntax. Many of these parts are optional, which is why URIs for different services look so different from one another.

Here is an example of a URI: ```https://en.wikipedia.org/wiki/Fish```

This URI has three visible parts, separated by a little bit of punctuation:

```https``` is the **scheme**;
```en.wikipedia.org``` is the **hostname**;
and ```/wiki/Fish``` is the **path**.
Different URIs can have different parts; we'll see more below.

## Scheme
The first part of a URI is the **scheme**, which tells the client how to go about accessing the resource. Some URI schemes you've seen before include **http**, **https**, and **file**. File URIs tell the client to access a file on the local filesystem. HTTP and HTTPS URIs point to resources served by a web server.

HTTP and HTTPS URIs look almost the same. The difference is that when a client goes to access a resource with an HTTPS URI, it will use an encrypted connection to do it. Encrypted Web connections were originally used to protect passwords and credit-card transactions, but today many sites use them to help protect users' privacy. We'll look more into HTTPS near the end of this course.

There are many other URI schemes out there, though. You can take a look at the [official list](http://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml)!

## Hostname
In an HTTP URI, the next thing that appears after the scheme is a **hostname** — something like ```www.udacity.com``` or ```localhost```. This tells the client which server to connect to.

You'll often see web addresses written as just a hostname in print. But in the HTML code of a web page, you can't write ```<a href="www.google.com">this</a>```and get a working link to Google. A hostname can only appear after a URI scheme that supports it, such as ```http``` or ```https```. In these URIs, there will always be a ```://``` between the scheme and hostname.

We'll see more about hostnames later on in the lesson. By the way, not every URI has a hostname. For instance, a mailto URI just has an email address: ```mailto:spam@example.net``` is a well-formed ```mailto``` URI. This also reveals a bit more about the punctuation in URIs: the ```:``` goes after the scheme, but the ```//``` goes before the hostname. Mailto links don't have a hostname part, so they don't have a ```//```.

## Path
In an HTTP URI (and many others), the next thing that appears is the ```path```, which identifies a particular resource on a server. A server can have many resources on it — such as different web pages, videos, or APIs. The path tells the server which resource the client is looking for.

On the demo server, the paths you see will correspond to files on your filesystem. But that's just the demo server. In the real world, URI paths don't necessarily equate to specific filenames. For instance, if you do a Google search, you'll see a URI path such as ```/search?q=ponies```. This doesn't mean that there's literally a file on a server at Google with a filename of ```search?q=ponies```. The server interprets the path to figure out what resource to send. In the case of a search query, it sends back a search result page that maybe never existed before.

When you write a URI *without* a path, such as ```http://udacity.com```, the browser fills in the default path, which is written with a single slash. That's why ```http://udacity.com``` is the same as ```http://udacity.com/``` (with a slash on the end).

The path written with just a single slash is also called the **root**. When you look at the root URI of the demo server — ```http://localhost:8000/``` — you're not looking at the root of your computer's whole filesystem. It's just the root of the resources served by the web server. The demo server won't let a web browser access files outside the directory that it's running in.

## Relative URI references
Take a look at the HTML source for the demo server's root page. Find one of the ```<a>``` tags that links to a file. In mine, I have a file called ```cliffsofinsanity.png```, so there's an ```<a>``` tag that looks like this:

```<a href="cliffsofinsanity.png">cliffsofinsanity.png</a>```

URIs like this one don't have a scheme, or a hostname — just a path. This is a **relative URI reference**. It's "relative" to the context in which it appears — specifically, the page it's on. This URI doesn't include the hostname or port of the server it's on, but the browser can figure that out from context. If you click on one of those links, the browser knows from context that it needs to fetch it from the same server that it got the original page from.

## Other URI parts
There are many other parts that can occur in a URI. Consider the difference between these two Wikipedia URIs:

- [https://en.wikipedia.org/wiki/Oxygen](https://en.wikipedia.org/wiki/Oxygen)
- [https://en.wikipedia.org/wiki/Oxygen#Discovery](https://en.wikipedia.org/wiki/Oxygen#Discovery)

If you follow these links in your browser, it will fetch the *same* page from Wikipedia's web server. But the second one displays the page scrolled to the section about the discovery of oxygen. The part of the URI after the ```#``` sign is called a **fragment**. The browser doesn't even send it to the web server. It lets a link point to a specific named part of a resource; in HTML pages it links to an element by id.

In contrast, consider this Google Search URI:

- [https://www.google.com/search?q=fish](https://www.google.com/search?q=fish)
The ```?q=fish``` is a query part of the URI. This does get sent to the server.

There are a few other possible parts of a URI. For way more detail than you need for this course, take a look at this Wikipedia article:

- [https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax)

# Hostnames
A full HTTP or HTTPS URI includes the hostname of the web server, like ```www.udacity.com``` or ```www.un.int``` or ```www.cheeseboardcollective.coop``` (my favorite pizza place in the world, in Berkeley CA). A hostname in a URI can also be an IP address: for instance, if you put [http://216.58.194.174/](http://216.58.194.174/) in your browser, you'll end up at Google.

> Why is it called a **host**name? In network terminology, a **host** is a computer on the network; one that could host services.
The Internet tells computers apart by their **IP addresses**; every piece of network traffic on the Internet is labeled with the IP addresses of the sending and receiving computers. In order to connect to a web server such as ```www.udacity.com```, a client needs to translate the hostname into an IP address. Your operating system's network configuration uses the **Domain Name Service (DNS)** — a set of servers maintained by Internet Service Providers (ISPs) and other network users — to look up hostnames and get back IP addresses.

In the terminal, you can use the host program to look up hostnames in DNS:
```
$ host www.google.com
```

Some systems don't have the ```host``` command, but do have a similar command called ```nslookup```. This command also displays the IP address for the hostname you give it; but it also shows the IP address of the DNS server that's giving it the answer:
```
$ nslookup www.google.com
```

To lookup localhost ip address
```
$ host localhost
```

## Localhost
The IPv4 address ```127.0.0.1``` and the IPv6 address ```::1``` are special addresses that mean "this computer itself" — for when a client (like your browser) is accessing a server on your own computer. The hostname ```localhost``` refers to these special addresses.

When you run the demo server, it prints a message saying that it's listening on ```0.0.0.0```. This is not a regular IP address. Instead, it's a special code for "every IPv4 address on this computer". That includes the ```localhost``` address, but it also includes your computer's regular IP address.

## example
```$ host en.wikipedia.org```
```$ host en.ja.wikipedia.org```
At least as of October 2016, these sites were on the same IP address. A single web server can have lots of different web sites running on it, each with their own hostname. When a client asks the server for a resource, it has to specify what hostname it intends to be talking to. We'll see more about this later, in the section on HTTP headers.

## Ports
When you told your browser to connect to the demo server, you gave it the URI ```http://localhost:8000/```. This URI has a port number of ```8000```. But most of the web addresses you see in the wild don't have a port number on them. This is because the client usually figures out the port number from the URI scheme.

For instance, HTTP URIs imply a port number of ```80```, whereas HTTPS URIs imply a port number of ```443```. Your Python demo web server is running on port ```8000```. Since this isn't the default port, you have to write the port number in URIs for it.

**What's a port number, anyway?** To get into that, we need to talk about how the Internet works. All of the network traffic that computers send and receive — everything from web requests, to login sessions, to file sharing — is split up into **messages** called **packets**. Each packet has the IP addresses of the computer that sent it, and the computer that receives it. And (with the exception of some low-level packets, such as ping) it also has the port number for the sender and recipient. IP addresses distinguish computers; port numbers distinguish *programs* on those computers.

We say that a server "listens on" a port, such as ```80``` or ```8000```. "Listening" means that when the server starts up, it tells its operating system that it wants to receive connections from clients on a particular port number. When a client (such as a web browser) "connects to" that port and sends a request, the operating system knows to forward that request to the server that's listening on that port.

> Why do we use port ```8000``` instead of ```80``` for the demo server? For historical reasons, operating systems only allow the administrator (or root) account to listen on ports below ```1024```. This is fine for production web servers, but it's not convenient for learning.

# HTTP GET requests
Take a look back at the server logs on your terminal, where the demo server is running. When you request a page from the demo server, an entry appears in the logs with a message like this:
```
127.0.0.1 - - [03/Oct/2016 15:45:50] "GET /readme.png HTTP/1.1" 200 -
```
Take a look at the part right after the date and time. Here, it says ```"GET /readme.png HTTP/1.1"``` This is the text of the request line that the browser sent to the server. This log entry is the server telling you that it received a request that said, literally, ```GET /readme.png HTTP/1.1```.

This request has three parts.

The word ```GET``` is the **method** or **HTTP verb** being used; this says what kind of request is being made. ```GET``` is the verb that clients use when they want a server to send a resource, such as a web page or image. Later, we'll see other verbs that are used when a client wants to do other things, such as submit a form or make changes to a resource.

```/readme.png``` is the **path** of the resource being requested. Notice that the client does not send the whole URI of the resource here. It doesn't say ```https://localhost:8000/readme.png```. It just sends the path.

Finally, ```HTTP/1.1``` is the **protocol** of the request. Over the years, there have been several changes to the way HTTP works. Clients have to tell servers which dialect of HTTP they're speaking. HTTP/1.1 is the most common version today.

## Exercise: Send a request by hand
You can use ncat to connect to the demo server and send it an HTTP request by hand. (Make sure the demo server is still running!)

Try it out:
> I used the command ```ncat 127.0.0.1 8000``` to connect to port 8000.
> Then I typed out an HTTP request on the next two lines.
Use ```ncat 127.0.0.1 8000``` to connect your terminal to the demo server.

Then type these two lines:
```
GET / HTTP/1.1
Host: localhost
```
After the second line, **press Enter twice**. As soon as you do, the response from the server will be displayed on your terminal. Depending on the size of your terminal, and the number of files the web server sees, you will probably need to **scroll up** to see the beginning of the response!

# HTTP Responses
Here's another one to try. Use ncat to connect to google.com port 80, and send a request for the path / on the host google.com:
```
GET / HTTP/1.1
Host: google.com
```

> Make sure to send ```Host: google.com``` exactly ... don't slip a ```www``` in there. These are actually different **hostnames**, and we want to take a look at the difference between them. And **press Enter twice**!

The HTTP response is made up of three parts: the **status line**, some **headers**, and a **response body**.

The status line is the first line of text that the server sends back. The headers are the other lines up until the first blank line. The response body is the rest — in this case, it's a piece of HTML.

## Status line
In the response you got from your demo server, the status line said ```HTTP/1.0 200 OK```. In the one from Google, it says ```HTTP/1.1 301 Moved Permanently```. The status line tells the client whether the server understood the request, whether the server has the resource the client asked for, and how to proceed next. It also tells the client which dialect of HTTP the server is speaking.

The numbers 200 and 301 here are **HTTP status codes**. There are dozens of different status codes. The first digit of the status code indicates the general success of the request. As a shorthand, web developers describe all of the codes starting with **2** as "**2xx**" codes, for instance — the x's mean "any digit".

- **1xx — Informational**. The request is in progress or there's another step to take.
- **2xx — Success**! The request succeeded. The server is sending the data the client asked for.
- **3xx — Redirection**. The server is telling the client a different URI it should redirect to. The headers will usually contain a **Location** header with the updated URI. Different codes tell the client whether a redirect is permanent or temporary.
- **4xx — Client error**. The server didn't understand the client's request, or can't or won't fill it. Different codes tell the client whether it was a bad URI, a permissions problem, or another sort of error.
- **5xx — Server error**. Something went wrong on the server side.
You can find out much more about HTTP status codes in this [Wikipedia article](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) or in the [specification for HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

The server response here is an example of good user interface on the Web. Google wants browsers to use ```www.google.com``` instead of ```google.com```. But instead of showing the user an error message, they send a redirect. Browsers will automatically follow the redirect and end up on the right site.

## Headers
An HTTP response can include many **headers**. Each header is a line that starts with a keyword, such as ```Location``` or ```Content-type```, followed by a colon and a value. **Headers are a sort of metadata for the response. They aren't displayed by browsers or other clients; instead, they tell the client various information about the response.**

Many, many features of the Web are implemented using headers. For instance, **cookies** are a Web feature that lets servers store data on the browser, for instance to keep a user logged in. To set a cookie, the server sends the ```Set-Cookie``` header. The browser will then send the cookie data back in a ```Cookie``` header on subsequent requests. You'll see more about cookies later in this course.

For the next quiz, take a look at the Content-type header sent by the Google server and the demo server. Both servers send the exact same value:
```
Content-type: text/html; charset=utf-8
```
What do you think this means?
The server is telling the cient that the response body is an HTML document written in UTF-8 text.

A ```Content-type``` header indicates the kind of data that the server is sending. It includes a general category of content as well as the specific format. For instance, a PNG image file will come with the Content-type ```image/png```. If the content is text (including HTML), the server will also tell what encoding it's written in. UTF-8 is a very common choice here, and it's the default for Python text anyway.

Very often, the headers will contain more metadata about the response body. For instance, both the demo server and Google also send a ```Content-Length``` header, which tells the client how long (in bytes) the response body will be. If the server sends this, then the client can reuse the connection to send another request after it's read the first response. Browsers use this so they can fetch multiple pieces of data (such as images on a web page) without having to reconnect to the server.

## Response body
The headers end with a blank line. Everything after that blank line is part of the **response body**. If the request was successful (a ```200 OK``` status, for instance), this is a copy of whatever resource the client asked for — such as a web page, image, or other piece of data.

But in the case of an error, the response body is where the error message goes! If you request a page that doesn't exist, and you get a ```404 Not Found``` error, the actual error message shows up in the response body.

## Exercise: Be a web server!
Use ```ncat -l 9999``` to listen on port 9999. Connect to it with your web browser at ```http://localhost:9999/```. What do you see in your terminal?

Keep that terminal open!

Next, send an HTTP response to your browser by typing it into the terminal, right under where you see the headers the browser sent to you:
```
HTTP/1.1 307 Temporary Redirect
Location: https://www.eff.org/
```
At the end, **press Enter twice** to send a blank line to mark the end of headers.


Do it again! Run ```ncat -l 9999``` to play a server, and get your browser to access it. But this time, instead of sending a 307 redirect, send a 200 OK with a piece of text in it:
```
HTTP/1.1 200 OK
Content-type: text/plain
Content-length: 50

Hello, browser! I am a real HTTP server, honestly!
```