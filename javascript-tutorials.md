# Learn JavaScript Debounce Function By Building the Wikipedia Search App

> **Summary:** in this tutorial, you’ll learn about JavaScript debounce 
function and how to use it to improve application performance.

To understand the debounce function, you’re going to build a Wikipedia
Search application using the debouncing programming technique.

## Create the project folder structure

First, create a new folder called `wikipedia-search` that will store
the files of the projects.

Second, create three folders inside the `wikipedia-search` folder called
`js`, `css`, and `img`. These folders will store JavaScript, CSS, and
images files accordingly.

Third, create the `style.css` in the `css` folder and the `app.js` in
the `js` Also, download the following image and copy it to the `img`
folder. You’ll use the logo to make the UI for the app.

![](https://www.javascripttutorial.net/wp-content/uploads/2020/09/wikipedia-logo.png)

Finally, create an `index.html` file in the root folder.

The project tructure will look like the following:

![](https://www.javascripttutorial.net/wp-content/uploads/2020/09/JavaScript-Debounce-Function-Project-Structure.png)

## Build the HTML page

Open the index.html file and add the following code:

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Wikipedia Search</title>
        <link rel="stylesheet" href="css/style.css">
    </head>
    <body>
        <header>
            <img src="./img/wikipedia-logo.png" alt="wikipedia">
            <h1>Wikipedia Search</h1>
            <input type="text" name="searchTerm" id="searchTerm" placeholder="Enter a search term...">
        </header>
        <main id="searchResult"></main>
        <script src="js/app.js"></script>
    </body>
    </html>

In this HTML file:

* First, link to the `style.css` file in the `<head>` section.
* Second, add a `<script>` tag whose `src` links to the `app.js` file
and place it right before the `</body>` tag.
* Third, add two sections to the body of the HTML page.
The first section is the header that shows the Wikipedia logo, the
heading, and the search box. The second section includes the `<main>`
tag that will display the search result.

## Copy the CSS code

Navigate to the [style.css](https://www.javascripttutorial.net/sample/dom/wikipedia-search/css/style.css) file, copy its code, and paste it into the
style.css file in the css folder. When you open the index.html file,
you should see something like the [following page](https://www.javascripttutorial.net/sample/dom/wikipedia-search/).

## Handle input events

First, select the `<input>` and search result elements using the
`querySelector()` method:

    const searchTermElem = document.querySelector('#searchTerm');
    const searchResultElem = document.querySelector('#searchResult');

Second, set the focus on the `<input>` element by calling the
`focus()` method:

    searchTermElem.focus();

Third, attach an `input` event listener for the `<input>` element:

    searchTermElem.addEventListener('input', function (event) {
        console.log(event.target.value);
    });

If you type some text on the `<input>` element, you’ll see that the
`input` event occurs, which shows the text to the Console.

For example, when you type the `debounce` in the `<input>` element:

![](https://www.javascripttutorial.net/wp-content/uploads/2020/09/JavaScript-Debounce-Function-Input-event.png)

... you’ll see the following texts in the Console:

![](https://www.javascripttutorial.net/wp-content/uploads/2020/09/JavaScript-Debounce-Function-Input-event-console.png)

## Fetch search results using Wikipedia API

The Wikipedia API is quite simple. It doesn’t require an API key.

To get the topics by a search term, you need to append the `srsearch`
query parameter:

    &srsearch=<searchTerm>

to the following URL:

    https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info|extracts&inprop=url&utf8=&format=json&origin=*&srlimit=10

... and send an HTTP `GET` request.

For example, you can get the topics related to the `debounce` keyword by
sending an HTTP `GET` request to the following URL:

    https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info|extracts&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=debounce

By the way, you can open the above URL in the web browser to see the response.

From JavaScript, you can use the fetch API, which is available in all
the modern web browsers, to send an HTTP `GET` request.

The following creates the `search()` function accepts a search term,
makes an HTTP `GET` request to Wikipedia, and shows the search results
to the Console:

    const search = async (searchTerm) => {
        try {
            const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info|extracts&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${searchTerm}`;
            const response = await fetch(url);
            const searchResults = await response.json();

            // show the search result in the console
            console.log({
                'term': searchTerm,
                'results': searchResults.query.search
            });

        } catch (error) {
            console.log(error);
        }
    }

How it works.

First, construct the API URL by adding the `srsearch` query parameter
to the endpoint:

    const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info|extracts&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${searchTerm}`;

Second, use the `fetch()` method to send a HTTP GET request. Since
the `fetch()` method returns a promise, you need to use `await` keyword
to wait for the response.

The promise returned by the `fetch()` function has many methods,
one of them is `json()`. The `json()` method also returns another promise
that resolves to a result in JSON format.

Because of the await keyword, you need to mark the `search()` function
as an async function like this:

    const search = async (searchTerm) = {
       /// ...
    };

The returned object of the `json()` method has many properties.
And to get the search results, you need access
the `searchResults.query.search` property.

To test the `search()` method, you call it in the `input` event
listener as follows:

    searchTermElem.addEventListener('input', function (event) {
        search(event.target.value);
    });

The following shows the complete `app.js` file:

    const searchTermElem = document.querySelector('#searchTerm');
    const searchResultElem = document.querySelector('#searchResult');

    searchTermElem.select();

    searchTermElem.addEventListener('input', function (event) {
        search(event.target.value);
    });

    const search = async (searchTerm) => {
        try {
            const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info|extracts&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${searchTerm}`;
            const response = await fetch(url);
            const searchResults = await response.json();

            // show the search result in the console
            console.log({
                'term': searchTerm,
                'results': searchResults.query.search
            });

        } catch (error) {
            console.log(error);
        }
    }

Now, if you open the `index.html` file and type the `debounce` keyword in
the input element, you’ll see the following results in the Console:

![](https://www.javascripttutorial.net/wp-content/uploads/2020/09/JavaScript-Debounce-Function-too-many-requests.png)

The output indicates that the `search()` function executes for every
character you type. It calls the API for every text input,
which isn’t efficient.

To limit the number of requests, you’ll send API requests only when
necessary. In other words, you’ll send an API request only after users
pause or stop typing for a period of time e.g., a half-second.

To do so, you can use the `setTimeout()` and `clearTimeout()` function:

* When users type a character, use the `setTimeout()` function to schedule
to execute the `search()` function after a period of time.
* If users keep typing, cancel that timer using the `clearTimeout()` function.
In case the users pause or stop typing, let the timer to execute the
scheduled function to search.

The following shows the new version of the `search()` function:

    let timeoutId;

    const search = (searchTerm) => {
        // reset the previous timer
        if (timeoutId) {
            clearTimeout(timeoutId);
        }

        // set up a new timer
        timeoutId = setTimeout(async () => {
            try {
                const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info|extracts&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${searchTerm}`;
                const response = await fetch(url);
                const searchResults = await response.json();

                // show the search result in the console
                console.log({
                    'term': searchTerm,
                    'results': searchResults.query.search
                });
            } catch (error) {
                console.log(error);
            }
        }, 500);
    };

Since the `await` related code is moved to the callback function of
the `setTimeout()`, you need to mark the callback with the `async` keyword
and remove the `async` keyword from the `search()` function.

If you open the `index.html` file in the web browser and type the keyword
debounce without pausing (for a half-second) and stop, you’ll see
that the application will make only one API request.

And this technique is known as **debouncing**.

## What is debouncing

If you have a time-consuming task like an API request that fires often,
it’ll impact application performance.

**Debouncing** is a programming technique that limits the number of
times a function gets called.

## Develop a reusable debounce function

The `debounce()` function needs to accept a function (`fn`), limits the
number of calls to it, and returns a function:

    const debounce = (fn) => {
       return (arg) => {
          // logic to limit the number of call fn
          fn(arg);
       };
    };

The following uses the `clearTimeout()` and `setTimeout()` functions
to debounce the fn function:

    const debounce = (fn) => {
        let timeoutId;

        return (arg) => {
            // cancel the previous timer
            if (timeoutId) {
                clearTimeout(timeoutId);
            }
            // setup a new timer
            timeoutId = setTimeout(() => {
                fn(arg);
            }, 500);
        };
    };

Typically, the `fn` function will accept more than one argument.
To invoke the `fn` function with a list of arguments,
you use the `apply()` method:

    const debounce = (fn, delay=500) => {
        let timeoutId;

        return (...args) => {
            // cancel the previous timer
            if (timeoutId) {
                clearTimeout(timeoutId);
            }
            // setup a new timer
            timeoutId = setTimeout(() => {
                fn.apply(null, args);
            }, delay);
        };
    };

How it works:

* First, replace the hardcoded number `500` with the `delay` argument so
that you can specify the amount of time to wait before executing the `fn`
function. The default value of the delay is 500 ms.
* Second, add the `...args` to the returned function. The `...arg`
is a rest parameter that allows you to collect all the arguments
of the `fn()` function into an array `args`.
* Third, the `fn.apply(null, args)` executes the `fn()` function with
the arguments specified in the `args` array.

## Use the debounce function

The following removes the debouncing logic from the `search()` function
and use the `debounce()` function instead:

    const search = debounce(async (searchTerm) => {
        try {
            const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info|extracts&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${searchTerm}`;
            const response = await fetch(url);
            const searchResults = await response.json();

            // show the search result in the console
            console.log({
                'term': searchTerm,
                'results': searchResults.query.search
            });
        } catch (error) {
            console.log(error);
        }
    });

## Convert the search results to HTML

We’ll show the title and snippet of every search result in the output.
Before doing it, we’ll need some utility functions:

## Strip HTML tags

The `title` and `snippet` from the search result of the API call may
contain HTML tags. And it’s safe to strip all HTML tags
before rendering them.

The following utility function strips the HTML tags from a string:

    const stripHtml = (html) => {
        let div = document.createElement('div');
        div.textContent = html;
        return div.textContent;
    };

The `stripHtml()` function accepts an HTML string. It creates a
temporary `<div>` element, assign its `innerHTML` the HTML string,
and return its `textContent` property.

> Note that this function will only work on web browsers because it
relies on the web browser’s DOM API.

## Highlight the search term

It’s more intuitive if the search terms are highlighted in the search result.

This `highlight()` function highlights all the occurrences of the
`keyword` in the `str` by wrapping each occurrence of the keyword in
a `<span>` tag with the `highlight` class:

    const highlight = (str, keyword, className = "highlight") => {
        const hl = `<span class="${className}">${keyword}</span>`;
        return str.replace(new RegExp(keyword, 'gi'), hl);
    };

Note that the function uses the regular expression to replace all
occurrences of the keyword by the `<span>` element.

## Convert the search results to HTML

The following `generateSearchResultHTML()` function converts the search
results to HTML:

    const generateHTML= (results, searchTerm) => {
        return results
            .map(result => {
                const title = highlight(stripHtml(result.title), searchTerm);
                const snippet = highlight(stripHtml(result.snippet), searchTerm);

                return `<article>
                    <a href="https://en.wikipedia.org/?curid=${result.pageid}">
                        <h2>${title}</h2>
                    </a>
                    <div class="summary">${snippet}...</div>
                </article>`;
            })
            .join('');
    }

How it works.

* First, use the `map()` method to return the HTML representation of each
search result and the `join()` method to join search results
(in HTML format) into a single HTML string.
* Second, strip the HTML tags and highlight the search term in
the `title` and `snippet` returned from the API call.

## Show the search results

Change the `search()` method that uses the `generateSearchResultHTML()`
function and append its result to the `searchResultElem`.
Also, reset the search result if the search term is empty:

    const search = debounce(async (searchTerm) => {

        // if the search term is removed, 
        // reset the search result
        if (!searchTerm) {
            // reset the search result
            searchResultElem.innerHTML = '';
            return;
        }

        try {
            // make an API request
            const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info|extracts&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${searchTerm}`;
            const response = await fetch(url);
            const searchResults = await response.json();

            // render search result
            const searchResultHtml = generateSearchResultHTML(searchResults.query.search, searchTerm);

            // add the search result to the searchResultElem
            searchResultElem.innerHTML = searchResultHtml;
        } catch (error) {
            console.log(error);
        }
    });

Now, if you open the index.html in the web browser, you’ll see the
working application.

## Summary

In this tutorial, you’ve learned the following key points:

* Use the `fetch()` API to make HTTP `GET` requests.
* Use the `async/await` keywords to make the asynchronous code
looks cleaner.
* Understand the debouncing programming technique and develop a
reusable JavaScript `debounce()` function.

# JavaScript insertAfter

> **Summary:** in this tutorial, you will learn how to insert a
new node after an existing node as a child node of a parent node.

JavaScript DOM provides the `insertBefore()` method that allows
you to insert a new after an existing node as a child node.
However, it hasn’t supported the `insertAfter()` method yet.

To insert a new node after an existing node as a child node,
you can use the following approach:

* First, select the next sibling node of the existing node.
* Then, select the parent node of the existing node and call
the `insertBefore()` method on the parent node to insert a new node
before that immediate sibling node.

The following `insertAfter()` function illustrates the logic:

    function insertAfter(newNode, existingNode) {
        existingNode.parentNode.insertBefore(newNode, existingNode.nextSibling);
    }

Suppose that you have the following list of items:

    <ul id="menu">
        <li>Home</li>   
        <li>About</li>
        <li>Contact</li>
    </ul>

The following snippet inserts a new node after the first list item:

    let menu = document.getElementById('menu');
    // create a new li node
    let li = document.createElement('li');
    li.textContent = 'Services';

    // insert a new node after the first list item
    menu.insertBefore(li, menu.firstElementChild.nextSibling);

How it works:

* First, select the `ul` element by its id (`menu`) using
the `getElementById()` method.
* Second, create a new list item using the `createElement()` method.
* Third, use the `insertBefore()` method to insert the list item element
before the next sibling of the first list item element.

Put it all together.

    <!DOCTYPE html>
    <html>

    <head>
        <meta charset="utf-8">
        <title>JavaScript insertAfter() Demo</title>
    </head>

    <body>
        <ul id="menu">
            <li>Home</li>
            <li>About</li>
            <li>Contact</li>
        </ul>
        <script>
            let menu = document.getElementById('menu');
            // create a new li node
            let li = document.createElement('li');
            li.textContent = 'Services';

            // insert a new node after the first list item
            menu.insertBefore(li, menu.firstElementChild.nextSibling);
        </script>
    </body>

    </html>

## Summary

* JavaScript DOM hasn’t supported the `insertAfter()` method yet.
* Use the `insertBefore()` method and the `nextSibling` property
to insert a new before an existing node as a child of a parent node.

# JavaScript Infinite Scroll

> **Summary:** in this tutorial, you’ll learn how to implement the
JavaScript infinite scroll feature.

## What you’re going to build

The following picture illustrates the web application that
you’re going to build:

![](https://www.javascripttutorial.net/wp-content/uploads/2020/09/JavaScript-Infinite-Scroll-Example.png)

The page will display a list of quotes that come from an API.
By default, it shows 10 quotes.

If you scroll down to the bottom of the page, the web application will
display a loading indicator. In addition, it’ll call the API to fetch
more quotes and append them to the current list.

The URL of the API that you’re going to use is as follows:

    https://api.javascripttutorial.net/v1/quotes/?page=1&limit=10

The API accepts two query strings: `page` and `limit`. These query
strings allow you to paginate the quotes from the server.

The quotes are divided into the pages determined by the `page` query string.
And each page has a number of quotes specified by the `limit` parameter.

![](https://www.javascripttutorial.net/wp-content/uploads/2020/09/JavaScript-Infinite-Scroll-API.png)

[Click here to see the final web application that uses the JavaScript infinite scroll feature.](https://www.javascripttutorial.net/sample/dom/infinite-scroll/)

## Create a project structure

First, create a new folder called `infinite-scroll`. Inside that folder,
create two subfolders `css` and `js`.

Second, create the `style.css` in the css folder and `app.js` in the js folder.

Third, create a new HTML file `index.html` in the `infinite-scroll` folder.

The final project folder structure will look like this:

![](https://www.javascripttutorial.net/wp-content/uploads/2020/09/JavaScript-Infinite-Scroll-Project-Structure.png)

## Add code to the index.html file

Open the `index.html` and add the following code to it:

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>JavaScript Infinite Scroll - Quotes</title>
        <link rel="stylesheet" href="css/style.css">
    </head>
    <body>

        <div class="container">
            <h1>Programming Quotes</h1>

            <div class="quotes">
            </div>

            <div class="loader">
                <div></div>
                <div></div>
                <div></div>
            </div>
        </div>
        <script src="js/app.js"></script>
    </body>
    </html>

In the `index.html` file, place the `style.css` in the head section
and `app.js` in the body section.

The body section has a `div` with the class name `container`.
The container element has four child elements:

* A heading one (h1) that shows the page heading.
* A `div` with the class `quotes` that will be the parent element of
all the quotes.
* A loader that displays the loading indicator. By default,
the loading indicator is invisible.

## Making the app.js

The following uses the `querySelector()` to select the div with
class `quotes` and the `loader`.

    const quotesEl = document.querySelector('.quotes');
    const loader = document.querySelector('.loader');

## The `getQuotes()` function

The following `getQuotes()` function calls the API and return the quotes:

    const getQuotes = async (page, limit) => {
        const API_URL = `https://api.javascripttutorial.net/v1/quotes/?page=${page}&limit=${limit}`;
        const response = await fetch(API_URL);
        // handle 404
        if (!response.ok) {
            throw new Error(`An error occurred: ${response.status}`);
        }
        return await response.json();
    }

The `getQuotes()` function accepts two arguments: `page` and `limit`.
It uses the Fetch API to fetch data from the API.

Since the `fetch()` returns a promise, you can use the `await` syntax
to get the response. And you call the `json()` method of the response
object to get the json data.

The `getQuotes()` returns a promise that will resolve to the JSON data.

Since the `getQuotes()` function use the `await` keyword, it has to be
an `async` function.

## The `showQuotes()` function

The following defines the `showQuotes()` function that generates
the `<blockquote>` elements from the `quotes` array and appends them
to the `quotes` element:

    // show the quotes
    const showQuotes = (quotes) => {
        quotes.forEach(quote => {
            const quoteEl = document.createElement('blockquote');
            quoteEl.classList.add('quote');

            quoteEl.innerHTML = `
                <span>${quote.id})</span>
                ${quote.quote}
                <footer>${quote.author}</footer>
            `;

            quotesEl.appendChild(quoteEl);
        });
    };

How it works:

The `showQuotes()` uses the `forEach()` method to iterate over
the `quotes` array.

For each quote object, it creates the `<blockquote>` element with
the `quote` class:

    <blockquote class="quote">
    </blockquote>

And it generates the HTML representation of a quote object using
the template literal syntax. It adds the HTML to the `<blockquote>` element.

The following shows an example of the generated `<blockquote>` element:

    <blockquote class="quote">
       <span>1)</span>
          Talk is cheap. Show me the code.
        <footer>Linus Torvalds</footer>
    </blockquote>

At the end of each iteration, the function appends the `<blockquote>`
element to the child elements of the `quotesEl` element by
using the `appendChild()` method.

## Show/hide loading indicator functions

The following defines two functions that show and hide the loading indicator element:

    const hideLoader = () => {
        loader.classList.remove('show');
    };

    const showLoader = () => {
        loader.classList.add('show');
    };

The loading indicator has the opacity 0, which is invisible by default.
The `.show` class sets the opacity of the loading indicator to 1
that will make it visible.

To hide the loading indicator, you remove the `show` class from the
loading indicator element. Similarly, to show the loading indicator,
you add the `show` class to its class list.

## Define control variables

The following declares the currentPage variable and initialize it to one:

    let currentPage = 1;

When you scroll down to the end of the page, the application will make
an API request to get the next quotes. Before doing so, you need to
increase the `currentPage` variable by one.

To specify the number of quotes that you want to fetch at a time,
you can use a constant like this:

    const limit = 10;

The following `total` variable stores the total of quotes
returned from the API:

    let total = 0;

## The hasMoreQuotes() function

The following `hasMoreQuotes()` function returns `true` if:

* It’s the first fetch (`total === 0`)
* Or there are more quotes to fetch from the API (`startIndex < total`)

    const hasMoreQuotes = (page, limit, total) => {
        const startIndex = (page - 1) * limit + 1;
        return total === 0 || startIndex < total;
    };

## The loadQuotes() function

The following defines a function that performs four actions:

* Show the loading indicator.
* Get the quotes from the API by calling the `getQuotes()` function
if there are more quotes to fetch.
* Show the quotes on the page.
* Hide the loading indicator.

    // load quotes
    const loadQuotes = async (page, limit) => {
        // show the loader
        showLoader();
        try {
            // if having more quotes to fetch
            if (hasMoreQuotes(page, limit, total)) {
                // call the API to get quotes
                const response = await getQuotes(page, limit);
                // show quotes
                showQuotes(response.data);
                // update the total
                total = response.total;
            }
        } catch (error) {
            console.log(error.message);
        } finally {
            hideLoader();
        }
    };

If the `getQuotes()` function executes very fast,
you won’t see the loading indicator.

To make sure that the loading indicator always showing,
you can use the `setTimeout()` function:

    // load quotes
    const loadQuotes = async (page, limit) => {

        // show the loader
        showLoader();

        // 0.5 second later
        setTimeout(async () => {
            try {
                // if having more quotes to fetch
                if (hasMoreQuotes(page, limit, total)) {
                    // call the API to get quotes
                    const response = await getQuotes(page, limit);
                    // show quotes
                    showQuotes(response.data);
                    // update the total
                    total = response.total;
                }
            } catch (error) {
                console.log(error.message);
            } finally {
                hideLoader();
            }
        }, 500);

    };

By adding the `setTimeout()` function, the loading indicator will
show for at least a half-second. And you can tweak the delay by
changing the second argument of the `setTimeout()` function.

## Attach the scroll event

To load more quotes when users scroll to the bottom of the page,
you need to attach a scroll event handler.

The scroll event handler will call the `loadQuotes()` function
if the following conditions are met:

* First, the scroll position is at the bottom of the page.
* Second, there are more quotes to fetch.

The scroll event handler will also increase the `currentPage`
variable before loading the next quotes.

    window.addEventListener('scroll', () => {
        const {
            scrollTop,
            scrollHeight,
            clientHeight
        } = document.documentElement;

        if (scrollTop + clientHeight >= scrollHeight - 5 &&
            hasMoreQuotes(currentPage, limit, total)) {
            currentPage++;
            loadQuotes(currentPage, limit);
        }
    }, {
        passive: true
    });

## Initialize the page

When the page loads for the first time, you need to call the
`loadQuotes()` function to load the first batch of quotes:

    loadQuotes(currentPage, limit);

## Wrap `app.js` code in an `IIFE`

To avoid the conflict of variables and functions that you have defined,
you can wrap the whole code in the `app.js` file in an IIFE.

The final `app.js` will look like this:

    (function () {

        const quotesEl = document.querySelector('.quotes');
        const loaderEl = document.querySelector('.loader');

        // get the quotes from API
        const getQuotes = async (page, limit) => {
            const API_URL = `https://api.javascripttutorial.net/v1/quotes/?page=${page}&limit=${limit}`;
            const response = await fetch(API_URL);
            // handle 404
            if (!response.ok) {
                throw new Error(`An error occurred: ${response.status}`);
            }
            return await response.json();
        }

        // show the quotes
        const showQuotes = (quotes) => {
            quotes.forEach(quote => {
                const quoteEl = document.createElement('blockquote');
                quoteEl.classList.add('quote');

                quoteEl.innerHTML = `
                <span>${quote.id})</span>
                ${quote.quote}
                <footer>${quote.author}</footer>
            `;

                quotesEl.appendChild(quoteEl);
            });
        };

        const hideLoader = () => {
            loaderEl.classList.remove('show');
        };

        const showLoader = () => {
            loaderEl.classList.add('show');
        };

        const hasMoreQuotes = (page, limit, total) => {
            const startIndex = (page - 1) * limit + 1;
            return total === 0 || startIndex < total;
        };

        // load quotes
        const loadQuotes = async (page, limit) => {

            // show the loader
            showLoader();

            // 0.5 second later
            setTimeout(async () => {
                try {
                    // if having more quotes to fetch
                    if (hasMoreQuotes(page, limit, total)) {
                        // call the API to get quotes
                        const response = await getQuotes(page, limit);
                        // show quotes
                        showQuotes(response.data);
                        // update the total
                        total = response.total;
                    }
                } catch (error) {
                    console.log(error.message);
                } finally {
                    hideLoader();
                }
            }, 500);

        };

        // control variables
        let currentPage = 1;
        const limit = 10;
        let total = 0;


        window.addEventListener('scroll', () => {
            const {
                scrollTop,
                scrollHeight,
                clientHeight
            } = document.documentElement;

            if (scrollTop + clientHeight >= scrollHeight - 5 &&
                hasMoreQuotes(currentPage, limit, total)) {
                currentPage++;
                loadQuotes(currentPage, limit);
            }
        }, {
            passive: true
        });

        // initialize
        loadQuotes(currentPage, limit);

    })();

Here is the final version of [the web application](https://www.javascripttutorial.net/sample/dom/infinite-scroll/).