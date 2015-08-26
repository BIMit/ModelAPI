## List Models -- eeE BIM-API Model Services

**Retrieve a collection of model descriptions where the currently logged on user has read access.**

* [Model Services Overview](./model_service.md)

Version: 0.4 2015.08.25 AET


**Resource URLs** 

(1): *GET {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/models*

(2): *GET {path-to-service}/{version}/models*

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of eeEmbedded Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Identifies which project to look for model in
*domain_id*	|Identifies which assiged domain to check for model 


** NOTE: **

One or more of the *xxx/xxx_id* elements can be omitted, effectively to be used as filters. See [Repository Services Overview](./README.md) for more examples.

Response: List containing zero or more elements {model_url,{[model_meta_data](./a_schemata/model_meta_data.md)}} 

**Example:**

This example shows two versions of one model and one version of another in project with id "ABCD"

```
GET https://example.com/path-to-service/0.3/projects/ABCD/models

Response:
[{
    "model_url ": "http://example.com/path-to-service/0.3/models/CFCA23AA59BEEE444222CC",
    "model_meta_data ":
    {
        "model_guid": "CFCA23AA59BEEE444222CC",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"model_type": "IFC4",
	"model_name": "HVAC_alt_1",
	"model_type": "IFC4",
	"model_version": "V1",
	"description": "Alternative 1 for the HVAC solution of Use Case 1",
    }
},
{
    "model_url ": "http://example.com/path-to-service/0.3/models/CFCA23AA59BEEE444FFFFF",
    "model_meta_data ":
    {
        "model_guid": "CFCA23AA59BEEE444FFFFF",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"model_type": "IFC4",
	"model_name": "HVAC_alt_1",
	"model_type": "IFC4",
	"model_version": "V1",
	"description": "Alternative 1 for the HVAC solution of Use Case 1",
    }
},
{
    "model_url ": "http://example.com/path-to-service/0.3/models/ADFE23AA11BCFF444122BB",
    "model_meta_data ":
    {
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"model_type": "IFC4",
	"model_name": "HVAC_alt_2",
	"model_type": "IFC4",
	"model_version": "V1",
	"description": "Alternative 2 for the HVAC solution of Use Case 1",
    }
}]
```
