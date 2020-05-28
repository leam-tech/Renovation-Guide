# .auth

Authentication Module is responsible for all authentication functionality. Supported authentication mechanisms are:

* Simple \(Username/email & password\)
* Pin \(quick login\) using username & pin
* SMS \(OTP\) using mobile number & OTP

On change of the session, i.e: user logs in or logs out, `SessionStatus`, a `BehaviorSubject`, is updated.

## .login\(\)

To login using username/email and password. This method returns a `SessionStatusInfo` object.

{% hint style="info" %}
 On successful verification, the `SessionStatus` is updated with the current user.
{% endhint %}

#### Input

> `LoginParams`

| property | type | required | description |
| :--- | :--- | :---: | :--- |
| email | string | yes | The email or username of the user |
| password | string | yes | The password of the user |

#### Output

> `SessionStatusInfo`

#### Example

_async-await_

```javascript
const authResponse = await renovationInstance.auth.login({
  email: "test@abc.com",
  password: "supercomplexpassword"
});

console.log("Successful login", authResponse.data.currentUser);
```

_Classic Promise_

```javascript
renovationInstance.auth
  .login({
    email: "test@abc.com",
    password: "supercomplexpassword"
  })
  .then(authResponse =>
    console.log("Successful login", authResponse.data.currentUser)
  );
```

#### Possible Errors

**Incorrect credentials**

> **HTTP code:** `401`
>
> **type:** `AuthenticationError`
>
> **cause:** Incorrect Password
>
> **suggestion:** Enter the correct credentials

**Non-existing user**

> **HTTP code:** `404`
>
> **type:** `NotFoundError`
>
> **cause:** User disabled or missing
>
> **suggestion:** Enter the correct credentials

## .pinLogin\(\)

This method is mostly used to quickly login, for instance POS. As with `.login()`, it returns `SessionStatusInfo`.

{% hint style="info" %}
 On successful verification, the `SessionStatus` is updated with the current user.
{% endhint %}

#### Input

> `PinLoginParams`

| property | type | required | description |
| :--- | :--- | :---: | :--- |
| username | string | yes | The username of the user |
| pin | string | yes | The pin of the user \(all digits\) |

#### Output

> `SessionStatusInfo`

#### Example

_async-await_

```javascript
try {
  const authResponse = await renovationInstance.auth.pinLogin({
    user: "user1",
    pin: "123456"
  });

  console.log("Successful login", authResponse.data.currentUser);
} catch (err) {
  console.error("Logging in failed", err);
}
```

_Classic Promise_

```javascript
renovationInstance.auth
  .login({
    user: "user1",
    pin: "123456"
  })
  .then(authResponse =>
    console.log("Successful login", authResponse.data.currentUser)
  )
  .catch(err => console.error("Logging in failed", err));
```

#### Possible Errors

**Wrong PIN**

> **HTTP code:** `401`
>
> **type:** `AuthenticationError`
>
> **cause:** Wrong PIN is entered
>
> **suggestion:** Re-enter the PIN correctly

## .sendOTP\(\)

To get OTP using mobile number through SMS, this function can be used.

{% hint style="warning" %}
 The SMS settings should be configured correctly in the backend before using this function.
{% endhint %}

{% hint style="warning" %}
This method does not authenticate the user. The next function `.verifyOTP()` will provide the option to authenticate the user and create a session.
{% endhint %}

#### Input

> `SendOTPParams`

| property | type | required | description |
| :--- | :--- | :---: | :--- |
| mobile | string | yes | The mobile number of the user |
| newOTP | boolean | no | Whether to generate a new OTP or reuse the existing one |

#### Output

> `SendOTPResponse`

#### Example

_async-await_

```javascript
const authResponse = await renovationInstance.auth.sendOTP({
  mobile: "+971000000000"
});
console.log("OTP sent", authResponse.data.status);
```

_Classic Promise_

```javascript
renovationInstance.auth
  .sendOTP({
    mobile: "+971000000000"
  })
  .then(authResponse => console.log("OTP sent", authResponse.data.status));
```

#### Possible Errors

{% hint style="danger" %}
The errors depends mostly on the SMS provider setup in the backend and would be propagated through this function \(API\)
{% endhint %}

## .verifyOTP\(\)

To be used for verifying the OTP received after invoking `.sendOTP()`.

{% hint style="info" %}
On successful verification, the `SessionStatus` is updated with the current user.
{% endhint %}

{% hint style="warning" %}
Unlike `.login()`, this method will not return a `SessionStatusInfo`, instead a `VerifyOTPResponse` is returned.
{% endhint %}

#### Input

> `VerifyOTPParams`

| property | type | required | description |
| :--- | :--- | :---: | :--- |
| mobile | string | yes | The mobile number of the user |
| OTP | string | yes | The OTP received by through SMS |
| loginToUser | boolean | yes | Whether to generate a new OTP or reuse the existing one |

{% hint style="warning" %}
Note: If `loginToUser` is set to `false`, the user is not logged in and `SessionStatus` is not updated. This can be useful in some applications like validating a change of mobile number.
{% endhint %}

#### Output

> `VerifyOTPResponse`

#### Example

_async-await_

```javascript
const authResponse = await renovationInstance.auth.verifyOTP({
  mobile: "+971000000000",
  OTP: "123456",
  loginToUser: false
});
if (authResponse.success) {
  console.log("OTP Verified", authResponse.data.status);
} else {
  console.error("OTP Not Verified", authResponse.error);
}
```

_Classic Promise_

```javascript
renovationInstance.auth
  .verifyOTP({
    mobile: "+971000000000",
    OTP: "123456",
    loginToUser: false
  })
  .then(authResponse => {
    if (authResponse.success) {
      console.log("OTP Verified", authResponse.data.status);
    } else {
      console.error("OTP Not Verified", authResponse.error);
    }
  });
```

#### Sample Response

```javascript
{
  "data": {
    "status": "verified",
    "mobile": "+971502760387"
  },
  "success": true,
  "httpCode": 200
}
```

#### Possible Errors

**Invalid Pin**

> **HTTP code:** `401`
>
> **type:** `AuthenticationError`
>
> **cause:** Wrong OTP entered
>
> **suggestion:** Enter correct OTP received by SMS or generate a new OTP

**User not found / No Linked User**

> **HTTP code:** `404`
>
> **type:** `AuthenticationError`
>
> **cause:** User is either not registered or does not have a mobile number
>
> **suggestion:** Create user or add a mobile number

## .checkLogin\(\)

Checks the status of the login first locally and then verifies with the backend. `SessionStatusInfo` is returned.

#### Output

> `SessionStatusInfo`

#### Example

_async-await_

```javascript
const authResponse = await renovationInstance.auth.checkLogin();
if (authResponse.success) {
  console.log("Logged In", authResponse.data);
} else {
  console.error("Logged Out", authResponse.error);
}
```

_Classic Promise_

```javascript
renovationInstance.auth.checkLogin().then(authResponse => {
  if (authResponse.success) {
    console.log("Logged In", authResponse.data);
  } else {
    console.error("Logged Out", authResponse.error);
  }
});
```

#### Sample Response

```javascript
{
  "home_page": "/desk",
  "message": "Logged In",
  "user": "sample@user.com",
  "full_name": "A Sample User",
  "has_quick_login_pin": false
}
```

## .getCurrentUserRoles\(\)

Gets the roles of the current user logged in.

{% hint style="warning" %}
If the user is not logged in, the roles of _**Guest**_ are loaded by default.
{% endhint %}

#### Output

> `string[]`

#### Example

_async-await_

```javascript
const userRoles = await renovationInstance.auth.getCurrentUserRoles();
if (userRoles.success) {
  console.log("User Roles loaded", userRoles.data);
} else {
  console.error("User roles failed", userRoles.error);
}
```

_Classic Promise_

```javascript
renovationInstance.auth.getCurrentUserRoles().then(userRoles => {
  if (userRoles.success) {
    console.log("User Roles loaded", userRoles.data);
  } else {
    console.error("User roles failed", userRoles.error);
  }
});
```

#### Sample Response

```javascript
{
  "success": true,
  "httpCode": 200,
  "data": [
    "Translator",
    "Renovation User",
    "Blogger",
    "All",
    "Guest"
  ]
}
```

## .logout\(\)

Logs out the user locally followed by ending the session in the backend.

#### Output

> `any`

#### Example

_async-await_

```javascript
const response = await renovationInstance.auth.logout();
if (response.success) {
  console.log("Logged out", response.data);
} else {
  console.error("Error", response.error);
}
```

_Classic Promise_

```javascript
renovationInstance.auth.getCurrentUserRoles().then(response => {
  if (response.success) {
    console.log("Logged out", response.data);
  } else {
    console.error("Error", response.error);
  }
});
```

#### Sample Response

```javascript
{
  "success": true,
  "httpCode": 200,
  "data": {}
}
```

{% hint style="warning" %}
Even if the server's response is a failure, the user will need to re-login since the user is logged out in the front-end.
{% endhint %}

### Reference

#### **`SessionStatusInfo`**

| property | type | description |
| :--- | :--- | :--- |
| loggedIn | boolean | Whether the user is logged in |
| timestamp | number | The timestamp when the `SessionStatusInfo` was updated |
| currentUser | string | The current user logged in, if any |

In addition, the interface contains an _indexed property_ \(`[x: string]: any`\), containing information such as `full_name`.

