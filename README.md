#hyperfriendly+json

A simple and friendly JSON hypermedia format inspired by [hyper+json](https://github.com/cainus/hyper-json-spec).

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

##With links

A hyperfriendly+json object MAY contain a _links object. If present, this object MUST contain at least one link. A link MUST contain a href which can be an absolute or a releative uri.

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

##Templated urls

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

##Profile

A link MAY contain a profile element which MUST be a valid URI. The URI SHOULD be dereferencable to a documentation in a given format (e.g. html, plain text, json schema, etc.)

```javascript
{
  "_links": {
    "create": {
      "href": "/users",
      "method": "POST",
      "profile": "http://schema.hyperfriendly.net/collection"
    }
  }
}
```
