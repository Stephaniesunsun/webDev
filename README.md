# WebDev

An incomplete list of key concepts for web development. Keep updating.

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
  Allows us to build Single Page Applications (SPAs): "An SPA is a web application or website that interacts with the user by dynamically rewriting the current page rather than loading entire new pages from a server"

- Fetch('URL'): the Fetch API

  1.  The simplest use of fetch takes one argument- the path to the resource you want to fetch- and returns a promise containing the response body. you can make HTTP requests (GET, POST etc.), downlaod, and upload files
  2.  Use fetch() with async/ await

  ```
  async function fetchSomething(){ 
  const response= await fetch(resource[, options]); 
  console.log(response); 
  }
  ```

  `resource` is the URL string where the data locate at
  `options` is the configuration object with properties like method, headers, body, etc

  `fetch()` starts a request and returns promise. When the request completes, the promise is resovled with the Response object, which is a generic placeholder for multiple data formats. For example, you can specify the data type JSON by doing the following:

  ```
  const response= await fetch(resourceURL); 
  const data=await response.json(); 
  retrun data;
  ```

  `response.json()` is a method on the Response object that lets you extract a JSON object from the response. Other methods are: `response.text()`, `response.formData()`...

  If the request fails due to some network problems, the promsise is rejected.

  3. error handling

  `fetch()` rejects only if a request cannot be made or a request cannot be retrieved. It might happen because of network problems: no internet connection, host not found, the server is not responding.

  `fetch()` does not throw an error for a missing URL, but considers this as a completed HTTP request.

  For example, for a request that returns 404,
  `response.ok; //returns false`
  `response.status; //returns 404`

  We can thus use these 2 properties to do error handling:

  ```
  if(!response.ok){ 
    const message=`error:${response.status}`; 
    throw new Error(message); }
   ```

- Axios

  1. It allows you to

  - Make XMLHttpRequests from the browser
  - Make HTTP requests from Node.js
  - Supports the promise API
  - Automatically transforms to JSON data (i.e. when making POST requests, we don't need JSON.stringify the body data into JSON format, which we would need for fetch())

  2. Response timeout
     you can use the optional `timeout` property in the config object to set the number of msec before the request is aborted. Example:
     `axios({ `
      `method:'post', `
      `url:'/login', `
      `timeout: 4000, //4s `
      `data:{ firstName:'John', lastName:'Doe' } }) `
      `.then(response=>{console.log()}) `
      `.catch(error=?console.error('timeout));`
     which is not as simple using `fetch()`

  3. Automatic JSON data transformation
     Axios automatically stringifies the data when sending requests

  4. HTTP interceptors
     One of the key features of Axios is its ability to intercept HTTP requests. HTTP intercepts come in handy when you need to examine or change HTTP requests from your application to the server or vica versa. With interceptors, you won't have to write separate code for each HTTP request. In the following example, `axios.interceptors.request.use()` method is used to define a code to be run before an HTTP request is sent.
     ```
     axios.interceptors.request.use(config=>{
         console.log('request was sent);
         return config;
     });
     axios.get(URL)
     .then(response=>{
         console.log(response.data);
     })
     ```
  5. Simutaneous requests

  ```axios.all([
      axios.get(URL1),
      axios.get(URL2)
  ])
  .then(axios.spread((obj1,obj2)=>{
      //both requests are now complete
      //handle responses
  }))
  ```
  
- Javascript
  
  1. Array.prototype.splice()
      Changes the contents of an array by removing or replacing existing elements and/or add new elements in place. Example:
      ```
      const arr=[1,2,3]
      arr.splice(1,2) //return 2,3
      arr //return 1
      ```
  2. Array.prototype.slice()
      Returns a shallow copy (the original array not modified) of an array into a new array object selected from start to end (end not included), where start and end are indices, can be used to return a portion of an array. Example:
      ```
      const arr=[1,2,3]
      arr.slice(1,2)//return 2
      arr //return 1,2,3
      ```
   3. Array/ Object spread operator
      When using on an object, it spreads the object to get all the properties (spreading over the keys and values). When we are assigning a new object with different certain properties, we only need to specifies the ones being changed, left other properties the same. 
      
      When using inside another array, it's similar to Array.concat, where we spread all the elements of the first array into the new one, and it allows duplicate values.
      
      As a rest operator: as the function's argument. It enables us to create functions that can take an indefinite number of arguments, also called functions of variable arity or variadic functions. Example:
      ```
      function sum (...nums){
        return nums.reduce((accu, element))=>{accu+=element;}
      }
      sum(1,6,4);
      ```
      As what we would normally do (or not), we pass the arguments without spreading. But note that by doing this we are essentially passing in an object with all the values and a ```length``` property to the function.
  
  Sources:

1. https://www.w3schools.com/js/js_json_xml.asp
2. https://www.w3schools.com/xml/xml_http.asp
3. http://comet.lehman.cuny.edu/sfulakeza/su19/tpp/slides/Day%206/AJAX,%20fetch,%20and%20Axios.pdf
4. https://dmitripavlutin.com/javascript-fetch-async-await/
5. https://blog.logrocket.com/axios-or-fetch-api/
6. https://oprearocks.medium.com/what-do-the-three-dots-mean-in-javascript-bc5749439c9a
