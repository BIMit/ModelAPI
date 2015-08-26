## Create Model Service - eeE BIMAPI

** Create an empty model in server repository **

* [Model Services Overview](./model_service.md)

Version: 0.4 2015.08.25 AET

To forms of resource URLs can be utilized:

* (1)POST {path-to-service}/{version}/models

All meta-data in HTTP multipart request.Enables auto-creation of domain.

* (2)POST {path-to-service}/{version}/projects/*project_id*/domains/*domain_id*/models

Project and domain is now given in URL. Additional meta-data in HTTP multipart request. Can only be used if domain already exist.

Combinations of the above are possible. Both are described on this page.



### Create new model (1)

**Resource URL**: POST {path-to-service}/{version}/models

Request: JSON body according to [model meta data](./a_schemata/model_meta_data.md):

e/o |element | explanation
--|--------|-----------|
-| *path-to-service*	|URL pointing to an instance of eeEmbedded Repository Services|
-| *version*	|States version of the API to use, allowing multiple versions of API for upgrading.
either | *project_id*	|Project to create model in, must exist
or | *project_name*	|Project to create model in, must exist and be unique on  server
either | *domain_name*	|Domain to assign model to. If ***domain auto create*** is enabled, the domain is created on the fly if it does not exist 
or |*domain_id*	|Domain to assign model to, must exist. 
 - | *model_name* | Name of model 
 - | *model_type* | "IFC2x3", "IFC4", "...." 
 - | *schema_url* | Informative only at time of writing
 - | *desciption* | Informative only

Returns list containing single element {model_url, {[model meta data](./a_schemata/model_meta_data.md)}}. JSON Schema not shown (trivial).


**NOTE :**

* If a model already exist inside a project  with same name and domain, no model is created, and an error return is sent.

**Example:**

Domain "LCC" and model "Cost model" assumed non existing => auto-created

```
POST https://example.com/eee/bim-api/0.4/models
Request:
{
	"project_name": "munchen-parkhaus",
	"domain_name": "LCC",
	"model_name": "Cost model",
	"model_type": "XLSX",
	"description": "Cost model Alternative 1 for the HVAC",
}

Response:
[{
    "model_url ": "http://example.com/eee/bim-api/0.4/models/CFCA23AA59BEEE444FEEEE",
    "model_meta_data ":
    {
	"model_guid": "CFCA23AA59BEEE444FEEEE",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "cdab",
	"domain_name": "LCC",
	"model_name": "Cost model",
	"model_type": "XLSX",
	"model_version": "V1",
	"description": "Cost model Alternative 1 for the HVAC",
    }
}]
```

### Create new model (2)

**Resource URL**: POST {path-to-service}/{version}/projects/*project_id*/domains/*domain_id*/models

Request: JSON body according to [model meta data](./a_schemata/model_meta_data.md):

e/o |element | explanation
--|--------|-----------|
-| *path-to-service*	|URL pointing to an instance of eeEmbedded Repository Services|
-| *version*	|States version of the API to use, allowing multiple versions of API for upgrading.
-| *project_id*	|Project to create model in, must exist
-|*domain_id*	|Domain to assign model to, must exist. 
 - | *model_type* | "IFC2x3", "IFC4", "...." 
 - | *schema_url* | Informative only at time of writing
 - | *desciption* | Informative only

Returns list containing single element {model_url, {[model meta data](./a_schemata/model_meta_data.md)}}. JSON Schema not shown (trivial).


**NOTE :**

* If a model already exist inside a project with same name and domain, no model is created, and an error return is sent.

**Example:**

```
POST https://example.com/eee/bim-api/0.4/projects/ABCD/domains/fdfd/models
Request:
{
	"model_type": "IFC4",
	"description": "Alternative 1 for the HVAC solution of Use Case 1",
}

Response:
[{
    "model_url ": "http://example.com/eee/bim-api/0.4/models/CFCA23AA59BEEE444FFFFF",
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
}]
```
