## Delete File - 7FP BIM-API File Services

* [File Services Overview](./file_service.md)

Version: 0.5 2015.09.18 AET

**Resource URLs** 

(1): *DELETE {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/files/**file_id***

(2): *DELETE {path-to-service}/{version}/files/**file_id***

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of 7FP Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Identifies which project to look for file in 
*domain_id*	|Identifies which assiged domain to check for file 
*file_id**	| Identifies which file to delete

** NOTE: **

As indicated, one or more of the URL elements can be omitted. If they are present, the server will check that the file really is placed where indicated. Whether this is a useful security measure or not is left to the API user to decide.

Response: List containing single element {file_url,{[file_meta_data](./a_schemata/file_meta_data.md)}} for the deleted file

**Example :**



```
DELETE https://example.com/fp7/bim-api/0.5/files/ABCD2233

Response:
[{
    "file_url" : "https://example.com/7fp/bim-api/0.5/files/ABCD2233",
    "file_meta_data ":
    {
	"project_id": "DABB",
	"project_name": "oslo-office",
	"domain_id": "fcfc",
	"domain_name": "HVAC",
	"file_guid": "ABCD2233",
	"file_name": "HVAC_alt2_attributes",
	"file_type": "XLS",
	"file_version": "V33",
	"mime_type": "application/bin", <TODO>
	"description": "Alternative 2 data HVAC solution of Use Case 1",
 }]
```
