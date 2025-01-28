# 100 Days Of Code - Log

- [Day 1: October 24, 2024](#day-1-october-24-2024)
  - [Today's Progress](#todays-progress)
  - [Thoughts on GitHub Profile pages](#thoughts-on-github-profile-pages)
  - [Thoughts on Prototyping](#thoughts-on-prototyping)
  - [Link to work](#link-to-work)
- [Day 2: October 25, 2024](#day-2-october-25-2024)
  - [Today's Progress](#todays-progress-1)
  - [Thoughts on Architecture](#thoughts-on-architecture)
  - [Link to work](#link-to-work-1)

## Day 1: October 24, 2024

### Today's Progress

1. Created my GitHub Profile page.
1. Revisited building prototypes using Xcode's Interface Builder and faking implementation
   details using animations.

### Thoughts on GitHub Profile pages

I enjoyed creating my GitHub Profile page. Writing copy takes way more brain power than I
expected it would—just thinking about word choice and the different tones they evoke.

### Thoughts on Prototyping

I’m currently implementing a vertically sliced spike of my weather app, Maeve, in order to get
a minimum viable product (MVP) up and running as quickly as possible. My focus is on designing
the user interface and simulating expected features so that I can dedicate the bulk of my time
implementing niche features that will attract users. For instance, I can use animations and
static data to mimic loading weather data from a remote API, but then actually implement
whatever needs to be implemented to have it tell the user what to wear that day.

### Link to work

- [My GitHub Profile Page](https://github.com/webdavis/webdavis)
- [Maeve](https://github.com/webdavis/Maeve)

## Day 2: October 25, 2024

### Today's Progress

1. Created a Bento profile.
1. Practiced implementing dependency diagrams.
1. Implemented a module-based dependency diagram for Maeve.

### Thoughts on Architecture

For this round I'm implementing a vertically sliced spike so that I know what is and isn't
possible for this app.

As a result, I won't be test-driving it. That's actually beneficial here because for UI modules
Xcode boots the simulator every time tests are run, which makes development painfully slow.
(Not writing tests avoids that 🤣.)

↕️  Since it's a vertical slice, a monolithic architecture makes the most sense.

💡 However, that doesn't mean I won't attempt to make thoughtful architectural decisions.

Even though the code won't be divided into separate physical modules, I can still organize
concerns using folders and treat them as "virtual modules." For example, `<Weather Service>` is
a clear boundary that facilitates communication between the other modules.

### Link to work

- [My Bento Profile](https://bento.me/webdavis)
- [Add module-based dependency diagram as SVG](https://github.com/webdavis/Maeve/commit/2290ab56eaf22adbc116bbb7bade15fd024a177b)

## Day 1 (Restarting): January 10, 2025

### Today's Progress

1. I went back through the first few lectures from the Networking Module on iOS Lead
   Essentials. I took notes on the following concepts:
   - TDD: Start by Testing Initialization
   - Tests Should Start and End in a Clean State
   - Protocols／Interfaces Begin as Guidelines
   - Locating-Clients-in-Memory versus Being-Given-a-Client
   - Test Spies Should Track Message Values, Invocation Count, and Order
   - Mixing Stubbing and Capturing Values: A Code Smell
   - Refactoring an Array to a Computed Property
   - Designing Better Code with Enums: Make Invalid Paths Non-Representable

### Locating-Clients-in-Memory versus Being-Given-a-Client

Making HTTP clients Singletons／singletons (lowercase "s") is an anti-pattern.

Using HTTP clients as Singletons means the client is globally _located in memory_, making it
accessible throughout the system. This creates `hidden dependencies` that are hard to track,
as different parts of the system can interact with the client in unpredictable ways. Managing
its state and behavior becomes challenging as the system grows.

In contrast, _being given a client_—by injecting it—provides explicit control over where and
how it’s created and used. This approach improves maintainability, testability, and clarity, as
dependencies are clear and easier to manage. By avoiding Singletons, we make the system more
predictable and manageable.

#### How this relates to the Open/Closed Principle

The Open/Closed Principle (OCP) states that software should be **open for extension** but
**closed for modification**.

Using HTTP clients as Singletons can violate this principle because it introduces hidden
dependencies and tightly couples components to the client. This makes it difficult to extend or
modify behavior without altering the global state or the parts of the system relying on it.

By injecting clients, you allow the system to be _open for extension_—new behavior can be
introduced by providing different implementations of the client without changing existing code.

When you inject a client into a component, you’re giving that component the ability to interact
with the client without tightly coupling it to a specific instance. This means you can change
which client the component uses (for example, switching from a `ClientSpy` in tests to a real
HTTP `Client` in production) without needing to change the component itself.

So, *closed for modification* means you don’t need to alter the component’s code to change its
behavior (like switching clients). You only need to modify the **client** or how you provide
it, not the component that uses it.

This makes the code:

- **More flexible:** because you can swap out implementations.
- **More maintainable:** because the components don’t need updates when clients change
- **Easier to test:** because you can inject mock clients or different implementations without
  touching the component itself.

In short, injecting clients lets you extend the system (by adding new clients) without changing
existing code, which is exactly what the Open/Closed Principle advocates.

### Link to work

- [GitHub: webdavis/essential-feed-case-study](https://github.com/webdavis/essential-feed-case-study)

## Day 2: January 12, 2025

### Today's Progress

1. I reviewed a particularly difficult lecture from the iOS Lead
   Essentials Networking module. I found the following topics interesting:
   - Behaviors that should be verified when TDDing HTTP Clients
   - Adding Tests Might NOT Alter Production Code
   - How to Represent an Empty JSON List as a String
   - Using `reduce` to Remove `nil` Values from a dictionary, `compactMapValues` can do the
     same thing with less code
   - Implementing `Decodable` can Implicitly Couple Modules

### Implementing Decodable can Implicitly Couple Modules

Implementing `Decodable` directly on our Interface/Boundary model may unintentionally couple
our API module to other modules, such as the caching or database module.

To prevent this, we can map the API model to the interface model within the API module itself.
This keeps each module independent and avoids leaking implementation details.

### Link to work

- [GitHub: webdavis/essential-feed-case-study: Commit 695abde](https://github.com/webdavis/essential-feed-case-study/commit/695abdebf667e9839c09303ab3dc8dae1a4d9740)

## Day 3: January 14, 2025

### Today's Progress

1. Wrapped my head around protocols, closures, and delegates.

### Protocols/Closures are Behavioral Contracts and Delegates Decouple the Class from the Behavior

Let's make it make sense!

Functions are Closures, and Closures are Unnamed Functions.

Here is a function:

```swift
func convertStringToInt(_ text: String) -> Int {
    guard let number = Int(text) else {
        fatalError("'\(text)' is not a valid number.")
    }
    return number
}
```

What is the signature of this function? 🤔

The signature is `(String) -> Int`.

Now let's define a closure that does the same thing:

```swift
func convertStringToInt: (String) -> Int = { text in
    guard let number = Int(string) else {
        fatalError("'\(string)' is not a valid number.")
    }
    return number
}
```

What's the signature of this closure?

Again, the signature is `(String) -> Int`.

So they do exactly the same thing, but they just look a little different.

🤔 What do they have in common?

The signature!

What's another way to pass around a signature?

By using a protocol!

```swift
protocol StringToInt {
    func convert(_ text: String) -> Int
}
```

Protocols are just closures. If we need a contract that requires more than one behavior, then
it may make more sense to use one protocol with two functions instead of two separate closures.
It's really just a preference 🤷.

Now, how does all of this relate to Delegates?

Well, Delegates are just protocols or closures. They are an abstraction on a client, so that we
can extend it's functionality. If we want the client to have a different behavior, then we just
implement the protocol or closure.

Apple uses delegates to extend the functionality of it's view controllers.

View controller's have views, and those views have delegates and datasources in the form of
protocols.

For example, `UIKit.UITableViewController` comes with a `UITableView` property, and
`UITableView` includes a DataSource／Delegate.

Without it, a view controller's behavior would always be fixed!

Apple implemented this pattern to allow us to customize a view controller’s behavior. By
_implementing_ the DataSource／Delegate, we can define exactly how it should function. In other
words, it's a *boundary for behavior*.

If we want to add more behavior to the view controller then we just add another another adapter
that implements the delegate:

```swift
// MARK: - Custom DataSource
class TableViewDataSource: NSObject, UITableViewDataSource {
  let data: [String]
  
  init(data: [String]) {
    self.data = data
  }

  func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return data.count
  }
  
  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
    cell.textLabel?.text = data[indexPath.row]
    return cell
  }
}

// MARK: - Custom Delegate
class TableViewDelegate: NSObject, UITableViewDelegate {
  weak var dataSource: TableViewDataSource?
  
  func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    guard let data = dataSource?.data, indexPath.row < data.count else {
      print("Invalid selection.")
      return
    }
    print("Selected: \(data[indexPath.row])")
    tableView.deselectRow(at: indexPath, animated: true)
  }
}

// MARK: - Client Class
class YourCustomTableViewController: UITableViewController {
  var tableViewDataSource: TableViewDataSource?
  var tableViewDelegate: TableViewDelegate?
  
  override func viewDidLoad() {
    super.viewDidLoad()
    
    tableView.dataSource = tableViewDataSource
    tableView.delegate = tableViewDelegate
    tableView.register(UITableViewCell.self, forCellReuseIdentifier: "cell")
  }
}
```

> This is the **Open／Closed Principle (OCP)** in action! We don't have to change the view
> controller to extend it's behavior.

## Day 4: January 15, 2025

### Today's Progress

1. Reviewed a particularly difficult topic on using generics to map `Swift.Error` types to
   domain-specific typed errors. Going to have to come up with a decent spike to drive this one
   home in my brain.
1. Did some research on when to write _more_ end-to-end tests.

### When to Write More End-To-End Tests

The less we trust a counterpart (e.g., APIs, remote servers, databases, file systems, etc.),
the more we should rely on end-to-end tests to increase confidence and reduce risks.

**How do rely more on end-to-end tests?** Simply put, by writing more of them.

For example, let's say our app relies on a remote API service (like a Weather API), and the
service owners introduce breaking changes in their next update and fail to warn us.

Full stop, if this is the case, then we should be looking for a new API. If our app is designed
to be composable, replacing the API shouldn't be too difficult.

However, if replacing the API isn't possible, then we can mitigate this issue by adding more
end-to-end tests.

### What are End-to-End Tests?

These are tests that check our system against other services, like remote APIs. In end-to-end
tests we hit the database and the real services that our app uses.

End-to-end tests verify our app against external services, such as remote APIs, databases, or
file systems. These tests interact with real counterparts to ensure the app behaves as expected
in a production-like environment.

For example, we make a real network request to that Weather API using a real instance of
`URLSession` and then check that the weather data returned by the API maps onto our model
objects exactly how we expect them to.

## Day 5: January 16, 2025

### Today's Progress

1. Learned about `Swift.Error`, including error handling in Swift, as well as generics.

### Handling Errors by Converting them to Optional Values

Converting an error to an optional value is a great way to handle situations where we want to
throw our domain-specific error in favor of a third-party error that we don't control (e.g.
`DecodingError` thrown by `JSONDecoder.decode(_:from:)`).

Take a look at the following `static` function. Why isn't there a `do-catch` block to catch the
potential error that **`JSONDecoder().decode(_:from:)`** throws?

```swift
  internal static func map(_ data: Data, from response: URLResponse) -> RemoteFeedLoader.Result {
    guard response.statusCode == OK_200, let root = try? JSONDecoder().decode(Root.self, from: data) else {
      return .failure(.invalidData)
    }

    return .success(root.feed)
  }
```

This is because Swift allows us to convert errors to optional values.

If `decode` throws an error it's converted to an optional value **`nil`**.

Here is what the Apple docs say about it:

> "You use `try?` to handle an error by converting it to an optional value. If an error is
> thrown while evaluating the `try?` expression, the value of the expression is `nil`."

We could convert this to a `do-catch` block and implement the same behavior, like so:

```swift
  internal static func map(_ data: Data, from response: URLResponse) -> RemoteFeedLoader.Result {
    let root: FeedItemsMapper.Root?
    do {
      root = try JSONDecoder().decode(Root.self, from: data)
    } catch {
      root = nil
    }
    guard response.statusCode == OK_200, root != nil else {
      return .failure(.invalidData)
    }

    return .success(root!.feed)
  }
```

We could do this, but it seems kinda convoluted. Maybe that's why the Swift team implemented
this.

## Day 6: January 27, 2025

### Today's Progress

1. Learned how the compiler auto-synthesizes `enum` conformance to `Equatable`.

### Compiler Magic: Automatic Conformance to Equatable

Take a look at the following code:

```swift
/////////////////////
// MARK: - Production
/////////////////////
import Foundation

public struct FeedItem: Equatable {
  public let id: UUID
  public let description: String?
  public let location: String?
  public let imageURL: URL
  
  public init(id: UUID, description: String?, location: String?, imageURL: URL) {
    self.id = id
    self.description = description
    self.location = location
    self.imageURL = imageURL
  }
}

public enum LoadFeedResult<Error: Swift.Error> {
  case success([FeedItem])
  case failure(Error)
}

//extension LoadFeedResult: Equatable where Error: Equatable {}

protocol FeedLoader {
  associatedtype Error: Swift.Error

  func load(completion: @escaping (LoadFeedResult<Error>) -> Void)
}

public final class RemoteFeedLoader: FeedLoader {
  private let client: HTTPClient
  private let url: URL

  public enum Error: Swift.Error {
    case connectivity
    case invalidData
  }
  
  public typealias Result = LoadFeedResult<Error>
  
  public init(url: URL, client: HTTPClient) {
    self.url = url
    self.client = client
  }
  
  public func load(completion: @escaping (Result) -> Void) {
    client.get(from: url) { [weak self] result in
      guard self != nil else { return }
      
      switch result {
      case let .success(data, response):
        completion(FeedItemsMapper.map(data, from: response))
      case .failure:
        completion(.failure(.connectivity))
      }
    }
  }
}

/////////////////////
// MARK: - Test Class
/////////////////////
class RemoteFeedLoaderTests: XCTestCase {

  //...

  private func expect(_ sut: RemoteFeedLoader, toCompleteWith result: RemoteFeedLoader.Result, when action: () -> Void, file: StaticString = #file, line: UInt = #line) {
    var capturedResults = [RemoteFeedLoader.Result]()
    sut.load { capturedResults.append($0) }
    
    action()
    
    XCTAssertEqual(capturedResults, [result], file: file, line: line)
  }

  //...
}
```

If we comment out the `extension` that conforms `LoadFeedResult` to `Equatable` if `Error` is
`Equatable` (as depicted below), then Swift gives us the following error:

```swift
// extension LoadFeedResult: Equatable where Error: Equatable {}
```

```
Global function 'XCTAssertEqual(_:_:_:file:line:)' requires that 'RemoteFeedLoader.Result' (aka
'LoadFeedResult<RemoteFeedLoader.Error>') conform to 'Equatable'

! Requirement from conditional conformance of '[RemoteFeedLoader.Result]' (aka
'Array<LoadFeedResult<RemoteFeedLoader.Error>>') to 'Equatable' (Swift.Array)
```

The problem is that in order to be able to compare the two `LoadFeedResult` instances, the
`Error` type passed to `LoadFeedResult` must conform to `Equatable`.

#### Understanding How Error Conforms to Equatable

If we look at `LoadFeedResult`:

```swift
public enum LoadFeedResult<Error: Swift.Error> {
  case success([FeedItem])
  case failure(Error)
}
```

##### The success Case

The `success` case has an associated value of `[FeedItem]`, which is an array. Arrays conform
to `Equatable` only if their elements conform to `Equatable`, and in this case `FeedItem` does
conform to `Equatable`.

##### The failure Case

The `failure` case has an associated value of `Error`. Therefore, `Error` must conform to
`Equatable` for the failure case to satisfy `Equatable`.

If `Error` in our production code doesn’t conform to `Equatable`, the conditional conformance
can’t be satisfied. In this case we have the following solutions:

1. We can either make `Error` conform to `Equatable` (if it makes sense for our domain logic), or
2. We must use a custom equality check in our test instead of relying on `XCTAssertEqual`.

> **Tip**: Solution **2** is ultimately the solution we end up going with so that we don't
> alter our production code just for the sake of tests. That's bad practice!

#### The Fix

In `RemoteFeedLoader` we declare `RemoteFeedLoader.Error` like so:

```swift
public enum Error: Swift.Error {
  case connectivity
  case invalidData
}
```

Enums without associated values can automatically conform to `Equatable`. This is because there
are no additional types (like `String`, `Int`, etc.) in the `enum` cases to complicate the
equality check. The compiler synthesizes the `Equatable` conformance by comparing the enum
cases directly.

For example:

```swift
let error1: RemoteFeedLoader.Error = .connectivity
let error2: RemoteFeedLoader.Error = .connectivity
XCTAssertEqual(error1, error2)
```

Thus, when we uncomment the extension:

```swift
extension LoadFeedResult: Equatable where Error: Equatable {}
```

The compiler checks whether `RemoteFeedLoader.Error` conforms to `Equatable`. Since the enum
has no associated values and satisfies the rules for automatic synthesis, the compiler deems it
`Equatable`. This enables `LoadFeedResult<RemoteFeedLoader.Error>` to conform to `Equatable`.

#### In Summary

The compiler isn’t “synthesizing `Error` to `Equatable`” — it’s using automatic synthesis
because our `RemoteFeedLoader.Error` enum meets the criteria for `Equatable`. By uncommenting
the extension, we enable the compiler to extend `LoadFeedResult` with `Equatable` conformance,
leveraging the `Equatable` status of both `FeedItem` and `RemoteFeedLoader.Error`.
