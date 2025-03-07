# Reactor for .NET

Reactor is a lightweight, reactive state management library for .NET applications inspired by [Fusion](https://elttob.uk/Reactor/) for luau. It provides a simple and elegant way to create reactive applications with automatic dependency tracking and state propagation.

Reactor focuses on being user-friendly and reducing boilerplate code rather than prioritizing raw performance.

## Why Reactor?

Reactor prioritizes developer experience and code readability over raw performance. Its design goals include:

- **Minimal Boilerplate**: Write reactive code with minimal ceremony
- **Intuitive API**: Clear, consistent and discoverable methods and properties
- **Predictable Behavior**: Reactive updates follow an understandable pattern
- **Composability**: Reactive elements can be easily combined and transformed

While Reactor is designed to be performant enough for most applications, it makes trade-offs favoring developer experience rather than maximum performance. For extremely performance-critical applications, you might need a more specialized solution.

## Features

- **Reactive State Containers**: Create observable state that automatically notifies dependents when values change
- **Computed Values**: Define values that are derived from other state and automatically update when dependencies change
- **Dependency Tracking**: Automatic tracking of dependencies between states and computed values
- **Extension Methods**: Comprehensive set of LINQ-like extension methods for transforming reactive state
- **Type Safety**: Fully type-safe API with generics support throughout
- **No Dependencies**: Written in vanilla C# with no external dependencies

> **Note:** Reactor only currently supports single-threaded use and is not thread-safe.

## Installation

Reactor is currently available only via GitHub. To use it in your project:

1. Clone this repository
2. Reference the project in your solution

```bash
git clone https://github.com/daireb/Reactor.git
```

## Basic Usage

```csharp
using Reactor;

// Create reactive state
var count = new State<int>(0);
var message = new State<string>("Hello");

// Create computed values that react to state changes
var countDoubled = new Computed<int>(() => count.Value * 2);

// More concise syntax using extension methods
var countMessage = count.Select(c => $"{message.Value}, Count: {c}");

// Observe changes to state and computed values
count.Subscribe(value => Console.WriteLine($"Count changed to {value}"));
countMessage.Subscribe(msg => Console.WriteLine(msg));

// Update state values
count.Value = 1; // Triggers updates to countDoubled, countMessage, and observers

// Remember: All operations should be performed on a single thread
```

## Creating and Combining Reactive State

```csharp
// Create basic state
var firstName = new State<string>("John");
var lastName = new State<string>("Doe");

// Combine states into a computed value
var fullName = new Computed<string>(() => $"{firstName.Value} {lastName.Value}");

// Chain transformations
var greeting = fullName.Select(name => $"Hello, {name}!");

// Observe changes
fullName.Subscribe(name => Console.WriteLine($"Name changed to: {name}"));

// Update state to trigger reactions
firstName.Value = "Jane"; // Will update fullName and greeting
```

## Working with Collections

```csharp
var items = new State<List<string>>(new List<string> { "Apple", "Banana", "Cherry" });

// Transform each item in the collection
var upperItems = items.Select<List<string>, string, string>(item => item.ToUpper());

// Filter items
var filteredItems = items.Where<List<string>, string>(item => item.StartsWith("A"));

// Observe changes
upperItems.Subscribe(list => {
    Console.WriteLine("Upper items: " + string.Join(", ", list));
});

// Update collection
var newList = new List<string>(items.Value) { "Dragonfruit" };
items.Value = newList; // Triggers update to all derived computations
```

## Architecture

Reactor is built around a few core concepts:

- **State\<T>**: A container for reactive values
- **Computed\<T>**: A derived value that automatically updates when its dependencies change
- **Observer**: Subscribes to changes in state or computed values
- **DependencyTracker**: Handles the dependency graph and automatic tracking

## Project Status

Reactor is currently in active development. API may change before reaching version 1.0.

## Requirements

- .NET Standard 2.1 or higher
- Single-threaded environment (not thread-safe)

## License

[MIT License](LICENSE)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
