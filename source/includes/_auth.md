# Authentication

To use a 365View connector you must first log into it. Login information vary from connector to connector as they may require additionnal properties to acces the platform.

In this section we will only cover the basics of authentication in 365View and leaves connector-specific information to the connectors' section.

## The login object

```json
{
    "kind": "login",
    "title": "title",
    "password": {
        "value": "password",
        "former": "old_password",
        "reset": false
    }
}
```

> Minimal JSON necessary to login to a connector.

<!-- #region -->

```csharp
// To parse JSON, add NuGet 'Newtonsoft.Json' then do:
//
//    using Three65;
//
//    var data = Login.FromJson(jsonString);
namespace Three65
{
    using System;
    using System.Net;
    using System.Collections.Generic;

    using Newtonsoft.Json;

    public partial class Login
    {
        [JsonProperty("kind")]
        public string Kind { get; set; }

        [JsonProperty("title")]
        public string Title { get; set; }

        [JsonProperty("path")]
        public string Path { get; set; }

        [JsonProperty("password")]
        public Password Password { get; set; }
    }

    public partial class Password
    {
        [JsonProperty("value")]
        public string Value { get; set; }

        [JsonProperty("former")]
        public string Former { get; set; }

        [JsonProperty("reset")]
        public bool Reset { get; set; }
    }

    public partial class Login
    {
        public static Login FromJson(string json) => JsonConvert.DeserializeObject<Login>(json, Converter.Settings);
    }

    public static class Serialize
    {
        public static string ToJson(this Login self) => JsonConvert.SerializeObject(self, Converter.Settings);
    }

    public class Converter
    {
        public static readonly JsonSerializerSettings Settings = new JsonSerializerSettings
        {
            MetadataPropertyHandling = MetadataPropertyHandling.Ignore,
            DateParseHandling = DateParseHandling.None,
        };
    }
}
```

<!-- #endregion -->

<!-- #region -->

```java
// Converter.java

// To use this code, add the following Maven dependency to your project:
//
//     com.fasterxml.jackson.core : jackson-databind : 2.9.0
//
// Import this package:
//
//     import three65.Converter;
//
// Then you can deserialize a JSON string with
//
//     Login data = Converter.fromJsonString(jsonString);

package three65;

import java.util.Map;
import java.io.IOException;
import com.fasterxml.jackson.databind.*;
import com.fasterxml.jackson.core.JsonProcessingException;

public class Converter {
    // Serialize/deserialize helpers

    public static Login fromJsonString(String json) throws IOException {
        return getObjectReader().readValue(json);
    }

    public static String toJsonString(Login obj) throws JsonProcessingException {
        return getObjectWriter().writeValueAsString(obj);
    }

    private static ObjectReader reader;
    private static ObjectWriter writer;

    private static void instantiateMapper() {
        ObjectMapper mapper = new ObjectMapper();
        reader = mapper.reader(Login.class);
        writer = mapper.writerFor(Login.class);
    }

    private static ObjectReader getObjectReader() {
        if (reader == null) instantiateMapper();
        return reader;
    }

    private static ObjectWriter getObjectWriter() {
        if (writer == null) instantiateMapper();
        return writer;
    }
}

// Login.java

package three65;

import java.util.Map;
import com.fasterxml.jackson.annotation.*;

public class Login {
    private String kind;
    private String title;
    private String path;
    private Password password;

    @JsonProperty("kind")
    public String getKind() { return kind; }
    @JsonProperty("kind")
    public void setKind(String value) { this.kind = value; }

    @JsonProperty("title")
    public String getTitle() { return title; }
    @JsonProperty("title")
    public void setTitle(String value) { this.title = value; }

    @JsonProperty("path")
    public String getPath() { return path; }
    @JsonProperty("path")
    public void setPath(String value) { this.path = value; }

    @JsonProperty("password")
    public Password getPassword() { return password; }
    @JsonProperty("password")
    public void setPassword(Password value) { this.password = value; }
}

// Password.java

package three65;

import java.util.Map;
import com.fasterxml.jackson.annotation.*;

public class Password {
    private String value;
    private String former;
    private boolean reset;

    @JsonProperty("value")
    public String getValue() { return value; }
    @JsonProperty("value")
    public void setValue(String value) { this.value = value; }

    @JsonProperty("former")
    public String getFormer() { return former; }
    @JsonProperty("former")
    public void setFormer(String value) { this.former = value; }

    @JsonProperty("reset")
    public boolean getReset() { return reset; }
    @JsonProperty("reset")
    public void setReset(boolean value) { this.reset = value; }
}
```

<!-- #endregion -->

This object allows a client to login to a connector by sending the correct authentication informations.

<aside class="notice">Some connectors may require additionnal fields to authenticate. Make sure to read the login section related to the connector you want to log into.</aside>

### Attributes

Attribute | type | Description  | mandatory
----------|------|--------------|----------
kind | string | Must be _login_ | true
title | string | The username you are logging with | true
password | Password | The password | true

### Password

The password object is a nested object within the login.

<aside class="notice">Password values are encrypted using DES. You should make sure to use the proper encryption otherwise password validation will fail.</aside>

Attribute | type | Description  | mandatory
----------|------|--------------|----------
value | string | The password value, encrypted. | true
former | string | The former password, used in the event a user wants to change its password. | false
reset | boolean | Flag indicating the password is to be reset. Either triggered by the user or an admin | false

## Connector information

Generate a random session id.

### HTTP request

`GET /login`

## prepare

```json
{
  "kind": "login",
  "title": "",
  "path": "",
  "password": {
      "value": "",
      "former": "",
      "reset": false
  }
}
```

> Example of JSON returned

This method allows you to retrieve a prefilled JSON to log into a connector. You can then fill the required fields with the correct values to login to the connector.

### HTTP request

`POST /login/prepare`

## login

```json
{
  "kind": "login",
  "title": "Administrator",
  "path": "Users/Administrator",
  "password": {
      "value": "4d0a83d823b07ac4ffa730bafec8e70b",
      "former": "",
      "reset": false
  }
}
```

> Example JSON object to login as the user <bold>Administrator</bold> with the encrypted password <bold>365admin</bold>

This method allows to login to a connector. You send a login object and retrieve a session id you can pass with further requests until the session is manually closed or expires.

### HTTP request

`POST /login/:locale`

### Path parameters

Parameter | Default | Description
----------|---------|------------
locale | en_EN | The client's locale.

### Request content

A login object as encrypted JSON.

### Return values

HTTP code | body | description
----------|------|------------
200 | | A session id as a string.
500 | | Login failed. See the message for details on the actual error.

## logoff

Close an active session.

### HTTP request

`DELETE /login/:sessionId`

### Path parameters

Parameter | Default | Description
----------|---------|------------
sessionId | - | The session id you want to log out of.
