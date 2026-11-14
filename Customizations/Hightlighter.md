# Custom highlight feature

You can keep everything in one snippet file, for example:
```
.obsidian/snippets/highlights.css
```

Use this content:
```
/* =========================================
   OBSIDIAN HIGHLIGHTS
   ========================================= */

/* ---------- Inline ==highlight== ---------- */

mark,
.cm-highlight {
  background: linear-gradient(
    transparent 35%,
    rgba(255, 215, 80, 0.55) 35%,
    rgba(255, 215, 80, 0.55) 88%,
    transparent 88%
  ) !important;

  color: inherit !important;
  padding: 0 0.12em;
  border-radius: 3px;
}


/* ---------- Custom highlight callout ---------- */

.callout[data-callout="highlight"] {
  --callout-color: 230, 180, 40;
  --callout-icon: lucide-highlighter;

  background: rgba(230, 180, 40, 0.10);
  border: 1px solid rgba(230, 180, 40, 0.30);
  border-left: 4px solid rgb(230, 180, 40);

  border-radius: 8px;
  padding: 0.75em 1em;
}


/* ---------- Important callout ---------- */

.callout[data-callout="important"] {
  --callout-color: 80, 140, 240;
  --callout-icon: lucide-circle-alert;

  background: rgba(80, 140, 240, 0.10);
  border: 1px solid rgba(80, 140, 240, 0.28);
  border-left: 4px solid rgb(80, 140, 240);

  border-radius: 8px;
}


/* ---------- Example callout ---------- */

.callout[data-callout="example"] {
  --callout-color: 90, 180, 120;
  --callout-icon: lucide-lightbulb;

  background: rgba(90, 180, 120, 0.10);
  border: 1px solid rgba(90, 180, 120, 0.28);
  border-left: 4px solid rgb(90, 180, 120);

  border-radius: 8px;
}


/* ---------- Warning callout ---------- */

.callout[data-callout="warning"] {
  --callout-color: 220, 100, 80;
  --callout-icon: lucide-triangle-alert;

  background: rgba(220, 100, 80, 0.10);
  border: 1px solid rgba(220, 100, 80, 0.28);
  border-left: 4px solid rgb(220, 100, 80);

  border-radius: 8px;
}


/* ---------- Slightly improve callout typography ---------- */

.callout-title {
  font-weight: 700;
}

.callout-content {
  line-height: 1.6;
}
```

Then enable it in **Settings → Appearance → CSS snippets**.

### Usage examples
For short inline highlights:
```
This is a normal sentence with ==one important idea== highlighted.
```

For a full sentence or multiple lines:
```
> [!highlight]
> Gradient descent updates the parameters in the direction that reduces the loss.
>
> This is one of the central ideas in optimization.
```

With a custom title:
```
> [!highlight] Key intuition
> A model learns by adjusting parameters so that its predictions become less wrong.
```

For important concepts:
```
> [!important]
> The training set and test set should remain separate.
```

For examples:
```
> [!example]
> If the learning rate is too large, gradient descent may overshoot the minimum.
```

For warnings or common mistakes:
```
> [!warning]
> Do not normalize the test set using statistics calculated independently from the training set.
```

You can also collapse longer sections:
```
> [!example]- Worked example
> Suppose:
>
> - prediction = 0.8
> - target = 1.0
>
> The error is 0.2.
```

The `-` after `[!example]` makes it collapsed initially. Use `+` if you want it expanded but collapsible:
```
> [!example]+ Worked example
> Detailed explanation here.
```

For study notes, a clean convention is **yellow = highlight/key insight**, **blue = important concept**, **green = example**, and **red = warning/common mistake**.


## Usage


> [!highlight]
> Gradient descent updates the parameters in the direction that reduces the loss.
>
> This is one of the central ideas in optimization.

> [!highlight] Key intuition
> A model learns by adjusting parameters so that its predictions become less wrong.

> [!important]
> The training set and test set should remain separate.

> [!example]
> If the learning rate is too large, gradient descent may overshoot the minimum.

> [!warning]
> Do not normalize the test set using statistics calculated independently from the training set.

> [!example]+ Worked example
> Suppose:
>
> - prediction = 0.8
> - target = 1.0
>
> The error is 0.2.

Note: The `-` after `[!example]` makes it collapsed initially. Use `+` if you want it expanded but collapsible
