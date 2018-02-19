---
title: 'Chaos Ajax - is your application ready to fail?'
media_order: flat-tyre.png
published: true
publish_date: '11-02-2018 12:43'
taxonomy:
    tag:
        - JavaScript
header_image_file: flat-tyre.png
---

You are creating an application that shows data to the user - doesn't matter
if he is searching for some specific thing or he is only listing everything you have.
What happens if the server responds with empty array? Or with error? Will your
application show only empty space or an error message?

===

If you want to test this kind of behavior, you can modify the back-end or you
know what keyword to search to get an empty response. But there is a better approach.

## Chaos Ajax

The [Chaos Ajax](https://github.com/juffalow/chaos-ajax) is a small script that mocks XHR so it knows when you are doing ajax request
and it modifies the response from time to time, so you have to thing about it
in advance. This means you should always implement catch / fail method.

#### Empty response

For example you want to have page where user can list and search for people from
Star Wars. There is an [API](https://swapi.co/) you can use for this.

And now let's change the API response :

```javascript
chaosAjax({
  urls: {
    'https://swapi.co/api/people': {
      responseText: '{"count":0,"results":[]}',
    }
  }
});
```

Because the `chaosAjax` is checking if the requested URL starts with our URL,
this applies on search too. So if user is searching for *Anakin*, your code
could look like:

```
fetch('https://swapi.co/api/people?search=anakin')
  .then((response) => {return response.text(); })
  .then((people) => {
    // render table, or whatever
  });
```

But if the user type *Annakin*, it returns empty array. Of course you know how to spell the name, so you never typed it wrong and you didn't implement fallback case. That means the user is going to see an empty page instead of warning that the name doesn't exist or he should check the spelling. But now, with `chaosAjax` running, you will see the problem almost immediately.

#### Auth

What if the user went away from the computer and then came back and clicked on
something. Meanwhile the server logged him out, so he cannot access the data again.
Will he see a login popup with warning that his token is no more valid or nothing happens
and after a while he just refresh the page and see that he was logged out?

You can set the `chaosAjax` to return `401 Unauthorized` from time to time and
see if your application throws a popup or not.

```javascript
chaosAjax({
  defaultResponse: {
    responseText: '{error:{code:1,message:"Auth token is not valid!"}}',
    status: 401,
  }
});
```

## Conclusion

I think there are lot of other examples where you can use this. It sounds funny
to add a script to your own page that should break its functionality, but when
you get used to it, your applications become more robust and you will be able
to sleep well.

Of course do not include this script in production!