---
layout: post
title: cURL, A Brief Overview
excerpt: "A succinct overview of curl command line tool for developers."
categories: [curl, tutorial, command line, networking]
tags:
      - curl
      - tutorial
      - command line
      - networking
comments: true
image:
  feature: /posts/curl/1.png
  credit: curl website
  creditlink: https://curl.haxx.se/logo/curl-logo.svg
---

### Introduction

**curl** is a tool to query URLs from the command line or the terminal, inbuilt into Linux and Mac Operating Systems. It is a tool to transfer data to and from a server using one of the supporting protocols - FTP, , Gopher, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, , SMB, SMBS, SMTP, SMTPS, Telnet and TFTP. is a C API library that can be used to get features of curl in the application code.

curl can be used for:

* *User Authentication.*
* *FTP upload.*
* *HTTP requests.*
* *Save responses to Files.*
* *SSL connections.*
* *Cookies.*
* *Test REST APIs and many more things.*

All of the above tasks can be performed through the Command Line using curl.

### Some Basic Functions

Now, let us take a look at some of the most basic functions that can be performed using curl:

* To display the entire response or the content of the URL in the terminal. We can force curl to log verbosely by using the flags ***-v*** and ***-trace.***

  {% highlight sh %}
  curl http://www.google.com
  curl http://localhost:5000/some-endpoint

  curl -v http://www.google.com
  curl -trace http://www.google.com
  {% endhighlight %}

* To display the content of the URL including the Response Headers.

  {% highlight sh %}
  curl -i http://www.google.com
  curl --include http://www.google.com
  {% endhighlight %}

* To print only the Headers of the Response.

  {% highlight sh %}
  curl -I http://www.google.com
  curl --head http://www.google.com
  {% endhighlight %}

* To make an HTTP request.

  The common format is: *curl {-X/--request} [POST \| PUT \| GET \| DELETE] {-d/--data} [actual data here] URL*

  {% highlight sh %}
  curl -X GET https://jsonplaceholder.typicode.com/users
  curl -X POST -d "UserId=2" https://jsonplaceholder.typicode.com/users
  curl --request POST --data "UserId=2" https://jsonplaceholder.typicode.com/users
  curl -X PUT -d "UserName=Robin, Williams" https://jsonplaceholder.typicode.com/users
  curl -X DELETE https://jsonplaceholder.typicode.com/users/3
  {% endhighlight %}

  If *-X* or *--request* flag is specified, but *-d* or *--data* flag is missing then by default, curl assumes it to be an HTTP POST request. For instance, in the example below HTTP POST is inferred.

  {% highlight sh %}
  curl -d "UserId=3" https://jsonplaceholder.typicode.com/users
  {% endhighlight %}

  We can pass parameters in the query string format using curl. The query string is usually appended to the URL using a *question mark(?)*.

  {% highlight sh %}
  curl -X POST https://jsonplaceholder.typicoe.com/posts?userId=1
  curl https://jsonplaceholder.typicoe.com/posts?userId=1
  {% endhighlight %}

  We can send the contents of a file using the HTTP POST request. The entire content of the file will be serialised in some binary format and sent across the network in the HTTP POST payload.

  {% highlight sh %}
  curl -X POST -d "@/path/to/file/filename" https://sampleserver.com/
  {% endhighlight %}

  curl can emulate a filled-in form in which a user has pressed the submit button. In this way, curl can **POST** data using the **Content-Type** *multipart/form-data*.

  {% highlight sh %}
  curl -X POST -F firstname=John -F lastname=Doe http://www.form.com
  curl -X POST -F name[first]=John -F name[last]=Doe http://www.form.com
  curl -X POST --form userId=1 --form name=Doe http://www.form.com
  {% endhighlight %}

* We can set request headers in curl using the *-H* or *--header* flag.

  {% highlight sh %}
  curl https://www.authservice.com -H "Accept:application/json" -H "x-auth:345tyu" -H "x-custom-header:value"
  curl https://www.authservice.com --header "Accept:application/json" -header "x-auth:345tyu" -header "x-custom-header:value"
  {% endhighlight %}

  **Authorization:Basic** - It is a basic authorization technique where the client sends the username/password with each request.

  **x-auth-token** - The client sends the username/password the first time the session is established with a server and then the server authenticates the client and sets an "x-auth-token" in the response which the client needs to send in every future communication with the server in the same session.

* To authenticate a user, we can use the *-u* or *--user* flag. We can specify the user-name and password to be used for the server authentication. If we simply specify the user-name, curl will prompt for a password.

  {% highlight sh %}
  curl -u username http://www.example.com
  curl -u username:password http://www.example.com
  curl --user username http://www.example.com
  curl --user username:password http://www.example.com
  {% endhighlight %}

  But rather than passing the plain username and the password string on the command line which could be a security hazard, we can use the - -- flags. It makes curl scan the *.netrc(_netrc on Windows)* file in the user's home directory for login name and password. curl will not complain if that file doesn't have the right permissions(it should not be either world- or group-readable). The environment variable *HOME* is used to find the home directory. A sample format of the contents of the .netrc file is as below:

  {% highlight ascii %}
  machine host.domain.com login myself password secret
  {% endhighlight %}

  Here, the **machine** and **password** are the keywords. The actual values of the host machine, username password associated with that username are *host.domain.com*, *myself* and *secret* respectively.

  {% highlight sh %}
  curl --netrc-file path-to-my-password-file http://www.example.com
  curl -n path-to-my-password-file http://www.example.com
  {% endhighlight %}

* There are many ways to download the content from an URL.

  The command below creates a new file *output.html* and writes the contents of the response into it.

  {% highlight sh %}
  curl www.yahoo.com > output.html
  {% endhighlight %}

  The commands below downloads the file *index.html* from the given URL. You can also use the flag to download multiple files simultaneously.

  {% highlight sh %}
  curl -o index.htm http://www.yahoo.com
  curl --output index.html http://www.yahoo.com
  curl -o aa -o bb http://www.example.com http://www.example.net
  {% endhighlight %}

  We can persist the contents of a remote file using the *-O* or *--remote-name* flags. It creates a new file in the local file system.

  {% highlight sh %}
  curl -O http://www.yahoo.com/index.html
  curl --remote-name index2.html http://www.yahoo.com/index.html
  {% endhighlight %}

  Both the *-o* and the *-O* flags display a progress bar while downloading the content of the requested file. Now suppose, you had started a download using curl and stopped it midway using *Ctrl+C* then you can resume the download using the *"-C -"* or *--continue-at* flag.

  {% highlight sh %}
  curl -C - -O http://www.yahoo.com/index.html
  curl --continue-at -O http://www.yahoo.com/index.html
  {% endhighlight %}

* We can download resources from an FTP URL. We can also provide a regex at the end of the FTP URL to download resources whose name matches the regex.

  {% highlight sh %}
  curl "ftp://user:password@myftpsite/users/myfolder/myfile.raw"
  curl "ftp://user:password@myftpsite/users/myfolder/myfile.raw/[A-E]"
  {% endhighlight %}

* We can download files from a URL that has been modified later than the given time and date, or one that has been modified before that time using the *-z* or the *--time-cond* flags. Start the date expression with a *dash (-)* to make it request for a document that is older than the given date/time, the default is a document that is newer than the specified date/time.

  {% highlight sh %}
  curl -z 21-Jul-2017 -O http://www.yahoo.com/index.html

  curl -z -21-Jul-2017 -O http://www.yahoo.com/index.html

  curl --time-cond 22-Oct-2017 -O http://www.yahoo.com/index.html
  {% endhighlight %}

* We can limit the download speed using the *--limit-rate* file. In the example below, we limit the download speed to 1000B per second.

  {% highlight sh %}
  curl --limit-rate 1000B -O http://www.yahoo.com/index.html
  {% endhighlight %}

* We can upload files using the *- --upload-file* flags.

  {% highlight sh %}
  curl -u ftpusr:ftppsw -T "filename" FTPURL/directory
  curl -u ftpusr:ftppsw -T "{file1,file2,file3}" FTPURL/directory
  curl --upload-file ftpusr:ftppsw -T "filename" FTPURL/directory
  {% endhighlight %}

* We can ask curl to follow redirections if the response received from a requested URL is *HTTP 301* or *HTTP 302*. By default, curl will never redirect to the newer locations.

  {% highlight sh %}
  curl -L http://www.google.com
  curl --location http://www.google.com
  {% endhighlight %}

  Some basic difference between **HTTP 301** and **HTTP 302**:

  | HTTP 301 | HTTP 302 |
  |:--------|:-------|
  | permanent redirection  | temporary redirection  |
  | The page is permanently moved to a new location  | The page is temporarily moved to a new location   |
  | Search Engines will start indexing new page and will remove old indexes gradually  | Search Engines will not index new page but will continue index old page   |
  | Good practice for SEO | Bad practice for SEO  |
  | Tough to implement  | Easy to implement  |
  |----

* To make use of the proxy servers.

  For computers who don't have direct access to the Internet and need to access the Internet through a proxy server, they need to connect to proxy server initially and make requests through it. To connect to the proxy we can use the *- --proxy* flags.

  {% highlight sh %}
  curl -x <proxy-server> [usual curl command for the resource]
  curl --remote <proxy-server> [usual curl command for the resource]
  curl -x <protocol>://<user>:<password>@<host>:<port> [curl command]
  curl --proxy http://125.119.175.48:8909 http://www.example.com
  {% endhighlight %}

##### Proxy Servers

Proxy servers are used to improve security in an organization. An organization's secured internal network connects a lot of computers with private IP addresses, these computers don't have individual public IP and hence cannot access the Internet directly. They have to access the Internet through a system which lies between the company's internal network and the Internet. This machine is the **Proxy.**

![Proxy Basic1]({{ site.url }}/img/posts/curl/2.png)

Proxy servers are used to obscure the client IPs and identities - The computers in the local network have private IPs, they cannot be routed on the Internet. The proxy server will convert the private IP of a system to a public IP when the system makes a request for a resource over the Internet. The host server on the Internet thinks that the request is coming from a client on the Internet with a public IP and responds to the request on the same IP. The response is received by the proxy and routed to the concerned internal system. The proxy acts a barrier between internal system and the external Internet and everything needs to be routed via the proxy.

![Proxy Basic2]({{ site.url }}/img/posts/curl/3.png)

i) The proxy blocks malicious traffic as the servers on the Internet doesn't know the actual IP(private IP) of the client it cannot directly communicate with the client. The client remains unidentified and the proxy is what the external system has to contend with.

ii) The proxy can also block requests to the prohibited sites and can act as a **Filter**.

iii) The proxy **monitors** and **logs** the traffic activity etc.

iv) The proxy can improve network performance by **caching** responses such that the next time an internal system requests a similar resource, it can fulfil it from its cache rather than connect to the Internet.

![Proxy Basic2]({{ site.url }}/img/posts/curl/4.png)

##### Bibliography
[curl manual](http://curl.haxx.se){:target="_blank"}
