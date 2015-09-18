## 7FP Repository Services Schemata : File Meta Data ##

* [Level Up](../README.md)
* [Overview](./README.md)

Version: 0.5 2015.09.18 AET

Here is the components so far:

## File Meta Data

For each file the following attributes are used for meta data, as members of a file meta data element:
 
 Attribute   | Type | Comment |
-------------|------|---------|
project_id   |String|Id for the project this model belongs to
project_name |String|Names the project this model belongs to
domain_id    |String|Id for the domain this model belongs to
domain_name |String|Name for the domain this model belongs to
file_guid   |String|Unique identifier of the model. The guid is generated on the server, and differs between versions
file_name   |String|Name of the model. Also used as "model id". Then name is same across versions
file_type   |String|Type of the file as free text (e.g. xlsx, xml, ifcXML, ….) 
file_version|String|Version of the model, usually server generated in the form of V21,V2,V3,...
mime_type   |String| MIME Type of the file (optional)
description  |String|Human readable description of the file, informative only, no functional impact

The attributes are mandatory or optional depending on the service used.


###XSD rep

###JSON rep:

```
{
"$schema": "http://json-schema.org/draft-04/schema#" 
	"title": "7fp_file_meta_data",
	"description": "Schema for file meta data, 7FP REST API.",
	"type": "object",
			"properties": {
				"project_id": {"type": ["string","null"]},
				"project_name": {"type": ["string","null"]},
				"domain_id ": {"type": ["string","null"]
				"domain_name ": {"type": ["string","null"]
				"file_guid ": {"type": ["string","null"]},
				"file_name": {"type": ["string","null"]},
				"file_type ": {"type": ["string","null"]},
				"mime_type ": {"type": ["string","null"]},
				"file_version ": {"type": ["string","null"]},
				"description": {"type": ["string","null"]}
				},
			}
	}
}
```

