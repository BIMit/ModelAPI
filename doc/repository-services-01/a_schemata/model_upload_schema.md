## EeB BIM-API Upload Model Schema ##

* [Level Up](../README.md)
* [Overview](./README.md)
* [Create Model Service](../model_service_create.md)
* [Upload Model Service](../model_service_upload.md)

Version: 0.5 2015.09.25 AET

The schema for [model-meta-data](./model_meta_data.md) is same for response and request, but the rules for which fields that are supplied are different. The schemata in this page is derived from the "ORIGINAL" meta data schema. The rules/usage applies to CREATE and UPLOAD when a new model is implicitly created.

###Parameters

Parameters may be supplied in the URL or in request body. In the table below the "default" usage is depicted in the first column as U(rl) or B(ody)

U/B|Optional |element | explanation
--|--|--------|-----------|
U | - |either *project_id*	|Project to create model in, must exist
U | - |or  *project_name*	|Project to create model in, must exist
U | - |either *domain_name*	|Domain to assign model to. If ***domain auto create*** is enabled, the domain is created on the fly if it does not exist 
U | - | or *domain_id*	|Domain to assign model to, must exist. 
B | - | *model_name* | Name of model 
B | - | *model_type* | "IFC2x3", "IFC4", "...." 
B |(opt)| *schema_url* | Informative only at time of writing
B |(opt)| *model_treat_as_binary* | Boolean - if present and true, the model content should NOT be parsed into database objects
B |(opt)| *model_mime_type* | Enforce MIME type for this model - usually not used
B |(opt)| *description* | Human readable description of this model

 

###JSON Schema
```
{
"$schema": "http://json-schema.org/draft-03/schema#" 
	"title": "eeb_model_POST",
	"description": "Schema for upload template POST, EeB REST API.",
	"type": "object",
	"properties": {
		"model_meta_data ": {
			"title": " model_meta_data ",
			"type": "object"
			"properties": {
				"project_id": {"type": ["string","null"]},
				"project_name": {"type": ["string","null"]},
				"domain_id ": {"type": ["string","null"]
				"domain_name ": {"type": ["string","null"]
				"model_guid ": {"type": ["string","null"]},
				"model_mime_type": {"type": ["string","null"]},
				"model_name": {"type": ["string","null"]},
				"model_treat_as_binary": {"type": ["boolean","null"]},
				"model_type ": {"type": ["string","null"]},
				"schema_url": {"type": ["string","null"]},
				"description": {"type": ["string","null"]}
				},		},
		"model_is_external": {
			"required": true,
			"type": "boolean"
		},
		"model_content ": {
		  "type": ["string","null"]
		}
	}
}
```