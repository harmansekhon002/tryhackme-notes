a website is made up of basically two components
1. front end (client side) the way your browser renders a website
2. back end (server side) a server that processes your request and returns a response
## html
- html to build websites and their structures
- css to make websites look pretty 
- javascript to implement complex features like interactivity
### html
- hyper text markup language is the language websites are written in 
- elements or tags are the building blocks of html pages and tells the browser how to display content 
- the code snippet below is the basic structure of a html page
- https://assets.tryhackme.com/additional/how-websites-work/example_html.png
### elements of the code
- - The `<!DOCTYPE html>` defines that the page is a HTML5 document. This helps with standardisation across different browsers and tells the browser to use HTML5 to interpret the page.
- - The `<html>` element is the root element of the HTML page - all other elements come after this element.
- - The `<head>` element contains information about the page (such as the page title)
- - The `<body>` element defines the HTML document's body; only content inside of the body is shown in the browser.
- - The `<h1>` element defines a large heading
- - The `<p>` element defines a paragraph
- - There are many other elements (tags) used for different purposes. For example, there are tags for buttons (`<button>`), images (`<img>`), lists, and much more.
### tags
- Tags can contain attributes such as the class attribute which can be used to style an element (e.g. make the tag a different color) `<p class="bold-text">`, or the _src_ attribute which is used on images to specify the location of an image: `<img src="img/cat.jpg">.`An element can have multiple attributes each with its own unique purpose, e.g., <p attribute1="value1" attribute2="value2">.
- Elements can also have an id attribute (`<p id="example">`), which is unique to the element. Unlike the class attribute, where multiple elements can use the same class, an element must have different id's to identify them uniquely. Element id's are used for styling and to identify it by JavaScript.
## javascript
- this language is used to make a webpage interactive 
- it can dynamically update a page in real time
- change the style of a button upon the click of a button (an event)
- JavaScript is added within the page source code and can be either loaded within `<script>` tags or can be included remotely with the src attribute: `<script src="/location/of/javascript_file.js"></script>`
- The following JavaScript code finds a HTML element on the page with the id of "demo" and changes the element's contents to "Hack the Planet" : `document.getElementById("demo").innerHTML = "Hack the Planet";`
- HTML elements can also have events, such as "onclick" or "onhover" that execute JavaScript when the event occurs. The following code changes the text of the element with the demo ID to Button Clicked: `<buttononclick='document.getElementById("demo").innerHTML = "Button Clicked";'>Click Me!</button>` - onclick events can also be defined inside the JavaScript script tags, and not on elements directly.
## sensitive data exposure
- usually in a site's front end code sensitive data isnt properly protected and is thus shown to the front end user
- we know we can see the source code of a page by clicking inspect the page
- sometimes the login credentials are not hidden and other details too
- ex there could be a html comment with temporary login credentials and we can access deep parts of the site
- first thing to do in web security is to review the page source code for any sensitive info
## html injection
- it is a vulnerability that occurs when unfiltered user input is displayed on the page 
- if a web fails to sanitise user input and the input contains malacious html code 
- another variation is dbms injection
- The general rule is never to trust user input. To prevent malicious input, the website developer should sanitise everything the user enters before using it in the JavaScript function; in this case, the developer could remove any HTML tags.
- Whatever the user inputs into the "What's your name" field is passed to a JavaScript function and output to the page, which means if the user adds their own HTML or JavaScript in the field, it's used in the sayHi function and is added to the page - this means you can add your own HTML (such as a <h1> tag) and it will output your input as pure HTML.
 