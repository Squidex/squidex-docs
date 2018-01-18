Webhooks are endpoints where Squidex will send events for a given schema to:

* **Created**: The content item has been created.
* **Published**: The content item has been updated.
* **Unpublished**: The content item has been unpublished.
* **Updated**: The content item has been saved.
* **Deleted**: The content item has been deleted.

The secret is used to sign the requests for security reasons. Each request will contain a `X-Signature` header which is calculated the following way: 

    ToBase64String(Sha256(RequestBody + Secret))

> By default the built-in .net framework sha256 hasher returns a byte array. Squidex encodes this byte array to a base64 string.
> `Convert.ToBase64String` 

You can calculate the signature when you receive an event to check if the request has been sent by Squidex. Do **not share** the secret.

The request body format is the following for all the events above:
```json
{
    "type": "...",
    "payload": {
        "data": {
        },
        "contentId": "00000000-0000-0000-0000-000000000000",
        "appId": "00000000-0000-0000-0000-000000000000",
        "schemaId": "00000000-0000-0000-0000-000000000000",
        "actor": "subject:597b5b99f9ed0f3138a138c3"
    },
    "timestamp": "2018-01-18T16:56:10Z"
}
```
| Field         | Description                                                                                              |
| ------------- | -------------------------------------------------------------------------------------------------------- |
| **type**      | The event type,  e.g if schema is news, and a new news entry has been created, this is NewsCreatedEvent  | 
| **payload**   | The payload, an object with the data, and the identifier fields.                                         |
| **data**      | An object which holds the schema's fields and their values.                                              |
| **contentId** | The id of the content.                                                                                   |
| **appId**     | The id of the app which holds the schema and its data.                                                   |
| **schemaId**  | The id of the schema.                                                                                    |
| **actor**     | The id of the actor which initiated the event.                                                           |
| **timestamp** | The time when the event occured.                                                                         |

Example event:
```json
{
  "type": "PersonCreatedEvent",
  "payload": {
    "data": {
      "name": {
        "iv": "Test person two"
      },
      "birthplace": {
        "iv": "Japan"
      },
      "description": {
        "de": "nil",
        "en": "nil"
      },
      "slug": {
        "iv": "test-person-two"
      },
      "slug-id": {
        "iv": 998875
      },
      "link": {
        "iv": "http://example.com"
      },
      "is-incomplete": {
        "iv": false
      },
      "birthday": {
        "iv": "2018-01-18T16:56:04Z"
      }
    },
    "contentId": "167bf812-f995-41e2-8b70-04a4d28fe921",
    "schemaId": "6ffeeedc-1630-4337-b1e5-e0c3a62b4f44,person",
    "appId": "4a2beea0-ed84-45e9-9ebc-097af67a9e6a,portal",
    "actor": "subject:597b5b99f9ed0f3138a138c3"
  },
  "timestamp": "2018-01-18T16:56:10Z"
}
```    
    

Squidex will make several attempts to send an event:

1. After a few seconds.
2. After 5 minutes
3. After 1 hour.
4. After 6 hours.
6. After 12 hours.

An request will be treated as failed if the response does not return a 2XX status code or when it times out after 2 seconds. Consider internal queues for slow operations.

Events expire after 2 days and will be deleted automatically.
