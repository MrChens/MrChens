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
