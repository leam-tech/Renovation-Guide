---
description: You can modify the behavior of Link fields using the query-options
---

# Link Field Custom Queries

## Basic Usage

You can specify a custom python function using the `query` if you need fine control over the results.

```typescript
core.ui.setLinkQueryOptions("Item", "item_price", {
    // optional custom query
    query: "my_app.custom_item_price_link",

    // custom filters
    filters: (core, doc) => {
        return {
            item: doc.name
        }
    }
});
```

