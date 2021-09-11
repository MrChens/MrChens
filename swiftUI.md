1. [apple swiftui](https://developer.apple.com/tutorials/swiftui)
2. [standford CS193p 2021](https://www.bilibili.com/video/BV1fy4y1u7hX/?spm_id_from=autoNext)
    - [resource](https://cs193p.sites.stanford.edu)
4. [Demystify SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10022/)
5. [Data Flow Through SwiftUI](https://developer.apple.com/videos/play/wwdc2019/226/)
6. [sf-symbols](https://developer.apple.com/sf-symbols/)
7. [Formatting Quick Help](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html)



@State
- View-local
- Value
- Framework Managed


@BindableObject
- External
- Reference
- Developer Managed

@Binding
- First class reference to data
- Great for reusability
- Use $ to derive from source 

Read-only: Swift property, Environment
Read-write: @Binding
Prefer immutable access


Using State Effectively
- Limit use if possible
- Use derived Binding or value
- Prefer BindableObject for persistence
- Example: Button highlighting


## Managing your app's data

SwiftUI has two guiding principles for managing how data flows through your app:
- `Data access = dependency:`Reading a piece of data in your view creates a dependency for that data in that view. Every view is a function of its data dependencies - its inputs or state.
- `Single source of truth:` Every piece of data that a view reads has a source of truth, which is either onwned by the view or external to the view. Regardless of where the source of truth lies, you should always have a single source of truth.

### Tools for data flow
`Property wrappers` augment the behavior of properties. SwiftUI-specific wrappers like @State, @Binding, and @EnvironmentObject declare a view's dependency on the data represented by the property.
- A `@State` property is a source of truth. One view owns it and passes its value or reference, known as a binding, to its subviews.当你把它当作参数传递（双向绑定），则需要`$xxxx`,当作变量使用则`xxxx`
- A `@Binding` property is a reference to a `@State` property owned by another view. It gets its initial value when the other view passes it a binding, using the $ prefix. Having this reference to the source of truth enables the subview to change the property's value, and this changes the state of any view that depends on this property.
- `@EnvironmentObject` declares dependency on some shared data - data that's visible to all views in a subtree of the app. It's a convenient way to pass data indirectly instead of passing data from parent view to child to grandchild, especially if the in-between child view donesn't need it.
