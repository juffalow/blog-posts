---
title: 'Express server with JWT authentication'
media_order: express-js.png
date: '24-02-2017 23:20'
taxonomy:
    tag:
        - JavaScript
        - Node.js
        - Express
        - JWT
header_image_file: express-js.png
---

I needed to create a simple server, that would have one “public” route `/login` and other routes are available only for users with valid [JWT](https://jwt.io/) token.

That means, the user is allowed to log into the system and then get any data from it. If he is not logged in and he tries to get some data, he only get an error message and 401 ( Unauthorized ) status code will be returned.

===

## Example project step by step

I count that you are familiar with Node.js and JavaScript. You also know a bit about Express and JWT and why you may want to use it. This is mainly about how to put these things together.

#### Create new project

Let’s create a directory for our project :

```
$ mkdir express-jwt-example
```

We are going to work with Node, so we have to create package.json file, or better, initialize new project :

```
# jump into project directory
$ cd experss-jwt-example

# initialize project ( you may use npm init )
$ yarn init
```

This will ask you a bunch of questions and than it creates *package.json* file.

#### Add dependencies

We are going to work with exress and jwt, so you have to install two packages, `exress` and `jsonwebtoken` :

```
yarn add express jsonwebtoken

# or if you use npm
npm install express jsonwebtoken
```

#### Directory structure

```
express-jwt-example/
    controllers/
        helloWorld.js
        user.js
    middlewares/
        auth.js
    index.js
    config.js
    package.json
```

#### Index.js

Create a basic index file, which will be listening on port *8080* and every route will return some JSON. You can run the script with <kbd>node index.js</kbd> command and try to load `http://localhost:8080/` URL in your browser. It should print the JSON on the screen.

```
// index.js

var express = require('express');
var app = express();
/*
 * Create basic route that handles everything and returns some JSON, so you
 * are able to try if it's working
 */
app.get('/*', function(req, res) {
    res.json({message: 'Hello World!'});
});

app.listen(8080);
```

Later, we are going to modify this, but now, if it’s working, move on to the next step.

#### Config.js

When you have complex applications where you need to connect to various services and you have development and production environment you are going to need some configuration file. Usually, this file is not committed into repository, because there are sensitive informations like passwords, etc. We need to set two things – port and JWT secret :

```
// config.js

module.exports = {
    PORT: 8080,
    JWT_SECRET: 'secretpassword'
};
```

#### Login route *controllers/user.js*

Now we are going to create a login route, where user can post his username and password and in the response he gets either success message with JWT token, or error message. We are not using any database, so I keep this as simple as possible and the only valid username and password will be *admin* and *admin*.

```
// controllers/user.js

var jwt = require('jsonwebtoken');
var config = require('../config.js');

module.exports = function(router) {
    router.post('/login', function(req, res) {
        /*
         * Check if the username and password is correct
         */
        if( req.body.username === 'admin' && req.body.password === 'admin' ) {
            res.json({
                id: 1,
                username: 'admin',
                jwt: jwt.sign({
                    id: 1,
                }, config.JWT_SECRET, { expiresIn: 60*60 })
            });
        } else {
            /*
             * If the username or password was wrong, return 401 ( Unauthorized )
             * status code and JSON error message
             */
            res.status(401).json({
                error: {
                    message: 'Wrong username or password!'
                }
            });
        }
    });

    return router;
}
```

#### Example route *controllers/helloWorld.js*

This is going to be the protected route – avialable only for logged in users. See, that there is no authentication. It is route as any other. Imagine you have tens or hundreds of routes and you really don’t want to have authentication code in each one. You just create the routes and then you decide which ones will be protected and which won’t.

```
// controllers/helloWorld.js

module.exports = function(router) {
    router.get('/hello-world', function(req, res) {
        res.json({data: 'Hello World!'});
    });

    return router;
}
```

#### Middleware *middlewares/auth.js*

Here is the important part which is protecting the routes. You can define functions which are called before your request handler. This allows us to create a function to check if there is an authorization header with valid JWT token. If this function detects there is no authorization header or the token is invalid or its expiration time passed, it returns 401 status code with an error message.

Very nice definition of Middleware function :

> Middleware is any number of functions that are invoked by the Express.js routing layer before your final request handler is, and thus sits in the middle between a raw request and the final intended route.
> - [Roman Shtylman](https://twitter.com/defunctzombie)

```
// middlewares/auth.js

var jwt = require('jsonwebtoken');
var config = require('../config.js');

module.exports = function(req, res, next) {
    /*
     * Check if authorization header is set
     */
    if( req.hasOwnProperty('headers') && req.headers.hasOwnProperty('authorization') ) {
        try {
            /*
             * Try to decode & verify the JWT token
             * The token contains user's id ( it can contain more informations )
             * and this is saved in req.user object
             */
            req.user = jwt.verify(req.headers['authorization'], config.JWT_SECRET);
        } catch(err) {
            /*
             * If the authorization header is corrupted, it throws exception
             * So return 401 status code with JSON error message
             */
            return res.status(401).json({
                error: {
                    msg: 'Failed to authenticate token!'
                }
            });
        }
    } else {
        /*
         * If there is no autorization header, return 401 status code with JSON
         * error message
         */
        return res.status(401).json({
            error: {
                msg: 'No token!'
            }
        });
    }
    next();
    return;
}
```

#### Index.js completition

We are almost done. Every part of the system is ready and we only have to merge them. Be careful about the middleware! The order metters and if you add the middleware after the routes, it won’t work.

```
// index.js

var express = require('express');
var app = express();
var router = express.Router();
var bodyParser = require('body-parser');
var config = require('./config.js');

app.use(bodyParser.json());
/*
 * Add middleware. Because we defined the first parameter ( '/api' ), it will run
 * only for urls that starts with '/api/*'
 */
app.use('/api', require('./middlewares/auth.js'));
/*
 * Add the protected route '/hello-world' after '/api'
 * So now it is available on /api/hello-world
 */
app.use('/api', require('./controllers/helloWorld.js')(router));
/*
 * Add the '/login' route handler
 */
app.use('/', require('./controllers/user.js')(router));
/*
 * Start server
 */
app.listen(config.PORT);
```

## How to test it?

I know that according to [TDD](https://en.wikipedia.org/wiki/Test-driven_development) we should write the tests first. But I wanted to show how to create this kind of application.

We need to test “everything” if it is working correctly. For this, we need to install few other packages :

```
$ yarn add mocha chai sinon supertest
```

Very shortly to these packages : mocha is test framework – run tests, create reports, etc. Chai is assertion library – checks the type of variable, if an object has some property, etc. Sinon has spies, stubs and mocks and SuperTest is for testing HTTP requests. You can read more about each module on pages mentioned below in references.

#### Test ‘/login’ route

There are few things need to be tested : it it takes the right username and password and refuse bad one and if it returns JWT token.

```
// test/controllers/user.js

var chai = require('chai');
var expect = chai.expect;
var request = require('supertest');
var app = require('../../server.js');

describe('POST /login', function(){
    it('it responds with 401 status code if bad username or password', function(done) {
        request(app)
            .post('/login')
            .type('json')
            .send('{"username":"bad","password":"wrong"}')
            .expect(401)
            .end(function(err, res) {
                if (err) return done(err);
                done();
        });
    });

    it('it responds with 200 status code if good username or password', function(done) {
        request(app)
            .post('/login')
            .type('json')
            .send('{"username":"admin","password":"admin"}')
            .expect(200)
            .end(function(err, res) {
                if (err) return done(err);
                done();
        });
    });

    it('it returns JWT token if good username or password', function(done) {
        request(app)
            .post('/login')
            .type('json')
            .send('{"username":"admin","password":"admin"}')
            .end(function(err, res) {
                if (err) return done(err);

                expect(res.body).have.property('jwt');

                done();
        });
    });
});
```

#### Test middleware

Test if the middleware is doing its job : when no token or invalid token is provided, it returns 401 status code end stop executing additional middlewares. If there is a good token, it should save its content into `request.user` object and call next middleware.

```
// test/middleware/auth.js

var chai = require('chai');
var expect = chai.expect;
var sinon = require('sinon');
var auth = require('../../middlewares/auth.js');
var jwt = require('jsonwebtoken');
var config = require('../../config.js');

describe('Test Auth Middleware', function(){
    var request;
    var response;
    var next;

    beforeEach(function() {
        request = {};
        response = {
            status: sinon.stub().returnsThis(),
            json: sinon.spy()
        };
        next = sinon.spy();
    });

    it('next should not be called if no token provided', function() {
        auth(request, response, next);
        expect(next.called).to.equal(false);
    });

    it('should return 401 status code if no token provided', function() {
        auth(request, response, next);
        expect(response.status.getCall(0).args[0]).to.equal(401);
    });

    it('next should not be called if bad token was provided', function() {
        request.headers = {};
        request.headers.authorization = 'some authorization header';
        auth(request, response, next);
        expect(next.called).to.equal(false);
    });

    it('shoudl return 401 status code if bad token was provided', function() {
        request.headers = {};
        request.headers.authorization = 'some authorization header';
        auth(request, response, next);
        expect(response.status.getCall(0).args[0]).to.equal(401);
    });

    it('request should contain user info if good token was provided', function() {
        request.headers = {};
        request.headers.authorization = jwt.sign({ id: 1 }, config.JWT_SECRET);
        auth(request, response, next);
        expect(request).to.have.property('user');
        expect(request.user).to.have.property('id');
        expect(request.user.id).to.be.equal(1);
    });

    it('next should be called once if good token was provided', function() {
        request.headers = {};
        request.headers.authorization = jwt.sign({ id: 1 }, config.JWT_SECRET);
        auth(request, response, next);
        expect(next.calledOnce).to.equal(true);
    });
});
```

#### Test the protected route

The protected route has to be avialable only if valid token is provided and it should respond with JSON.

```
var request = require('supertest');
var app = require('../../server.js');
var jwt = require('jsonwebtoken');
var config = require('../../config.js');

describe('GET /hello-world', function(){
    it('it responds with 401 status code if no authorization header', function(done) {
        request(app).get('/api/hello-world').expect(401).end(function(err, res) {
            if (err) return done(err);
            done();
        });
    });

    it('it responds with JSON if no authorization header', function(done) {
        request(app).get('/api/hello-world').expect('Content-Type', /json/).end(function(err, res) {
            if (err) return done(err);
            done();
        });
    });

    it('it responds with 200 status code if good authorization header', function(done) {
        var token = jwt.sign({
            id: 1,
        }, config.JWT_SECRET, { expiresIn: 60*60 });
        request(app)
            .get('/api/hello-world')
            .set('Authorization', token)
            .expect(200)
            .end(function(err, res) {
                if (err) return done(err);
                done();
        });
    });

    it('it responds with JSON if good authorization header', function(done) {
        var token = jwt.sign({
            id: 1,
        }, config.JWT_SECRET, { expiresIn: 60*60 });
        request(app)
            .get('/api/hello-world')
            .set('Authorization', token)
            .expect('Content-Type', /json/)
            .end(function(err, res) {
                if (err) return done(err);
                done();
        });
    });
});
```

#### Run the tests

The last step is to modify *package.json* file. Add there new key `scripts` where we define what it can do. The *package.json* should now look like this :

```
{
    "name": "express-jwt-example",
    "version": "1.0.0",
    "description": "Example project for Express.js with JWT blogpost",
    "main": "index.js",
    "license": "MIT",
    "dependencies": {
        "body-parser": "^1.16.1",
        "chai": "^3.5.0",
        "express": "^4.14.1",
        "jsonwebtoken": "^7.3.0",
        "mocha": "^3.2.0",
        "sinon": "^1.17.7",
        "supertest": "^3.0.0"
    },
    "scripts": {
        "start": "node index.js",
        "test": "mocha --recursive"
    }
}
```

So to run the tests just type <kbd>npm test</kbd> and if you want to run the app, type <kbd>npm start</kbd>.

## Conclusion

You can find the whole project on [GitHub](https://github.com/juffalow/express-jwt-example). I hope I mentioned everything important. If you find anything confusing, leave me a comment or write me an email.

#### References

[Express.js Middleware Demystified](https://www.safaribooksonline.com/blog/2014/03/10/express-js-middleware-demystified/)

[Mocha JavaScript test framework](https://mochajs.org/)

[Chai Assertion Library](http://chaijs.com/)

[Sinon.JS](http://sinonjs.org/)

[SuperTest](https://github.com/visionmedia/supertest)