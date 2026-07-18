IHTSDO [Snowstorm](https://github.com/IHTSDO/snowstorm) is an open source terminology server with special support for SNOMED CT. It is built on top of Elasticsearch, with a focus on performance and enterprise scalability.

## APIs
Snowstorm has two APIs:
- HL7 FHIR API 🔥
  - Implements the Terminology Module
  - Recommended for implementers
  - Supports SNOMED CT, LOINC, ICD-10, ICD-10-CM and other code systems
- Specialist SNOMED CT API
  - Supports the management of SNOMED CT code systems
  - Supports the SNOMED CT Browser
  - Supports authoring SNOMED CT editions

## TermX
TermX utilize Snowstorm SNOMED CT API including 
- search, 
- management of concepts and descriptions, 
- release management, 
- import and export. 


## Learn more about Snowstorm
- SNOMED CT Terminology Services [Course Guide](https://elearning.ihtsdotools.org/mod/book/view.php?id=6135&chapterid=1809).
- Snowstorm [@SNOMED eLearning site](https://snowstorm-training.snomedtools.org/snowstorm/snomed-ct/swagger-ui.html).
- Snowstorm FHIR API [Postman collection](https://documenter.getpostman.com/view/462462/S1TVXJ3k).
- The TermX uses [public](https://snowstorm-public.kodality.dev/swagger-ui/index.html) or [private](https://snowstorm.kodality.dev/swagger-ui.html) Snowstorm API for queries and edition SNOMED data.
- [Snowstorm server installation guide](page:snowstorm-server).

***


## Import of the SNOMED RF2 files<i class="mdi mdi-file"></i>

You can use REST API to upload SNOMED RF2 files.
The example below describes creating International and Estonian editions and uploading RF2 files.

### Prerequisites
- Download SNOMED RF2 files from [SNOMED MLDS](https://mlds.ihtsdotools.org).
> If you don't have acces contact the [National Release Center](https://www.snomed.org/our-stakeholders/members) or become a member of SNOMED .
{.is-info}
- Ensure that Snowstorm is running.

### Import of International Edition
1. Start the import process by creating a new import job. You can use Snowstorm Swagger API or curl.
Within Swagger UI open `POST: /imports` the endpoint, insert it into the body
```json
{
  "branchPath": "MAIN",
  "createCodeSystemVersion": true,
  "type": "SNAPSHOT"
}
```
and press "Try it now".

Within curl
```json
curl -v -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
  "branchPath": "MAIN",
  "createCodeSystemVersion": true,
  "type": "SNAPSHOT"
}' 'http://localhost:8080/imports'
```
Where 'http://localhost:8080/imports' is the address of Snowstorm server.

Store the identifier of the import (it will look like this - d0b30d96-3714-443e-99a5-2f282b1f1b0). It will be needed for the next steps.

2. Upload SNOMED archive to the Snowstorm.
Within Swagger UI select `POST imports/archive` the endpoint and specify the import identifier from the previous step and the file name.
Within curl 
```json
curl -X POST --header 'Content-Type: multipart/form-data' \
--header 'Accept: application/json' \
-F file=@init/'1. SnomedCT_InternationalRF2_PRODUCTION_20200731T120000Z.zip' \
'http://localhost:8080/imports/<import-id>/archive'
```

### Import of the local edition or extension
1. On the Swagger interface in the ‘Code Systems’ section look for the `Create a code system` endpoint. 
Use the following in the request to create the CodeSystem. 
```json
{
  "branchPath": "MAIN/SNOMEDCT-EE",
  "shortName": "SNOMEDCT-EE"
}
```
or within curl
```json
curl -v -X POST --header 'Content-Type: application/json' \
--header 'Accept: application/json' -d '{
  "branchPath": "MAIN/SNOMEDCT-EE",
  "shortName": "SNOMEDCT-EE"
}' 'http://localhost:8080/codesystems'
```

2. You now need to import the local extension or edition. Create a new import job. 
Look for the `Import` endpoint in Swagger UI and then create a new import using:
```json
{
  "branchPath": "MAIN/SNOMEDCT-EE",
  "createCodeSystemVersion": true,
  "type": "SNAPSHOT"
}
```
or use curl
```json
curl -v -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
  "branchPath": "MAIN/SNOMEDCT-EE",
  "createCodeSystemVersion": true,
  "type": "SNAPSHOT"
}' 'http://localhost:8080/imports'
```

3. Import extension.

Within Swagger UI select `POST imports/archive` the endpoint and specify the import identifier from the previous step and the file name.

Within curl
```json
curl -v -X POST --header 'Content-Type: multipart/form-data' --header 'Accept: application/json' \
-F file=@SnomedCT_ManagedServiceEE_PRODUCTION_EE1000181_20200530T120000Z.zip \
'http://localhost:8080/imports/<importid>/archive'
```


