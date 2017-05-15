---
published: true
---

In order to document our API endpoints related to document stamping, we (Nitro) chose to use Swagger in our Play application. However, I found it hard to find a guide about using this technology from scratch. My goal in this post is to make it easier for newcomers to get up and running with Play-Swagger. 

## Swagger

Swagger is a framework generating specifications which act as the RESTful contract for your API, detailing all of its resources and operations in a human and machine readable format for easy development, discovery, and integration. [play-swagger](https://github.com/iheartradio/play-swagger) is a library written by [iHeartRadio](https://github.com/iheartradio/play-swagger) that generates swagger specs from route files and case class reflection (no code annotation needed). 
 
## Configuration

To get you started, we will take the example of generating a specification for creating a stamp on a document. First, you should:

Add to plugin.sbt:

`addSbtPlugin("com.iheart" % "sbt-play-swagger" % "0.5.4-PLAY2.4")`

Enable the plugin in build.sbt in the folder containing your *.routes files:

`lazy val server = project.settings(name := "service-name-server").enablePlugins(PlayScala, SwaggerPlugin)`

Here are some additional configurations you need:

To generate specification for a specific case class, without having to detail it manually, you can define a schema pointing to this case class the following way: 

`$ref: '#/definitions/com.orgName.documents.signing.StampRequest'`. 

To auto generate those swagger definitions, you will also need to add domain package names to play-swagger in build.sbt: 

`swaggerDomainNameSpaces := Seq("com.orgName.documents")` 

This will make classes and objects in documents visible and will enable you to reference them in your schema definitions.


## Getting started

To start documenting your endpoints, create 2 files in your conf directory:

- swagger.yml: contains your basic configurations and definitions. You should add `Bearer <token>` to add authentication support to the swagger UI, that way you can test endpoints right from swagger.
- swagger-custom-mappings.yml: swagger may encounter some difficulties converting some case classes to json objects. In this file you define a set of mapping to help with the conversion. For example, it won't be able to recognize our custom case classes defined in our custom *orgLibrary*, e.g. Id., the solution is to define a regex and map it to the expected type, e.g. we map com\.orgName\.orgLibrary\.models\.Id.* to string. Also you should manually define enum type fields.
- routes: add the documentation before a specific route definition:

```
###
#  summary: some description
#  parameters:
#    MORE DETAILS IN our services
#  responses:
#    MORE DETAILS IN our services
###
```
And add the following route definition:

```
### NoDocs ###
GET   /docs/swagger.json            controllers.Assets.at(path ="/public", file = "swagger.json")
```

## Swagger UI

To test your definitions about custom input you can use swagger UI, by having your Play aplication running the swagger-ui: 

- Add the following dependency: 

`libraryDependencies += "org.webjars" % "swagger-ui" % "2.2.0"`

- Add to your route file: 

```
### NoDocs ###
GET   /docs/swagger-ui/*file        controllers.Assets.at(path:String="/public/lib/swagger-ui", file:String)

### NoDocs ###
GET   /assets/*file                 controllers.Assets.versioned(path="/public", file: Asset)
```

- `sbt project/run` and then navigate to [swagger-ui](http://localhost:9000/docs/swagger-ui/index.html?url=/assets/swagger.json)

Here's an example of what can be seen in the swagger-ui, w.r.t. the stamp specification: 

![Specification for stamp in swagger-ui]({{site.baseurl}}https://github.com/kevllino/kevllino.github.io/blob/master/images/Screen%20Shot%202017-05-14%20at%2014.34.57.png?raw=true)

## Debugging with Swagger Editor

1. Run `sbt server/swagger` => this will generate a `swagger.json` file under `target/swagger`
1. Copy the output after using a json formatter, in IntelliJ (`Cmd + Alt + Shift + L`)
1. Paste it in the [swagger online editor](http://editor.swagger.io/#/)

### Additional information

- To hide an endpoint documentation, include `### NoDocs ###` before the route definition.
- Swagger doesn't like `Iterable`, convert to `Seq`
