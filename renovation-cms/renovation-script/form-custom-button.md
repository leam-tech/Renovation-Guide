---
description: You can add custom button to form view of a doctype
---

# Form Custom Button

## Basic Usage

```typescript
core.bus.post({
    id: "form_add_custom_button",
    data: {
        label: "Send Customer Email",
        
        // optional
        group?: "CRM",
        
        onclick: (core, doc) => {
            // do something on click
        }
    }
)
```

It is pretty straightforward. No extra hidden tricks.

