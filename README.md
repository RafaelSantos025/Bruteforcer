# Bruteforcer
Advanced header wordlist bruteforce tool. 
It's a <b>Free</b> option to Burp Suite Professional Intruder.
<i> [I don't have money to buy Burp Suite Professional Intruder, so I wrote my own.]</i>)

# Setup

```git clone ...```

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
You can pass an file with the desired raw request or an URL

```./bruteforcer [File path/URL] wordlist```

<b>Key to replace: </b>

<i>Like Intruder, the advanted is to replace any string with the desired wordlist</i>

<b>Passing an URL: </b> The exmaple bellow will make a bruteforce attack in the path parameter (indicated by the <i>Replace Key (^^)</i>)

```./bruteforcer "https://www.example.com/^toReplace^" wordlist.txt```

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

### More Examples
Like Burp Intruder, you can change the request as you want. More examples that you can do with <b>Bruteforcer</b>: 

<b>Requests with body: </b>

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

<b></b>

### Implemented Methods

```
GET, POST, HEAD, PUT, DELETE, OPTIONS, PATCH
```

Using a different method than the listed above, the program will return an "<i>Not implemented method</i>" error.
All the methods will be implemented in a near future.

### Help

```./bruteforcer -h```

### Using Threads

### Recursive Mode

### Printing Filters

### Set Proxy

### Sleep After Request

### Burp Suite Proxy Integration

# Recommended Wordlists

# Limitations

All the limitations will be fixed soon.
