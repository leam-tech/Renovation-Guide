# Getting started

## Installation

Renovation is available as an `npm` package and can be installed using:

```bash
npm i --save @leam-tech/renovation-core
```

> If you feel edgy or live dangerously, use the cutting-_**edge**_ features of Renovation, it can be installed using `@leam-tech/renovation-core@edge`.

## Initialization

After the installation of the package, you can initialize by calling the `.init()`.

### Input

| property | type | required | description |
| :--- | :--- | :---: | :--- |
| backend | string | no | The type of backend to be used. |
| hostUrl | string | no | The URL to the backend |
| clientId | string | no | The client ID used for Nginx |

### Usage

```javascript
import { Renovation } from "./renovation";

const renovation = new Renovation();
const backend = "frappe";
const hostURL = "https://test-site.abc.com";

(async () => await renovation.init(backend, hostURL, "test-site"))();

// Start using Renovation Core....

renovation.auth
  .login({
    email: "test-user@abc.com",
    password: "mostcomplexpassword"
  })
  .then(response => console.log(response));

// .....
```

## Things to Know

### [RequestResponse](http://core-docs.surge.sh/classes/requestresponse.html)

Across the SDK, functions will almost always return a `RequestResponse` object as a `Promise`. The structure of the class is explained below.

#### _Properties_

| property | type | required |
| :--- | :--- | :---: |
| httpCode | number | yes |
| success | boolean | yes |
| data | `T` \(Generics\) | yes |
| exc | `RenovationError` | no |
| error | `ErrorDetail` | no |

> Unless mentioned, all returns from the methods are enclosed within `<Promise<RequestResponse<T>>` where `T` is a the return type. If the _Output_ is not as `<Promise<RequestResponse<T>>>`, the _Output_ will be marked with an asterisk \(\*\) which means the stated Output is the actual return type.

For example, `RenovationDocument`

> `RenovationDocument`

This mean that the return is actually `<Promise<RequestResponse<RenovationDocument>>>`

## [API Documentation](http://core-sdk-guide.surge.sh)

