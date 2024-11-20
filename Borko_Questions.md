# Answers to Mr Borkos Questions
## Which implementation of diff-patch al?
The one provided with the fast-json-patch package
## Where are the documents and the shadows?
```js
    /**
     * @param {Object} options.jsonpatch json-fast-patch library instance (REQUIRED)
     * @param {String} options.thisVersion version tag of the receiving end
     * @param {String} options.senderVersion version tag of the sending end
     * @param {Boolean} options.useBackup indicate if use backup copy (DEFAULT true)
     * @param {Boolean} options.debug indicate if print out debug message (DEFAULT false)
     */
    constructor({ jsonpatch, thisVersion, senderVersion, useBackup = true, debug = false }) {
        if (!jsonpatch) {
            throw "jsonpatch instance is required";
        }
        this.jsonpatch = jsonpatch;
        this.thisVersion = thisVersion;
        this.senderVersion = senderVersion;
        this.useBackup = useBackup;
        this.debug = debug;
    }
```
in this constructor they are defined and then stored in memory as there is no persistence

## How and why can we adjust hte sync-cycle? what are the dis-/advanteages?

More Syncs faster updates, more traffic

currently the cycle is event driven instead of time driven, meaning to change the time between syncs one either would have to implement a wait time or change it to be time based

## Where/How was the edit stack implemented?

the stack is called shadows.edits internally example of this:
```js
shadow.edits.push({
    [thisVersion]: shadow[thisVersion],
    patch // The generated diff
});

```


## Is it possible to deploy a Peer-to-Peer version of this implementation?

Currently this is not possible as it explicitly uses the server in the client code, it could however be changed with relatively little work as every clienratt could also act like a server in a multi server configuration.

## How is it possible to use the API in other JS-Projects?

there is  a websocket over which the api requests can be transmitted, theoretically as long as the client can interpret the api anybody can use it.
examples of such requests:
```json
{
  "type": "JOIN",
  "payload": {
    "name": "Alice" // Optional: Client's name
  }
}

```

## Are the JSON-Docs interchangeable with other docs?
not currently as it uses a json patch and diff library, with a different library it would theoretically be possible to use a different type of document.

## How are the conflicts solved?
Using the fast-json-patch library

## What are possible improvements to this code?
In the client there is a XSS vulnerabilty in the text boy where new users can write their name. This has been fixed in a already submitted pull request

## Is it possible to combine this implementation with a NOSQL Doc Store?

Yes, but not without changing the code. This shouldn't be to hard however as these stores already support JSON Docs.