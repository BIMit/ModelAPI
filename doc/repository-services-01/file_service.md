## Energy-efficient Buildings BIM-API File Services ##

[Level Up](../README.md)
[Overview](./README.md)

Version: 0.5 2015.11.19 AET


### File Services

Files: to store and retrieve files, use the [Model Services](model_service.md) but with the "**model_treat_as_binary**" flag set to true, see [file_meta_data](./a_schemata/file_meta_data.md)

Excerpt:

* **model_treat_as_binary** is used to have the MODEL services act like pure FILE services. That is, if a file is uploaded to server the server might try to parse it into database object if possible, based on **model_type** field. If this process fails due to the contents of the file, an error return will be generated,and no storage will take place. If the **treat_as_binary** field is set to **true**, the server will just store the file and not try to look inside it at all. 

###Example: list files

Example is using filtering on metadata field as part of URL. We could also use JSON body.

**Resource URLs** 

(1): *GET {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/models?model_treat_as_binary=**true***

(2): *GET {path-to-service}/{version}/models?model_treat_as_binary=**true***

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of EeB Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Identifies which project to look for model in
*domain_id*	|Identifies which assiged domain to check for model 
*model_treat_as_binary*	|If set to true, only return entriees with this attribute *model_treat_as_binary*= **true** 


** NOTE: **

One or more of the *xxx/xxx_id* elements can be omitted, effectively to be used as filters. See [Repository Services Overview](./README.md) for more examples.

Response: List containing zero or more elements {model_url,{[model_meta_data](./a_schemata/model_meta_data.md)}} 

**Example:**

This example shows two variants of model in project with id "ABCD"

```
GET https://example.com/path-to-service/0.5/projects/ABCD/models

Response:
[{
    "model_url ": "http://example.com/path-to-service/0.5/models/CECA23AA59BEEE444222CC",
    "model_meta_data ":
    {
        "model_guid": "CECA23AA59BEEE444222CC",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"model_type": "XLSX",
	"model_name": "HVAC_alt_1_materials",
	"schema_url": "http://example.com/office/excel/2013/xlsx.xsd,
	"model_treat_as_binary": "true",
	"model_version": "V1",
	"description": "MAterials alt1 for the HVAC solution of Use Case 1",
    }
},
{
    "model_url ": "http://example.com/path-to-service/0.5/models/ADFE23AA11BCFF444122BB",
    "model_meta_data ":
    {
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"model_type": "XLSX",
	"model_name": "HVAC_alt_2_materials",
	"schema_url": "http://example.com/office/excel/2013/xlsx.xsd,
	"model_treat_as_binary": "true",
	"model_version": "V1",
	"description": "Materials alt 2 for the HVAC solution of Use Case 1",
    }
}]
```

