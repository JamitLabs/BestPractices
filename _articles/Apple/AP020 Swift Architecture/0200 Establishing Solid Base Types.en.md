---
section:    Apple
topic:      Swift Architecture
refid:      AP020-0200
permalink:  /articles/AP020-0200.html
title:      Establishing Solid Base Types
date:       2016-08-30 00:00:00
author:     Florian Pfisterer
---

This article is intended to provide a set of solid base types that you can use in almost any Swift project. These types,
if used intelligently, make your code more clean & succinct, maintainable and easier to unit-test.

## 1. The Result Type

In almost any project, there are calls to functions that might fail. In Swift, the common pattern in this case is that
either the function throws an error or it returns `nil`.
While this might be convenient in some cases, in others you neither want to `throw` and `rethrow` errors in
different nested function calls nor do you want to unwrap optionals all the time.

To solve this problem, consider the following generic Result type:

```swift
enum Result<T>
{
    case success(T)
    case error(ErrorType)
}
```

This type, when used as the result of a function call for example, represents one of the following cases:

1. The call succeeded, and here's the result: an object of type `T`
2. Somewhere there was an error, and here it is: an `ErrorType` case (you could use `NSError` as well)

The Result type basically extends the Swift standard library optional type by providing the error of why there is no
result object (`T`) available.

```swift
enum Optional<T>
{
    case some(T)
    case none
}
```

That's really all there is to Swift optionals, the "**?**"s and "**!**"s are just syntactic sugar.

### Example Usage

Consider the following function that asynchronously loads some users from a remote database (inside of a
`DatabaseClient`, for example):

```swift
static func loadUsers(completion: (Result<[User]>) -> Void)
{
    let queue =
        dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0)

    dispatch_async(queue, {
        do
        {
            // .. heavy network tasks to load the users
            let users: [User] = try networkOperationThatMightThrow()
            completion(.success(users))
        }
        catch let error
        {
            completion(.error(error))
        }
    })
}
```

When we then use this function, in our `ViewController` for example, we neither have to wrap the call in a
`do-try-catch` block, nor do we have to unwrap optionals. Our code stays clean, maintainable and easy to understand.

```swift
DatabaseClient.loadUsers { result in
    switch result
    {
    case .error(let error):
        // present an alert etc.
    case .success(let users):
        // update the UI and display the users
    }
}
```

## 2. A Model Type

Probably most iOS apps save and retrieve objects from a database

If you use Realm or CoreData as your local database, you will be provided with a superclass for your model types
(`Object` in Realm and `NSManagedObject` in CoreData). But if you don't - when making calls to a REST API on a server
for example - you need to create some base type for your model yourself.
Why? Because this prevents repetitions and therefore makes your code more maintainable. Instead of having to write a
`save` function for every single of your model types, they can all share the necessary properties and therefore all use
**one** implementation of a `save` function.

### How to Implement This Base Type?

The first idea might be to just create a `class` named Model, from which all the other types inherit. The Model class
could contain properties like a unique `id` and provide functionality like a `save` function. The problem with this
approach is that most of the time, it's better to use value-type `structs` instead of reference-type `classes`. I won't
go into the details here, but you can read about value vs. reference types in another article **TODO: insert link**.

So how do we share properties and functionality between `structs`? Inheritance is not an option here.

### Using Protocol Extensions

With Swift, we can now write `protocol extensions` to add functionality to `protocols`. Consider the following first
implementation of our Model type:

```swift
protocol Model
{
    var id: String { get }

    var createdAt: NSDate { get }
    var updatedAt: NSDate { get set }
}
```

Now every model type (like customer, poduct, invoice, etc. for example) has to implement these properties. They don't
share any functionality yet, but we'll add that now; with the magic of Swift's `protocol extensions`:

```swift
extension Model
{
    func save(completion: Result<Self>)
    {
        // save the object using the `id` property for example
    }
}
```

We could implement other functions like `update`, `delete`, etc., but the point is that every model type we need to
implement now can use the functionality implemented in the Model `protocol`. Hence, if something changes, we only have
to update our code in **one** place.

### A Little Side Note

If certain model types need to use their own implementation of the `save` function, you have to include the declarations
for this function right in the `protocol` declaration itself, not merely in an `extension`. That way, if we call `save`
on the respective object, it calls the correct implementation.
