---
description: You can show your own set of dialog using this set of commands.
---

# Custom Dialog

## Basic Usage

```typescript
const dialog_fields = [
    // List of DocFields
    { fieldname: "name", label: "Name", fieldtype: "Data" }
]

const dialog_data = {
    // the title to be shown on the modal window
    title: "Test Dialog",
    
    // the fields to fill in the modal
    fields: dialog_fields,
    
    // the events to fire when specific fields change
    events: {
        name: (core, doc) => {}
    },
    
    // to define the behaviour of the main button
    primary_button: {
        // you can set any label you want
        label: "Ok!",
        
        // function called on clicking this main button
        // modal will auto close after click
        onclick: (core, doc) => {
            // do something
        }
    },
    close_button: {
        label: "Close",
        onclick: (core, doc) => {
            // say bye ?
        }
    }
}

core.bus.post({
    id: "show_dialog",
    data: dialog_data
});
```

The above code snippet can get you going with the dialog box in no time. You can control the fields and you can listen on events raised on them

## Prevent Window Auto Close

If you would like to do some validations before the user window is closed, you can declare the `onclick` function with three params. The third param, usually named `close_dialog` is a function which when invoked will actually close the dialog/modal.

If the third function was declared and was not called, the modal will not close.

```typescript
...

primary_button: {
    label: "Save",
    onclick: (core, doc, close_dialog) => {
        if (core.model.saveDoc(doc)) {
            // the window can close now.
            close_dialog()
        } else {
            // something went wrong, let user recheck what he entered
            alert("Please try again")
        }
    }
}

...
```

