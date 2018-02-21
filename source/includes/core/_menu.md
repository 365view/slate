# Menu

In this category you will find information about the connector menu and how to retrieve the menu from a connector.

## The menu object

This object contains the top level structure of the sidebar menu. It wraps the three categories _actors_, _resources_ and _security_.

> An example of JSON for the menu.

```json
{
    "menuActor": {
        ...
    },
    "menuResource": {
        ...
    },
    "menuSecurity": {
        ...
    }
}
```

### Attributes

Name | Type | Description  | Required
-----|------|--------------|---------
menuActor | MenuCategory | Contains menu items related to actors | yes
menuResource | MenuCategory | Contains menu items related to resources | yes
menuSecurity | MenuCategory | Contains menu items related to security | yes

### MenuCategory

An object that contains multiple menu items.

> Example of category actor.

```json
{
    "menuType": "ACTOR",
    "titleDTO": {
        ...
    },
    "items": [
        ...
    ]
}
```

#### Attributes

Name | Type | Description  | Required
-----|------|--------------|---------
menuType | string | The type of menu. Can have the values <ul><li>ACTOR</li><li>RESOURCE</li><li>SECURITY</li><li>PREFERENCES</li></ul> | yes
titleDTO | ItemTitle | The category title. | yes
items | collection | The menu items within this category | yes

### MenuItem

> An example of menu item with title "groups"

```json
{
    "menuType": "ACTOR",
    "tabType": "VIEW_TREE",
    "dto": {
        ...
    },
    "titleDTO": {

    },
    "prefix": {
        "texts": [
            ...
        ]
    }
}
```

#### Attributes

Name | Type | Description  | Required
-----|------|--------------|---------
menuType | string | The type of menu. Can have the values <ul><li>ACTOR</li><li>RESOURCE</li><li>SECURITY</li><li>PREFERENCES</li></ul> | yes
tabType | string | the type of tab to open. Can have the values <ul><li>VIEW_TREE</li><li>VIEW_LIST</li><li>VIEW_DETAIL</li><li>VIEW_UPDATE</li><li>VIEW_ADD</li><li>VIEW_ASCENDANTS</li><li>VIEW_SECURITY_MATRIX</li><li>VIEW_REPORT</li><li>VIEW_ADD_USER</li><li>VIEW_UPDATE _USER</li></ul> | yes
dto | ObjectDTO | The DTO (?) | yes
titleDTO | ItemTitle | The item's display title. | yes
prefix | ItemTitle | Prefix for the title. Can be used to differentiate two items with the same title. | no

### ItemTitle

This object is the general representation of the title of an item. A title is composed of chunks of text being either a translated string or a localization key the application will have to translate before display.

> Example of a title with a translated chunk _Actors -_ and a chunk to translate using the key _actor.public.groups_

```json
{
    "parts": [
        {
            "part": "Actors - ",
            "translate": false
        },
        {
            "part": "actor.public.groups",
            "translate": true
        }
    ]
}
```

#### Attributes

Name | Type | Description  | Required
-----|------|--------------|---------
parts | array | an array of title part. Assembling the translated parts will give you the title. | yes
parts.part | string | A chunk of the title. It can either be a translated chunk or the localization key to translate this chunk | yes
parts.translate | boolean | A flag indicating whether the chunk must be translated. If yes, the application must find the corresponding string value in the current locale. | yes

## Get Menu

Retrieve the menu structure for the current connector.

### HTTP request

`POST /login/getMenu/:sessionId`

### Path parameters

Parameter | Default | Description
----------|---------|------------
sessionId | - | The active session

### Response

The menu as a JSON object of [Menu](#the-menu-object) type.