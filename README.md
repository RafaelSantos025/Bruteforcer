# Bruteforcer
Advanced header wordlist bruteforce tool. 

It's a <b>Free</b> option to Burp Suite Professional Intruder. 

<i>I don't have money to buy Burp Suite Professional Intruder, so I wrote my own.</i>

# Setup

```bash
git clone https://github.com/RafaelSantos025/Bruteforcer.git
```

```bash
cd Bruteforcer/
```

```bash
chmod +x bruteforcer
```

### Used Python Libs

* requests
* argparse
* threading
* os
* sys
* urllib3
* time
* queue

# How to use

### Basic usage: 
You can pass a file with the desired raw request or an URL, these commands are mandatory.

```bash
./bruteforcer [file-path/url] [wordlist]
```


<b>Key to replace: </b>

<i>Like Intruder, the advantage is to replace any string with the desired wordlist</i>

Replacement Key is the name of the character used to specify the string that you want to replace in the HTTP request or URL.

Burp Intruder choose the following character to do this job: ```§```, but it's hard to find on our Keyboard, difficulting the usage of my program. (On Burp Suite Intruder it works fine because the Replacement Key is added automatically).

So, I choose a character that is visibly present in almost Keyboards, and, at the same time, not commonly used in HTTP requests and URLs.

The chosen character is ```^```. In other words: Every string or character between two ```^``` will be replaced to the passed wordlist.


<b>Examples of How it works</b>

* Replacement key: ```^```
* String to replace: ```all animals```
* String to be replaced: ```I love dogs and ^cats^!```

* Result: ```I love dogs and all animals!```


<b>Examples in a URL</b>

* Replacement key: ```^```
* String to replace: ```admin```
* String to be replaced: ```https://www.example.com/^toReplace^```

* Result: ```https://www.example.com/admin```


<b>I Want to change the Replacement Key</b>

If the character used to be the default Replacement Key is being used to the target Server to understand the request (E.g: The Server needs the key ^ to complete the request), you can change the Replacement Key with the parameter -k, example bellow:

```bash
./bruteforcer [file/url] [wordlist] -k [replacement key]
```

* Changing to the Burp settings:

```bash
./bruteforcer "https://www.example.com/§" wordlist.txt -k "§"
```


<b>Passing an URL to attack: </b> 

* The example below will make a bruteforce attack in the path parameter (indicated by the <i>Replace Key (^^)</i>)

    ```bash
    ./bruteforcer "https://www.example.com/^toReplace^" wordlist.txt
    ```

* The following command will bruteforce some hidden URL parameters in the target:

    ```bash
    ./bruteforcer "https://www.example.com/create?^^=true" wordlist.txt
    ```
Note that when passing URLs as a parameter, the only request method that the program can perform is the GET method.


<b>Passing an request file: </b>

The request file, called "request.txt" will look like this:

```http
GET /^toReplace^ HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept-Encoding: gzip, deflate
Connection: close
```

And finally, the command will be like this:

```bash
./bruteforcer request.txt wordlist.txt
```

As you can see, this file will setup a bruteforce attack to the path parameter too (like the URL example), but now, you have total control of the request header.

### More Requests Examples
Like Burp Intruder, you can change the request as you want. More examples that you can do with <b>Bruteforcer</b> below: 

<b>Requests With Body: </b>

```http
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

```http
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

```http
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

```http
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

```http
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

```http
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

```http
GET /view?file=^^ HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: close
Accept-Encoding: gzip, deflate
Host: example.com
Cookie: session=hjchrhhnci4ofjniej203dvervweocikrm09kvriorver;
```


<b>Multi Strings Replacing: </b>

When the key string putting is done correctly, you can replace the strings the way you want

```http
GET /api/^repl^/^replace^.^replace^?^^=true HTTP/1.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: */*
Connection: ^replace^
Accept-Encoding: gzip, deflate
^haha^:^works^
Host: example.com
Cookie: session=^replaceToo^;
```


Bruteforcer accepts almost all Content-Type's and Body Structures when replacing and sending requests.

### Help

```bash
./bruteforcer -h
```

```bash
usage: bruteforcer [-h] [-t THREADSNUMBER] [-f FILTER] [-k KEY]
                   [-lf LENGHTFILTER] [-r] [-rc RECURSIVECODES]
                   [-rs RECURSIVESEPARATOR] [-e WORDLISTEXTENSION]
                   [-proxy PROXY] [-sleep SLEEPAFTER]
                   target wordlist

Bruteforcer: Advanced Header Wordlist Bruteforce Tool.

positional arguments:
  target                Will be your target, it can be a file containing the
                        header, or an URL.
  wordlist              Wordlist path.

optional arguments:
  -h, --help            show this help message and exit
  -t THREADSNUMBER      Number of threads to use.
  -f FILTER             Ignore listed code(s).
  -k KEY                Key to replace: ^string_to_replace^.
  -lf LENGHTFILTER      Ignore passed length responses.
  -r                    Use recursive mode.
  -rc RECURSIVECODES    Only release recursive wordlist to this codes.
  -rs RECURSIVESEPARATOR
                        use an arbitrary character separator when using the
                        recursive mode.
  -e WORDLISTEXTENSION  Add an extension to the passed wordlist. e.g: -e
                        ".txt".
  -proxy PROXY          Proxy the requests. Burp Proxy example: -proxy
                        "localhost:8080".
  -sleep SLEEPAFTER     Sleep before each request.
```

### Using Threads
The most impacting difference between the Burp Intruder Pro, and the Burp Intruder Community is the threads that you can use. The Community version is only allowed to use 1 thread, and it makes the attack really slow ..., consuming a lot of my time waiting for Intruder to discover a new API or path from the target.

So I developed Bruteforcer to support <b>1000</b> threads!


<b>Usage:</b>

The following example will setup 200 threads:

```bash
./bruteforcer [file/url] [wordlist] -t [threads]
```

```bash
./bruteforcer "https://www.example.com/^^" wordlist.txt -t 200
```

The default value is 3.

This feature makes a huge difference...

### Printing Filters
Every code can be returned when bruteforcing APIS, Headers, Variables, or Body Contents, and during the Recon process, depending on the target, some of them can be unnecessary or annoying.

With Bruteforcer you can filter these response codes or response lengths.

<b>Usage: </b>

* Response Code Filter

    The following command will filter the 301 and 404 codes (it means that the passed codes (301 and 404) will be ignored).

    ```bash
    ./bruteforcer [file/url] [wordlist] -f [codes]
    ```

    ```bash
    ./bruteforcer request.txt wordlist.txt -f "301,404"
    ```

* Response Lenght Filter

    Maybe you don't want to filter response codes, but response lengths, here you can do this with the following commands that will filter all responses with 1122 body length:

    ```bash
    ./bruteforcer [file/url] [wordlist] -lf [response lenght]
    ```
    
    ```bash
    ./bruteforcer request.txt wordlist.txt -lf 1122
    ```
    
If you wish, you can use both filters together:

```bash
./bruteforcer request.txt wordlist.txt -f "400,429,500,404" -lf 2222
```

The default value to response code filter is "404", and the length filter default value is "", it means that all the response lengths will be printed.

To show all the response codes you need to pass an empty string to the filter parameter (e.g: -f ""), the command below:

```bash
./bruteforcer request.txt wordlist.txt -f ""
```

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

### Replacing Methods
For this release, it's not possible to bruteforce HTTP Methods.

Passing a request file like the following will return a ```Method: ^GET^ is not implemented``` error

```http
^GET^ / HTTP/1.1
Connection: close
Accept: */*
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:71.0) Gecko/20100101 Firefox/71.0
Host: example.com
Content-Type: application/json
```

<b>All the limitations will be fixed soon.</b>
