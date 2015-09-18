## Create File Service - 7FP BIM API

** Create an empty file in server repository **

* [File Services Overview](./file_service.md)

Version: 0.5 2015.09.18 AET

To forms of resource URLs can be utilized:

* (1)POST {path-to-service}/{version}/files

All meta-data in HTTP multipart request.Enables auto-creation of domain.

* (2)POST {path-to-service}/{version}/projects/*project_id*/domains/*domain_id*/files

Project and domain is now given in URL. Additional meta-data in HTTP multipart request. Can only be used if domain already exist.

Combinations of the above are possible. Both are described on this page.



### Create new file (1)

**Resource URL**: POST {path-to-service}/{version}/files

Request: JSON body according to [file meta data](./a_schemata/file_meta_data.md):

e/o |element | explanation
--|--------|-----------|
-| *path-to-service*	|URL pointing to an instance of 7FP Repository Services|
-| *version*	|States version of the API to use, allowing multiple versions of API for upgrading.
either | *project_id*	|Project to create file in, must exist
or | *project_name*	|Project to create file in, must exist and be unique on  server
either | *domain_name*	|Domain to assign file to. If ***domain auto create*** is enabled, the domain is created on the fly if it does not exist 
or |*domain_id*	|Domain to assign file to, must exist. 
 - | *file_name* | Name of file 
 - | *file_type* | "xlsx", "xml", "...." 
 - | *mime_type* | The MIME type of the file (optional, if not givem XXX is assumed) 
 - | *desciption* | Informative only

Returns list containing single element {file_url, {[file meta data](./a_schemata/file_meta_data.md)}}. JSON Schema not shown (trivial).


**NOTE :**

* If a file already exist inside a project  with same name and domain, no file is created, and an error return is sent.

**Example:**

Domain "LCC" and file "Cost file" assumed non existing => auto-created

```
POST https://example.com/7fp/bim-api/0.5/files
Request:
{
	"project_name": "munchen-parkhaus",
	"domain_name": "LCC",
	"file_name": "Cost file",
	"file_type": "XLSX",
	"mime_type": "application/bin", <TODO>
	"description": "Cost file Alternative 1 for the HVAC",
}

Response:
[{
    "file_url ": "http://example.com/7fp/bim-api/0.5/files/CFCA23AA59BEEE444FEEEE",
    "file_meta_data ":
    {
	"file_guid": "CFCA23AA59BEEE444FEEEE",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "cdab",
	"domain_name": "LCC",
	"file_name": "Cost file",
	"file_type": "XLSX",
	"mime_type": "application/bin", <TODO>
	"file_version": "V1",
	"description": "Cost file Alternative 1 for the HVAC",
    }
}]
```

### Create new file (2)

**Resource URL**: POST {path-to-service}/{version}/projects/*project_id*/domains/*domain_id*/files

Request: JSON body according to [file meta data](./a_schemata/file_meta_data.md):

e/o |element | explanation
--|--------|-----------|
-| *path-to-service*	|URL pointing to an instance of 7FP Repository Services|
-| *version*	|States version of the API to use, allowing multiple versions of API for upgrading.
-| *project_id*	|Project to create file in, must exist
-|*domain_id*	|Domain to assign file to, must exist.
 - | *file_name* | Name of file  
 - | *file_type* | "IFC2x3", "IFC4", "...." 
 - | *mime_type* | The MIME type of the file (optional, if not givem XXX is assumed) 
 - | *desciption* | Informative only

Returns list containing single element {file_url, {[file meta data](./a_schemata/file_meta_data.md)}}. JSON Schema not shown (trivial).


**NOTE :**

* If a file already exist inside a project with same name and domain, no file is created, and an error return is sent.

**Example:**

```
POST https://example.com/7fp/bim-api/0.5/projects/ABCD/domains/fdfd/files
Request:
{
	"file_name": "Cost file",
	"file_type": "XLSX",
	"mime_type": "application/bin", <TODO>
	"description": "Cost file Alternative 1 for the HVAC",
}

Response:
[{
    "file_url ": "http://example.com/7fp/bim-api/0.5/files/CFCA23AA59BEEE444FFFFF",
    "file_meta_data ":
    {
	"file_guid": "CFCA23AA59BEEE444FEEEE",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "cdab",
	"domain_name": "LCC",
	"file_name": "Cost file",
	"file_type": "XLSX",
	"mime_type": "application/bin", <TODO>
	"file_version": "V1",
	"description": "Cost file Alternative 1 for the HVAC",
}]
```
