# Mastering Coding Style Guides for Large-Scale Projects

## Goals

Following this style guide should:

- Improve code scalability over time.
- Enhance readability and maintainability.
- Define best practices for structuring views and managing dependencies.
- Reduce cognitive load and improve team collaboration.

## Guiding Tenets

* This guide is in addition to the [Swift Style Guide](https://google.github.io/swift/), please make yourself familiar with the complete guide before reading this one.

## Table of Contents

- [SwiftUI Style Guide](#fiverr-swiftui-style-guide)
    - [1. File Organization](#1-file-organization)
    - [2. View Structure](#2-view-structure)
    - [3. Property wrappers](#3-property-wrappers)
    - [4. Coding Style And Best Practices](#4-coding-style-and-best-practices)
        - [4.1 General](#41-general)
        - [4.2 Architecture](#42-architecture)
        - [4.3 Views](#43-views)
    - [5. Previews](#5-previews)

---

## 1. File Organization

* **1.1** Within each top-level section, place content in the following order. This allows a new reader of your code to more easily find what they are looking for.

  * Nested types and type aliases
  * Static properties
  * Class properties
  * Instance properties
  * Static methods
  * Class methods
  * Instance methods

* **1.2** Within the properties section, place the properties in the following order.

  * public let
  * private(set) var
  * public var
  * public computed var / lazy var
  * SPACE
  * private let
  * private var
  * private computed var / lazy var

* **1.3** Use `// MARK:` to separate the contents of a type definition into the sections listed below in order. All type definitions should be divided up consistently, allowing a new reader of your code to easily jump to what they are interested in.

  * `// MARK: Lifecycle` for `init` and `deinit` methods.
  * `// MARK: Public` methods.
  * `// MARK: Private` methods.
  * Overridden methods should be under the Lifecycle mark.
  * The properties mark will be added if something above properties is not a property (structs, enums, etc.).
  * If the type in question is an enum, its cases should go above the first mark.
  * Do not subdivide each of these sections into subsections, as that makes the method dropdown more cluttered and, therefore, less useful. Instead, group methods by functionality and use smart naming to clarify which methods are related. If there are enough methods that sub-sections seem necessary, consider refactoring your code into multiple types.
  * If all of a type's definitions belong to the same category (e.g., the type only consists of internal properties), it is OK to omit the marks.
  * If the type in question is a simple value type (e.g., fewer than 20 lines), it is OK to omit the marks, as it would hurt legibility.
  * Extensions should be subdivided using marks.
  * Analytics methods can be used as a separate extension for readability.

* **1.4** Add empty lines between different kinds of property declarations. (e.g., between static properties and instance properties.)

```swift
// WRONG
static let gravityEarth: CGFloat = 9.8
static let gravityMoon: CGFloat = 1.6
var gravity: CGFloat

// RIGHT
static let gravityEarth: CGFloat = 9.8
static let gravityMoon: CGFloat = 1.6

var gravity: CGFloat
```

* **1.5** Computed properties and properties with property observers should appear at the end of the set of declarations of the same kind. (e.g., instance properties.)

```swift
// WRONG
var atmosphere: Atmosphere {
  didSet {
    print("oh my god, the atmosphere changed")
  }
}
var gravity: CGFloat

// RIGHT
var gravity: CGFloat
var atmosphere: Atmosphere {
  didSet {
    print("oh my god, the atmosphere changed")
  }
}
```

* **1.6** deinit should always appear before init.

## 2. View Structure

To maintain consistency across views, follow these guidelines:

* **2.1** The file organization and property order should remain the same as described in [Section 1](#1-file-organization), with the exception of property wrapper [Property Wrappers](#3-property-wrappers).
* **2.2** If the call site variable order is important, use initialization to control it, and do not change the properties order.
* **2.3** Initialization parameters variables should be private.
* **2.4** The file organization's mark order should remain the same as in section 1.3 of this guide. With the following additions:

  * `// MARK: Views` to group all views and content builders, positioned immediately after the Lifecycle mark.
  * `// MARK: Previews` at the bottom of the file, if needed.
  
* **2.5** Use view modifiers on a new line.
* **2.6** If the view modifiers order has no significance, set view modifiers with closures at the bottom.
* **2.7** As a rule of thumb, sizing, and spacing view modifiers (frame, padding, etc) should be declared on the call site instead of in the view builder.
* **2.8** Use global view modifiers at the bottom of the body view exclusively. Aim for the following order.

  * View lifecycle (onAppear, onDisappear, etc.)
  * Observers (onChange, onNotification, etc.)
  * Global variables and navigation stack (environment, accentColor, navigationBarHidden, navigationTitle, etc.)
  * Navigation (sheet, navigationDestination, etc.)

* **2.9** Add a line spacing between views.
* **2.10** Use pyramid initializers within views.

## 3. Property Wrappers
In order to maintain readability please keep the following order when declaring property wrappers:

* ObservedObject
* Binding
* Published
* Environment
* NameSpace
* StateObject
* GestureState
* State
* FetchRequest \ SectionedFetchRequest

## 4. Coding Style and Best Practices

### 4.1 General

* **4.1.1** Keep expressive names to view content closure variables (view proxy, forEach items, etc.).
* **4.1.2** If a call site contains more than one closure, use closures with explicit names.
* **4.1.3** Always prefer native SwiftUI components over custom ones.
* **4.1.4** Always prefer creating styles and custom modifiers to native SwiftUI components over creating custom views.
* **4.1.5** Always prefer the explicit animation view modifier.
* **4.1.6** Avoid using [Appearance API](https://developer.apple.com/documentation/uikit/uiappearance) unless absolutely needed.

### 4.2 Architecture

* **4.2.1** You can use multiple observable objects (as view models) if they fit your needs.
* **4.2.2** Avoid using large, overloaded view models. each view model (or model) should focus on specific responsibilities.
* **4.2.3** Use the Model suffix to mark view models.
* **4.2.4** Use the [task](https://developer.apple.com/documentation/swiftui/view/task%28priority:_:%29) modifier for view model initialization; avoid doing so in view initializers.

### 4.3 Views

* **4.3.1** Always use explicit initializers for Views to ensure clarity.
* **4.3.2** Implicit returns are allowed when creating views (`@ViewBulider` or any function or computed var that returns `some -> View`).
* **4.3.3** Only when needed, views should be declared with the `@ViewBuilder` property wrapper (or any other content builder like ToolbarContentBuilder, etc).
* **4.3.4** Views without parameters can be declared as variables. function vs variable order (as defined in 1.2) does not matter within the View pragma mark.
* **4.3.5** Avoid complex logic on view builders, extract it to standalone functions.
* **4.3.6** Avoid large view builders, instead, extract subviews to new view builders.
* **4.3.7** Prefer using a representable for existing UIKit components rather than rewriting them in SwiftUI.
* **4.3.8** When rewriting an existing UIKit view in SwiftUI, strive to fully replace its usage to prevent duplication.
* **4.3.9** Avoid one-liner closures.

## 5. Previews
To ensure views are tested under different conditions, follow these preview guidelines:

* **5.1** Keep previews to the bottom of the file.
* **5.2** Always use the preview mark.
* **5.3** Always use the [Preview](https://developer.apple.com/documentation/swiftui/preview%28_:body:%29) macro.
* **5.4** Always add dark\light representations.
* **5.5** For small views, use the preview layout `sizeThatFits`.
* **5.6** Include multiple device previews (e.g., iPhone, iPad) to ensure the view adapts well to different screen sizes.
* **5.7** When possible, add localization previews.
* **5.8** Use environment overrides (e.g., `.environment(\.sizeCategory, .extraLarge)`) to test accessibility settings like Dynamic Type.
