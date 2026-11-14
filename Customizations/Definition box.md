
Paste the following contents in one snippet file, for example:
```
cd ~/.../vault/

vim .obsidian/snippets/highlights.css
```


```
.callout[data-callout="definition"] {
    --callout-color: 40, 40, 40;
    --callout-icon: none;

    background: transparent;
    border: 2px solid var(--text-normal);
    border-radius: 0;
    padding: 20px 24px;
}

.callout[data-callout="definition"] .callout-icon {
    display: none;
}

.callout[data-callout="definition"] .callout-title {
    font-weight: 600;
    font-size: 1.15em;
}

.callout[data-callout="definition"] .callout-content {
    font-size: 1.05em;
}
```

## Usage
> [!definition] Random Variables:
> A random variable ($X$) is a function from the sample space to the real numbers.
>
> $$X:S\to\mathbb{R}$$

