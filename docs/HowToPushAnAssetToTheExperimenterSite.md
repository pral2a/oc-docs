# Tutorial: How to push an Assets to the Organicity Experimenter Site

**Precondition**:

* To be able to push an asset to the Organicity Experimenter Site (OC-Exp. Site), you need the **experimenter** role. This will be assigned to you by the Organicity team.

## Create an Experiment and an Application

First of all, you must create an experiment within the [Experimenter Portal](http://experimenters.organicity.eu). Please log in and click on **[NEW EXPERIMENT]** and follow the instructions. Make sure, the current date is between the the start date and end date.

After you created an experiment, you must create an application. Please select the experiment and click on **[APPS]**. After you created the application, please select that application. On top, you will find three IDs, for example:

* **Experimenter ID**: `cf2c1723-3369-4123-8b32-49abe71c0e57`
* **Experiment ID**: `57e127c010590cb31ca82aa4`
* **Application ID**: `57e1285a10590cb31ca82aa7`

In the next steps, we need that IDs.

Go back to the **[EXPERIMENTS]** and start the experiment by clicking on `STOPPED` in the state column. The state is `running` afterwards.

## Push a new asset

The Asset endpoint is located under [https://exp.orion.organicity.eu/v2/entities](https://exp.orion.organicity.eu/v2/entities).

### HTTP headers

To create a new asset, please do an HTTP POST on https://exp.orion.organicity.eu/v2/entities with the following HTTP headers:

```shell
Authorization: Bearer <AccessToken>
X-Organicity-Application: <ApplicationID>
X-Organicity-Experiment: <ExperimentID>
Content-Type: application/json
Accept: application/json

<Asset in JSON>
```

Here, you must use an [Access Token](HowToAuthenticateAnUser.md) which is [issued for you](HowToAuthenticateAnUser.md).

For the above IDs it looks like this:

```shell
POST https://exp.orion.organicity.eu/v2/entities

Authorization: Bearer <AccessToken>
Content-Type: application/json
Accept: application/json
X-Organicity-Application: 57e1285a10590cb31ca82aa7
X-Organicity-Experiment: 57e127c010590cb31ca82aa4

<Asset in JSON>
```

### HTTP body

The HTTP body is an asset in JSON format [as described here](AssetDataModel.md). Important for the creation is the correct Asset ID. An Asset ID has the form:

```json
urn:oc:entity:experimenters:<ExperimenterId>:<ExperimentId>:<AssetId>
```

The `<ExperimenterId>` and `<ExpeirmentID>` are the one from above. The `<assetID>` is application specifc. Thus, you decide how they look like. In our example, we wil just use an `1` as the Asset ID. In our example, it looks as follows:

```json
urn:oc:entity:experimenters:cf2c1723-3369-4123-8b32-49abe71c0e57:57e127c010590cb31ca82aa4:1
```

A simple valid asset could be:

```json
{
	"id": "urn:oc:entity:experimenters:cf2c1723-3369-4123-8b32-49abe71c0e57:57e127c010590cb31ca82aa4:1",
	"type": "urn:oc:entityType:demo",
  "TimeInstant": {
    "type": "urn:oc:attributeType:ISO8601",
    "value": "2016-10-04T13:45:00.000Z"
  }
}
```

### Wiring everything together

Next, we wire everthing together: The HTTP headers and the asset:

```shell
POST https://exp.orion.organicity.eu/v2/entities

Authorization: Bearer <AccessToken>
Content-Type: application/json
Accept: application/json
X-Organicity-Application: 57e1285a10590cb31ca82aa7
X-Organicity-Experiment: 57e127c010590cb31ca82aa4

{
	"id": "urn:oc:entity:experimenters:cf2c1723-3369-4123-8b32-49abe71c0e57:57e127c010590cb31ca82aa4:1",
	"type": "urn:oc:entityType:demo",
  "TimeInstant": {
    "type": "urn:oc:attributeType:ISO8601",
    "value": "2016-10-04T13:45:00.000Z"
  }
}
```

The response is a `201 Created`, which includes a `location` header:

```shell
Status Code: 201 Created
...
Content-Length: 0
Content-Type: text/html; charset=utf-8
Date: Tue, 20 Sep 2016 12:37:07 GMT
...
Location: /v2/entities/urn:oc:entity:experimenters:cf2c1723-3369-4123-8b32-49abe71c0e57:57e127c010590cb31ca82aa4:1?type=urn:oc:entityType:demo
...
```

### Validate, that the asset was created successfully with the Urban Data Obervatory

To verify, that the asset was created in Organicity Central Site, you must use the [Asset Discovery Service](https://organicityeu.github.io/api/AssetDiscovery.html).
You can simply do an HTTP GET on the `location` header returnd by the creation:

```shell
GET http://discovery.organicity.eu/v0/assets/urn:oc:entity:experimenters:cf2c1723-3369-4123-8b32-49abe71c0e57:57e127c010590cb31ca82aa4:1
```

## Push as an Participant

To push an asset as an participant, the participant must be part of you experiment. A participant can be invited
with the [Experimenter Portal](http://experimenters.organicity.eu/). If a participant logs in to your application,
the `participant` role will be assigned. The pushing itself works as abobe.

**HINTS**:
* In the Asset ID, you keep the `<ExperimenterId>`!
* A participant can just create or read assets. Update and delete is not supported for participants.

## Delete and update assets

See How to [Delete or Update an asset](HowToUpdateDeteleAnAsset.md).

## Demo

### Push a new asset

An XMLHTTPRequest-based example can be found here: https://organicityeu.github.io/demo/pushAsset.html

## TODO in this documentation

* Public/Private