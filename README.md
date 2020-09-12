# Bruteforcer
Advanced header wordlist bruteforce tool. 
It's a <b>Free</b> option to Burp Suite Professional Intruder.
<i> [I don't have money to buy Burp Suite Professional Intruder, so I wrote my own.]</i>)

# Setup

```git clone ...```

# Used Python Libs

# How to use

#### Basic usage: 
You can pass an file with the desired raw request or an URL

```./bruteforcer [File/URL] wordlist```

<b>Key to replace: </b>

<b>Passing an URL: </b> 

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

And the command will be like this:

```./bruteforcer request.txt wordlist.txt```

The URL and the File exmaples above will make a bruteforce attack in the path parameter, indicated by the <i>Replace Key (^^)</i>.

#### Help

```./bruteforcer -h```

#### Threads

#### Recursive Mode

#### Filters

#### Proxy

#### Sleep

#### Burp Suite Integration
