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
