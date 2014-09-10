#hyperfriendly+json

A simple and friendly JSON hypermedia format.

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

A hyperfriendly+json object MAY contain a _links object. If present, this object MUST contain at least one link. A link MUST contain a href which can be an absolute or a releative URI.

```javascript
{
  "_links": {
    "self": {
      "href": "/users/1"
    },
    "friends": {
      "href": "/friends/1"
    }
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

###Type
A link MAY contain a type element describing how to interact with the resource. (See section on types for more information)

```javascript
{
  "_links": {
    "users": {
      "href": "/users",
      "type": "http://api.example.com/types/user"
    }
  }
}
```



###Method
A link MAY contain a *method* element that describes which http method should be used to interact with the resource.

```javascript
{
  "_links": {
    "add_user": {
      "href": "/users",
      "method": "POST"
    }
  }
}
```

##Types
A resource MAY contain a *type* element which MUST be a valid URI. The URI SHOULD be dereferencable to a documentation of the expected payload (e.g. json schema, html, plain text, etc).

```javascript
{
  "_links": {
    "type": {
      "href": "http://api.example.com/types/user"
    }
  }
}
```

##Profiles

Semantics can be added to a hyperfriendly+json resource through profiles. Setting a profile is as simple as adding a *profile* link with a special URI. Some profiles have already been defined and are listed below.

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
