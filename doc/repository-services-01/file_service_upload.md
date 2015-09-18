## Upload File Service - 7FP BIM-API

* [File Services Overview](./file_service.md)

Version: 0.5 2015.09.18 AET

There are four distinct functions for upload file:

* Upload new file - data in local file
* Upload new file - data in URL reference
* Upload new version of file - data in local file
* Upload new version of file - data in URL reference

The first one is a composite of two operations:

* Create file - requires meta-data
* Upload file data - requires file data "file"

This is a bit challenging to do with a single HTTP request using JSON with standard software tools, but "workarounds" are perfectly possible. The option will be described last of the four. 

Alternatively, a client may use [Create file](./file_service_create) service to create the file, and then use upload new file version service to supply the data.

When file data is a link (URL reference), both operations are simply implemented, in this case it is no need to use a two-step create-upload sequence.

---
### Upload new file version - data in local file


**Resource URL: ** POST {path-to-service}/{version}/files/**file_id**


element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of 7FP Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*file_id* | file to upload new version for, must exist
multipart body	|file data 

Returns list containing single element {file_url, {[file meta data](./a_schemata/file_meta_data.md)}}. JSON Schema not shown (trivial).

It is possible, and sometimes necessary, to supply meta-data as URL parameters like **file_id**..?*description*="new version of file". 

-|element | explanation
-|--------|-----------|
-| *file_type* | ".ifc", ".xml", Necessary if file_type has more than one possible file type (NEW) 
- | *mime_type* | The MIME type of the file (optional, if not givem XXX is assumed) 
-| *desciption* | Informative only


** NOTE : **

* The access URL is the same as returned in **file_url** from LIST fileS service.
* Since new file version gets new DI, URL to this file will change

Using a full URL like:

* POST {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/files/*file_id*

might also work, but the exact behaviour is not defined.

**Example: **

```
POST https://example.com/7fp/bim-api/0.4/files/8A4B23AA4BFA6610007EBB?description="Alternative 1 for the HVAC solution of Use Case 1"

** file data in body of multi-part request **

Response:
[{
    "file_url ": "http://example.com/path-to-service/0.3/files/CFCA23AA59BEEE444FFFFF",
    "file_meta_data ":
    {
	"file_id": "CFCA23AA59BEEE444FFFFF",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"file_type": "IFC4",
	"file_name": "HVAC_alt_1",
	"file_type": "IFC4",
	"file_version": "V2",
	"mime_type": "application/bin", <TODO>
	"description": "Alternative 1 for the HVAC solution of Use Case 1",
    }
}]
```

---

### Upload new file version - data in URL reference


**Resource URL: ** POST {path-to-service}/{version}/files

Request: JSON body according to [Upload file schema](./a_schemata/file_upload_schema.md)

-|element | explanation
-|--------|-----------|
-|*path-to-service*	|URL pointing to an instance of 7FP Repository Services|
-|*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
-| *file_is_external*	|must be supplied and set to "*true*". If you do not, weird things may happen!
-|*file_id* | file to upload new version for, must exist
-| *file_type* | ".ifc", ".xml", Necessary if file_type has more than one possible file type (NEW) 
 - | *mime_type* | The MIME type of the file (optional, if not givem XXX is assumed) 
(opt) | *desciption* | Informative only

Returns list containing single element {file_url, {[file meta data](./a_schemata/file_meta_data.md)}}. JSON Schema not shown (trivial).

** NOTE : **

* DO NOT supply file_id in URL. If you do, server may think you are doing a local file upload and that JSON body is a file.
* It is possible to supply updates for meta-data as URL parameters like **file_id**..?*description*="new version of file" 
* The access URL is the same as returned in **file_url** from LIST fileS service.
* Since new file version gets new DI, URL to this file will change

Using a full URL like:

* POST {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/files/*file_id*

might also work, but the exact behaviour is not defined.

**Example: **

```
POST https://example.com/7fp/bim-api/0.4/files

Request:
{
    "file_meta_data ":
  	{
	"file_id": "/CFCA23AA59BEEE444FFFFF"
	"description": "Alternative 1 for the HVAC solution update",
	}
    "file_is_external": true,
    "file_content ": "https://example.com/yet-another-HVAC-file.ifc",
}

Response:
[{
    "file_url ": "http://example.com/path-to-service/0.3/files/8A4B23AA4BFA6610007EBB",
    "file_meta_data ":
    {
	"file_id": "8A4B23AA4BFA6610007EBB",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"file_type": "IFC4",
	"file_name": "HVAC_alt_1",
	"file_type": "IFC4",
	"file_version": "V2",
	"mime_type": "application/bin", <TODO>
	"description": "Alternative 1 for the HVAC solution update",
    }
}]
```
---

### Upload new file  - data in URL Reference

**Resource URL: ** POST {path-to-service}/{version}/files

Assuming **file name locking** is applied, here is the URL parameters:

Request: JSON body according to [Upload file schema](./a_schemata/file_upload_schema.md)

e/o |element | explanation
--|--------|-----------|
-| *path-to-service*	|URL pointing to an instance of 7FP Repository Services|
-| *version*	|States version of the API to use, allowing multiple versions of API for upgrading.
-| *file_is_external*	|must be supplied and set to "*true*". If you do not, weird things may happen!
either | *project_id*	|Project to create file in, must exist
or | *project_name*	|Project to create file in, must exist
either | *domain_name*	|Domain to assign file to. If ***domain auto create*** is enabled, the domain is created on the fly if it does not exist 
or |*domain_id*	|Domain to assign file to, must exist. 
 - | *file_name* | Name of file 
 - | *file_type* | "IFC2x3", "IFC4", "...." 
(opt)| *schema_url* | Informative only at time of writing
(opt)| *desciption* | Informative only
- |*file_content*		|URL to file data (file_is_external=true)


Returns list containing single element {file_url, {[file meta data](./a_schemata/file_meta_data.md)}}. JSON Schema not shown (trivial).

**NOTE :**

* If a file already exist inside a project with same name and domain, a new version is created,if not a new file is created.
* It is also possible to "identify" an existing file with the "path" using project_id and domain_id as indicated above. Same rule applies: if file name and domain name indicates an existing file, a new version is created. 
* Put another way -if you send exactly the same request multiple times, the first one will create a new file, the subsequent ones will create new versions.

**Example: **

```
POST https://example.com/7fp/bim-api/0.4/files

Request:
{
    "file_meta_data ":
  	{
	"project_id": "munchen-parkhaus",
	"file_name": "HVAC_alt_2",
	"file_type": "IFC4",
	"description": "Alternative 2 for the HVAC solution of Use Case 1",
	"domain_name": "HVAC",
	}
    "file_is_external": true,
    "file_content ": "https://example.com/yet-another-HVAC-file.ifc",
}

Response:
[{
    "file_url ": "http://example.com/path-to-service/0.3/files/CFCA23AA59BEEE444FFFFF",
    "file_meta_data ":
    {
	"file_id": "CFCA23AA59BEEE444FFFFF",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"file_type": "IFC4",
	"file_name": "HVAC_alt_1",
	"file_type": "IFC4",
	"file_version": "V1",
	"mime_type": "application/bin", <TODO>
	"description": "Alternative 1 for the HVAC solution of Use Case 1",
    }
}]
```

---

### Upload new file - data in local file

Due to both client side and server side challenges using HTTP for file upload, the operation is not as straight forward as you might expect. The main challenge is that neither client nor server side software libraries are too happy with multi-part requests where the request body itself consists of multiple parts.

However, for thin stateless clients like web pages the possibility to create and upload in one go is very valuable.The workaround is to supply metadata as URL parameters:

* POST {path-to-service}/{version}/files?*fieldname*=value&*fieldname*=value&,*fieldname*=value&...

It is possible to (partly) use URL itself, the two URLs below are equivalent:

* POST {path-to-service}/{version}/projects/*project_id*/domains/*domain_id*/files
* POST {path-to-service}/{version}/files?project_id="*project_id*&domain_id="*domain_id*"


**Resource URL**: POST {path-to-service}/{version}/files?*fieldname*=value&*fieldname*=value&,*fieldname*=value&...

Assuming **file name locking** is applied, here is the URL parameters:

e/o |element | explanation
--|--------|-----------|
-| *path-to-service*	|URL pointing to an instance of 7FP Repository Services|
-| *version*	|States version of the API to use, allowing multiple versions of API for upgrading.
either | *project_id*	|Project to create file in, must exist
or | *project_name*	|Project to create file in, must exist
either | *domain_name*	|Domain to assign file to. If ***domain auto create*** is enabled, the domain is created on the fly if it does not exist 
or |*domain_id*	|Domain to assign file to, must exist. 
-  | *file_name* | Name of file 
 - | *file_type* | "IFC2x3", "IFC4", "...." 
(opt)| *file_type* | ".ifc", ".xml", Necessary if file_type has more than one possible file type (NEW) 
 - | *mime_type* | The MIME type of the file (optional, if not givem XXX is assumed) 
(opt)| *description* | Informative only
-| - |
- | *file_is_external*	|boolean, indicating if the file data is supplied as URL or attachment
either|*file_content*		|URL to file data (file_is_external=true)
or  |*file_content*		|Attachment in multipart request: Input file data as "file"(file_is_external=false)



**NOTE :**

* If a file already exist inside a project with same name and domain, a new version is created,if not a new file is created.
* It is also possible to "identify" an existing file with the "path" using project_id and domain_id as indicated above. Same rule applies: if file name and domain name indicates an existing file, a new version is created. 
* Put another way -if you send exactly the same request multiple times, the first one will create a new file, the subsequent ones will create new versions.

**Example: **

```
POST https://example.com/7fp/bim-api/0.4/files?project_id="munchen-parkhaus"
	&domain_name="HVAC"
	&file_name="HVAC_alt_1"
	&file_type="IFC4"
	&description="Alternative 1 for the HVAC solution of Use Case 1"

** file data in body of multi-part request **

Response:
[{
    "file_url ": "http://example.com/path-to-service/0.3/files/CFCA23AA59BEEE444FFFFF",
    "file_meta_data ":
    {
	"file_id": "CFCA23AA59BEEE444FFFFF",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"file_type": "IFC4",
	"file_name": "HVAC_alt_1",
	"file_type": "IFC4",
	"file_version": "V1",
	"mime_type": "application/bin", <TODO>
	"description": "Alternative 1 for the HVAC solution of Use Case 1",
    }
}]
```
