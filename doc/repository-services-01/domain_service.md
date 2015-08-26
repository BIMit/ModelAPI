# eeE BIM-API Domain Services #

* [Level Up](../README.md)
* [Overview](./README.md)

Version: 0.4 2015.08.25 AET

**Important :**

* If server is set up for it domains are created on the fly when uploading. This has proven to be an effective concept for many usages. Example: two architects creates models for separate parts of building. Then domain can be set to **Arch-1** and **Arch-2** respectively at file upload. See [Model Services](./model_service.md) for more (later). 



## List Domains

**Resource URL (1) **: *GET {path-to-service}/{version}/projects/project-id/domains*


element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of eeEmbedded Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.|
*project_id*	|Project to list domains for. If skipped, all domains in server will be listed.

Returns list of {domain_url, {domain_meta_data}}. JSON Schema not shown (trivial). If server does not support direct concept of domain, *domain_url* is not returned.

**Example (project level):**

```
GET https://example.com/eee/bim-api/0.4/projects/ABCD/domains


Response:
[{
    "domain_url ": "http://example.com/eee/bim-api/0.4/projects/ABCD/domains/EFFE",
    "domain_meta_data ":
    {
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fafa",
	"domain_name": "Arch",
    	"description": "Architectural domain",
    }
},
{
    "domain_url ": "http://example.com/eee/bim-api/0.4/projects/ABCD/domains/EFFE",
    "domain_meta_data ":
    {
	"project_id": "ABCD",
	"project_name": "munchen-parkhaus",
	"domain_id": "fbfb",
	"domain_name": "HVAC",
    	"description": "HVAC domain",
    }
}]
```


## Retrieve Domain
**Resource URL**: *GET {path-to-service}/{version}/projects/project-id/domains/EFFE*

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of eeEmbedded Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Project to look for domain in. If skipped, entire server is searched for matching domain

Returns list containing single element {domain_url, {domain_meta_data}}. JSON Schema not shown (trivial). If domjain not found, return is empty list *[ ]*.

**Example:**

```
GET https://example.com/eee/bim-api/0.4/projects/ABCD/domains/fbfb

Response:
[]
```

Here server indicates that domain with id ***fbfb*** (HVAC above) is not used in project.


## Create Domain
**Resource URL**: *POST {path-to-service}/{version}/projects/project_id/domains*

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of eeEmbedded Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Project to create domain in. 
JSON body	|[domain_meta_data](./schemata/domain_meta_data.md) for the domain to create. 

Returns list containing single element {domain_url, {domain_meta_data}}. JSON Schema not shown (trivial). 

**Example:**

```
POST https://example.com/eee/bim-api/0.4/projects/DABB/domains
Request:
{
	"domain_name": "BCS",
	"description": "Control system domain for eeEmbedded Oslo-office project",
}

Response:
[{
    "project_url ": "http://example.com/eee/bim-api/0.4/projects/DABB",
    "project_meta_data ":
    {
	"project_id": "DABB",
	"project_name": "oslo-office",
	"domain_id": "fcfc",
	"domain_name": "BCS",
	"description": "Control system domain for eeEmbedded Oslo-office project",
    }
}]
```

## Update Domain
**Resource URL**: *PUT {path-to-service}/{version}/projects/**project_id**/domains/**domain_id***

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of eeEmbedded Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Project to search domain in. 
*domain_id*	|Id for the domain to update 
JSON body	|[domain_meta_data](./schemata/domain_meta_data.md) for the domain to update. 

Fields ***domain_name*** and ***description*** can be updated.


Returns list containing single element {domain_url, {domain_meta_data}}. JSON Schema not shown (trivial). 

**Example:**

```
POST https://example.com/eee/bim-api/0.4/projects/DABB/domains
Request:
{
	"domain_name": "BACS",
	"description": "Oslo-office project domain BACS (earlier BCS)",
}

Response:
[{
    "project_url ": "http://example.com/eee/bim-api/0.4/projects/DABB",
    "project_meta_data ":
    {
	"project_id": "DABB",
	"project_name": "oslo-office",
	"domain_id": "fcfc",
	"domain_name": "BACS",
	"description": "Oslo-office project domain BACS (earlier BCS)",
    }
}]
```

## Delete Domain
**Resource URL**: *DELETE {path-to-service}/{version}/projects/**project_id**/domains/**domain_id***

element | explanation
--------|-----------|
*path-to-service*	|URL pointing to an instance of eeEmbedded Repository Services|
*version*	|States version of the API to use, allowing multiple versions of API for upgrading.
*project_id*	|Project to search domain in. 
*domain_id*	|Id for the domain to delete 

Response: list containing single element {domain_url, {domain_meta_data}}. JSON Schema not shown (trivial). Empty list if not found. JSON Schema not shown (trivial).

**NOTE:**

* If the domain is used in the project it will not be deleted, and an error return will be sent.

**Example:**

```
DELETE https://example.com/eee/bim-api/0.4/projects/DABB/domains/fcfc

Response:
[{
    "project_url ": "http://example.com/eee/bim-api/0.4/projects/DABB",
    "project_meta_data ":
    {
	"project_id": "DABB",
	"project_name": "oslo-office",
	"domain_id": "fcfc",
	"domain_name": "BACS",
	"description": "Oslo-office project domain BACS (earlier BCS)",
    }
}]
```

