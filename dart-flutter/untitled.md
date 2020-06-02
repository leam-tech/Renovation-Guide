# Getting started

## Installation

To get started with Renovation, add the following under _dependencies_ in `pubspec.yaml`:

```yaml
lts_renovation_core: ^1.0.0
```

Then in the Terminal/CLI in the project's directory, run the following:

#### Dart \(Native/Web\)

```bash
pub get
```

#### Flutter

```bash
flutter pub get
```

## Usage

To get started, just import the package like:

```dart
import 'package:lts_renovation_core/core.dart';
```

The minimum lines to be added to initialize Renovation are:

```dart
import 'package:lts_renovation_core/core.dart';

void main() async {
  final renovationInstance = Renovation();
  await renovationInstance.init('https://backend.example.com');
}
```

{% hint style="info" %}
Renovation is instantiated as a _singleton_, so from any where in your app, as long as Renovation is initialized once, you can access it using`Renovation()`.
{% endhint %}

**Auth**

```dart
import 'package:lts_renovation_core/auth.dart';
```

**Model**

```dart
import 'package:lts_renovation_core/model.dart';
```

**Storage**

```dart
import 'package:lts_renovation_core/storage.dart';
```

**Misc**

```dart
import 'package:lts_renovation_core/misc.dart';
```

**Backend Specific**

```dart
import 'package:lts_renovation_core/backend.dart';
```

## Things to Know

### RequestResponse

Across the SDK, functions will almost always return a `RequestResponse` object as a `Future`. The structure of the class is explained below.

#### _Properties_

| property | type |
| :--- | :--- |
| httpCode | int |
| success | bool |
| data | `T` \(Generics\) |
| exc | `RenovationError` |
| error | `ErrorDetail` |
| rawResponse | `Response<String>` \(Dio's Response\) |

> Unless mentioned, all returns from the methods are enclosed within `<Future<RequestResponse<T>>` where `T` is a the return type. If the _Output_ is not as `<Promise<RequestResponse<T>>>`, the _Output_ will be marked with an asterisk \(\*\) which means the stated Output is the actual return type.

For example, `String`

> String

This mean that the return is actually `<Future<RequestResponse<String>>>`

