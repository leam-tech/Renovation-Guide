---
description: >-
  You can find all the necessary details to implement a bar graph in your
  dashboard here.
---

# Bar Graphs

Bar graphs can be grouped / ungrouped**.** The responses will have to made according to that.

![Grouped Bar Graph](../../.gitbook/assets/image%20%284%29.png)

## Grouped Bar Graph

```javascript
{
    xlabel: "Months",
    ylabel: "Sales",
    grouped: true,
    group_labels: ["1st Week", "2nd Week", "3rd Week", "4th Week"],
    data=[
        { label: "Jan", values: [300, 400, 350, 370] },
        { label: "Feb", values: [300, 400, 350, 370] },
        { label: "Mar", values: [300, 400, 350, 370] },
        { label: "Apr", values: [300, 400, 350, 370] },
        { label: "May", values: [300, 400, 350, 370] },
        // ...
    ]
}
```

