# Bruteforcer
Advanced header wordlist bruteforce tool. 
It's a <b>Free</b> option to Burp Suite Professional Intruder.
<i> [I don't have money to buy Burp Suite Professional Intruder, so I wrote my own.]</i>)

# Setup

```git clone https://github.com/RafaelSantos025/Bruteforcer.git```

```cd Bruteforcer/```

```chmod +x bruteforcer```

### Used Python Libs

```
requests
argparse
threading
os
sys
urllib3
time
queue
```

# How to use

### Basic usage: 
You can pass an file with the desired raw request or an URL, these commands are mandatory.

```./bruteforcer [file-path/url] [wordlist]```

<b>Key to replace: </b>

<i>Like Intruder, the advanted is to replace any string with the desired wordlist</i>


<b>Passing an URL: </b> 

* The exmaple below will make a bruteforce attack in the path parameter (indicated by the <i>Replace Key (^^)</i>)

    ```./bruteforcer "https://www.example.com/^toReplace^" wordlist.txt```

* The following command will bruteforce some hidden URL parameters in the target:

    ```./bruteforcer "https://www.example.com/create?^^=true" wordlist.txt```

Note that when passing URLs as a parameter, the only request method that the program can perform is the GET method.


<b>Passing an request file: </b>

The request file, called "request.txt" will looks like this:

```
GET /^toReplace^ HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept-Encoding: gzip, deflate
Connection: close
```

And finally, the command will be like this:

```./bruteforcer request.txt wordlist.txt```

As you can see, this file will setup a bruteforce attack to the path parameter too (like the url example), but now, you have total control of the request header.

### More Requests Examples
Like Burp Intruder, you can change the request as you want. More examples that you can do with <b>Bruteforcer</b> below: 

<b>Requests With Body: </b>

```
POST /^^ HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: close
Accept-Encoding: gzip, deflate
Host: www.example.com
Content-Type: application/json
Content-Lenght: 3

{}
```


<b>Replacing Headers: </b>

* The followinng request will try to bruteforce some boolean cookie (e.g: <i>isAdmin=true</i>):

```
GET /api/ HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: close
Accept-Encoding: gzip, deflate
Host: example.com
Content-Type: application/json
Cookie: ^replace^=true;
```

* The next request will bruteforce some hidden headers (e.g: <i>X-Forwarded-For</i>):

```
GET /api/ HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: close
Accept-Encoding: gzip, deflate
Host: example.com
^replace^: 127.0.0.1
Content-Type: application/json
Cookie: session=hjchrhhnci4ofjniej203dvervweocikrm09kvriorver;
```

* The next Request will bruteforce some Content-Type's:

```
PUT / HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: close
Accept-Encoding: gzip, deflate
Host: example.com
Content-Length: 0
Content-Type: ^application/json^
Cookie: session=hjchrhhnci4ofjniej203dvervweocikrm09kvriorver;
```


<b>Replacing Body: </b>

* The request below setup an automatic XSS attack against a blog comment post:

```
POST /api/comments HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: close
Accept-Encoding: gzip, deflate
Host: example.com
Content-Length: 16
Content-Type: application/json
Cookie: session=hjchrhhnci4ofjniej203dvervweocikrm09kvriorver;

{"comment":"^^"}
```

* The following request bruteforce a hidden parameter through a patch request:

```
PATCH /api/delete HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: close
Accept-Encoding: gzip, deflate
Host: example.com
Content-Length: 31
Content-Type: application/json
Cookie: session=hjchrhhnci4ofjniej203dvervweocikrm09kvriorver;

{"^^":"Test", "^^":true}
```


<b>Replacing URL parameters: </b>

* The next request will test some command execution vulnerabilities in the file parameter:

```
GET /view?file=^^ HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: close
Accept-Encoding: gzip, deflate
Host: example.com
Cookie: session=hjchrhhnci4ofjniej203dvervweocikrm09kvriorver;
```


<b>Multi Strings Replacing: </b>

When the key string putting is done correctly, you can replace the strings the way you wan't

```
GET /api/^repl^/^replace^.^replace^?^^=true HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: ^replace^
Accept-Encoding: gzip, deflate
^haha^:^works^
Host: example.com
Cookie: session=^replaceToo^;
```


Bruteforcer accepts almost all Content-Types and Body Structures when replacing and sending requests.

### Help

```./bruteforcer -h```

### Using Threads
The most impacting difference between the Burp Intruder Pro, and the Burp Intruder Community is the threads that you can use. The Community version is only allowed to use 1 thread, and it makes the attack really slow ..., consuming a lot of my time waiting for Intruder to discover a new API or path from the target.

So I developed Bruteforcer to support <b>1000</b> threads!


<b>Usage:</b>

The following example will setup 200 threads:

```./bruteforcer [file/url] [wordlist] -t [threads]```

```./bruteforcer "https://www.example.com/^^" wordlist.txt -t 200```

The default value is 3.

This feature makes a huge difference...

### Printing Filters
Every code can be returned when bruteforcing APIS, Headers, Variables, or Body Contents, and during the Recon process, depending on the target, some of them can be unnecessary or annoying.

With Bruteforcer you can filter these response codes or response lengths.

<b>Usage: </b>

* Response Code Filter

    The following command will filter the 301 and 404 codes (it means that the passed codes (301 and 404) will be ignored).

    ```./bruteforcer [file/url] [wordlist] -f [codes]```

    ```./bruteforcer request.txt wordlist.txt -f "301,404"```

* Response Lenght Filter

    Maybe you don't want to filter response codes, but response lengths, here you can do this with the following commands that will filter all responses with 1122 body length:

    ```./bruteforcer [file/url] [wordlist] -lf [response lenght]```
    
    ```./bruteforcer request.txt wordlist.txt -lf 1122```
    
If you wish, you can use the both filters together:

```./bruteforcer request.txt wordlist.txt -f "400,429,500,404" -lf 2222```

The default value to response code filter is "404", and the length filter default value is "", it means that all the response lengths will be printed.

To show all the response codes you need to pass an empty string to the filter parameter (e.g: -f ""), the command below:

```./bruteforcer request.txt wordlist.txt -f ""```

### Recursive Mode



### Wordlist Extention

### Set Proxy

### Sleep After Request

### Burp Suite Proxy Integration

# Recommended Wordlists

* <b>Fuzzdb</b>: https://github.com/fuzzdb-project/fuzzdb
* <b>Dirb</b>: https://github.com/v0re/dirb
* <b>api_wordlist</b>: https://github.com/chrislockard/api_wordlist

# Limitations

### Implemented Methods

```
GET, POST, HEAD, PUT, DELETE, OPTIONS, PATCH
```

Using a different method than the listed above, the program will return an "<i>Not implemented method</i>" error.
All the methods will be implemented in a near future.

<b>All the limitations will be fixed soon.</b>
