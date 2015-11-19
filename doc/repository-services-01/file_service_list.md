## List Files -- eeB BIM-API File Services

**Retrieve a collection of File descriptions where the currently logged on user has read access.**

* [File Services Overview](./file_service.md)

Version: 0.5 2015.11.17 AET


**Resource URLs** 

(1): *GET {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/files*

(2): *GET {path-to-service}/{version}/files*

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of EeB Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Identifies which project to look for file in
*domain_id*	|Identifies which assigned domain to check for file 


** NOTE: **

One or more of the *xxx/xxx_id* elements can be omitted, effectively to be used as filters. See [Repository Services Overview](./README.md) for more examples.

Response: List containing zero or more elements {file_url,{[file_meta_data](./a_schemata/file_meta_data.md)}} 

**Example:**

This example shows two versions of one file and one version of another in project with id "ABCD"

```
GET https://example.com/path-to-service/0.5/projects/ABCD/Files

Response:
[{
    "file_url ": "http://example.com/path-to-service/0.5/Files/CEA23AA59BEEE444222CC",
    "file_meta_data ":
    {
        "file_guid": "CECA23AA59BEEE444222CC",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"file_type": "XLSX",
	"file_name": "HVAC_alt_1_materials.xlsx",
	"schema_url": "http://example.com/office/excel/xlsx/2013",
	"file_version": "V1",
	"description": "Material list alt 1 for the HVAC solution of Use Case 1",
    }
},
{
    "file_url ": "http://example.com/path-to-service/0.5/Files/AEFE23AA11BCFF444122BB",
    "File_meta_data ":
    {
        "file_guid": "AEFE23AA11BCFF444122BB",
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fdfd",
	"domain_name": "HVAC",
	"file_type": "XLSX",
	"file_name": "HVAC_alt_2_materials.xlsx",
	"schema_url": "http://example.com/office/excel/xlsx/2013",
	"file_version": "V1",
	"description": "Material list alt 2 for the HVAC solution of Use Case 1",
    }
}]
```
