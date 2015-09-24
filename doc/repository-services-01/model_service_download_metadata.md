## Download Model Metadata - Eeb BIM-API Model Services

* [Model Services Overview](./model_service.md)

Version: 0.5 2015.09.25 AET

**Resource URLs** 

(1): *GET {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/models/**model_id**/metadata*

(2): *GET {path-to-service}/{version}/models/**model_id**/metadata*

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of EeB Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Identifies which project to look for model in 
*domain_id*	|Identifies which assiged domain to check for model 
*model_id*	| Identifies which model to download


** NOTE: **

As indicated, one or more of the URL elements can be omitted. If they are present, the server will check that the model really is placed where indicated. Whether this is a useful security measure or not is left to the API user to decide.


Response: List containing single element {model_url,{[model_meta_data](./a_schemata/model_meta_data.md)}} 

**Example: IFC data**

```
GET https://example.com/path-to-service/0.5/projects/ABCD/models/CFCA23AA59BEEE444222CC/metadata

Response:
[{
    "model_url ": "http://example.com/path-to-service/0.5/models/CFCA23AA59BEEE444222CC",
    "model_meta_data ":
    {
	"model_guid": "CFCA23AA59BEEE444222CC",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"model_type": "IFC4",
	"model_name": "HVAC_alt_1",
	"schema_url": "http://www.buildingsmart-tech.org/ifc/IFC4/Add1/IFC4_ADD1.exp",
	"model_version": "V1",
	"description": "Alternative 1 for the HVAC solution of Use Case 1",
    }
}]
```

**Example: binary data**

```
GET https://example.com/path-to-service/0.5/projects/ABCD/models/444222CCCFCA23AA59BEEE/metadata

Response:
[{
    "model_url ": "http://example.com/path-to-service/0.5/models/444222CCCFCA23AA59BEEE",
    "model_meta_data ":
    {
	"model_guid": "CFCA23AA59BEEE444222CC",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "abba",
	"domain_name": "LCC",
	"model_type": "iTwo",
	"model_name": "HVAC_alt_1_lcc_result",
	"schema_url": "",
	"model_treat_as_binary": "true",
	"model_version": "V1",
	"model_mime_type": "application/itwo",
	"description": "LCC model Alternative 1 for the HVAC solution of Use Case 1",
    }
}]
```

