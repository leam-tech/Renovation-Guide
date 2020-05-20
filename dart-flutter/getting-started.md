# Getting started

## Installation

To get started with Renovation, add the following under _dependencies_ in `pubspec.yaml`:

```yaml
renovation_core: ^1.0.2
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
import 'package:renovation_core/core.dart';
```

The minimum lines to be added to initialize Renovation are:

```dart
import 'package:renovation_core/core.dart';

void main() async {
  final renovationInstance = Renovation();
  await renovationInstance.init('https://backend.example.com');
}
```

{% hint style="info" %}
Renovation is instantiated as a _singleton_, so from any where in your app, as long as Renovation is initialized once, you can access it using`Renovation()`.
{% endhint %}

#### More Required Params

In addition to the `hostURL` being a required param, either of the below is required when initializing:

* `cookieDir` \(string\)
* `useJWT` \(bool\)

{% tabs %}
{% tab title="cookieDir" %}
When using cookies for session authentication, you must specify the cookies directory. For instance:

```dart
await renovation.init('https://backend.example.com',
      cookieDir: './cookies');
```
{% endtab %}

{% tab title="useJWT" %}
If JWT is to be used and `renovation_core` is installed in the backend, you can enable it as follows:

```dart
await renovation.init('https://backend.example.com', useJWT: true);
```
{% endtab %}
{% endtabs %}

## Libraries

**Auth**

```dart
import 'package:renovation_core/auth.dart';
```

**Model**

```dart
import 'package:renovation_core/model.dart';
```

**Storage**

```dart
import 'package:renovation_core/storage.dart';
```

**Misc**

```dart
import 'package:renovation_core/misc.dart';
```

**Backend Specific**

```dart
import 'package:renovation_core/backend.dart';
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

> Unless mentioned, all returns from the methods are enclosed within `<Future<RequestResponse<T>>` where `T` is a the return type. If the _Output_ is not as `<Future<RequestResponse<T>>>`, the _Output_ will be marked with an asterisk \(\*\) which means the stated Output is the actual return type.

For example, `String`

> String

This mean that the return is actually `<Future<RequestResponse<String>>>`

## Featured Methods

There are some methods that are either completely or partially dependent on a custom frappe app \([renovation\_core](https://github.com/leam-tech/renovation_core.git)\). When using a method/feature that requires a custom app, an error will be thrown in case the app is not installed in your site. For example: 

```javascript
Unhandled exception:
The app "renovation_core" is not installed in the backend.
Please install it to be able to use the feature(s):


withLinkFields
tableFields
```

Throughout the guide, there will be indications when a method is completely or partially dependent on a custom frappe app.

Completely dependent: ★

Partially dependent: ☆

{% hint style="info" %}
By default, if there are no indications, it means the method is independent of a custom frappe app.In other words, it supports Vanilla Frappé.
{% endhint %}

