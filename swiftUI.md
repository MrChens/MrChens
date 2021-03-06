1. [apple swiftui](https://developer.apple.com/tutorials/swiftui)
2. [standford CS193p 2021](https://www.bilibili.com/video/BV1fy4y1u7hX/?spm_id_from=autoNext)
    - [resource](https://cs193p.sites.stanford.edu)
4. [Demystify SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10022/)
5. [Data Flow Through SwiftUI](https://developer.apple.com/videos/play/wwdc2019/226/)
6. [sf-symbols](https://developer.apple.com/sf-symbols/)
7. [Formatting Quick Help](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html)
8. [@state @](https://www.youtube.com/watch?v=stSB04C4iS4)



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
- A `@State` property is a source of truth. One view owns it and passes its value or reference, known as a binding, to its subviews.当你把它当作参数传递（双向绑定），则需要`$xxxx`；作变量使用则`xxxx`
- A `@Binding` property is a reference to a `@State` property owned by another view. It gets its initial value when the other view passes it a binding, using the $ prefix. Having this reference to the source of truth enables the subview to change the property's value, and this changes the state of any view that depends on this property.
- `@EnvironmentObject` declares dependency on some shared data - data that's visible to all views in a subtree of the app. It's a convenient way to pass data indirectly instead of passing data from parent view to child to grandchild, especially if the in-between child view donesn't need it.
    - To be an EnvironmentObject, `xxx` must conform to the `ObservableObject` protocol. An `ObservableObject` is a publisher, like Timer.publisher.
    - To conform to `ObservableObject`, `xxx` must be a class, not a structure.

- `@AppStorage` is a property wrapper, similar to @State and @Binding, that allows interaction between UserDefaults and your SwiftUI views.



User data

- Documents, The main documents directory for the app.
- Library, The directory for files that you don't want to expose to the user.
- 
- SystemData
- tmp

App bundle


- `map(_:)`: similar to a for loop, map goes through each element individually, transforms the data to a new element and then comnines them all into a single array.
- `fillter(_:)`:filter one array to another array
- `reduce(_:_:)`:combines all the elements in an array into one value. `let result = [4, 2, 0].reduce(0) { runningTotal, value in runningTotal + value`


### Tools for managing data
- User interface values, like Boolean flags to show or hide views, text field text, slider or picker values.
- Data model objects, often collections of objects that model the app's data, like daily logs of completed exercises.

- Value types
   - State
   - Binding
   - Environment
   - environment
- Object types ObservableObject
   - StateObject
   - ObservedObject
   - environmentObject
   - EnvironmentObject


## NavigationLink
如果2个list是上下结构(VStack)，那么当第一个NavigationLiknk从屏幕中不见，则就算你触发了它的selection，它还是不能响应。
最好的办法是用ZStack

```
List {
        Section {
            NavigationLink(
                destination: Text("1"),
                label: {
                    AboutRow(title: "常见问题")
                })
        }

        Section {
            NavigationLink(
                destination: Text("2"),
                    title: L10n.FlowMonitor.accurateStatistics),
                label: {
                    AboutRow(title: L10n.FlowMonitor.accurateStatistics)
            })
        }

        Section {
            NavigationLink(
                destination: Text("3"),
                    title: "隐私条款"),
                label: {
                    AboutRow(title: "隐私条款")
            })
        }
}

List {
        Section {
            Button(action: {
                self.selection = .faq
            }) {
                HStack {
                    Text("常见问题")
                        .modifier(CustomFontModifier(size: 20))
                    Spacer()
                    Image(systemName: "arrow.right.circle")
                }
            }
        }

        Section {
            Button(action: {
                selection = .accurateStatistics
            }) {
                HStack {
                    Text(L10n.FlowMonitor.accurateStatistics).modifier(CustomFontModifier(size: 20))
                    Spacer()
                    Image(systemName: "arrow.right.circle")
                }
            }
        }

        Section {
            Button(action: {
                selection = .privacy
            }) {
                HStack {
                    Text("隐私条款")
                        .modifier(CustomFontModifier(size: 20))
                    Spacer()
                    Image(systemName: "arrow.right.circle")
                }
            }
        }

        Section {
            Button(action: shareButton) {
                HStack {
                    Text("分享应用")
                        .modifier(CustomFontModifier(size: 20))
                    Spacer()
                    Image(systemName: "arrow.up.right.circle")
                }
            }
        }
}

```
