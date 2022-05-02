# Treecko’s API documentation - CareReport

This document contains the documentation for CareReport's Controller.

#### Contents

- [Overview](#1-overview)
- [EndPoints](#2-endpoints)
  - [GET - Care Report Types](#21-carereport-types)
  - [GET - Care Report Tags](#22-carereport-tags)
  - [GET - Care Report by Patient](#23-carereport-patients)
  - [GET - Care Report by Working Group](#24-carereport-patients-group)
  - [POST - Create Care Report](#25-carereport-create)
  - [POST - Add Care Report Attachment](#26-carereport-add-attachment)

## 1. Overview

Treecko's Care report endpoint has the boundery to deal with all requests regarding Patient's care report and all services needed to suport its feature. Care reports services basicaly deal with Noventi structure throught Hoothoot's service, to delivery is result. In the next section it will be possible to find all services designed to delievery and maintain its solution.

### 1.1. Jira Users Story

| Ticket                                                      | Type         |
| ------------------------------------------------------------|--------------|
| [DEV-262 ](https://kenbi-tech.atlassian.net/browse/DEV-262) | EPIC         |
| [DEV-160 ](https://kenbi-tech.atlassian.net/browse/DEV-160) | User Story   |
| [DEV-327 ](https://kenbi-tech.atlassian.net/browse/DEV-327) | User Story   |
| [DEV-335 ](https://kenbi-tech.atlassian.net/browse/DEV-335) | User Story   |
| [DEV-525 ](https://kenbi-tech.atlassian.net/browse/DEV-525) | User Story   |


## 2. Endpoints

The API is RESTful and arranged around resources. All requests must be made using `https` with an integration or user's token defined for each scope.

The first request you make should be to acquire a credential token. This will confirm that your access token is valid, and give you a user id and claims that you will need for subsequent requests. If you don't know yet how to get a valid token you can check the authentication section on this document.

### 2.1. Carereport Types

Returns a list with all care report types and its subtypes grouped.

**Method:** GET <br>
**URL:** /carereport/types <br>
**Request parameter:** None 

#### Example request:

```
HTTP/1.1
GET https://treecko.dev.backend.kenbi.systems/carereport/types
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IjZFN....
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is a CareReportType object grouped by its unique identifier within a data envelope where every care report type has it own list of subtypes as follow:

#### Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "id": "457efcb8-e0de-4c7c-b6b0-fd3dda00e432",
        "name": "Altagsbegleitung",
        "subTypes": [
            {
                "id": "f4110a61-7f49-46b6-a82b-ecd4f983b34a",
                "name": "Verlaufsdokumentation"
            },
            {
                "id": "15ce953a-13fa-4cb5-b016-3ba8d6537907",
                "name": "Abweichungen"
            },
            {
                "id": "96e4f576-231a-49ae-9f9e-95ea244991ac",
                "name": "Sonstiges"
            },
            {
                "id": "3e36498c-c20b-468c-94d4-de526203336a",
                "name": "Termine"
            },
            {
                "id": "59640c8f-6852-4ec9-95f1-758ad9142241",
                "name": "Beschwerde / Konflikt"
            },
            {
                "id": "f1335377-6c6b-4495-aea1-6ea49e488422",
                "name": "Ereignis / Akutsituation"
            },
            {
                "id": "6173ef3f-6d90-4345-b8ef-28a9cbca4b6c",
                "name": "Beratung"
            }
        ]
    }
]
```

Where a CareReportType object is:

| Field      | Type   | Description                                     |
| -----------|--------|-------------------------------------------------|
| id         | string | A unique identifier for the type inside Noventi system. |
| name       | string | The care report type’s name on Noventi. This field is the main group and has a bunch of subtypes.  |

and a SubType object is:

| Field      | Type   | Description                                     |
| -----------|--------|-------------------------------------------------|
| id         | string | A unique identifier for the subtype.            |
| name       | string | The care report subtype name on Noventi. The id field is mandatory to create a new Care Report.  |


#### Possible errors message:

| Error code           | Description                                     |
| ---------------------|-------------------------------------------------|
| 400 Bad Request      | The `resquest` is invalid or is not formated properly. |
| 401 Unauthorized     | The `accessToken` is invalid or has been revoked. |
| 404 Not Found        | The was not found the object on database following the search parameters. |


### 2.2. Carereport Tags

Returns a list with all care report tags and its group.

**Method:** GET <br>
**URL:** /carereport/types <br>
**Request parameter:** None 


#### Example request:

```
HTTP/1.1
GET https://treecko.dev.backend.kenbi.systems/carereport/tags
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IjZFN....
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is a CareReportTag object grouped by its unique identifier within a data envelope where every care report tag has it own list of tag as follow:

#### Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
    "id": 10,
    "group": "Risiken",
    "tags": [
      {
        "id": 11,
        "name": "Dekubitus"
      },
      {
        "id": 12,
        "name": "Sturz"
      },
      {
        "id": 13,
        "name": "Inkontinenz"
      },
      {
        "id": 14,
        "name": "Schmerz"
      },
      {
        "id": 15,
        "name": "Ernährung/Flüssigkeit"
      },
      {
        "id": 16,
        "name": "Sonstiges Risiko"
      }
    ]
  },
  {
    "id": 30,
    "group": "SIS Themenfelder",
    "tags": [
      {
        "id": 31,
        "name": "Kognition+Kommunikation"
      },
      {
        "id": 32,
        "name": "Mobilität+Beweglichkeit"
      },
      {
        "id": 33,
        "name": "Krankheitsbez. Anforderungen"
      },
      {
        "id": 34,
        "name": "Selbstversorgung"
      },
      {
        "id": 35,
        "name": "Leben in soz. Beziehungen"
      },
      {
        "id": 36,
        "name": "Haushaltsführung"
      }
    ]
  }
]
```

Where a CareReportGroupTag object is:

| Field      | Type    | Description                                     |
| -----------|---------|-------------------------------------------------|
| id         | integer | A integer value for the tag group inside Noventi system. |
| group      | string  | The care report tag’s group name on Noventi. This field define the group name and has a bunch of tags.  |

and a CareReportTag object is:

| Field      | Type    | Description                                     |
| -----------|---------|-------------------------------------------------|
| id         | integer | A integer value for the tag inside Noventi system. |
| name       | string  | The care report tag’s name on Noventi. This field define the tag name.  |


#### Possible errors message:

| Error code           | Description                                     |
| ---------------------|-------------------------------------------------|
| 400 Bad Request      | The `resquest` is invalid or is not formated properly. |
| 401 Unauthorized     | The `accessToken` is invalid or has been revoked. |
| 404 Not Found        | The was not found the object on database following the search parameters. |

### 2.3. Carereport Patients

Returns a list with all care report for a given patient from the last 30 days.

**Method:** GET <br>
**URL:** /carereport/patient/{personid} <br>
**Request parameter:**

| Parameter       | Type     | Required   | Description                                     |
| -------------   |----------|------------|-------------------------------------------------|
| `personid`      | string   | true       | The patient squirtle unique identifier.         |


#### Example request:

```
HTTP/1.1
GET https://treecko.dev.backend.kenbi.systems/carereport/patient/25e9dbb9-7229-4ad9-9d19-afc467e5392f
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IjZFN....
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is a CareReportPatient object with its Type, Subtype and also the list of Tags and Attachments:

#### Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
  {
    "id": "c87aaf42-3129-4eb0-acd8-823516572565",
    "date": "2022-04-28T15:40:44Z",
    "patientId": "25e9dbb9-7229-4ad9-9d19-afc467e5392f",
    "employeeId": "891ab967-8228-467d-a708-bae86ad9e872",
    "notifiableType": 2,
    "careReportType": {
      "id": "97e07657-21a6-4e2b-af5b-50243782adec",
      "name": "Arzt",
      "subTypes": null
    },
    "careReportSubType": {
      "id": "96e4f576-231a-49ae-9f9e-95ea244991ac",
      "name": null
    },
    "description": "Große Pflege",
    "tags": [
      {
        "id": 11,
        "name": "Dekubitus"
      }
    ],
    "attachments": []
  }
]
```

Where a CareReportAttachmentPatient object is:

| Field              | Type                | Description                                     |
| -------------------|---------------------|-------------------------------------------------|
| id                 | string              | A string value for the care report inside Noventi system that unique identifier the report in the system. |
| date               | string              | The date and time the care report was created in the system.  |
| patientid          | string              | A string value with the squirtle identifier of the patient. |
| employeeId         | string              | A string value with the squirtle identifier of the employee. |
| notifiableType     | integer             | A integer value to identify the flag assigned to the report on Noventi Care. This value follow its rules.|
| careReportType     | CareReportType      | A value object with the Care Report type and its properties. |
| careReportSubType  | CareReportSubType   | A value object with the Care Report subtype and its properties. |
| description        | string              | A string value with a description texto for the patient's report. |
| tags               | list of Tags        | A value object with the Care Report list of tags assigned to the report. |
| attachments        | list of Attachment  | A value object with the Care Report attacment added to the report. |


and a CareReportType object is:

| Field      | Type               | Description                                     |
| -----------|--------------------|-------------------------------------------------|
| id         | string             | A string value for the care report type inside Noventi system. |
| name       | string             | The care report type’s name on Noventi.  |
| subTypes   | CareReportSubType  | A value object with the list of Care Report subtype and its properties. For this endpoint this data is not filled. |

and a CareReportSubType object is:

| Field      | Type               | Description                                     |
| -----------|--------------------|-------------------------------------------------|
| id         | string             | A string value for the care report subtype inside Noventi system. |
| name       | string             | The care report subtype’s name on Noventi.  |

and a CareReportTag object is:

| Field      | Type               | Description                                     |
| -----------|--------------------|-------------------------------------------------|
| id         | integer            | A integer value for the tag inside Noventi system. |
| name       | string             | The tag description on Noventi.  |

and a CareReportAttachment object is:

| Field      | Type               | Description                                     |
| -----------|--------------------|-------------------------------------------------|
| id         | string             | A string value that represents the unique identifier of the attachment. |
| fileName   | string             | A small description to identify the file.  |
| type       | string             | The file type.  |
| path       | string             | Complete path of the file including the file name to allow its download.  |

#### Possible errors message:

| Error code           | Description                                     |
| ---------------------|-------------------------------------------------|
| 400 Bad Request      | The `resquest` is invalid or is not formated properly. |
| 401 Unauthorized     | The `accessToken` is invalid or has been revoked. |
| 500 Server Error     | The server respond the error description if any exception is trow. |

### 2.4. Carereport Patients Group

Returns a list with all active and non archived care report for a employee and its working group.

**Method:** GET <br>
**URL:** /carereport/group/patients <br>
**Request parameter:** None


#### Example request:

```
HTTP/1.1
GET https://treecko.dev.backend.kenbi.systems/carereport/group/patients
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IjZFN....
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
```

The response is a CareReportPatient object with its Type, Subtype and also the list of Tags and Attachments:

#### Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
  {
    "id": "c87aaf42-3129-4eb0-acd8-823516572565",
    "date": "2022-04-28T15:40:44Z",
    "patientId": "25e9dbb9-7229-4ad9-9d19-afc467e5392f",
    "employeeId": "891ab967-8228-467d-a708-bae86ad9e872",
    "notifiableType": 2,
    "careReportType": {
      "id": "97e07657-21a6-4e2b-af5b-50243782adec",
      "name": "Arzt",
      "subTypes": null
    },
    "careReportSubType": {
      "id": "96e4f576-231a-49ae-9f9e-95ea244991ac",
      "name": null
    },
    "description": "Große Pflege",
    "tags": [
      {
        "id": 11,
        "name": "Dekubitus"
      }
    ],
    "attachments": []
  }
]
```

Where a CareReportAttachmentPatient object is:

| Field              | Type                | Description                                     |
| -------------------|---------------------|-------------------------------------------------|
| id                 | string              | A string value for the care report inside Noventi system that unique identifier the report in the system. |
| date               | string              | The date and time the care report was created in the system.  |
| patientid          | string              | A string value with the squirtle identifier of the patient. |
| employeeId         | string              | A string value with the squirtle identifier of the employee. |
| notifiableType     | integer             | A integer value to identify the flag assigned to the report on Noventi Care. This value follow its rules.|
| careReportType     | CareReportType      | A value object with the Care Report type and its properties. |
| careReportSubType  | CareReportSubType   | A value object with the Care Report subtype and its properties. |
| description        | string              | A string value with a description texto for the patient's report. |
| tags               | list of Tags        | A value object with the Care Report list of tags assigned to the report. |
| attachments        | list of Attachment  | A value object with the Care Report attacment added to the report. |


and a CareReportType object is:

| Field      | Type               | Description                                     |
| -----------|--------------------|-------------------------------------------------|
| id         | string             | A string value for the care report type inside Noventi system. |
| name       | string             | The care report type’s name on Noventi.  |
| subTypes   | CareReportSubType  | A value object with the list of Care Report subtype and its properties. For this endpoint this data is not filled. |

and a CareReportSubType object is:

| Field      | Type               | Description                                     |
| -----------|--------------------|-------------------------------------------------|
| id         | string             | A string value for the care report subtype inside Noventi system. |
| name       | string             | The care report subtype’s name on Noventi.  |

and a CareReportTag object is:

| Field      | Type               | Description                                     |
| -----------|--------------------|-------------------------------------------------|
| id         | integer            | A integer value for the tag inside Noventi system. |
| name       | string             | The tag description on Noventi.  |

and a CareReportAttachment object is:

| Field      | Type               | Description                                     |
| -----------|--------------------|-------------------------------------------------|
| id         | string             | A string value that represents the unique identifier of the attachment. |
| fileName   | string             | A small description to identify the file.  |
| type       | string             | The file type.  |
| path       | string             | Complete path of the file including the file name to allow its download.  |

#### Possible errors message:

| Error code           | Description                                     |
| ---------------------|-------------------------------------------------|
| 400 Bad Request      | The `resquest` is invalid or is not formated properly. |
| 401 Unauthorized     | The `accessToken` is invalid or has been revoked. |
| 500 Server Error     | The server respond the error description if any exception is trow. |


### 2.5. Carereport Create

Create a new care report for a patient.

**Method:** POST <br>
**URL:** /carereport/patient/{personid} <br>
**Request parameter:**

| Parameter       | Type     | Required   | Description                                     |
| -------------   |----------|------------|-------------------------------------------------|
| `personid`      | string   | true       | The patient squirtle unique identifier.         |

#### Example request:

```
HTTP/1.1
POST https://treecko.dev.backend.kenbi.systems/carereport/patient/25e9dbb9-7229-4ad9-9d19-afc467e5392f
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IjZFN....
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8

{
  "date": "2022-04-12T08:00Z",
  "notifiableType": 1,
  "careReportType": "97e07657-21a6-4e2b-af5b-50243782adec",
  "careReportSubType": "96e4f576-231a-49ae-9f9e-95ea244991ac",
  "description": "Große Pflege",
  "tags": "Dekubitus, Sturz"
}
```

With the following fields:

| Field                | Type     | Required   | Description                                     |
| ---------------------|----------|------------|-------------------------------------------------|
| date                 | datetime | true       | The date and time in UTC that of the report. |
| notifiableType       | integer  | true       | An integer value to identify the flag related with the report.  |
| careReportType       | string   | true       | Unique identifier assigned to care report to identify the report type.  |
| careReportSubType    | string   | optional   | Unique identifier assigned to care report to identify the report subtype.  |
| description          | string   | true       | Care report's text decription.  |
| tags                 | string   | optional   | Tags related to the report. This string value should be informed in a comma separeted string value. |

The response is a string value with the care report's unique identifier:

#### Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

"457efcb8-e0de-4c7c-b6b0-fd3dda00e432"
```


#### Possible errors message:

| Error code           | Description                                     |
| ---------------------|-------------------------------------------------|
| 400 Bad Request      | The `resquest` is invalid or is not formated properly. |
| 401 Unauthorized     | The `accessToken` is invalid or has been revoked. |
| 500 Server Error     | The server respond the error description if any exception is trow. |

### 2.6. Carereport Add Attachment

Create a new attachment and add to a existing care report for a patient.

**Method:** POST <br>
**URL:** /carereport/{id}/patient/{personid} <br>
**Request parameter:**

| Parameter       | Type     | Required   | Description                                     |
| -------------   |----------|------------|-------------------------------------------------|
| `id`            | string   | true       | The care report unique identifier to add the file attachment. |
| `personid`      | string   | true       | The patient squirtle unique identifier.         |

#### Example request:

```
HTTP/1.1
POST https://treecko.dev.backend.kenbi.systems/carereport/c87aaf42-3129-4eb0-acd8-823516572565/patient/25e9dbb9-7229-4ad9-9d19-afc467e5392f
Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IjZFN....
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8

{
  "extension": "jpg",
  "file": "AAAAHGZ0eXBpc29tAAACAGlzb21pc28ybXA0MQAAAAhmcmVlAAKZx21kYXTeBABMYXZjNTguMTM0LjEwMABCNZrf7vgAAQMQSQpdwugqpM1Uq6lSqlQpJQih7/ARx7iLPwYkL8M+RUEv5EWeoFCjj4IiETLjI28NWou4tAIVDDEod8RDxJfTH4D+vbBaGB+YD//LxBQ9xDb2RhBbBste8jabEdS1vN8OfG1DtoMWMWh4geIsR2zJDGvFMH68YhuvYRbcRQro46dka3sIpwE50T48AbuPLIyw78/np+6Am+eeba278xw4P2UzNq2+HC/5RNBRDN+EIPqgflKZ88zgicEC0SR8m6LoKqSrqRUqZK3et3FXi5mDfs6ShH/v1EXj5aSTMVVbmtnGDhiyQjUDluz8ZTCLBoEZz//bHH897BIYACRJvDqK9V6hwCEazn/+/AAAfqWyUSRECSX2VXW5l1upOr3nVVdSslSKaiYA5bt9ZZGnRpoxVkO49yWlGMLWy5xDsYpSmpynzlunXvVT6niYZHaHbAnxqBRtkIzurDD+4tE7fGoGwWuPhIUK8yWOqeGgC+hhkgnPI2eV97rIivepEATUjRVLvO86/qWw64Yt7/3NMjBxVHnceDLRbJa8hHWlizVOdo8WFGwJRKKu/BfDK3J4ehLBBLAyeEeyPgcOlTeVdRj1WUpK3mtCFlnxfV6mZQ2aQC5"
}
```

With the following fields:

| Field                | Type     | Required   | Description                                     |
| ---------------------|----------|------------|-------------------------------------------------|
| extension            | string   | true       | The file extension. The possible values are the following: - JPG or PNG for image format.<br> MP3 for audio format. <br> PDF for file format.) |
| file                 | string   | true       | The file content converted to base64.  |


The response is a string value with the attachment unique identifier:

#### Example response:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

"969fb441fe254cbfbd205a6d96bb1850"
```


#### Possible errors message:

| Error code           | Description                                     |
| ---------------------|-------------------------------------------------|
| 400 Bad Request      | The `resquest` is invalid or is not formated properly. |
| 401 Unauthorized     | The `accessToken` is invalid or has been revoked. |
| 500 Server Error     | The server respond the error description if any exception is trow. |
