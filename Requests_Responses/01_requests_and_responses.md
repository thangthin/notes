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

## URI
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
