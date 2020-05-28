---
description: Block the whole screen with spinners!
---

# Spinners / Loaders

## Basic Usage

```typescript
core.bus.post({
    id: "spinner_service",
    data: {
        show: true
    }
})
```

This will show an Overlay with a spinner to catch the users eyes. You can call this from anywhere in the app, be it from Dialogs, RScripts of a document, some other random event, this should work. Do not forget to call `show: false` once you are done with your heavy time consuming task! :D

