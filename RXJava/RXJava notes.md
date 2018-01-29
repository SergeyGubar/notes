### RXJava notes

#### Sources of data:

```kotlin
Observable<T>
Flowable<T>
```

Both emit items, terminates with complete or error. The main difference between these sources is back pressure (don't and do have it respectively). **Backpressure** - how fast a source emits items (data). For example - user touches is observable because we can't force user to click less or more, and db query can be flowable (give me 2 rows, give me 3 more, etc).

#### Subsets of observables:

```kotlin
Single<T> // emits value or error
// Subscribe to single, get item or error 

Completable<T> // no values, just success or not

Maybe<T> // error or value or no value
```

#### Types of observers

```kotlin
// Observer for Observable sourc
interface Observer<T> {
  fun onNext(t: T)
  fun onComplete() // for complete source of data in case of success
  fun onError(t: Throwable)
  fun onSubscribe(d: Disposable) // called when you subscribe
}

interface Disposable {
  fun dispose() // Clean up resources, unset the listener, dismiss request, etc.
}

// Observer for Flowable
interface Subscriber<T> {
  fun onNext(t: T)
  fun onComplete() // for complete source of data in case of success
  fun onError(t: Throwable)
  fun onSubscribe(s: Subscription)
}

interface Subscription {
  fun cancel() // The same as dispose
  fun request(r: Long) // Give me more items
}
```

#### Usage example 

```kotlin
interface UserManager {
  fun getUser(): Observable<User> // Now we can observe when our object changes
  fun setName(name: String): Completable // Have we changed something or error occured?
  fun setAge(age: Int): Completable 
}
```

