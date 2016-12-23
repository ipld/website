# Tutorial

In this tutorial, you will learn how to use IPLD. We'll start from the very basics, and end with some examples.

IPLD stands for InterPlanetary Linked Data. This is based off of [IPFS](https://ipfs.io), which is the InterPlanetary File System. IPFS starts with a desire to have filesystems that work even when there are long delays involved (such as would be the case with interplanetary space travel!). IPLD builds on top of this by allowing us to reference data in IPFS - or elsewhere.

When we get data from the internet by accessing a webpage in our browser, we know it comes from the right place if the URL is https. This is because the HTTPS protocol gives some security that the data hasn't been compromised, for instance by a man-in-the-middle attack. (Of course, sometimes not even HTTPS is enough! TODO: add symantec & fake certificates). However, while we know that the data comes from the right place, we _don’t_ know if it was the data we intended to get!

The same problem occurs with links: we don’t know if a link is still linking to the right page when we follow it. Someone may have removed the page, or changed it, or set up a redirect. Finally, the general problem of the current web is that we trust the source to give us the right data. This works fine - but it puts a bottleneck on where you can get the data. Other computers in the network cannot really help with sharing the load of serving data to you, even if they have the data themselves. This is inefficient and suboptimal.

There are three things that would solve each of these issues: secure linking, distributed naming and immutable content. Where does IPLD fit into this? Well, IPLD satisfies each of these three properties. IPLD is a data format to describe datasets where data can link to other data via cryptographic hashing. IPLD also provides a path scheme to point to some data in the datasets.

Let’s unroll what we just said, to make this more clear.

# The Basics

## Data model

Let’s start by explaining IPLD and its data model.

JSON (JavaScript Object Notation) is one of the most popular semi-structured data formats on the Web. The key reason JSON has great adoption is its simplicity. Although we don’t really "use" JSON in IPLD,  we use the the same notation.

Let's take an example: here is Mr. Slippery, one of the main characters of _True Names_ by Vinge, in JSON:

```js
var object1 = {
  name: "Mr. Slippery",
  job: ["Writer", "Spy"]
}
```

This is a JSON object (or, specifically, the variable `object1` here is assigned to the value of a JSON object). This is _also_ a valid IPLD object.

### Naming things with hashes

In IPLD, we refer to objects with their cryptographic hash. A cryptographic hash is the result of a [one way function](https://en.wikipedia.org/wiki/One-way_function). For any given input, it returns a string of bits. Thst string is always the same size as any other returned string by the function, and looks like this: `QmT78zSuBmuS4z925WZfrqQ1qHaJ56DQaTfyMUF7F8ff5o`. Normally, the string is a combination of symbols that look random. Importantly, every object has only one cryptographic hash that can be returned by a given cryptographic function. This means you could put the same input through a cryptographic function a million times, and it would always return the same result.

Also importantly, it is impossible (well, really really difficult) to tell, given a hash, what content it refers to. That is why it is a one way function - data doesn't flow both ways. You also can't tell, given two hashes, if the content was similar.

The hash of `object1` is [`QmW2FHoz1kuYMVHcBoHWBsBXNXqC9VTZi8HNjb5Bm81Saw`](https://ipfs.io/ipfs/QmW2FHoz1kuYMVHcBoHWBsBXNXqC9VTZi8HNjb5Bm81Saw). (Well, this is a [multihash](https://github.com/multiformats/multihash).) If we remove `Mr`, the hash is [`QmXeCqMmgkb73xUxK9bZqM7iQXsLTUvtzcCkmZ5oCYHT1e`](https://ipfs.io/ipfs/QmXeCqMmgkb73xUxK9bZqM7iQXsLTUvtzcCkmZ5oCYHT1e). The hashes are completely different!

In IPLD, cryptographic hashes ensure that the content cannot change. If we ask a network (like the IPFS network) for a hash, then, they cannot lie about the content that they are giving, since you can generate the hash of the content received and check if it is the same as the one that you asked for!

EXERCISE: Write your own IPLD object and use the IPFS command line to add it to the network

EXERCISE: Show how to do that in code too

### Path name into one file

IPLD offers a path scheme that allow you to link inside an IPLD object (and as we will see later, across IPLD objects). In IPLD, Every key is like a folder. We can explore the previous object in this way: `QmW2FHoz1kuYMVHcBoHWBsBXNXqC9VTZi8HNjb5Bm81Saw/name` points to: `"Mr. Slippery"`. By adding the name of the key at the end of the hash, you are able to refer to that key in the given object. You can also use indices: `QmW2FHoz1kuYMVHcBoHWBsBXNXqC9VTZi8HNjb5Bm81Saw/job/0` points to `"Writer"`.

## Linking objects

What about secure linking? Differently from standard JSON, we can link two objects by using a special keyword: `/`.

For example:

```js
object1 = {
  name: "Mailman",
}
```

The hash for this object, without the variable assignment of `object1 = `, is [`QmVNZm1m2XHpu58mGpe9s3EQHD3z5fp7nF2iqZLXwy1Cgb`](https://ipfs.io/ipfs/QmVNZm1m2XHpu58mGpe9s3EQHD3z5fp7nF2iqZLXwy1Cgb).

```js
object2 = {
  name: "Mr. Slippery",
  job: ["Writer", "Spy"],
  friends: [{"/": "YYYY"}]
}
```

This hash is [`QmRu7RdtqEgvro4FXLDhXyL3ykgpua9iDA28uRLf236JCD`](https://ipfs.io/ipfs/QmRu7RdtqEgvro4FXLDhXyL3ykgpua9iDA28uRLf236JCD).


Links are cryptographic hashes. We often call those Merkle links (in reference to a [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree)). Since we are using cryptographic hashes, then the links can only be one way. This has to be the case; if one object is the input for another, then the second object cannot be used as the input for the first object, since then the first object would have to change. You would end up in a constantly updating feedback loop if you tried to keep updating the first object to include the second object, and the second to include the first.

TODO Link to stackoverlow issue that mentions this.

The IPLD path scheme affords the ability to point one way links across objects. For example, `QmRu7RdtqEgvro4FXLDhXyL3ykgpua9iDA28uRLf236JCD/friends/0/name` would point to `"Mailman"`!

EXERCISE: Show How to link objects in JS

## Merkle DAG

If you have many objects linked to each other, then you are building a Merkle DAG (directed acyclic graph) that could look exactly like this:

TODO: Show a big Merkle DAG

The great feature about IPLD and Merkle DAGs is that you only need the hash of the root of the dag to authenticate ("validate the correctness") of the whole Merkle DAG!

# Dive deep into IPLD

Let's move beyond these examples and let's learn more about IPLD.

## Path scheme

An IPLD address is composed by a CID and a path. The CID is a content identifier that is composed of X,Y,Z. TODO Clarify this.

Earlier, we parenthetically mentioned [multihash](https://github.com/multiformats/multihash). A multihash is a self-describing hash which conforms to the multihash specification. It is not just a hash: it is the hash prefixed by the hashing function and the length of the hash. This means that if the cryptographic hash function changes, the hash will also change; this affords us the ability to to decide what cryptographic hash function we want to use when we hash content. This is incredibly useful; otherwise, we would be locked in to using a particular hash function. Functionally, a multihash can be used anywhere a normal hash would.

A [multicodec](https://github.com/multiformats/multicodec) - yes, it sounds similar, because it is part of a suite of formats described by [multiformats](https://github.com/multiformats/multiformats) - specifies the type of content we are addressing.

In the examples above we used IPLD+CBOR. Thanks to recent work in IPLD, we can natively address other types of content. For example, a format we recognize is an Ethereum block. So we can now content address an ethereum block and use our path scheme on it!

TODO Show example

## Libraries
- TODO
## Command line

# Practical examples
- blockchain
- filesystem
- social network

# link to other resources
