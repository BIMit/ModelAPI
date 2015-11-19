## Energy-efficient Buildings BIM-API File Services ##

[Level Up](../README.md)
[Overview](./README.md)

Version: 0.5 2015.11.19 AET


### File Services

Files:

* [Create File Service](file_service_create.md) - create an empty file (placeholder)
* [Delete File Service](file_service_delete.md) 
* [Download File Service](file_service_download.md) 
* [Download File Metadata Service](file_service_download_metadata.md) - get metadata for a file
* [List Files Service](file_service_list.md) - list metadata for files
* [Update File Service](file_service_update.md)
* [Upload File Service](file_service_upload.md) - create a new (non-empty) file or file version


File services closely resembles [Model Services](model_service.md) apart from from storage mode on the server; a FILE is handled as a binary object, the server does not know anything of the internals of a file.

