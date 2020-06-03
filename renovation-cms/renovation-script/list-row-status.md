---
description: Add status indicators on ListView
---

# List Row Status

## Basic Usage

This is pretty straight forward. Provide a function that returns the status to be applied on each row.

```typescript
core.bus.post({
    id: "list_row_status",
    data: {
        // this function will be invoked on each row to get its status
        fn: (core, row) => {
            if (row.expired) {
                return "Expired";
            } else {
                return "Active";
            }
        },
        
        // What additional fields to get that may be useful for
        // computing the status
        include_fields: ["expired"]
    }
});
```

