1. [apple swiftui](https://developer.apple.com/tutorials/swiftui)
2. [standford CS193p 2021](https://www.bilibili.com/video/BV1fy4y1u7hX/?spm_id_from=autoNext)
    - [resource](https://cs193p.sites.stanford.edu)
4. [Demystify SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10022/)
5. [Data Flow Through SwiftUI](https://developer.apple.com/videos/play/wwdc2019/226/)
6. [sf-symbols](https://developer.apple.com/sf-symbols/)



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
