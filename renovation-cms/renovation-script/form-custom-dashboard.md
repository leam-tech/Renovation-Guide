---
description: 'You can add custom dashboards to Form view, beside the Attachments and Tags.'
---

# Form Custom Dashboard

You have to get familiarized with the interface `DashboardElement`, which is the basic building block of custom dashboards.

### DashboardElement Interface

```typescript
interface DashboardElement {
  type: "text" | "link" | "link-href" | "btn" | "icon-btn" | "loop";
  /** When type = Text  */
  text?: string;
  /** Property in context doc */
  property?: string;
  class?: string;
  style?: string;
  target?: string;
  /** Type: icon-btn */
  icon?: string;

  onclick?: (
    core: Renovation,
    doc: RenovationDocument,
    frm: FormController
  ) => void;
  /** type: link-href */
  href?: (item: unknown) => string;
  /** Router link (type: link) (Returns array of route) */
  link?: (item: unknown) => string[];
  /** type: loop */
  template?: DashboardElement[];
}
```

### Basic Usage

You have to post to `form_dashboard_item` with the dashboard data

```typescript
const dasboardData = {
    // some random id to distinguish your dashboard
    id: "test_dashboard",
    
    header: DasboardElement[],
    items: DasboardElement[],
    itemsClass,
    itemsMaxHeight,
    itemsStyle,
    itemsListClass,
    itemsTemplate: DasboardElement[],
    footer: DasboardElement[],
    
    // optional init function
    // this gets executed onload of form
    init: (core, doc, frm) => {
    },
    
    // optional function that gets executed whenever
    // frappe-docevent gets updated
    onDocInfoLoaded: (core, doc, frm) => {
    },
    
    // optional function to decide whether to show this dash or not
    condition: (core, doc, frm) => {
    }
};

core.bus.post({
    id: "form_dashboard_item",
    data: dasboardData
});
```

### Attachment Dashboard Example

```typescript
const dashData = {
      condition: (core, doc) => {
        return !doc.__islocal;
      },
      header: [
        { type: "text", text: "Attachments", class: "title" },
        {
          type: "icon-btn",
          icon: "plus",
          class: "p-0",
          onclick: (core, doc) => {
            this.openUploader(null);
          }
        }
      ],
      init: function(core, doc) {},
      onDocInfoLoaded: function(core, doc) {
        form.updateAttachmentsDashboard();
      },
      failed: function() {
        // to show load fail
        this.items = null;
        this.footer = [
          {
            type: "text",
            text: "No Attachments",
            class: "text-center mt-1 h6 uppercase"
          }
        ];
      },
      items: [],
      itemsTemplate: [
        {
          type: "link-href",
          href: (item: { file_url: string }) =>
            form.coreService.core.storage.getUrl(item.file_url),
          target: "_blank",
          class: "flex-grow-1 text-secondary small",
          property: "file_name"
        },
        {
          type: "icon-btn",
          icon: "close",
          onclick: (core, item) => {
            this.deleteAttachment(item);
          }
        }
      ],
      footer: []
    };
```

### Assignment Dashboard Example

```typescript
const dashData = {
      condition: (core, doc, frm) => {
        return !doc.__islocal;
      },
      header: [
        { type: "text", text: "Assign Doc", class: "title" },
        {
          type: "icon-btn",
          icon: "plus",
          class: "p-0",
          onclick: (core, doc) => {
            this.openAssignDocDialog();
          }
        }
      ],
      items: [],
      init: async function(core: Renovation, doc) {
        this.items = [];
        this.footer = [];
        const r = await core.model.getUsersAssignedToDoc({
          doctype: me.doc.doctype,
          docname: me.doc.name
        });
        if (r.success && r.data && r.data.length) {
          this.items = r.data;
        } else {
          this.footer = [
            {
              type: "text",
              text: "No Assignments",
              class: "text-center mt-1 h6 uppercase"
            }
          ];
        }
      },
      itemsTemplate: [
        {
          type: "link",
          link: (user: GetUsersAssignedToDocResponse) => [
            "/",
            "form",
            "User",
            user.assignedTo
          ],
          target: "_blank",
          class: "flex-grow-1 text-secondary small",
          property: "assignedTo"
        },
        {
          type: "icon-btn",
          icon: "close",
          onclick: (core: Renovation, item: GetUsersAssignedToDocResponse) => {
            core.messages.next({
              isPrompt: true,
              title: "Unassign Document",
              message: `Are you sure to unassign ${item.assignedTo} from the document ?`,
              handler: async v => {
                if (!v) {
                  return;
                }
                this.unassignDocument(item.assignedTo);
              }
            });
          }
        }
      ]
    };
```

