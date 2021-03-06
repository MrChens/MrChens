[Core Data Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CoreData/index.html#//apple_ref/doc/uid/TP40001075-CH2-SW1)

# coreData stack

- NSManagedObjectModel
  - represents each object type in your app's data model, the properties they can have, and the relationships between them. like database of SQLite.
- NSPersistentStore
  - reads and writes data to whichever storage method you've decided to use.
  - core data provides four types of NSPersistentStore out of the box: three atomic and one non-atomic.
    - An atomic persistent store needs to be completely deserialized and loaded into memory before you can make any read or write operations.
    - A non-atomic persistent store can load chunks of itself onto memory as needed.
- NSPersistentStoreCoordinator
  - is the bridege between the managed object model and the persistent store.
  - it's responsible for using the model and the persistent stores to do most of the hard work in Core Data.
  - It understands the NSManagedObjectModel and knows how to send information to, and fetch information from, the NSPersistentStore.
- NSManagedObjectContext
  - A context is an in-memory scratchpad for working with your managed objects.
  - You do all of the work with your Core Data objects within a managed object context.
  - Any changes you make won't affect the underlying data on disk until you call save() on the context.
  - The context manages the lifecycle of the objects it creates or fetches. This lifecycle management includes powerful features such as faulting, inverse relationship handling and validation.
  - A managed object cannot exist without an associated context. In fact, a managed object and its context are so tightly coupled that every managed object keeps a reference to its context, which can be accessed like so: `let managedContext = employee.managedObjectContext`
  - Contexts are very territorial; once a managed objecg has been associated with a particular context, it will remain associated with the same context for the duration of its lifecycle.
  - An application can use more than one context - most non-trivial Core Data applications fall into this catgory. Since a context is an in-memory scratch pad for what's on disk, you can actually load the same Core Data object onto two defferent contexts simultaneously.
  - A context is not thread-safe. The same goes for managed object: You can only interact with contexts and managed objects on the same thread in which they were created.
- NSPersistentContainer
  - It's a container that holods everything together. Instead of wasting your time writing boilerplate code to wire up all four stack components together, you can simply initialize an NSPersistentContainer, load its persistent stores, and you're good to go.
