#hyperfriendly+json

A simple and extensible JSON hypermedia format. 

The hyperfriendly+json spec adds link semantics to a JSON object but is also extendable by using profiles.

**media-type:** "vnd/hyperfriendly+json"

##Vanilla JSON

hyperfriendly+json is a superset of JSON. The following is therefore valid hyperfriendly+json.

```javascript
{
  "firstName": "Bob",
  "lastName": "Anderson",
  "address": {
    "street": "Somestreet",
    "postalCode": "1337",
    "city": "Leetville"
  }
}
```

##Links

A hyperfriendly+json object MAY contain a _links object. If present, this object MUST contain at least one rel containing one or more links. If a rel contains a single link it MUST be represented as an JSON object. If a rel contains multiple links it MUST be represented as a JSON array.

A link MUST contain a href which can be an absolute or a releative URI.

```javascript
{
  "_links": {
    "self": {
      "href": "/users/1"
    },
    "friends": {
      "href": "/friends/1"
    },
    "alternate": [
      {
        "href": "/people/1"
      },
      {
        "href": "/customers/1"
      }
    ]
  },
  "firstName": "Bob",
  "lastName": "Anderson",
  "address": {
    "street": "Somestreet",
    "postalCode": "1337",
    "city": "Leetville"
  }
}
```

###Templated links

Links MAY contain templated uris conforming to the [spec](http://tools.ietf.org/html/rfc6570)

```javascript
{
  "_links" : {
    "byName": {
      "href": "/users{?name}"
    }
  }
}
```

##Profiles

Semantics can be added to a hyperfriendly+json resource through profiles. Setting a profile is as simple as adding one or more *profile* links pointing to a special URI. Anyone can extend hyperfriendly+json with their own profiles.

Some profiles have already been defined and are listed below.

###Collection

**PROFILE:** http://profiles.hyperfriendly.net/collection

A collection resource MUST contain an _items array. The collection resource MAY contain a next link and MAY contain a prev link.

```javascript
{
  "_links" : {
    "profile": {
      "href": "http://profiles.hyperfriendly.net/collection"
    },
    "self": {
      "href": "/users/page=2"
    },
    "next": {
      "href": "/users?page=3"
    },
    "prev": {
      "href": "/users?page=1"
    }
  },
  "_items": [
    {
      "_links": {
        "self": {
          "href": "/users/11"
        }
      },
      "name": "A user"
    },
    {
      "_links": {
        "self": {
          "href": "/users/12"
        }
      },
      "name": "Another user"
    }
  ]
}
```

###Error

**PROFILE:** http://profiles.hyperfriendly.net/error

An error resource MUST contain an _errors collection. A error item MUST have a title and a message.

```javascript
{
  "_links" : {
    "profile": "http://profiles.hyperfriendly.net/error"
  },
  "_errors": [
    {
      "title": "Some error",
      "message": "Some error has occurred"
    }
  ]
}
```

###Method hint

**PROFILE:** http://profiles.hyperfriendly.net/method-hint

The method hint profile adds semantics to a link in the following way.

A link MAY contain a *method* element that describes which http method should be used to interact with the resource.

```javascript
{
  "_links": {
    "profile": {
      "href": "http://profiles.hyperfriendly.net/method-hint"
    },
    "add_user": {
      "href": "/users",
      "method": "POST"
    }
  }
}
```

###Schema

**PROFILE:** http://profiles.hyperfriendly.net/json-schema

The json schema profile adds the following semantics to a link.

A link MAY contain a schema element conforming to the [json schema spec](http://json-schema.org/)

```javascript
{
  "_links": {
    "profile": {
      "href": "http://profiles.hyperfriendly.net/json-schema"
    },
    "create": {
      "href": "/users",
      "method": "POST",
      "schema": {
        "title": "Create user",
        "type": "object",
        "properties": {
          "firstName": {
            "type": "string",
            "required": true
          },
          "lastName": {
            "type": "string",
            "required": true
          },
          "address": {
            "type": "object",
            "properties": {
              "street": {
                "type": "string"
              },
              "postalCode": {
                "type": "integer",
                "minimum": 0,
                "maximum": 9999
              }
            }
          }
        }
      }
    }
  }
}
```

Json schema also supports referencing other schema.

```javascript
{
  "_links": {
    "profile": {
      "href": "http://profiles.hyperfriendly.net/json-schema"
    },
    "create": {
      "href": "/users",
      "method": "POST",
      "schema": {
        "$ref": "http://api.test.com/schema/new-user.json"
      }
    }
  }
}
```

###Feed

**PROFILE:** http://profiles.hyperfriendly.net/feed

A feed is a special collection where the payload is an event wrapped in a special envelope. It contains a history of events and can be chained using next and prev links.

The envelope MUST contain a messageType, sequenceNumber and a body object. It MAY contain a headers object with metadata.

```javascript
{
  "_links" : {
    "profile": "http://profiles.hyperfriendly.net/feed"
  },
  "_items": [
  {
    "messageType": "SomethingHappened",
    "sequenceNumber": 1,
    "body": {
      "foo": 1,
      "bar": "something"
    },
    "headers": {
      "meta": "data"
    }
  }
  ]
}
```
