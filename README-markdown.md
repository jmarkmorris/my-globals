Test Markdown document for color coding.

# Header 1
## Header 2
### Header 3

**This is bold text**

*This is italic text*

`This is inline code`

"this is a quote"

> This is a blockquote

<THIS IS TEXT IN BRACKETS WITH NO SPACE AT FRONT END >

Lists
- Item 1
- Item 2
- Item 3

1. First item
2. Second item
3. Third item

```json
"editor.tokenColorCustomizations": {
    "[Your Theme Name]": {
        "textMateRules": [
            {
                "scope": "markup.inline.raw.string.markdown",
                "settings": {
                    "foreground": "#ff0000"  // Change to your desired color
                }
            }
        ]
    }
}
```