---
published: true
---

In order to document our API endpoints related to Stamp and Fields logic, we chose to use Swagger in our Play application. Howver, I found it hard to find a guide about using this technology from scratch. My goal in this post is to make it easy for newcomers to learn Play-Swagger. 

## Swagger

Swagger is a framework generating specifications which act as the RESTful contract for your API, detailing all of its resources and operations in a human and machine readable format for easy development, discovery, and integration. [play-swagger](https://github.com/iheartradio/play-swagger) is a library that generates swagger specs from route files and case class reflection (no code annotation needed). We use play-swagger to document our API endpoints. This guide is meant to get you quickly started with Swagger.
