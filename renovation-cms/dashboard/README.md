---
description: Powerful & Flexible dashboards can give you a quick glimpse of your business.
---

# Dashboard

Dashboards in Renovation can be custom made in any form you want. You can implement custom logic to them, have them work in your way.

![](../../.gitbook/assets/image%20%283%29.png)

There are two main doctypes, `Renovation Dashboard` & `Renovation Dashboard Layout`. The `Renovation Dashboard` is a single dashboard item, it can be a `bar` or a `pie` graph. `Renovation Dashboard Layout` can be then used to lay out these graphs of yours in a way that fits your needs.

## Renovation Dashboard Layout

Layouts can be made easily. Just create a new `Renovation Dashboard Layout`, specify a title for it and start listing all the dashboards in the order you require, with their sizes \(widths & heights\). You can specify if the items can resized / rearranged. Priority and Roles can give you more finer controls in making it work for your needs.

## Renovation Dashboard

These form the basic building block of your dashboard. You can mention the `title` , `subtitle` and other required parameters. There are various properties on this document on which you will need more information on:

* `Type` Decides what kind of dashboard this is going to be. It will take values `list | lines | bar | pie | percent | change | simple | list` .
* `Is Standard` check this to make a python module for your dashboard
* `Custom Caching` when checked, it will look for a `clear_cache_on_doc_events` on the module where you can manage the cache yourself. This is helpful when the cache needs to be cleared in a more finer way.
* `Exc Type`
* `cmd`
* `params`
* `roles`
* `Purge Cache`

