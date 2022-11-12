# Complete Flutter Course Bundle - Summary

This is not the official repo of [Complete Flutter Course Bundle](https://codewithandrea.com/courses/complete-flutter-bundle/). I only made this note by following the course.

## Navigation with GoRouter

### Setup GoRouter

In first step must add package/dependency in your flutter project

```bash
flutter pub add gorouter
```

And then next step, create a variable to store gorouter & list of routes.

```dart
final goRouter = GoRouter(
  initialLocation: '/',
  debugLogDiagnostics: true,
  routes: [...],
```

Property `debugLogDiagnostics: true` in `GoRouter` object work for show route when initialize/navigate routes.

And then setup goRouter in your materialApp, like this:

```dart
return MaterialApp.router(
    routerConfig: goRouter,
    ...
);
```

### Work with Route

To create a route in GoRouter, put in `routes` at GoRouter.

```dart
GoRouter(
  initialLocation: '/',
  debugLogDiagnostics: true,
  routes: [
    GoRoute(
        path: '/',
        builder: (context, state) => const SomePage(),
    ),
  ],
)
```

And in gorouter you can nested navigation, with subroute. The trick is to put it in one of the `GoRoute`s and place it in the `routes` property like this:

```dart
GoRoute(
    path: '/',
    builder: (context, state) => const SomePage(),
    routes: [
        GoRoute(
            path: 'detail',
            builder: (context, state) => const SomeDetailPage(),
        ),
    ]
),
```

> Tips: In subroute, the route path is not allowed to start with `/`

And then how to navigate with a specific route path gorouter like this:

```dart
GoRouter.of(context).push('/');
Gorouter.of(context).push('/detail');
```

And if you use the context extension, there is no need for `GoRouter.of(context)` to call the method directly.

```dart
context.push('/');
context.push('/detail');
```

This is an article to explain about gorouter navigation use `push` or `go` [GoRouter push or go](https://codewithandrea.com/articles/flutter-navigation-gorouter-go-vs-push/)

### Add Parameter in a Route

```dart
GoRoute(
    path: 'detail/:id',
    builder: (context, state) => SomeDetailPage(id: state.params['id']),
)
```

```dart
context.push('/detail/12');
```

## Flutter App Architecture

### Most Popular App Archicetures in Flutter

Good architecture should help you handle complexity without getting on the way. But it's not always easy to get it right:

- The absence of it creates an overall lack of organization in your code
- The excess use of it leads to over-engineering, making it hard to make even simple changes

In practice, things can be quite nuanced and it takes some practice and experience to get the right balance.

There are many architectures that are often used in Flutter, such as:

1. BLoC
2. MVC
3. MVP
4. MVVM
5. Clean Architecture
6. Riverpod

### Riverpod Architecture

While building Flutter apps of varying complexity, I've experimented a lot with app architecture and developed a deep understanding about what works well and what doesn't.

This architecture is composed of four layers (data, domain, application, presentation).

![](https://codewithandrea.com/articles/flutter-app-architecture-riverpod-introduction/images/flutter-app-architecture.webp)

Each of these layers has its own responsibility and there's a clear contract for how communication happens across boundaries.

### Feature First vs Layer First

This is example project structure feature first and layer first.

#### Layer First (Feature inside layer)

With this approach, we can add all the relevant Dart files inside each feature folder, ensuring that they belong to the correct layer (widgets and controllers inside presentation, models inside domain, etc.).

```dart
‣ lib
  ‣ src
    ‣ presentation
      ‣ feature1
      ‣ feature2
    ‣ application
      ‣ feature1
      ‣ feature2
    ‣ domain
      ‣ feature1
      ‣ feature2
    ‣ data
      ‣ feature1
      ‣ feature2
```

#### Feature First

The feature-first approach demands that we create a new folder for every new feature that we add to our app. And inside that folder, we can add the layers themselves as sub-folders.

```dart
‣ lib
  ‣ src
    ‣ features
      ‣ feature1
        ‣ presentation
        ‣ application
        ‣ domain
        ‣ data
      ‣ feature2
        ‣ presentation
        ‣ application
        ‣ domain
        ‣ data
```

## State Management with Riverpod

Riverpod is one of most popular state management in Flutter. Riverpod is a reactive caching and data-binding framework that was born as an evolution of the Provider package.

Riverpod is very versatile, and you can use it to:

- catch programming errors at compile-time rather than at runtime
- easily fetch, cache, and update data from a remote source
- perform reactive caching and easily update your UI
- depend on asynchronous or computed state
- create, use, and combine providers with minimal boilerplate code
- dispose the state of a provider when it is no longer used
- write testable code and keep your logic outside the widget tree

### Setup Riverpod

In first need to add dependency `flutter_riverpod` in your flutter project.

```dart
flutter pub add flutter_riverpod
```

Once Riverpod is installed, we can wrap our root widget with a ProviderScope:

```dart
void main() {
  // wrap the entire app with a ProviderScope so that widgets
  // will be able to read providers
  runApp(ProviderScope(
    child: MyApp(),
  ));
}
```

### Create & Use Riverpod

This is example a provider to provide `Hello World` data

```dart
// provider that returns a string value
final helloWorldProvider = Provider<String>((ref) {
  return 'Hello world';
});
```

And this is a many different ways to consuming a provider of Riverpod.

1. Using ConsumerWidget as alternative StatelessWidget

```dart
// 1. widget class now extends [ConsumerWidget]
class HelloWorldWidget extends ConsumerWidget {
  @override
  // 2. build method has an extra [WidgetRef] argument
  Widget build(BuildContext context, WidgetRef ref) {
    // 3. use ref.watch() to get the value of the provider
    final helloWorld = ref.watch(helloWorldProvider);
    return Text(helloWorld);
  }
}
```

2. Using Consumer Widget

```dart
class HelloWorldWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // 1. Add a Consumer
    return Consumer(
      // 2. specify the builder and obtain a WidgetRef
      builder: (_, WidgetRef ref, __) {
        // 3. use ref.watch() to get the value of the provider
        final helloWorld = ref.watch(helloWorldProvider);
        return Text(helloWorld);
      },
    );
  }
}
```

2. Using ConsumerStatefulWidget as alternative StatefulWidget

```dart
// 1. extend [ConsumerStatefulWidget]
class HelloWorldWidget extends ConsumerStatefulWidget {
  @override
  ConsumerState<HelloWorldWidget> createState() => _HelloWorldWidgetState();
}

// 2. extend [ConsumerState]
class _HelloWorldWidgetState extends ConsumerState<HelloWorldWidget> {
  @override
  void initState() {
    super.initState();
    // 3. if needed, we can read the provider inside initState
    final helloWorld = ref.read(helloWorldProvider);
    print(helloWorld); // "Hello world"
  }

  @override
  Widget build(BuildContext context) {
    // 4. use ref.watch() to get the value of the provider
    final helloWorld = ref.watch(helloWorldProvider);
    return Text(helloWorld);
  }
}
```

**What is a WidgetRef?**

As we have seen, we can watch a provider's value by using a ref object of type WidgetRef. This is available as an argument when we use Consumer or `ConsumerWidget`, and as a property when we subclass from ConsumerState.

The Riverpod documentation defines WidgetRef as an object that allows widgets to interact with providers.

Note that there are some similarities between BuildContext and `WidgetRef`:

- BuildContext lets us access ancestor widgets in the widget tree (such as `Theme.of(context)` and `MediaQuery.of(context))`
- WidgetRef lets us access any provider inside our app

In other words, `WidgetRef` lets us access any provider in our codebase (as long as we import the corresponding file). This is by design because all Riverpod providers are global.

#### Different Kind of Provider in Riverpod

Riverpod offers eight different kinds of providers, all suited for separate use cases:

1. Provider
1. StateProvider (legacy)
1. StateNotifierProvider (legacy)
1. FutureProvider
1. StreamProvider
1. ChangeNotifierProvider (legacy)
1. NotifierProvider (new in Riverpod 2.0)
1. AsyncNotifierProvider (new in Riverpod 2.0)

#### AutoDispose Modifier

If we're working with FutureProvider or StreamProvider, we'll want to dispose of any listeners when our provider is no longer in use.

We can do this by adding an autoDispose modifier to our provider:

```dart
final authStateChangesProvider = StreamProvider.autoDispose<User?>((ref) {
  // get FirebaseAuth from another provider
  final firebaseAuth = ref.watch(firebaseAuthProvider);
  // call method that returns a Stream<User?>
  return firebaseAuth.authStateChanges();
});
```

#### Caching with Timeout

If desired, we can call `ref.keepAlive()` to preserve the state so that the request won't fire again if the user leaves and re-enters the same screen:

```dart
final movieProvider = FutureProvider.autoDispose<TMDBMovieBasic>((ref) async {
  // get the repository
  final moviesRepo = ref.watch(fetchMoviesRepositoryProvider);
  // an object from package:dio that allows cancelling http requests
  final cancelToken = CancelToken();
  // when the provider is destroyed, cancel the http request
  ref.onDispose(() => cancelToken.cancel());
  // if the request is successful, keep the response
  ref.keepAlive();
  // call method that returns a Future<TMDBMovieBasic>
  return moviesRepo.movie(movieId: 550, cancelToken: cancelToken);
});
```

#### Family Modifier

`.family` is a modifier that we can use to pass an argument to a provider.

It works by adding a second type annotation and an additional parameter that we can use inside the provider body:

```dart
final movieProvider = FutureProvider.autoDispose
    // additional movieId argument of type int
    .family<TMDBMovieBasic, int>((ref, movieId) async {
  // get the repository
  final moviesRepo = ref.watch(fetchMoviesRepositoryProvider);
  // call method that returns a Future<TMDBMovieBasic>, passing the movieId as an argument
  return moviesRepo.movie(movieId: movieId, cancelToken: cancelToken);
});
```

Then, we can just pass the value we want to the provider when we call `ref.watch` in the build method:

```dart
final movieAsync = ref.watch(movieProvider(550));
```

#### When to use ref.read or ref.watch

In the examples above, have encountered two ways of reading providers: ref.read and ref.watch.

To get the value of a provider inside a build method, have always used ref.watch. This ensures that if the provider value changes, rebuild the widgets that depend on it.

But there are cases when shouldn't use ref.watch.

Example:
```dart
final counterStateProvider = StateProvider<int>((_) => 0);

class CounterWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // 1. watch the provider and rebuild when the value changes
    final counter = ref.watch(counterStateProvider);
    return ElevatedButton(
      // 2. use the value
      child: Text('Value: $counter'),
      // 3. change the state inside a button callback
      onPressed: () => ref.read(counterStateProvider.notifier).state++,
    );
  }
}
```

#### Listen Provider Scope Changes

Sometimes we want to show an alert dialog or a SnackBar when a provider state changes.

We can do this by calling `ref.listen()` inside the `build` method:

```dart
class CounterWidget extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    // if we use a StateProvider<T>, the type of the previous and current 
    // values is StateController<T>
    ref.listen<StateController<int>>(counterStateProvider.state, (previous, current) {
      // note: this callback executes when the provider value changes,
      // not when the build method is called
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Value is ${current.state}')),
      );
    });
    // watch the provider and rebuild when the value changes
    final counter = ref.watch(counterStateProvider);
    return ElevatedButton(
      // use the value
      child: Text('Value: $counter'),
      // change the state inside a button callback
      onPressed: () => ref.read(counterStateProvider.notifier).state++,
    );
  }
}
```

## Automated Testing


## Error Handling



### References
- [Complete Flutter Course Bundle](https://codewithandrea.com/courses/complete-flutter-bundle/)
- [GoRouter](https://pub.dev/packages/go_router)
- [Riverpod](https://riverpod.dev/)
- [Flutter Riverpod Ultimate Guide](https://codewithandrea.com/articles/flutter-state-management-riverpod)