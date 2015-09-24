# Energy-efficient Buildings (EeB) BIM-API

###Contents:

* [Reference documentation](doc/README.md)

###Change log:

Version: 0.5 2015.09.25 AET:

* Added meta_data field **model_treat_as_binary** to model_meta_data, to enable "true" FILE service
* Added meta_data field **model_mime_type** to model_meta_data, tto enable enforced MIME type for file download (if necessary)
* Added resource **.../models/<guid>/metadata** to enable simple retrieval of metadata for a model
* Improved MODEL UPLOAD service description to match HTTP multipart upload as well as no-multipart

Version: 0.4 2015.08.25 AET:

* Consolidated service descriptions before eeEmbedded Malaga meeting

