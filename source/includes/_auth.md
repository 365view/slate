# Authentication

## The login object

```json
{
    "kind": "login",
    "title": "title",
    "path": "path",
    "password": {
        "value": "password",
        "former": "old_password",
        "reset": false
    }
}
```
### Attributes

Attribute | type | Description  | mandatory
----------|------|--------------|----------
kind | string | The object's kind. Must be "login" for all login objects | true
title | string | The login's title. Should be a valid user name | true
path | string | The login's path within the system. | false
password | Password | The password object. | true

### Password

The password object is a nested object within the login.

Attribute | type | Description  | mandatory
----------|------|--------------|----------
value | string | The password value, encrypted. | true
former | string | The former password, used in the event a user wants to change its password. | false
reset | boolean | Flag indicating the password is to be reset. Either triggered by the user or an admin | false


<aside class="warning">Some connectors may require additionnal fields to authenticate. Depending on the connector login will fail if one or more specific field are not present.</aside>

## Connector information

### HTTP request

`GET /login`

## prepare

This method allows you to retrieve a prefilled JSON to log into a connector. You can then fill the required fields and use the resulting object to log into the connector. 

### HTTP request

`POST /login/prepare`

## login

### HTTP request

`POST /login/:locale`

### Path parameters

Parameter | Default | Description
----------|---------|------------
locale | en_EN | The client's locale.

### Request content

A login object as encrypted JSON


## logoff

### HTTP request

`DELETE /login/:sessionId`

### Path parameters

Parameter | Default | Description
----------|---------|------------
sessionId | - | The session id you want to log out of.
