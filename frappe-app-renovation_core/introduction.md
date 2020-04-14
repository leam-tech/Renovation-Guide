# Introduction

### Doctypes & Pages

There are multiple doctypes between single, regular and child tables across three Modules.

#### Single

* Renovation Image Settings \[Renovation Setup\]: Apply thumbnails and apply image scaling automatically on images uploaded.

#### List

* Broadcast Message\[Renovation Core\]: Used to send messages through different channels instantly \(FCM, Email, SMS\).
* Renovation Dashboard \[Renovation Dashboard Def\]: Create dashboards to be rendered in the frontend. It can be customized to be shown for certain roles.
  * Renovation Dashboard Layout\[Renovation Dashboard Def\]: The layout criteria of a list of dashboards in a grid for instance \(width x height\).
* Renovation DocDefaults\[Renovation Setup\]: Rules to set fields of a doctype by default. There are _three_ options: Static, Evaluate \(write python code in the doctype\), or Method which is invoking a python function written in the app.
* Renovation DocField\[Renovation Core\]: The data holding doctype for DocField Manager Page.
* Renovation Report\[Renovation Setup\]: An extension to "Reports" to add filtering functionality of the fields in the report.
* Renovation Script\[Renovation Core\]: A JS script written for a doctype that will be evaluated in the frontend impacting UI and/or functionality in the form. Check Renovation-CMS.
* Renovation Sidebar\[Renovation Core\]: Managing the sidebar in the front-end app. Can be specific per roles.

#### Page

* DocField Manager\[Renovation Core\]: Control what fields of a doctype to show/hide \(maybe development specific field that need not be shown to a client\). It can be configured globally, per role or per user. This is different than permissions.

### Features & APIs

In addition to the functionalities that the doctypes/pages bring to the table, below are the features that `renovation_core` provide either behind the scenes or as API

* Authentication using SMS
* Using JWT instead of SID in cookies for authentication. Useful in case of SSR applications and in Flutter applications.
* Integration with FCM \(Token registration, unregister, and mark as seen\).
* Added FCM as a channel in _**Notification**_ doctype.
* Uploading file using _**socket.io**_ with progress indication \(instead of a single blocking request\).
* Custom getList API to get child tables details and link fields as objects
* Logging settings for handling logging in a logging site. Check ****[**renovation\_logging**](https://github.com/leam-tech/renovation_loggging.git)\*\*\*\*
* Additional settings in SMS settings to handle JSON requests and '+' as prefix to mobile numbers.
* Utility function to translate fields of docs fetched through APIs \(mainly guest users\).

