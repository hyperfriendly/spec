#hyperfriendly+json

A simple and extensible JSON hypermedia format.

**media-type:** "vnd/hyperfriendly+json"

The hyperfriendly+json spec adds link semantics to a JSON object. Its semantics are extendable by adding resource- and link profiles. Anyone can extend hyperfriendly+json with their own profiles.

For an introduction to hyperfriendly+json, see the [wiki][wiki]

##Links

A hyperfriendly+json object MUST contain a _links object. The _links object contains rels with one or more links. If a rel contains a single link it MUST be represented as an JSON object. If a rel contains multiple links it MUST be represented as a JSON array.

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

##Link profiles

The semantics of a link can be extended by adding a *link profile* to the OPTIONAL linkProfiles array. A link profile MUST be a valid URI.

 * [Templated link][templated]
 * [Method hint][methodhint]
 * [JSON Schema][jsonschema]

##Profiles

The semantics of an entire hyperfriendly+json resource can be added through profiles. Setting a profile is as simple as adding one or more *profile* links pointing to a special URI.

 * [Collection][collection]
 * [Error][error]
 * [Feed][feed]

[wiki]: https://github.com/hyperfriendly/spec/wiki/Home
[templated]: http://profiles.hyperfriendly.net/templated-link
[methodhint]: http://profiles.hyperfriendly.net/method-hint
[jsonschema]: http://profiles.hyperfriendly.net/json-schema
[collection]: http://profiles.hyperfriendly.net/collection
[error]: http://profiles.hyperfriendly.net/error
[feed]: http://profiles.hyperfriendly.net/feed
