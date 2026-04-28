# Mobile-First CSS Template for ANVAYA Emails

## Mobile Responsive CSS

Copy this CSS into the `<style>` section in the `<head>` of every email:

```css
@media only screen and (max-width: 640px) {
  /* Main wrapper becomes full width */
  table[class="wrapper"] {
    width: 100% !important;
  }

  /* Stack columns vertically and center all content */
  td[class="stack-column"] {
    display: block !important;
    width: 100% !important;
    text-align: center !important;
    padding-left: 0 !important;
    padding-right: 0 !important;
  }

  /* Center images with spacing below */
  td[class="stack-column"] img {
    margin: 0 auto 15px !important;
    display: block !important;
    float: none !important;
    max-width: 100% !important;
  }

  /* Center all headings */
  td[class="stack-column"] h1,
  td[class="stack-column"] h2,
  td[class="stack-column"] h3 {
    text-align: center !important;
  }

  /* Center all paragraphs and text */
  td[class="stack-column"] p {
    text-align: center !important;
  }

  /* Reset direction for RTL rows to maintain proper stacking */
  tr[dir="rtl"] {
    direction: ltr !important;
  }

  /* Full width for inner tables */
  table[width="600"],
  table[width="640"] {
    width: 100% !important;
  }

  /* Add small padding to text content on mobile */
  td[class="stack-column"] h3 {
    padding-left: 10px !important;
    padding-right: 10px !important;
  }
  td[class="stack-column"] p {
    padding-left: 10px !important;
    padding-right: 10px !important;
  }
}
```

## How to Use

1. Add this CSS to the `<style>` block in the HTML `<head>`
2. Apply `class="stack-column"` to ALL `<td>` cells in alternating sections
3. For even sections (text left, image right in desktop), use `dir="rtl"` on the table row and `dir="ltr"` on the cells
4. For odd sections (image left, text right in desktop), use normal table structure

## Section HTML Template

### Odd Sections (Image Left, Text Right)

```html
<table role="presentation" width="600" cellspacing="0" cellpadding="0">
  <tr>
    <!-- Image Cell (First in source = Top in mobile) -->
    <td class="stack-column" width="50%" style="padding: 10px; vertical-align: top;">
      <img src="image-url.jpg" width="300" alt="Description" style="display: block; width: 100%; max-width: 300px; height: auto;">
    </td>
    <!-- Text Cell (Second in source = Bottom in mobile) -->
    <td class="stack-column" width="50%" style="padding: 10px; vertical-align: top;">
      <h3 style="margin: 0 0 10px; color: #698995; font-size: 24px; font-weight: 600;">Section Title</h3>
      <p style="margin: 0; color: #333333; font-size: 14px; line-height: 1.6;">Section description text goes here.</p>
    </td>
  </tr>
</table>
```

### Even Sections (Text Left, Image Right in Desktop - but Image FIRST in source)

```html
<table role="presentation" width="600" cellspacing="0" cellpadding="0" dir="rtl">
  <tr>
    <!-- Image Cell (First in source = Top in mobile) -->
    <td class="stack-column" width="50%" style="padding: 10px; vertical-align: top;" dir="ltr">
      <img src="image-url.jpg" width="300" alt="Description" style="display: block; width: 100%; max-width: 300px; height: auto;">
    </td>
    <!-- Text Cell (Second in source = Bottom in mobile) -->
    <td class="stack-column" width="50%" style="padding: 10px; vertical-align: top;" dir="ltr">
      <h3 style="margin: 0 0 10px; color: #698995; font-size: 24px; font-weight: 600;">Section Title</h3>
      <p style="margin: 0; color: #333333; font-size: 14px; line-height: 1.6;">Section description text goes here.</p>
    </td>
  </tr>
</table>
```

## Key Points

1. **Mobile First:** Image cell is ALWAYS first in HTML source (ensures image on top in mobile)
2. **Desktop Alternating:** Use `dir="rtl"` on even sections to reverse visual order in desktop
3. **Proper Centering:** The `!important` flags ensure mobile styles override inline styles
4. **Stack Column Class:** Apply `class="stack-column"` to all cells that should stack on mobile
5. **Text Padding:** Small padding (10px) is added to h3 and p tags within stack-column cells on mobile
6. **Full Width Tables:** Inner tables become 100% width on mobile

## Why This Works

- **Image First in Source:** Guarantees image stacks on top in mobile (most important for UX)
- **`text-align: center !important`:** Overrides any inline `text-align: left` or `text-align: right`
- **`margin: 0 auto !important`:** Centers images with proper spacing below
- **`padding-left: 0 !important; padding-right: 0 !important`:** Removes desktop padding that interferes with centering
- **`td[class="stack-column"] h3` and `td[class="stack-column"] p` padding:** Adds small 10px padding to prevent text from touching edges
- **`tr[dir="rtl"]` reset:** Ensures proper stacking order on mobile

## Testing

Always test emails at mobile width (<640px) to verify:
- Images appear on top, centered
- Text appears below, centered
- All sections follow same pattern
- Proper spacing between elements
