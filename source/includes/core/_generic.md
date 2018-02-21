# Generic Objects

The main category of methods you will be dealing with when interacting with a 365View connector. There are nine methods in this category to manipulate actors and resources from a connector.

## Get a specific object

```java
    User parameter = new User("12345");
    User result = connectorClient.getGenericObjectDTO(connectionInfos, parameter);
```

> Get a user with id 12345.

### HTTP request

`POST /get/:sessionId`

### Path parameter

Name | description | required
-----|-------------|---------
sessionId | The id of your active session. | true

### Request Body

```json
{
    "kind": "user",
    "id": "12345"
}
```

> Example JSON of a user with id 12345.

A JSON object with the kind and id of the requested object as fields.

### Response status

HTTP code | 365View code | Description
----------|--------------|------------
200 | - | The object has been found and returner correctly
500 | * | The object was not found or an error happened. Check the error message and 365View error code for details.

### Response content

A JSON object corresponding to the requested object.

## <method_name>

### HTTP request

`<http_method> generic/<path>`

### Request body