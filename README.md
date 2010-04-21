# Application handler

## Information

A JavaScript library that handles path maps that direct control to handlers,
along with the query string parameters.

Recommended use is for supporting paths in the hash part of the address bar.
You can do this using a hash history library (such as [Hash or the jQuery hash
plugin][js-hash]) and call the exec() method when the hash changes.

## Example

    var UserHandler = Application.handler(function (username, section) {
        alert('Hi ' + username + '!');
        if (section) alert('Showing ' + section + ', page ' +
                           this.get_param('page', 1));
    });

    var HomeHandler = Application.handler(function () {
        alert('Home!');
    });

    var app = new Application([
        ['^home$', HomeHandler],
        ['^user/([^/]+)(?:/([^/]+))?$', UserHandler]
    ]);

    app.exec('user/bob/articles?page=2');
    // Handler UserHandler will be found (because the pattern matches), and
    // then be used like this:
    // var handler = new UserHandler(app, 'user/bob/articles?page=2',
    //                               'user/bob/articles',
    //                               ['bob', 'articles'], {page: '2'});
    // handler.run('bob', 'articles');

The above example will open a dialog with the text "Hi bob!", followed by
another dialog with the text "Showing articles, page 2". Experiment with the
`app.exec(...)` call!

## Example 2 (using Hash and jquery.hash)

    // Create application for handling the site.
    var app = new Application([
        ['^$', HomeHandler],
        ['^project/([^/]+)$', ProjectHandler],
        ['^.*$', NotFoundHandler]
    ]);

    // Set up application to use the hash as its path.
    $(document).hashchange(function (e, hash) {
        app.exec(hash);
    });

    // Enable the hash library.
    $.hash.init();

Note that in the above code, the handlers haven't been included. The example
code is only to show how to set up the Application library to use [the Hash
library with the jQuery hash plugin][js-hash].

## MIT license

This project is licensed under an MIT license.

Copyright (c) 2009-2010 Andreas Blixt <andreas@blixt.org>

http://www.opensource.org/licenses/mit-license.php

[js-hash]: http://github.com/blixt/js-hash
