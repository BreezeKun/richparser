# Custom Markup Parser

A lightweight Python library that converts an underscore-based markup syntax into HTML for telegram.<br>
reference: <a href="https://core.telegram.org/bots/api#rich-html-style">Rich HTML</a>

## Installation

```bash
pip install git+https://github.com/BreezeKun/richparser.git
```

Or clone and import directly:

```python
from richparser import Parse
```

## Quick Start

```python
from richparser import Parse

html = parse("_ol&start=3_li:1_li:2")

print(html)
# <ol start="3"><li>1</li><li>2</li></ol>
```

---

## Syntax Reference

### Tags

Prefix any HTML tag name with `_` to open it.

```
_tag
```

| Input | Output |
|-------|--------|
| `_b` | `<b>` |
| `_h1` | `<h1>` |

---

### Text Content

Append `:text` to a tag to set its inner text. Self-closing tags (like `<img>`) ignore text content.

```
_tag:text
```

| Input | Output |
|-------|--------|
| `_li:Hello` | `<li>Hello</li>` |
| `_p:Welcome` | `<p>Welcome</p>` |
| `_h1:Title` | `<h1>Title</h1>` |

---

### Attributes

Append `&key=value` pairs after a tag to set attributes. Boolean/flag attributes use `&flag` without a value.

```
_tag&key=value&flag
```

| Input | Output |
|-------|--------|
| `_img&src=a.jpg&alt=hello` | `<img src="a.jpg" alt="hello"/>` |
| `_input&type=checkbox&checked` | `<input type="checkbox" checked/>` |
| `_a&href=https://example.com` | `<a href="https://example.com">` |

---

### Nested Structures

Tags are chained with `_`. Child tags are automatically closed and wrapped within their parent.

```
_ul_li:1_li:2
```

```html
<ul>
  <li>1</li>
  <li>2</li>
</ul>
```

Nesting can go as deep as needed

---

## Examples

### Ordered list starting at 3

```python
parser("_ol&start=3_li:1_li:2")
```

```html
<ol start="3">
  <li>1</li>
  <li>2</li>
</ol>
```

---

### Figure with image and caption

```python
parse("_figure_img&src=a.jpg_figcaption:Hello")
```

```html
<figure>
  <img src="a.jpg"/>
  <figcaption>Hello</figcaption>
</figure>
```
---

## Syntax Cheat Sheet

| Symbol | Purpose | Example |
|--------|---------|---------|
| `_` | Tag separator / tag prefix | `_div`, `_ul_li` |
| `:` | Adds inner text to a tag | `_p:Hello` |
| `&` | Adds an attribute | `_img&src=a.jpg` |
| `&flag` | Boolean attribute (no value) | `_input&checked` |

---

## Notes

- `_` separates tokens and prefixes every tag name.
- `:` sets a tag's text content; only the first `:` per token is treated as the separator.
- `&` chains one or more `key=value` attribute pairs onto a tag.
- Some tags enforce structural rules — for example, `ul` and `ol` expect `li` children.
- Self-closing tags (`img`, `input`, `br`, `hr`, etc.) are rendered without a closing tag.
- Tag nesting is inferred from the token order; every opened container tag is closed automatically.

## License

MIT
