---
title: Right-to-left Languages
description: Support for right-to-left languages like Arabic and Hebrew in DocSpring
sidebar:
  order: 8
---

DocSpring supports right-to-left languages such as Arabic and Hebrew.

## String Order

When submitting data that contains Hebrew, Arabic, or other right-to-left text, please provide the strings in **logical order** (left-to-right). DocSpring will automatically detect and convert the text to **visual order** (right-to-left) when rendering it on the PDF. We will also embed a TTF font (Arial) for any fields that contain Hebrew or Arabic text.

### What is Logical Order?

**Logical order** is the sequence in which characters are stored in memory and typed on a keyboard. This is the standard way text is handled in modern software systems, following the Unicode standard. When you type Hebrew or Arabic text, even though it appears right-to-left on your screen, the characters are stored in the order they were typed.

**Visual order**, on the other hand, is how the text appears when displayed. For RTL languages, this means the text is rendered right-to-left.

### Field Alignment

When setting up a PDF template that uses a right-to-left language, you should change the default text field alignment from left to right. This will ensure that your text is right-aligned in your fields. You can change this by visiting your Template Settings, then scroll down to the "Text Defaults" section on the right. This default text alignment setting can be overridden for each field, so your template can also include some fields that use a left-to-right language.

### Example

When sending Hebrew or Arabic text to DocSpring's API, submit it in the same character sequence as stored in your source files or database. This is the standard Unicode representation.

```json
{
  "fields": {
    "hebrew_greeting": "שלום עולם",
    "mixed_text": "Hello שלום 123",
    "arabic_text": "مرحبا بالعالم"
  }
}
```

The text should be encoded in UTF-8 and sent in logical order (the order in which characters are stored in memory). DocSpring will automatically apply the Unicode Bidirectional Algorithm to render the text correctly as right-to-left on the PDF.

### Common Issues with Mixed LTR/RTL Text

When mixing Latin text with Hebrew or Arabic, certain combinations can produce unexpected results due to how the Unicode Bidirectional Algorithm handles neutral characters (like punctuation and spaces). This is not a bug but rather the specified behavior according to the [Unicode Bidirectional Algorithm (UAX #9)](https://www.unicode.org/reports/tr9/).

#### Example: "S&P500" in RTL Context

A common issue occurs with text like "S&P500" when placed in an RTL paragraph. The ampersand (&) and any following spaces are neutral characters. In an RTL context, they resolve toward the Hebrew/Arabic text on their right, causing the number block to be pulled in front of the Latin letters, resulting in "P500&S".

This issue is documented in [Section 6.4: Separating Punctuation Marks](https://www.unicode.org/reports/tr9/#Separators) of the Unicode specification.

:::note
All major text rendering engines (ICU, FriBidi, Pango, HarfBuzz) implement this behavior consistently. The output you see follows the Unicode specification.
:::

#### Solutions

To prevent this reordering:

1. **Use directional marks**: Insert a Left-to-Right Mark (`LRM`, `U+200E`) or Right-to-Left Mark (`RLM`, `U+200F`) to control text direction:
   - Instead of: `"S&P500"`
   - Use: `"S&P\u200E500"` (`LRM` after &)

2. **Use directional isolates**: Wrap Latin text in Left-to-Right Isolate (`LRI`, `U+2066`) and Pop Directional Isolate (`PDI`, `U+2069`) characters:
   - Instead of: `"S&P500"`
   - Use: `"\u2066S&P500\u2069"` (`LRI` + text + `PDI`)

3. **Pre-compensate for the reordering**: If directional marks aren't a viable solution, you can work around the issue by submitting the text in a way that anticipates the reordering:
   - `"P&S 500"` will render as "S&P 500" after reordering
   - `"P500&S"` will render as "S&P500" after reordering

:::note
DocSpring automatically strips directional control characters (`U+2066`, `U+2069`, `U+200E`, `U+200F`) after applying the bidirectional algorithm, since these characters can render as boxes or strange characters in some PDF viewers. This means you can safely use these characters to control text direction without worrying about them appearing in the final PDF.
:::

---

## Example

Here is a screenshot of an example generated PDF that includes both Hebrew and English text:

![Hebrew and English text example](@images/hebrew-and-english.png)

:::note
We are planning to add support for custom fonts in the future.
:::

_Please contact [support@docspring.com](mailto:support@docspring.com) if you need a different font._
