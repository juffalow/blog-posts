---
title: 'Backbone.js and cross domain API'
media_order: backbone.png
date: '04-04-2016 14:51'
header_image_file: backbone.png
---

I am playing a little bit with PHP micro framework Slim and javascript Backbone.js. Therefor I created two separate projects with URLs : `mywebapp.dev` for front-end ( Backbone.js ) and `api.mywebapp.dev` for back-end ( Slim ). The problem occurred nearly immediately : Cross-Origin Resource Sharing ( CORS ).

===

## Backbone.js and CORS

When you create your first model, define the route and try to load it via console, the browser will object and throw exception :
`No 'Access-Control-Allow-Origin' header is present on the requested resource` in Chrome.

When you start to google it out every time you end up reading about CORS problem. So what it is, why is it a problem and how to solve it?

#### What is Cross-Site Sharing and why does it exist

Good example can be found on Wikipedia page **Same-origin policy** :

> The main reason to have this restriction is because without the same-origin policy there would be a security risk. Assume that a user is visiting a banking website and doesn’t log out. Then he goes to any random other site and that site has some malicious JavaScript code running in the background that requests data from the banking site. Because the user is still logged in on the banking site, that malicious code could do anything on the banking site. For example get a list of your last transactions, create a new transaction, etc.
> - [Wikipedia](https://en.wikipedia.org/wiki/Same-origin_policy)

#### Solution

To solve `Allow-Origin` problem you need to add new header to the response. This can be done in many ways :

```
// pure php
header("Access-Control-Allow-Origin: *");

// slim 3
$response->withHeader('Access-Control-Allow-Origin', '*');

// .htaccess
Header add Access-Control-Allow-Origin "*"
```

<div class="alert alert-info" role="alert">If you use <i>.htaccess</i> solution and get <i>Internal Server Error</i> the problem is in Apache. The <code>Header</code> directive is a module and you need to enable it. Open command line and type <code>sudo a2enmod headers</code> and then restart apache <code>sudo service apache2 restart</code>.</div>

Now if you try to load any model it should work. The problem is, when you try to modify it, or create a new one. Or, to be more precise, there are two problems. First, Bakcbone send data to server in `JSON` format and that violates **Content Security Policy**.
In Chrome console you see this error : `Request header field Content-Type is not allowed by Access-Control-Allow-Headers in preflight response`.

```
// pure php
header("Access-Control-Allow-Headers: Content-Type");

// slim 3
$response->withHeader('Access-Control-Allow-Headers', 'Content-Type');

// .htaccess
Header add Access-Control-Allow-Headers "Content-Type"
```

And the second problem is that actions like this invoke HTTP PUT methods call and the browser will stand against them again. Problem is, this method is not considered as safe. Therefore, another header is required :

```
// pure php
header("Access-Control-Allow-Methods: GET,POST,OPTIONS,DELETE,PUT");

// slim 3
$response->withHeader('Access-Control-Allow-Methods', 'GET,POST,OPTIONS,DELETE,PUT');

// .htaccess
Header add Access-Control-Allow-Methods "GET,POST,OPTIONS,DELETE,PUT"
```

Otherwise you get back only error message `Method PUT is not allowed by Access-Control-Allow-Methods in preflight response`.

<div class="alert alert-info" role="alert"><i>With this header you allowed all mentioned HTTP methods!</i></div>

#### .htaccess

his is final *.htaccess* file for Slim 3 framework and CORS allowed :

```
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule . index.php [L]
Header add Access-Control-Allow-Origin "*"
Header add Access-Control-Allow-Methods "GET,POST,OPTIONS,DELETE,PUT"
Header add Access-Control-Allow-Headers "Content-Type"
```

#### Still a problem ( *405 method not allowed* )

If you are still getting some error, for example 405 method not allowed, this can be caused by calling the *OPTIONS* method first. The browser cannot know if it can call *POST* ( or *GET* / other methods ), so it does a preflight request that checks if it is allowed to make that type of request. But you do not have this method implemented, so the framework returns an error and therefore the browser will not make the call – even if the headers are set correctly!

```
var express = require('express');
var app = express();
app.use(function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Methods", "GET,HEAD,OPTIONS,POST,PUT");
    res.header("Access-Control-Allow-Headers", "Content-Type");
    if ( req.method == 'OPTIONS' ) {
        res.sendStatus(200);
    } else {
        next();
    }
});
```

## Conclusion

Now you can load and save your models as you wish. If you want to know more, read the links below. And of course when I am done with my little app, I will share the experiences and write another post about it.

#### References

[CORS](https://www.w3.org/TR/cors/)

[Cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) page on Wikipedia. You find there useful information with an example and I recommend you to read some of the linked pages also.

[Same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy) page on Wikipedia, where you can navigate from CORS page. It is good to at least read the post to get some overview.

[Content Security Policy](https://en.wikipedia.org/wiki/Content_Security_Policy) page on Wikipedia.
