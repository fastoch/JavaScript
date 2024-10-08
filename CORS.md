# Definition

- CORS stands for **Cross-Origin Resource Sharing**
- It's a security mechanism that allows web browsers to make requests to resources on different domains than the one serving the web page.  

# Purpose & Functionality

- CORS enables servers to specify which origins (domains) are allowed to access their resources
- It relaxes the Same-Origin Policy (**SOP**), which normally restricts web pages from making requests to domains other than their own

# How CORS works?

- When a web page makes a cross-origin request, the browser sends a preflight request to the server hosting the resource
- The server responds with headers indicating whether the actual request is allowed
- If approved, the browser then sends the actual request

# Benefits of CORS

- **Enhanced API Security**: Helps prevent unauthorized cross-origin requests
- **Cross-Origin Authentication**: Enables secure authentication across domains
- **API Integration**: Allows web applications to utilize third-party APIs and services

# Implementation

- CORS is implemented by web browsers and supported by all major browsers
- Servers need to be configured to send the appropriate CORS headers

By using CORS, web applications can securely access resources from different domains, enabling more flexible and powerful web services while maintaining security.
