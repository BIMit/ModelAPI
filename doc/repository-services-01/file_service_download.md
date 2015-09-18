## Download File - 7FP BIM-API File Services

* [File Services Overview](./file_service.md)

Version: 0.5 2015.09.18 AET

**Resource URLs** 

(1): *GET {path-to-service}/{version}/projects/**project_id**/domains/**domain_id**/files/**file_id***

(2): *GET {path-to-service}/{version}/files/**file_id***

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of 7FP Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Identifies which project to look for file in 
*domain_id*	|Identifies which assiged domain to check for file 
*file_id*	| Identifies which file to download


** NOTE: **

As indicated, one or more of the URL elements can be omitted. If they are present, the server will check that the file really is placed where indicated. Whether this is a useful security measure or not is left to the API user to decide.

Response: Binary stream	with file data from target server 


JSON Schema not available (trivial)

##REST/JSON Example

This example uses default repository by not supplying one

```
GET https://example.com/7fp/bim-api/0.5/files/ABCD2233

Response: file data as “file”
```
