## EeB Repository Services Schemata : Model Meta Data ##

* [Level Up](../README.md)
* [Overview](./README.md)

Version: 0.5 2015.09.25 AET

Here is the components so far:

## Model Meta Data

For each model the following attributes are used for model meta data, as members of a model meta data element:
 
 Attribute   | Type | Comment |
-------------|------|---------|
project_id   |String|Id for the project this model belongs to
project_name |String|Names the project this model belongs to
domain_id    |String|Id for the domain this model belongs to
domain_name |String|Name for the domain this model belongs to
model_guid   |String|Unique identifier of the model. The guid is generated on the server, and differs between versions
model_name   |String|Name of the model. Also used as "model id". Then name is same across versions
model_type   |String|Type of the model (e.g. IFC2x3, IFC4, XML, CSV, ifcXML, ….) 
model_version|String|Version of the model, usually server generated in the form of V21,V2,V3,...
schema_url   |String|URL to the model schema
model_treat_as_binary|Boolean|If present and true, the model content should/is NOT be parsed into database objects
model_mime_type    |String|MIME type for this model/feil when exported/downloaded 
description  |String|Human readable description of the model, informative only, no functional impact

The attributes are mandatory or optional depending on the service used, some general bullets:

* **model_treat_as_binary** is used to have the MODEL services act like pure FILE services. That is, if a file is uploaded to server the server might try to parse it into database object if possible, based on **model_type** field. If this process fails due to the contents of the file, an error return will be generated,and no storage will take place. If the **treat_as_binary** field is set to **true**, the server will just store the file and not try to look inside it at all. 

* **model_mime_type** is an optional field that can be used for setting the mime type in the HTTP headers of the content on download. If it is not set or blank, the MIME type in header is left to the server to decide, which in many cases may be the best.  


###XSD rep

###JSON rep:

```
{
"$schema": "http://json-schema.org/draft-04/schema#" 
	"title": "eeb_model_meta_data",
	"description": "Schema for model meta data, EeB REST API.",
	"type": "object",
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
				"model_version ": {"type": ["string","null"]},
				"schema_url": {"type": ["string","null"]},
				"description": {"type": ["string","null"]}
				},
			}
	}
}
```

