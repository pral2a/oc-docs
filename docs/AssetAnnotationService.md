# Asset Annotation Service

Data Annotation Service manages the data annotation process on assets of all Organicity sites.
The primary goal of this service is to provide to the programmable clients an available set of
tag domains and then receive and store tags (tag attachment on an asset by a user). The Organicity
platform will expose the data annotation service through the Annotation API.

Since the UDO is creating a new type of urban data repository and provides a starting point for exploration of
urban data across different city environments, it is crucial to stimulate extraction and generation of knowledge from the
raw data streams. Aiming at enhancing the urban data sources with useful information, OrganiCity has developed this service for
enabling collaborative data annotation. The utilized data model and annotation services are flexible enough to enable various types
of labels from online resources on the Web, social media and references to rich multimedia content online (images, video, etc.)
 to free-text labels or numeric values. A set of methods has been created for maintaining dynamic label
 categories, labels and labelling of data.

![UDO and Annotations](../images/udo-annotations.png "UDO and Annotations")

Acquiring labels for a specific set of data sources can be parameterized under the scope of an experiment. Experimenters can define a set
of label categories to be used by the applications associated with their experiment. Moreover, experimenters, or other end-users,
can retrieve the various labels and the corresponding data under the scope of an experiment.  Finally, they can create customized applications
to acquire annotations from participants, or applications to visualize them.

Along with the crowdsourcing annotation process, experimenters can utilize machine learning algorithms that enable more autonomous
learning, semi-supervised learning or reinforcement learning techniques, exploiting the acquired annotations as training sets.
It is possible in this way to use the created models as classifiers for automatic labelling of urban resources, events or anomaly detection.
Furthermore, as users are constantly contributing with annotations, experimenters are possible to perform verification, cross-validation
on the extracted models and create adaptive models using reinforcement learning methodologies.

References to the specification of the Asset Annotation Service
- Annotation Service Endpoint: http://annotations.organicity.eu/
- Annotation Service Swagger UI: http://annotations.organicity.eu/swagger-ui.html

## Data Model

The underlying data model of the annotation service is depicted in the following figure.
![Annotation Model](../images/annotation_model.png "Annotation Model")

- **Service Entities** represent utility/urban services or in general urban processes. An example of a service might be Garbage Collection, Noise Monitoring etc. The basic usage of
service entities is the organization and discovery of tag collections (e.g. what tags are usually used for characterizing the noise level sensors etc.)
- **TagDomains Entities** represent collections of tags. Usually a tag domain is associated with a service and experiment/application specify which tag domains they will use.
- **Tag Entities** represent the actual labels to be used by end-user using an annotation client (application or experiment)
- **Application Entities** (a.k.a. experiments) represent the client applications used by end-users during the annotation process.
- **Asset Entities** are the assets of Urban data observatory that are annotated (associated by user with an existing tag/label)


There are some underlying restriction and rules to Organicity users on accessing and using the annotation service. For the following Organicity roles:
User Roles:
- OC Admin (OC-A):
    - can create, read, update and delete (CRUD) Service, TagDomain, Tag, Application and Annotatation entities
- OC Experimenter (OC-E):
    - can only Read Service, TagDomain, Tag entities that are public
    - CRUD the ones involved in their experiment
    - OC-E can CRUD all annotations of his applications
- OC Participant (OC-P):
    - can only Read Service, TagDomain, Tag entities that are public
    - OC-P can CRUD only his annotations
- OC Anonymous (OC-AN)
    - OC-AN can R only COUNT aggregations of annotations

### Entity Description

The objects of the entities are comply with the following schema:

### Tag

```json
{
    "id": 0,
    "name": "string",
    "urn": "string"
}
```
Example:
```json
{
    id: 106,
    urn: "urn:tag:faulty",
    name: "faulty reading"
}
```

### TagDomain

```json
{
"description": "string",
"id": 0,
"urn": "string",
"services": [
    {
        "description": "string",
        "id": 0,
        "urn": "string",
		      "user":"string"
    }
 ],
 "tags": [
    {
    "id": 0,
       "name": "string",
       "urn": "string"
    }
 ]
}
```

Example:
```json
  {
  id: 104,
  urn: "urn:tagDomain:faultyReadings",
  description: "This is tag domain to classify readings into faulty or normal",
  tags: [
      {
          id: 106,
          urn: "urn:tag:faulty",
          name: faulty reading
      },
      {
          id: 105,
          urn: "urn:tag:normal",
          name: "normal reading"
      }
   ],
  services: [
      {
          id: 107,
          urn: "urn:service:environmentalMonitoring",
          description: "environmental monitoring",
          user:"372f3a46-0596-41cf-a038-a2845f06eb24"
      }
   ]
  }
```
### Service
```json
{
    "description": "string",
    "id": 0,
    "urn": "string",
    "user":"string"
}
```
Example:
```json
{
    id: 107,
    urn: "urn:service:environmentalMonitoring",
    description: "environmental monitoring",
    user:"372f3a46-0596-41cf-a038-a2845f06eb24"
}
```

### Experiment
```json
{
    "description": "string",
    "id": 0,
    "tagDomains": [
        {
          "description": "string",
          "id": 0,
          "services": [
            {
              "description": "string",
              "id": 0,
              "urn": "string",
              "user": "string"
            }
          ],
          "tags": [
            {
              "id": 0,
              "name": "string",
              "urn": "string",
              "user": "string"
            }
          ],
          "urn": "string",
          "user": "string"
        }
      ],
    "urn": "string",
    "user": "string"
}
```

### Asset

```json
{
    "id": 0,
    "urn": "string"
}
```
Example:
```json
{
    "id": 12 ,
    "urn": "urn:oc:entity:london:enableiot:fixed:98-4F-EE-00-0F-76"
}
```


### Annotation

```json
{
    "annotationId": 0,
    "application": "string",
    "assetUrn": "string",
    "datetime": "string",
    "numericValue": 0,
    "tagUrn": "string",
    "textValue": "string",
    "user": "string"
}
```
Example:
```json
{
    "annotationId": 0,
    "application": "57eab2c2ad0302ad0b5c92c6",
    "assetUrn": "urn:oc:entity:london:enableiot:fixed:98-4F-EE-00-0F-76",
    "datetime": "2016-10-21 09:01:12:123",
    "numericValue": 0,
    "tagUrn": "urn:tag:faulty",
    "textValue": "string",
    "user": "86d7edce-5092-44c0-bed8-da4beaa3fbc6"
}
```


## Annotation Service API Overview

- Annotation Service Swagger UI: http://annotations.organicity.eu/swagger-ui.html

Annotation API is organized in three major parts 
 
- **Tag and Tag Domain browsing**: for reading Application,Service, TagDomain, Tag entities
- **Annotation Parameters Management**: creating, deleting, updating Application,Service, TagDomain, Tag entities
- **Annotation Management**: Getting/setting annotations on Organicity Assets

### Experiment, Tag Domain and Tag browsing
|Method| Path | Operation|
|---|---|---|
|GET| /tagDomains                                   |Get Tag Domains|
|GET| /tagDomains/{tagDomainUrn}                    |Get a Tag Domain|
|GET| /tagDomains/{tagDomainUrn}/tags               |Get Tags of a Tag Domain|
|GET| /tags/{tagUrn}                                |Get a Tag|

### Annotation Parameters Management
|Method| Path | Operation|
|---|---|---| 
|POST   |/admin/experiments                               |Create an Experiment|
|DELETE |/admin/experiments/{experimentUrn}               |Delete an Experiment|
|DELETE |/admin/experiments/{experimentUrn}/tagDomains    |Disassociate a TagDomain of an Experiment|
|GET    |/admin/experiments/{experimentUrn}/tagDomains    |Get TagDomains of an Experiment|
|POST   |/admin/experiments/{experimentUrn}/tagDomains    |Associate a TagDomain with an Experiment|
| | | | 
|POST   |/admin/services                                    |Create a Service|
|DELETE |/admin/services/{serviceUrn}                       |Delete a Service|
| | | | 
|POST   |/admin/tagDomains                                  |Get TagDomains|
|DELETE |/admin/tagDomains/{tagDomainUrn}                   |Delete a TagDomains|
|POST   |/admin/tagDomains/{tagDomainUrn}                   |Update a TagDomains|
|DELETE |/admin/tagDomains/{tagDomainUrn}/services          |Disassociate a TagDomain with a Service|
|GET    |/admin/tagDomains/{tagDomainUrn}/services          |Get associated services with a Tag Domain|
|POST   |/admin/tagDomains/{tagDomainUrn}/services          |Associate a TagDomain with a Service|
|DELETE |/admin/tagDomains/{tagDomainUrn}/tags              |Delete a Tag from a Tag Domain|
|POST   |/admin/tagDomains/{tagDomainUrn}/tags              |Add a Tag in a Tag Domain|
|POST 	 |/admin/tagDomains/{tagDomainUrn}/tag               |Create Tag to TagDomain|

### Annotation Management
|Method| Path | Operation|
|---|---|---| 
|GET    |/admin/annotations/delete/{assetUrn} |Delete Annotation of an Asset|
|GET    |/annotations/                        |Get Annotation for Application, User and Tag| 
|DELETE |/annotations/{assetUrn}              |deleteAnnotation|
|POST   |/annotations/{assetUrn}              |Create Annotation|
|GET    |/annotations/{assetUrn}/all          |Get Annotations of an Asset|
|GET    |/annotations/{tagDomain}             |Get Annotation of a TagDomain|

## Example

Let userId=86d7edce-5092-44c0-bed8-da4beaa3fbc6 and experimentId=57eab2c2ad0302ad0b5c92c6

Creation of a Tag Domain
 
 
```shell
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
  "description": "a tag domain for faulty noise level",
  "services":  null,
  "tags": [
    {
      "id": 123,
      "name": "faulty",
      "urn": "urn:tag:faulty"
    },
    {
          "id": 132,
          "name": "normal",
          "urn": "urn:tag:normal"
    }
  ],
  "urn": "urn:tagDomain:86d7edce-5092-44c0-bed8-da4beaa3fbc6:malfunctioning"
}' 'https://annotations.organicity.eu/admin/tagDomains'
```

Get of Tag Domain
```shell
curl -X GET --header 'Accept: application/json' 'https://annotations.organicity.eu/tagDomains/urn%3AtagDomain%3A86d7edce-5092-44c0-bed8-da4beaa3fbc6%3Amalfunctioning'
```

Get Tags of TagDomain
```shell
curl -X GET --header 'Accept: application/json' 'https://annotations.organicity.eu/tagDomains/urn%3AtagDomain%3A86d7edce-5092-44c0-bed8-da4beaa3fbc6%3Amalfunctioning/tags'
```

Create Experiment
```shell
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '
{
  "description": "Experiment 86d7edce-5092-44c0-bed8-da4beaa3fbc6",
  "urn": "urn:application:86d7edce-5092-44c0-bed8-da4beaa3fbc6"
}' 'https://annotations.organicity.eu/admin/applications'
```

Associate Experiment with an Tag Domain
```shell
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' 'https://annotations.organicity.eu/admin/applications/urn%3Aapplication%3A86d7edce-5092-44c0-bed8-da4beaa3fbc6/tagDomains?tagDomainUrn=urn%3AtagDomain%3A86d7edce-5092-44c0-bed8-da4beaa3fbc6%3Amalfunctioning'
```


Post an Annotation

Let an experimenter who has developed an application to his smartphone in order to collect data concerning 
the wind speed. Assuming that the application contains a TagDomain that describes the wind speed levels, 
the creation of an annotation can be implemented as follows:

```javascript
userId = 86d7edce-5092-44c0-bed8-da4beaa3fbc6
experimentId = 57eab2c2ad0302ad0b5c92c6
assetUrn = urn:oc:entity:experimenters:62afc265-af9a-47e7-afb5-caab21ed09b4:57f210e59324fdd11103d93c:14
tagUrn = urn:oc:tagDomain:WindSpeedLevel:calm
	
function createAnnotation(){
        var annotationJson = {
                "annotationId": null,
                "application": experimentId,
                "assetUrn": assetUrn,
                "datetime": null,
                "numericValue": 0,
                "tagUrn": tagUrn,
                "textValue": "textValue",
                "user": userId,
        };

          $.ajax({
             url: "https://annotations.organicity.eu/annotations/"+ assetUrn.name,
             data: JSON.stringify(annotationJson),
             type: "POST",
             beforeSend: function(xhr){
                 xhr.setRequestHeader('Accept', 'application/json');
                 xhr.setRequestHeader('Accept', 'application/json');
                 xhr.setRequestHeader('Content-Type', 'application/json');

                 
             },
             success: function() {
                $.ajax({
                    url: "https://annotations.organicity.eu/annotations/" + assetUrn +"/all",
                                   
                    type: "GET",
                    beforeSend: function(xhr){
                        xhr.setRequestHeader('Accept', 'application/json');
                        xhr.setRequestHeader('Accept', 'application/json');
                        xhr.setRequestHeader('Content-Type', 'application/json');

                    },
                    success: function(response) {
                        alert(response);
                    }
                 });
             },
             error: function(){
                alert('an error occurred.');
              }
          });
    } 
```

Get All Annotations
```shell
curl -X GET --header 'Accept: application/json' 'https://annotations.organicity.eu/annotations/urn%3Aoc%3Aentity%3Alondon%3Aweatherstation1/all'
```
