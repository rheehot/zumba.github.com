---
layout: post
title: Introducing Json Serializer - An open source library to serialize to JSON
description: Serialize PHP content in JSON, with support to unserialization, even for objects.
tags: [json, serializer, serialization, integration]
author: jrbasso
---

After the release of some of our libraries, such as [Symbiosis](https://github.com/zumba/symbiosis),
[MongoUnit](https://github.com/zumba/mongounit) and [CSV Policy](https://github.com/zumba/csv-policy),
we are proud to announce our newest open source project: [Json Serializer](https://github.com/zumba/json-serializer).
The Json Serializer&ndash;as the name suggests&ndash;serializes content in JSON format. It is a very simple concept, but very
useful in some cases.

### Use Case

Applications are becoming increasingly more complex, and companies are worried about their consumers. At Zumba we are no
different. Our ecommerce system handles thousands of consumers daily and we have many new features coming. Without
a decoupled and maintainable system we can't grow very fast.

In our shopping cart we implement some entity classes and some other classes to group the entities together. For example,
we have entities to represent the cart item, the cart message, the promotions/coupons applied, etc. All these entities
are associated to a main cart entity that represents the consumer cart. This cart entity is stored in our database
so users can switch between devices and/or come back a few days later and add the items to the cart, etc. The system does not
rely on the user session.

For some time we just serialized the cart using the PHP `serialize()` function and stored it in a binary field in
a MongoDB collection. It worked very well until we had to extract the data to use in marketing emails, analytics, etc.
The serialize function doesn't generate an easily encoded string that is easy to parse by another system that is not PHP
based. We couldn't keep creating code around our shopping cart to have analytics or marketing information either. The
solution was to store the cart in a format that would be easy for PHP to quickly serialize/unserialize and
also easy to integrate with partners or different tools. The solution was to encode the entities in
JSON.

JSON is very well supported by many languages and tools. Since we are using MongoDB to store the carts we can easily
generate queries directly from MongoDB to have some statistics. This change decreased the performance of managing carts,
but in the main scope it was a gain.

### What about `JsonSerializable` in PHP 5.4?

The [`JsonSerializable` interface](http://php.net/JsonSerializable) is used with the `json_encode()` function to serialize an
object. This means it is very useful to convert your class to JSON when you are
responding with the entity to your API, for example; however, it doesn't support unserialization.

### What about using `json_encode` with the `Serializable` interface?

Nope. If your entity implements `Serializable` you can define how to serialize and unserialize, but PHP wraps their
serializable code around it which means the output is not 100% JSON.

### Hmmm, so what about JMS Serializer?

[JMS Serializer](http://jmsyst.com/libs/serializer) is a great library and does serialize to JSON, but it doesn't have
as many features as this library. For example, when you de-serialize, you have to pass the class where the
data was generated. Also, they don't support nested encoding.

### Why not just create a method to aggregate all the data and respond as JSON?

Yes, you can. But are you sure you want to make a method to cover all situations and nested levels to aggregate
everything? In our case it would be painful and would require more code than this library. If it works for you,
go for it. You probably will get better performance, and it will probably be easier to store the JSON however you want
(I mean not in the same structure of your classes, as the `JsonSerializer` does).

### You are convincing me, but I have so much crap on my classes. Can I ignore some of the properties?

Not a problem. The library supports the magic methods `__sleep()` and `__wakeup()`.  In each class you can
define what will be stored in the JSON and what to execute when unserialized. It works exactly the same way as the
`serialize()` and `unserialize()` functions.

### Ok, but my code is a little complex. I have some double linked lists, some recursing objects, etc.

This is also not a problem. The library handles code recursion. You can call the serialize method and enjoy it.

### F\*cking amazing, but I doubt it will work in my old environment.

The library supports PHP 5.3.6 and above. We don't need any other dependency. If you are using any PHP version before
that, stop now, read some blogs, and go upgrade your system! PHP 5.3 only has a few months of support left.

### Is the library compatible with `Serializable` or `JsonSerializable`?

Yep. Each one has different functionality, so everything is fine. You can still use `Serializable` with
`serialize()` and `unserialize()` functions. You can use `JsonSerializable` with `json_encode()` to respond to your
API and our library to serialize the object for internal storage.

### You are talking too much; you're like a car salesmen. Do you have some code to show?

Indeed, let's go to an example:

<script src="https://gist.github.com/jrbasso/e0801a89bacad0262189.js?file=json_serializer.php"> </script>

Is that clear?

### Wow, amazing! How can I get it?

You can install via composer (package `zumba/json-serializer`), or download directly from [GitHub](https://github.com/zumba/json-serializer).
If you find something wrong, or something that we can improve, feel free to open a [GitHub issue](https://github.com/zumba/json-serializer/issues).
If you want to make a pull request, even better. :)

You are welcome to check our other open source projects on [https://github.com/zumba](https://github.com/zumba).
