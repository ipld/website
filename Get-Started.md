# Tutorial
- This tutorial will guide you from the very basics to real example of IPLD.
- IPLD stands for the Inter Planetary Linked Data
- When we get data from the internet, we know it comes from the right place, if the URL is https (some times not even! TODO: add symantec & fake certificates), but we don’t know if it was the data we intended to get!
- The same problem is with links, we don’t know if a link is still linking to the right page
- Finally, the general problem of the current web is that we trust the source to give us the right data, so other computers in the network cannot really help sharing the same data!
- These 3 properties are secure linking, distributed naming and immutable content!
- Where does IPLD fit into this? IPLD is a data format to describe datasets where data can link to other data via cryptographic hashing, IPLD also provides a path scheme to point to some data in the datasets.
- Let’s unroll what we just said.

# The Basics
## Data model
- Let’s start explaining IPLD and its data model.
- JSON is one of the most popular semi-structured data formats on the Web.
- The key reason JSON has great adoption is their simplicity.
- Although we don’t really “use” JSON in IPLD,  we use the the same notation

```
object1 = {
  name: “Mr. Slippery”,
  job: [“Writer”, “Spy”]
}
// Mr. Slippery is the main character of True Names by Vinge
```

- Alright, this is a JSON object, which is also a valid IPLD object.

### Naming things with hashes
- In IPLD, we refer to objects with their cryptographic hash.
- A cryptographic hash is a one way function (link to wikipedia).
- For any given input it returns a string of bits of the same size.
- Every object has one cryptographic hash.
- It is impossible (well, really really difficult) given a hash to know what content it refers to 
- and given two hashes to know if the content was similar.

- The hash of object1 is XXXX (well, this is a multihash, read more here)
- if we remove the dot from Mr, the hash is YYYY.
- In IPLD cryptographic hashes ensure that the content cannot change.
- If we ask to a network (like the ipfs network) for a hash
- then, they cannot lie about the content that they are giving
- since you can generate the hash of the content received and check if it is the same one to the one that you asked for!

EXERCISE: Write your own IPLD object and use the IPFS command line to add it to the network

EXERCISE: Show how to do that in code too

### Path name into one file

- IPLD offers a path scheme that allow you to link inside an IPLD object (and as we will see later, across IPLD objects)
- Every key is like a folder.
- We can explore the previous object in this way:
- XXXXX/name points to: “Mr. Slippery”
- XXXXX/job/0: writer

## Linking objects

- What about secure linking?
- Differently from standard JSON, we can link two objects by using a special keyword: “/“
- For example:

```
object1 = {
  name: “Mailman”,
}

TODO hash of object1

object2 = {
  name: “Mr. Slippery”,
  job: [“Writer”, “Spy”],
  friends: [{“/”: “YYYY”}]
}

TODO: hash of object2


```

- Links are cryptographic hashes, we often call those merkle links (in reference to Merkle Tree)
- Since we are using cryptographic hashes, then the links can only be one way (link to stackoverlow issue that mentions this)
- The IPLD path scheme can go across objects. For example
- YYYY/friends/0/name would point to Mailman!

EXERCISE: Show How to link objects in JS

## Merkle Dag

- If you have many objects linked to each other, then you are building a Merkle DAG that could look exactly like this
- TODO: Show a big Merkle DAG
- The great feature about IPLD and Merkle Dags is that you only need the hash 
- of the root of the dag to authenticate (“validate the correctness”) of the whole Merkle Dag!

# Dive deep into IPLD
- Let's move beyond toy examples and let's learn more about IPLD
## Path scheme
- An IPLD address is composed by a CID and a path.
- The CID is a content identifier that is composed of X,Y,Z.
- A multihash is not just a hash: it is the hash prefixed by the hashing function and the length of the hash. In this way we can decide to what cryptographic hash function we want to hash the content.
- A multicodec specifies the type of content we are addressing.
- In the examples above we used IPLD+CBOR
- Thanks for recent work in IPLD, we can address natively other type of content. For example, a format we recognize is an Ethereum block. So we can now content address an ethereum block and use our path scheme on it!
## Libraries
- TODO
## Command line

# Practical examples
- blockchain
- filesystem
- social network

# link to other resources
