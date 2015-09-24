## Upload Model Service - EeB BIM-API

* [Model Services Overview](./model_service.md)

Version: 0.5 2015.09.25 AET

There are four distinct functions for upload model:

* Upload new model - data in local file
* Upload new model - data in URL reference
* Upload new version of model - data in local file
* Upload new version of model - data in URL reference

The first one is a composite of two operations:

* Create model - requires meta-data
* Upload model data - requires model data "file"

This is a bit challenging to do with a single HTTP request using JSON with standard software tools, but "workarounds" are perfectly possible. The option will be described last of the four. 

Alternatively, a client may use [Create Model](./model_service_create) service to create the model, and then use upload new model version service to supply the data.

When model data is a link (URL reference), both operations are simply implemented, in this case it is no need to use a two-step create-upload sequence.

---
### Upload new model version - data in local file


**Resource URL: ** POST {path-to-service}/{version}/models/**model_id**


element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of EeB Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*model_id* | Model to upload new version for, must exist
multipart body	|file data 

Returns list containing single element {model_url, {[model meta data](./a_schemata/model_meta_data.md)}}. JSON Schema not shown (trivial).

It is possible, and sometimes necessary, to supply meta-data as URL parameters like **model_id**..?*description*="new version of model". 

-|element | explanation
-|--------|-----------|
-| *file_type* | ".ifc", ".xml", Necessary if model_type has more than one possible file type (NEW) 
-| *schema_url* | Informative only at time of writing
-| *desciption* | Informative only


** NOTE : **

* The access URL is the same as returned in **model_url** from LIST MODELS service.
* Since new model version gets new DI, URL to this model will change

Using a full URL like:

* POST {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/models/*model_id*

might also work, but the exact behaviour is not defined.

**Example: **

```
POST https://example.com/eeb/bim-api/0.5/models/8A4B23AA4BFA6610007EBB?description="Alternative 1 for the HVAC solution of Use Case 1"

** Model data in body of multi-part request **

Response:
[{
    "model_url ": "http://example.com/path-to-service/0.5/models/CFCA23AA59BEEE444FFFFF",
    "model_meta_data ":
    {
	"model_id": "CFCA23AA59BEEE444FFFFF",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"model_type": "IFC4",
	"model_name": "HVAC_alt_1",
	"model_type": "IFC4",
	"model_version": "V2",
	"description": "Alternative 1 for the HVAC solution of Use Case 1",
    }
}]
```

---

### Upload new model version - data in URL reference


**Resource URL: ** POST {path-to-service}/{version}/models

Request: JSON body according to [Upload model schema](./a_schemata/model_upload_schema.md)

-|element | explanation
-|--------|-----------|
-|*path-to-service*	|URL pointing to an instance of EeB Repository Services|
-|*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
-| *model_is_external*	|must be supplied and set to "*true*". If you do not, weird things may happen!
-|*model_id* | Model to upload new version for, must exist
-| *file_type* | ".ifc", ".xml", Necessary if model_type has more than one possible file type (NEW) 
(opt) | *schema_url* | Informative only at time of writing
(opt) | *desciption* | Informative only

Returns list containing single element {model_url, {[model meta data](./a_schemata/model_meta_data.md)}}. JSON Schema not shown (trivial).

** NOTE : **

* DO NOT supply model_id in URL. If you do, server may think you are doing a local file upload and that JSON body is a file.
* It is possible to supply updates for meta-data as URL parameters like **model_id**..?*description*="new version of model" 
* The access URL is the same as returned in **model_url** from LIST MODELS service.
* Since new model version gets new DI, URL to this model will change

Using a full URL like:

* POST {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/models/*model_id*

might also work, but the exact behaviour is not defined.

**Example: **

```
POST https://example.com/eeb/bim-api/0.5/models

Request:
{
    "model_meta_data ":
  	{
	"model_id": "/CFCA23AA59BEEE444FFFFF"
	"description": "Alternative 1 for the HVAC solution update",
	}
    "model_is_external": true,
    "model_content ": "https://example.com/yet-another-HVAC-model.ifc",
}

Response:
[{
    "model_url ": "http://example.com/path-to-service/0.5/models/8A4B23AA4BFA6610007EBB",
    "model_meta_data ":
    {
	"model_id": "8A4B23AA4BFA6610007EBB",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"model_type": "IFC4",
	"model_name": "HVAC_alt_1",
	"model_type": "IFC4",
	"model_version": "V2",
	"description": "Alternative 1 for the HVAC solution update",
    }
}]
```
---

### Upload new model  - data in URL Reference

**Resource URL: ** POST {path-to-service}/{version}/models

Assuming **model name locking** is applied, here is the URL parameters:

Request: JSON body according to [Upload model schema](./a_schemata/model_upload_schema.md)

e/o |element | explanation
--|--------|-----------|
-| *path-to-service*	| URL pointing to an instance of EeB Repository Services|
-| *version*		| States version of the API to use, allowing multiple versions of API for upgrading.
-| *model_is_external*	| must be supplied and set to "*true*". If you do not, weird things may happen!
-| *model_meta_data*	| See [Upload model schema](./a_schemata/model_upload_schema.md)
-|*model_content*	| URL to model data (model_is_external=true)


Returns list containing single element {model_url, {[model meta data](./a_schemata/model_meta_data.md)}}. JSON Schema not shown (trivial).

**NOTE :**

* If a model already exist inside a project with same name and domain, a new version is created,if not a new model is created.
* It is also possible to "identify" an existing model with the "path" using project_id and domain_id as indicated above. Same rule applies: if model name and domain name indicates an existing model, a new version is created. 
* Put another way - if you send exactly the same request multiple times, the first one will create a new model, the subsequent ones will create new versions.

**Example: **

```
POST https://example.com/eeb/bim-api/0.5/models

Request:
{
    "model_meta_data ":
  	{
	"project_id": "munchen-parkhaus",
	"model_name": "HVAC_alt_2",
	"model_type": "IFC4",
	"description": "Alternative 2 for the HVAC solution of Use Case 1",
	"domain_name": "HVAC",
	}
    "model_is_external": true,
    "model_content ": "https://example.com/yet-another-HVAC-model.ifc",
}

Response:
[{
    "model_url ": "http://example.com/path-to-service/0.5/models/CFCA23AA59BEEE444FFFFF",
    "model_meta_data ":
    {
	"model_id": "CFCA23AA59BEEE444FFFFF",
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

---

### Upload new model - data in local file

Due to both client side and server side challenges using HTTP for file upload, the operation is not as straight forward as you might expect. The main challenge is that neither client nor server side software libraries are too happy with multi-part requests where the request body itself consists of multiple parts.

However, for thin stateless clients like web pages the possibility to create and upload in one go is very valuable.The workaround is to supply metadata as URL parameters:

* POST {path-to-service}/{version}/models?*fieldname*=value&*fieldname*=value&,*fieldname*=value&...

It is possible to (partly) use URL itself, the two URLs below are equivalent:

* POST {path-to-service}/{version}/projects/*project_id*/domains/*domain_id*/models
* POST {path-to-service}/{version}/models?project_id="*project_id*&domain_id="*domain_id*"

Hence, we end up with alternatives:

**A1: Multipart request**

* **Resource URL**: POST {path-to-service}/{version}/models?
* Part 1: JSON body according to [Upload model schema](./a_schemata/model_upload_schema.md)
* Part 2: model content

**A2: No multipart request, parameters in URL**

* **Resource URL**: POST {path-to-service}/{version}/models?*fieldname*=value&*fieldname*=value&,*fieldname*=value&...
* URL Parameters according to [Upload model schema](./a_schemata/model_upload_schema.md) in URL
* model content as request body

Parameters are the same:

element | | explanation
--|--------|-----------|
*path-to-service*| | URL pointing to an instance of EeB Repository Services|
*version*	| | States version of the API to use, allowing multiple versions of API for upgrading.
*model_is_external*| | boolean, indicating if the model data is supplied as URL or attachment
*model_meta_data*| | See [Upload model schema](./a_schemata/model_upload_schema.md). In the case of parameters in the URL,the fieldnames are given directly WITHOUT any mentioning of *model_meta_data* 
*model_content*	|either | URL to model data (model_is_external=true)
            	|or | Attachment in multipart request: Input model data as "file"(model_is_external=false)


**NOTE :**

* Some model types have more than one possible filetype. For example, an IFC model can be represented in SPF (.ifc) or XML (ifcxml). In this case there are some options; we can look into the file, we can use **schema_url** or introduce extra parameters. At time of writing this is not decided.

* If a model already exist inside a project with same name and domain, a new version is created,if not a new model is created.
* It is also possible to "identify" an existing model with the "path" using project_id and domain_id as indicated above. Same rule applies: if model name and domain name indicates an existing model, a new version is created. 
* Put another way -if you send exactly the same request multiple times, the first one will create a new model, the subsequent ones will create new versions.

**Example: **

```
POST https://example.com/eeb/bim-api/0.5/models?project_id="munchen-parkhaus"
	&domain_name="HVAC"
	&model_name="HVAC_alt_1"
	&model_type="IFC4"
	&description="Alternative 1 for the HVAC solution of Use Case 1"

** Model data in body of multi-part request **

Response:
[{
    "model_url ": "http://example.com/path-to-service/0.5/models/CFCA23AA59BEEE444FFFFF",
    "model_meta_data ":
    {
	"model_id": "CFCA23AA59BEEE444FFFFF",
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
