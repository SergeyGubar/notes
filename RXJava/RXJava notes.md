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

#### Creating sources of data

All types of sources have static methods:

```kotlin
Flowable.just("Hello")
Observable.just("Hello")
Maybe.just()
Single.just()

Observable.fromArray()
Observable.fromIterable()

// Example with calling some hyphotetical http client
Observable.fromCallable(() -> client.request().response)
// If we don't want to return any value
Maybe.fromAction(() -> sout("hello")) 
Maybe.fromRunnable(() -> sout("hello"))

Observable.create(e -> { // e is ObservableEmmiter, lambda is subscribe() method
  e.onNext("Hello")
  e.onNext("Hello")
  e.OnComplete()
})
// So now we can model async work

Observable.create(e -> {
  client.newCall(request).enqueue(Callback() {
    override fun onResponse(r: Response) {
      e.onNext(r.body().toString())
      e.onComplete()
    }
    override fun onFailure(ex: Exception) {
      e.onError(ex)
    }
  })
})
```

#### Observing sources of data

```kotlin
val observable = Observable.just("Hello")
// instead of DisposableObserver could be DisposableMaybeObserver etc
val disposable: Disposable = observable.subscribeWith(DisposableObserver<String>(){
  override fun onNext(s: String)
  override fun onComplete()
  override fun onError(t: Throwable)
})
disposable.dispose()

// use CompositeDisposable for multiple disposables
```

#### Operators

````kotlin
// bg thread
val user: Observable<User> = um.getUser()
// ui thread
val mainThreadUser: Observable<User> = user.observeOn(AndroidSchedulers.mainThread())

// subscribeOn - operator, ensures that Observable works on bg thread (Schedulers.io is just a thread pool)
// observe the result on main thread, and map response to String
val response: Observable<String> = Observable.fromCallable(() -> {
  return client.newCall(request).execute()
}).subscribeOn(Schedulers.io())
  // we call map before observe, because we want our subscribers to receive String, not Response
  .map(response -> response.body().toString)  
  .observeOn(AndroidSchedulers.mainThread())
  
observable.first() // return Single
observable.firstElement() // return Maybe

````

