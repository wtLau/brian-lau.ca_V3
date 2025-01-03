---
title: 'Introduction to Fetch'
publishDate: '2023-10-25'
excerpt: "Exploring the Fetch API's intricate components in my personal journey, aiming to explain their complexities for a better understanding."
isFeatured: true
tags:
  - Web development
seo:
  image:
    src: '/post-9.jpg'
    alt: Mountains
---

![Mountains](/post-9.jpg)

Hello there! As web developers, we might have used Fetch or similar technology to establish connections with various servers. This layer of protocol operates like magic, and despite having a distaste for 'black boxes', I aim to demystify this layer of abstraction. In my career, I've used the Fetch API numerous times and yet I've rarely dive deep into understanding its workings, except for a few occasions where I encountered odd bugs related to redirects and caches. After extensive debugging and learning about the Fetch API, I discovered there's much more hidden within it, and I'm eager to share my learnings with you!

On a high level, fetch is a modern and straightforward method for making network requests that return a [Promise object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). It is platform agnostic, available on all major browsers as a global method in both window and worker scope, as well as in JS runtimes such as Node.js, Deno and Bun.

## Overview

### The Basic Syntax

```tsx
fetch(url)
```

The common approach when using `fetch()` is with a promise or async/await syntax. While Promises have a built-in error handling method with catch(), for Async/Await, we need to use a Try/Catch block for error handling.

**Promise**

```jsx
const url = 'www.brian-lau.ca/blog' // the url you are requesting data from

function getData() {
  fetch(url)
    .then((resp) => {
      if (!resp.ok) throw new Error('was not a valid response')
      return resp.json() // method to extract JSON string and convert it to an Object
    })
    .catch((err) => {
      console.warn(err.message) // in case of error, we log the error on console
    })
}
```

**Async/Await with Try/Catch Error Handling**

```tsx
const url = 'www.apiendpoint.com/list'

async function getData() {
  try {
    let response = await fetch(url)
    if (!response.ok) throw new Error('not a valid response')
    let data = await response.json()
    return data
  } catch (err) {
    console.warn(err.message)
  }
}
```

The above snippets provide a simple overview of how to use the fetch API, suitable for most read-only use cases. The API can delve deeper into how we communicate with HTTP protocols. Keep reading to learn more about the details.

## URL

As demonstrated above, we require a URL pointer to request the resources, and at times these URLs are constructed with logic based on different states of our applications. In my experience, we often need to manipulate URL parameters or pathnames to build a desired string representation of the URL. For example, retrieving a specific item with a unique ID, syncing URLs with application states, or loading a page with a custom UI based on parameters, etc. We could construct it using a string and concatenate the parts together, but this method is often prone to bugs and requires additional steps to encode special characters such as using `encodeURIComponent`.

Nowadays, there is a handy [`URL()` web API](https://developer.mozilla.org/en-US/docs/Web/API/URL) that we can use to read and update the components to construct a URL.

**Basic Usages**

```jsx
const url = new URL('https://brian-lau.ca')

// or with a relative path
const withPath = new URL('../blog', 'https://brian-lau.ca')
```

**Interesting Aspects of the URL object**

```jsx
const urlObj = new URL('/blog?id=0', 'https://brian-lau.ca')

urlObj.searchParams.get('id') // return '0'
urlObj.searchParams.set('id', '1&0') // setting the id value to '1&0'

urlObj.host // 'www.brian-lau.ca'
urlObj.toString() // 'https://brian-lau.ca/blog?id=0'

urlObj.host = 'www.brian.ca'
console.log(urlObj.host) // 'www.brian.ca'
```

We can pass this into `fetch` to ensure it's a URL object with type inferences (when using TS).

```jsx
const url = new URL('/blog?id=0', 'https://brian-lau.ca')

fetch(url)
```

## HTTP Request & Response

Another part of Fetch is the request object, for which we have a `Request()` object that we can pass to provide additional context to communicate with the server. In return, we receive a `Response()` object as the result of our request.

Both request and response are constructed similarly, with a header and a body. The header is where the configuration and settings go that specify more about how and when. The body carries the content being transmitted. A response could be in various formats such as text, HTML, XML, JSON, image, video, etc. We need different handlers such as `response.blob()`, `response.text()`, `response.json()` to manage different kinds of media.

```jsx
// Creating a requset object
const request = new Request(url, {
  headers: {
    'content-type': 'application/json',
    'x-brian': 'hello', // this is custom header since it starts with x-
  },
  method: 'GET',
  cache: 'no-store', // get from the server without cache lookup, there are other options
})
```

```jsx
// Initialize a text file
const file = new File(['foo'], 'foo.txt', {
  type: 'text/plain',
})

// creating a response object
let response = new Response(file, {
  status: 200,
  statusText: 'boo',
  headers: {
    'content-type': 'text/plain',
    'content-length': file.size,
    'x-brian': 'custom header name',
  },
})
```

### Generating Content

After receiving a valid response, we can update the HTML to render useful information using the `.setHTML()` method.

```jsx
let list = document.getElementById('list') // target element

fetch(json) // URL
  .then((response) => {
    if (!response.ok) throw new Error('invalid')
    return response.json()
  })
  .then((data) => {
    const contentElement = data
      .map(({ id, name }) => {
        return `<li class="listitem" data-uid="${uid}">
          <p>${name}</p>
        </li>`
      })
      .join('') // concatenate all elements as a string

    // setHTML is used to parse and sanitize a string of HTML and 'render' it
    // note: Firefox v94 & Safari not supported yet.
    //       You can either sanitize the string manually or
    //       polyfill it to an older version
    list.setHTML(contentElement)
  })
  .catch(console.warn)
```

## Authorization & Credentials

It wouldn't be secure if the APIs are not protected by authorization because anyone would be able to access that information. To retrieve private data, we have to provide special keys to authenticate our identity. There are various implementations for achieving this level of security, and the most common ways involve using URL query strings, headers, and cookies.

The following code expands on previously learned concepts, diving deeper into our Fetch configuration to provide more context to our server. The exact way to communicate successfully depends on how the backend is implemented, and it's best to coordinate with the backend team on how this should be approached. For simplicity, we could add an access key in our search params when a user enters their API on the input form. To establish identity, we need to modify the header to include the application's API key as well as a JSON Web Token for user authentication. Again, this a just a simple example on how this could be approached.

```jsx
let url = new URL('https://www.someapi.com/?privateblog=true')
url.searchParams.append('access-key', 'fg7Z2V6iJfAt$a')

let header = new Headers()
header.append('x-api-key', 'heUHY85e*VL36R') // API key
header.append('Authorization', 'heUHY85e*VL36R123') // JSON Web Token

let request = new Request(url, {
	method: 'GET',
	header,
	cache: 'default',
	credentials: 'same-origin',
})

fetch(request)
	.then()
	...
```

## Uploading Information

We often need to upload information back to the server to perform more complex operations, like submitting a form for an application, uploading an image to the cloud, or updating social media status.

In contrast to a `GET` request, we need to change the request method to `POST`, specify the content-type (as [MIME Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)) in the header to indicate what the user is uploading, and attach the data (as [ReadableStream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)) we intend to upload to the request body. In the example below, I used the [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData) constructor to extract multiple MIME type pieces from the HTML form.

```jsx
function onSubmit(){
	// Getting the form data we want to upload
	let body = new FormData(document.getElementById('contact-form'))

	let headers = new Headers({
    // As we're using form submission, we need to set the content-type to 'multipart/form-data'
		'Content-Type': 'multipart/form-data'
	})
	let request = new Request(url, {
		method: 'POST',
		body,
		headers
	})

	fetch(request).then(...)
}
```

## CORS

The last thing I wanted to cover is cross-origin resource sharing (CORS). It is a protective access-control protocol in HTTP to gate API calls that are different from its own resources.

The main idea is to conduct a "pre-flight" check with the third-party server using an "OPTION" method to seek approval for this request. Upon a successful check, the server would respond with an `Access-Control-Allow-Origin: *` header (or to that particular domain) along with the requested resources. This step is required for making a change that could cause a side effect on a third-party server.

There are cases where CORS isn't required, such as making a `GET` request that retrieves specific media types like `x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`.

## Summary

Thinking more abstractly, the web comprises a network of connections that communicate with different machines remotely. Fetch serves as the digital bridge to make requests and manage responses to ensure users get their data smoothly and accurately. In fact, this is the JavaScript interface for making HTTP calls. It's one of the pillars I have yet to explore and become familiar with. There are other protocols that aren't as common in the web development world, such as TCP, FTP, and UDP, etc. Although there are libraries that abstract those for us, I believe it's important to dive deep and understand each of these technologies well so that I can function across different layers. I'll be sharing more of my learning in the future. Good luck and happy coding!

## References & Readings

- [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [MDN URL API](https://developer.mozilla.org/en-US/docs/Web/API/URL)
