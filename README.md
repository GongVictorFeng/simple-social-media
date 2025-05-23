# RESTFUL-Web-Service
This repository demonstrates the development of a REST API for a Social Media Application using Spring Boot, focusing on best practices and advanced features. Key components include:

## REST API with Spring Boot

### Goals:
  * How to build a great REST API
    * Identifying Resources (/users, /users/{id}/posts)
    * Identifying Actions (GET, POST, PUT, DELETE,...)
    * Defining Request and Response structures
    * Using appropriate Response Status (200, 404, 500,..)
    * Understanding REST API Best Practices
      * Thinking from the perspective of your consumer
      * Validation, Internationalization - i18n, Exception Handling, HATEOAS, Versioning,...

### Approach:
1. Build a REST API for a Social Media Application
   * Design and Build a great REST API
     * Choosing the right URI for resources (/users, /users/{id}, /users/{id}/posts)
     * Choosing the right request method for actions (GET, POST, PUT, DELETE,...)
     * Designing Request and Response structures
     * Implementing Security, Validation and Exception Handling
   * Build Advanced REST API Features
     * Internationalization, HATEOAS, Versioning, Documentation, Content Negotiation,...
2. Connect REST API to a Database
   * Fundamentals of JPA and Hibernate
   * Use H2 and MySQL as databases

### What is happening in the Background
1. How requests are handled
   * DispatcherServlet  Front Controller Pattern
     * Mapping servlets: dispatcherServlet urls = [/]
     * Auto Configuration (DispatcherServletAutoConfiguration)
     * map requests to the right controller method
2. How Bean object get converted to JSON
   * @ResponseBody + JacksonHttpMessageConverters
     * Auto Configuration (JacksonHttpMessageConvertersConfiguration)
3. Error mapping configuration
   * Auto Configuration (ErrorMvcAutoConfiguration)
4. jas availability (Spring, Spring MVC, Jackson, Tomcat)
   * Starter Projects - Spring Boot Starter Web (spring-webmvc, spring-web, spring-boot-starter-tomcat, spring-boot-starter-json)

### Request Methods for REST API
  * GET - Retrieve details of a resource
  * POST - Create a new resource
  * PUT - Update an existing resource
  * PATCH - Update part of a resource
  * DELETE - Delete a resource 

### Social Media Application - Resources & Methods
  * Users REST API
    * Retrieve all Users
      * GET /users
    * Create a User
      * POST /users
    * Retrieve one User
      * GET /users/{id} -> /users/1
    * Delete a User 
      * DELETE /users/{id} -> /users/1
  * Posts REST API
    * Retrieve all posts for a User
      * GET /users/{id}/posts
    * Create a post for a User
      * POST /users/{id}/posts
    * Retrieve details of a post
      * GET /users/{id}/posts/{post_id}

### Response status for REST API
  * Return the correct response status
    * Resource is not found => 404
    * Server exception => 500
    * Validation error => 400
  * Important Response Statuses
    * 200 - Success
    * 201 - Created
    * 204 - No Content
    * 401 - Unauthorized (when authorization fails)
    * 400 - Bad Request (such as validation error)
    * 404 - Resource Not Found
    * 500 - Server Error

## Advanced REST API Features

### REST API Documentation
  * REST API consumers need to understand the REST API:
    * Resources
    * Actions
    * Request/Response Structure (Constraints/Validations)
  * Challenges:
    * Accuracy: how to ensure that documentation is upto date and correct
    * Consistency: how to ensure consistency with large number of REST API in an enterprise
  * Options:
    * Manually Maintain Documentation
      * Additional effort to keep it in sync with code
    * Generate from code
  * OpenAPI Specification: Standard, language-agnostic interface
    * Discover and understand REST API
    * Earlier called Swagger Specification
  * Swagger UI: Visualize and interact with REST API
    * can be generated from OpenAPI Specification: ![rest_documentation_swaggerUI.png](src%2Fassets%2Frest_documentation_swaggerUI.png)

### Content Negotiation
  * Same Resource - Same URI
    * Different Representations are possible
      * Example: Different Content Type -XML or JSON or ...
      * Example: Different Language - English or Dutch or ...
  * How a consumer tell the REST API provider what they want 
    * Content Negotiation
  * Example: Accept header (MIME types - application/xml, application/json,...)
  * Example: Accept-Language header (en,fr,nl,...)

### Internationalization - i18n
  * REST API might have consumers from around the world
  * How to customize it to users around the world
    * Internationalization - i18n
  * HTTP Request Header - Accept-Language is used
    * Accept-Language - indicates natural language and locale that the consumer prefers
    * Example: en - English (Good Morning)
    * Example: nl - Dutch (Goedemorgen)
    * Example: en - English (Bonjour)

### Versioning REST API
  * an amazing REST API
    * Has 100s of consumers
    * need to implement a breaking change
      * Example: Split name into firstName and lastName
  * Solution: Versioning REST API
    * Variety of options
      * URL
      * Request Parameter
      * Header
      * Media Type
    * No clear Winner
  * URI Versioning -Twitter
    * http://localhost:8080/v1/person
    * http://localhost:8080/v2/person
  * Request Parameter versioning - Amazon
    * http://localhost:8080/person?version=1
    * http://localhost:8080/person?version=2
  * (Custom) headers versioning - Microsoft
    * SAME-URL headers=[X-API-VERSION=1]
    * SAME-URL headers=[X-API-VERSION=2]
  * Media type versioning ("content negotiation" or "accept header") - GitHub
    * SAME-URL produces=application/vnd.company.app-v1+json
    * SAME-URL produces=application/vnd.company.app-v2+json
  * Factors to consider
    * URI Pollution (creating too many urls)
    * Misuse of HTTP Headers (headers are never meant to use for versioning)
    * Caching (usually caching is based on URL)
    * can execute the request on the browser
    * API Documentation
    * Summary: No perfect solution
  * Recommendations
    * Think about versioning even before you need it
    * One Enterprise - One Versioning Approach

### HATEOAS
  * Hypermedia as the Engine of Application State (HATEOAS)
  * Websites allow you to: 
    * See Data And Perform Actions (using links)
  * enhancing REST API to tell consumers how to perform subsequent actions
    * HATEOAS
  * Implementation Options:
    * Custom Format and Implementation
      * Difficult to maintain
    * Use standard implementation
      * HAL (JSON Hypertext Application Language): Simple format that gives a consistent and easy way to hyperlink between resources in API
      * Spring HATEOAS: generate HAL responses with hyperlinks to resources
      * see implementation: https://github.com/GongVictorFeng/simple-social-media/commit/fd731076c757b51fd8885f1f899e5068561bfa49

### Customizing REST API Responses - Filtering and more...
  * Serialization: Convert object to stream (example: JSON)
    * Most popular JSON Serialization in Java: Jackson
  * How to customize the REST API response returned by Jackson framework
    * Customize field names in response
      * @JSONProperty
    * Return only selected fields
      * filtering 
      * Example: Filter out Passwords
      * Two types:
        * Static Filtering: Same filtering for a bean across different REST API
          * @JsonIgnoreProperties, @JsonIgnore
        * Dynamic Filtering: Customize filtering for a bean for specific REST API
          * JSONFilter with FilterProvider
        * see implementation: https://github.com/GongVictorFeng/simple-social-media/commit/876985c13a705881c47a311b963a1e22cf2f396f

### Connect REST API to Database
  * JPA defines the specification, it is an API
    * how to define entities
    * how to map attributes
    * how to manage the entities
  * Hibernate is one of the popular implementation of JPA
  * Using Hibernate directly would result in a lock in to Hibernate
    * There are other JPA implementations (Toplink, for example)
  * See implementation: https://github.com/GongVictorFeng/restful-web-service/commit/d4259e7de26450c0d88918929af799fa159d748e