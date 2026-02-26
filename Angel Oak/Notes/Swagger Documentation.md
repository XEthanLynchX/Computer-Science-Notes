# What is Swagger  
Swagger is the brand that is wrapped around OpenAPI and reads `openapi.yaml` and creates a pretty interactive API documentation for it. Swagger can be used to as you code to help document as you go and can also be used to after the code has been written to provide ease of use to an api. 

# Writing OpenAPI Docs 
--- 
When writing an OpenAPI doc it can be written as YAML or JSON file. OpenAPI Docs must conform to the standard of definitions below to be considered a OpenAPI document. An OpenAPI Document compatible with OAS 3.*.* contains a required [`openapi`](https://swagger.io/specification/#oas-version) field which designates the version of the OAS that it uses.
### **Definitions** 
*To see OpenAPI object go to https://swagger.io/specification/ Specification -> Schema 
1. **OpenAPI Description**
	- Formally describes the surface of the API with `info object` 
	- Must contain at least one path field, component field, or webhook field
	- Ideally include tags for multiple main paths to easily identify them within the document
2. **Schema 
	- A "schema" is a formal description of syntax and structure.
	-  Schemas are a good place to put fields in which are required/applicable for api calls 
3. 

#  Autogen Swagger Docs 
- Tools that for swagger-autogen typically rely on router frameworks as they look for things such as app.get, app.post, etc
- 



