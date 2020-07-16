# Swagger

[Swagger Docs](https://swagger.io/docs/specification/about/)

### Note of Swagger 

### Basic Concepts

### OpenAPI

**OpenAPI Specification** (formerly Swagger Specification) is an API description format for REST APIs. An OpenAPI file allows you to describe your entire API, including:

- Available endpoints (`/users`) and operations on each endpoint (`GET /users`, `POST /users`)
- Operation parameters Input and output for each operation
- Authentication methods
- Contact information, license, terms of use and other information.

### What is Swagger

A set of open-source tools built around the OpenAPI Specification that can help you design, build, document and consume REST APIs

Major Swagger tools include

- [Swagger Editor](http://editor.swagger.io/) – browser-based editor where you can write OpenAPI specs.
- [Swagger UI](https://swagger.io/swagger-ui/) – renders OpenAPI specs as interactive API documentation.
- [Swagger Codegen](https://github.com/swagger-api/swagger-codegen) – generates server stubs and client libraries from an OpenAPI spec.

### OpenAPI Usage

OpenAPI definitions in [YAML](https://en.wikipedia.org/wiki/YAML) or [JSON](https://en.wikipedia.org/wiki/JSON). All keyword names are **case-sensitive**.A sample OpenAPI 3.0 definition written in YAML looks like:

```yaml
openapi: 3.0.0
info:
  title: Sample API
  description: Optional multiline or single-line description in [CommonMark](http://commonmark.org/help/) or HTML.
  version: 0.1.9
servers:
  - url: http://api.example.com/v1
    description: Optional server description, e.g. Main (production) server
  - url: http://staging-api.example.com
    description: Optional server description, e.g. Internal staging server for testing
paths:
  /users:
    get:
      summary: Returns a list of users.
      description: Optional extended description in CommonMark or HTML.
      responses:
        '200':    # status code
          description: A JSON array of user names
          content:
            application/json:
              schema: 
                type: array
                items: 
                  type: string
```

Basic Structure

- Metadata
- Servers
- Paths
- Parameters
- Request Body
- Responses
- Input and Output Models
- Authentication