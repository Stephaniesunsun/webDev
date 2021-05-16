# webDev

An incomplete list of key concepts for web development. Keep updating

# 1. XML v.s. JSON

- Both JSON and XML can be used  to receive data from a web server.
- Similarities:
  1. Both of them are "self-describing" (human readable)
  2. Both are hierarchical (values within values)
  3. Both can be parsed and used by lots of programming languages
  4. Both can be fetched with and XMLHttpRequest
- Differences:
  1. JSON doesn't use and tag
  2. JSON can use arrays
  3. XML has to be parsed with ann XML parser. JSON can be parsed by a standard JS function

# 2. XML HttpRequest

- The XMLHttpRequest object can be used to request data from a web server
- The XMLHttpRequest object can be used to exchange data with a server behind the scences. This means that it is possible to update parts of a web page, without reloading the whole page.
- XMLHttpRequest object methods
- XMLHttpRequest object properties

# 3. AJAX, fetch (with async/ await) and Axios

- Javascript is a single-thread language, we don't want blocking script- imagine if every network that took time to give us a response blocked any other operations from executing

- AJAX: "Asynchronous Javascript and XML"
  allows us to build Single Page Applications (SPAs): "An SPA is a web application or website that interacts with the user by dynamically rewriting the current page rather than loading entire new pages from a server"

- fetch('URL'): the Fetch API

  1.  the simplest use of fetch takes one argument- the path to the resource you want to fetch- and returns a promise containing the response body. you can make HTTP requests (GET, POST etc.), downlaod, and upload files
  2.  use fetch() with async/ await

  `async function fetchSomething(){ const response= await fetch(resource[, options]); console.log(response); }`

  resource is the URL string where the data locate at
  options is the configuration object with properties like method, headers, body, etc

  fetch() starts a request and returns promise. When the request completes, the promise is resovled with the Response object, which is a generic placeholder for multiple data formats. For example, you can specify the data type JSON by doing the following:

  `const response= await fetch(resourceURL); const data=await response.json(); retrun data;`

  `response.json()` is a method on the Response object that lets you extract a JSON object from the response. Other methods are: `response.text()`, `response.formData()`...

  If the request fails due to some network problems, the promsise is rejected.

  3. error handling

  `fetch()` rejects only if a request cannot be made or a request cannot be retrieved. It might happen because of network problems: no internet connection, host not found, the server is not responding.

  `fetch()` does not throw an error for a missing URL, but considers this as a completed HTTP request.

  For example, for a request that returns 404,
  `response.ok; //returns false`
  `response.status; //returns 404`

  We can thus use these 2 properties to do error handling:

  `if(!response.ok){ const message=`error:${response.status}`; throw new Error(message); }`

- Axios

  1. It allows you to

  - Make XMLHttpRequests from the browser
  - Make HTTP requests from Node.js
  - Supports the promise API
  - Automatically transforms to JSON data (i.e. when making POST requests, we don't need JSON.stringify the body data into JSON format, which we would need for fetch())

  2. Response timeout
     you can use the optional `timeout` property in the config object to set the number of msec before the request is aborted. Example:
     `axios({ method:'post', url:'/login', timeout: 4000, //4s data:{ firstName:'John', lastName:'Doe' } }) .then(response=>{console.log()}) .catch(error=?console.error('timeout));`
     which is not as simple using `fetch()`

  3. Automatic JSON data transformation
     Axios automatically stringifies the data when sending requests

  4. HTTP interceptors
     One of the key features of Axios is its ability to intercept HTTP requests.

  Sources:

1. https://www.w3schools.com/js/js_json_xml.asp
2. https://www.w3schools.com/xml/xml_http.asp
3. http://comet.lehman.cuny.edu/sfulakeza/su19/tpp/slides/Day%206/AJAX,%20fetch,%20and%20Axios.pdf
4. https://dmitripavlutin.com/javascript-fetch-async-await/
5. https://blog.logrocket.com/axios-or-fetch-api/
