---
description: Renovation CMS supports receiving custom routing commands from the scripts
---

# Routing

## Basic Usage

The data is forwarded as it is to the `@angular/router`

```typescript
core.bus.post({
    // the subject id
    id: "navigate_router",

    // routing data
    data: ["/form", "Item", "ITEM-00001"]
});
```

To get more finer controls over the routing, you can specify Angular's `NavigationExtras` along with the path array.

```javascript
core.bus.post({
    id: "navigate_router",
    data: {
        router: ["/form", "Item", "ITEM-00001"],
        extras: {
            // NavigationExtras
        }
    }
});
```

## Standard Routes in the Application

### Forms

The parameters required to route to a Form is the doctype and the name of the document  
`["/form", "Item", "ITEM-0001"]`

### Lists

The list view requires just the doctype  
`["/list", "Item"]`

