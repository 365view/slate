# Menu

## The menu object

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

Attribute | type | Description  | mandatory
----------|------|--------------|----------
menuActor | MenuCategory | Contains menu items related to actors | true
menuResource | MenuCategory | Contains menu items related to resources | true
menuSecurity | MenuCategory | Contains menu items related to security | true

### MenuCategory

> An example of category actor with title "System actors"

```json
{
    "menuType": "ACTOR",
    "titleDTO": {
        "parts": [
            {
                "part": "System actors",
                "translate": false
            }
        ]
    },
    "items": [
        {
            ...
        },
        { 
            ...
        }
    ]
}
```
#### Attributes

Attribute | type | Description  | mandatory
----------|------|--------------|----------
menuType | string | The type of menu. Can have the values <ul><li>ACTOR</li><li>RESOURCE</li><li>SECURITY</li><li>PREFERENCES</li></ul> | true
titleDTO | ItemTitle | The category title. | true
items | collection | The menu items within this category | true

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
            "groups"
        ]
    }
}
```

#### Attributes

Attribute | type | Description  | mandatory
----------|------|--------------|----------
menuType | string | The type of menu. Can have the values <ul><li>ACTOR</li><li>RESOURCE</li><li>SECURITY</li><li>PREFERENCES</li></ul> | true
tabType | string | the type of tab to open. Can have the values <ul><li>VIEW_TREE</li><li>VIEW_LIST</li><li>VIEW_DETAIL</li><li>VIEW_UPDATE</li><li>VIEW_ADD</li><li>VIEW_ASCENDANTS</li><li>VIEW_SECURITY_MATRIX</li><li>VIEW_REPORT</li><li>VIEW_ADD_USER</li><li>VIEW_UPDATE _USER</li></ul> | true
dto | ObjectDTO | The DTO (?) | true
prefix | ItemTitle | an object containing an array of string that defines the localization key for the menu item. | true

## Get Menu

Retrieve the menu.

### HTTP request

`POST /login/getMenu/:sessionId`

### Path parameters

Parameter | Default | Description
----------|---------|------------
sessionId | - | The active session.