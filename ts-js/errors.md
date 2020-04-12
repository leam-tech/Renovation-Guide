# Errors

The interface `ErrorDetail` is used across the SDK to represent errors in a unified and structural manner.

### ErrorDetail

| property | type | required |
| :--- | :--- | :---: |
| title | string | no |
| description | string | no |
| type | `RenovationError` | no |
| info | object | yes |
| info.server\_messages | string\[\] | no |
| info.httpCode | number | no |
| info.cause | string | no |
| info.suggestion | string | no |
| info.data | data | no |
| info.rawResponse | `AxiosResponse` | no |
| info.rawError | `AxiosError` | no |

## Generic Error

In case there are not specific errors or errors that are unhandled by the front-end, a generic error is returned across all functions in the SKD.

The data within the generic error modifies the error object by:

* Setting the `title` as _Generic Error_, if the error is not set with a `title`.
* Setting the error `type` as `GenericError`, if the error is not set with a `type`.
* Setting the `httpCode` as `400`, if the error is not set with an `httpCode`.

{% hint style="info" %}
Within this documentation under each function of a module, there maybe a _Possible Errors_ section specific to the function and handled with the controller. If the section doesn't exist, however, there are no specific errors that need to be handled in the front-end, hence, a _Generic Error_ is returned.
{% endhint %}

