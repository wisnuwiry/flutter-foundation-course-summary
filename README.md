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


## Automated Testing


## Error Handling



### References
- [Complete Flutter Course Bundle](https://codewithandrea.com/courses/complete-flutter-bundle/)
- [GoRouter](https://pub.dev/packages/go_router)
- [Riverpod](https://riverpod.dev/)