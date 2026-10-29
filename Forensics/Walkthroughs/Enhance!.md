> Download this image file and find the flag. [Download image file](https://challenge-files.cylabacademy.net/library/83bfcd323363337f297f4d4ee3bff06df8564155aa2037148151710fa8baec04/drawing.flag.svg)

---

## Challenge Access

| Field      | Value                                                                                                                                                        |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Category   | Forensics                                                                                                                                                    |
| Difficulty | Medium                                                                                                                                                       |
| Author     | LT 'syreal' Jones                                                                                                                                            |
| Tags       | svg                                                                                                                                                          |
| Files      | [`drawing.flag.svg`](https://challenge-files.cylabacademy.net/library/83bfcd323363337f297f4d4ee3bff06df8564155aa2037148151710fa8baec04/drawing.flag.svg)<br> |

---

## Commands Used

- `strings` → Dump readable text out of the SVG's XML
- Browser Ctrl-A / Ctrl-F → Select and search rendered page content
- CyberChef (Remove Whitespace) → Clean up stray spacing in the recovered text

---

## Solution

### Step 1 -- Understand the Problem

An SVG isn't a pixel grid like a JPEG or PNG, it's XML: a text-based description of shapes, paths, and text elements that a renderer draws on the fly. That means an SVG can contain a `<text>`/`<tspan>` element holding real text that never visually registers to a viewer, if its styling makes it unreadable (zero size, a color matching the background, etc.), even though the text is sitting in the file in plain, searchable form. "Enhance!" plus a forensics/SVG tag is a strong hint that the interesting content isn't in what renders, it's in the markup itself.

---

### Step 2 -- Inspect the Raw SVG Text

```bash
$ strings drawing.flag.svg
...
<tspan ... style="font-size:0.00352781px;fill:#ffffff" ...>c</tspan>
<tspan ... style="font-size:0.00352781px;fill:#ffffff" ...>o</tspan>
<tspan ... style="font-size:0.00352781px;fill:#ffffff" ...>C</tspan>
<tspan ... style="font-size:0.00352781px;fill:#ffffff" ...>T</tspan>
<tspan ... style="font-size:0.00352781px;fill:#ffffff" ...>F</tspan>
<tspan ... style="font-size:0.00352781px;fill:#ffffff" ...>{</tspan>
...
```

`strings` pulls every printable run out of the file regardless of structure, so it surfaces the text content buried inside the SVG's XML even though a normal image viewer would never show it. Scrolling to the tail of the output lands on a long run of single-character `<tspan>` elements, each styled with a font size around `0.0035px` (visually zero) and `fill:#ffffff` (white on a white canvas). That combination is why the flag is invisible when the SVG renders normally: the text is technically present and drawn, just at a size and color that make it imperceptible.

---

### Step 3 -- Recover the Flag an Easier Way

Picking the fragments out of raw `strings` output by hand works but is tedious and error-prone with dozens of tiny `<tspan>` elements. A faster route: open the SVG directly in a browser, press **Ctrl-A** to select all rendered content (invisible text included, since it's still real DOM/text content, just unstyled to the eye), then **Ctrl-F** to search. The selected/searchable text includes the hidden characters, and they surface in the search or copy-paste output as a readable (if spaced-out) string.

---

### Step 4 -- Clean Up the Extracted Text

Either extraction path leaves the flag broken into fragments with extra whitespace between characters, an artifact of each letter living in its own separate `<tspan>`. Running the recovered text through CyberChef's **Remove Whitespace** operation strips the stray spaces and joins the characters back into a single clean string.

```
academy{...}
```

> Respecting the decision I made for the OverTheWire Walkthroughs, I won't be leaking any flags

---

## Key Takeaways

- **Vector formats hide data differently than raster ones.** SVG, being XML, lets you hide a flag as literal text content styled to be invisible (zero-ish font size, background-matching fill), a technique that doesn't exist for JPEG/PNG, where the equivalent would be LSB steganography or appended/metadata tricks.
- **`strings` works on more than just binaries.** Even against a text-based format like SVG, `strings` is a fast way to dump everything printable in the file and scan for anomalies, like one character per line of markup.
- **The browser is a legitimate forensics tool.** Select-all plus find-in-page will surface DOM text a renderer draws but a human eye can't perceive, which is often faster than manually parsing markup by hand.