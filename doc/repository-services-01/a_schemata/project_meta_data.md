## EeB BIM-API Repository Services : Project Meta Data ##

* [Level Up](../README.md)
* [Overview](./README.md)
* [Project Services](../project_service.md)

Version: 0.5 2015.09.25 AET


## Project Meta Data

For each project the following attributes are used for project meta data, as members of a project meta data element:
 
 Attribute   | Type | Comment |
-------------|------|---------|
project_id   |String|Id for the project 
project_name |String|Names the project 
description  |String|Human readable description of the project, informative only, no functional impact

The attributes are mandatory or optional depending on the service used.


###XSD rep

###JSON rep:

```
{
"$schema": "http://json-schema.org/draft-04/schema#" 
	"title": "eeb_multimodel_meta_data",
	"description": "Schema for project meta data, EeB REST API.",
	"type": "object",
			"properties": {
				"project_id": {"type": ["string","null"]},
				"project_name": {"type": ["string","null"]},
				"description": {"type": ["string","null"]}
				},
			}
	}
}
```

