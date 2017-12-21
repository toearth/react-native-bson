# This is bson for react-native [![npm version](https://badge.fury.io/js/react-native-bson.svg)]() [![npm](https://img.shields.io/npm/dy/react-native-bson.svg)]()

The method in [bson](https://github.com/mongodb/js-bson) dose not work for me. I got the way from the test code.

If you don't yet know what BSON actually is, read [the spec](http://bsonspec.org).

This package can be used to serialize JSON documents into the BSON format or the other way around. If you want to use it within the browser, give [browserify](https://github.com/substack/node-browserify) a try (it will help you add this package to your bundle). The current build is located in the `browser_build/bson.js` file.

A simple example of how to use BSON in `react-native`:

```js

import {BSON,Long, ObjectID,Binary,Code,DBRef,Symbol,Double,Timestamp,MaxKey,MinKey} from 'bson'
import {Buffer} from 'buffer'

var bson = new BSON([Long, ObjectID, Binary, Code, DBRef, Symbol, Double, Timestamp, MaxKey, MinKey])
var user = {
    username: 'username',
    password: 'anymore'
}

// Serialize a document
var data = bson.serialize(user, false, true, false)
console.log('data:', data)

// Deserialize the resulting Buffer
// response is return from server by library react-native-fetch-blob
// the type of response.text() is base64
// so far forgot other choices, only leave this code
var userData = bson.deserialize(new Buffer(response.text(),'binary'))
console.log('userData:', userData)
```

## API

The API consists of two simple methods to serialize/deserialize objects to/from BSON format:
=======
## Installation

`npm i react-native-bson`

## API

### BSON serialization and deserialiation

**`new bson.BSONPure.BSON()`** - Creates a new BSON seralizer/deserializer you can use to serialize and deserialize BSON.

  * BSON.serialize(object, checkKeys, asBuffer, serializeFunctions)
     * @param {Object} object the Javascript object to serialize.
     * @param {Boolean} checkKeys the serializer will check if keys are valid.
     * @param {Boolean} asBuffer return the serialized object as a Buffer object **(ignore)**.
     * @param {Boolean} serializeFunctions serialize the javascript functions **(default:false)**
     * @return {TypedArray/Array} returns a TypedArray or Array depending on what your browser supports

  * BSON.deserialize(buffer, options, isArray)
     * Options
       * **evalFunctions** {Boolean, default:false}, evaluate functions in the BSON document scoped to the object deserialized.
       * **cacheFunctions** {Boolean, default:false}, cache evaluated functions for reuse.
       * **cacheFunctionsCrc32** {Boolean, default:false}, use a crc32 code for caching, otherwise use the string of the function.
       * **promoteBuffers** {Boolean, default:false}, deserialize Binary data directly into node.js Buffer object.
     * @param {TypedArray/Array} a TypedArray/Array containing the BSON data
     * @param {Object} [options] additional options used for the deserialization.
     * @param {Boolean} [isArray] ignore used for recursive parsing.
     * @return {Object} returns the deserialized Javascript Object.

### ObjectId

**`bson.ObjectId.isValid(id)`** - Returns true if `id` is a valid number or hexadecimal string representing an ObjectId.
**`bson.ObjectId.createFromHexString(hexString)`** - Returns the ObjectId the `hexString` represents.
**`bson.ObjectId.createFromTime(time)`** - Returns an ObjectId containing the passed time.
* `time` - A Unix timestamp (number of seconds since the epoch).

**`var objectId = new bson.ObjectId(id)`** - Creates a new `ObjectId`.
* `id` - Must either be a 24-character hex string or a 12 byte binary string.

**`objectId.toJSON()`**
**`objectId.toString()`**
**`objectId.toHexString()`** - Returns a hexadecimal string representation of the ObjectId.

**`objectId.equals(otherObjectId)`** - Returns true if the ObjectIds are the same, false otherwise.

**`objectId.getTimestamp()`** - Returns a `Date` object containing the time the objectId was created for.

**`objectId.getTimestamp()`** - Returns a `Date` object containing the time the objectId contains.
