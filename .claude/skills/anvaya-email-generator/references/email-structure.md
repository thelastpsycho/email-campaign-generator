# ANVAYA Email Structure Reference

This document provides detailed technical specifications for generating responsive HTML emails for ANVAYA Beach Resort Bali.

## HTML Email Best Practices

### DOCTYPE and Meta Tags

Always use the HTML5 email boilerplate:

```html
<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml" xmlns:v="urn:schemas-microsoft-com:vml" xmlns:o="urn:schemas-microsoft-com:office:office">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="x-apple-disable-message-reformatting">
  <title></title>
  <!--[if mso]>
  <noscript>
    <xml>
      <o:OfficeDocumentSettings>
        <o:PixelsPerInch>96</o:PixelsPerInch>
      </o:OfficeDocumentSettings>
    </xml>
  </noscript>
  <![endif]-->
</head>
<body>
```

### Inline CSS Requirements

Email clients have limited CSS support. Use inline styles for all elements:
- No external stylesheets
- No `<style>` blocks in head (except for media queries)
- Inline styles on all elements
- Use `!important` sparingly and only when necessary

### Table-Based Layouts

Use nested tables for layout structure:
- Outer wrapper table with max-width 640px
- Inner tables for sections
- Use `role="presentation"` on layout tables
- Set `cellpadding="0" cellspacing="0" border="0"` on all tables

## Responsive Design Patterns

### Desktop Layout (640px max-width)

```html
<table role="presentation" width="640" align="center" style="margin: 0 auto; max-width: 640px;">
  <!-- Content -->
</table>
```

### Mobile Layout (100% width with media query)

Add media query in `<head>`:

```html
<style>
  @media only screen and (max-width: 640px) {
    table[class="wrapper"] {
      width: 100% !important;
    }
    td[class="stack-column"] {
      display: block !important;
      width: 100% !important;
      padding-bottom: 20px !important;
    }
    img[class="fluid"] {
      width: 100% !important;
      height: auto !important;
    }
  }
</style>
```

## Section Layout Logic

### Alternating Side-by-Side Pattern (Mobile-First: Image Always Top)

**CRITICAL:** For mobile responsiveness, always place the image cell FIRST in the HTML source. This ensures image appears on top when stacked on mobile. Use table direction to achieve alternating desktop layout.

**Section 1 (Image Left, Text Right):**
```html
<tr>
  <td class="stack-column" width="50%" style="padding: 10px;">
    <img src="image-url" width="300" style="display: block; width: 100%; max-width: 300px;">
  </td>
  <td class="stack-column" width="50%" style="padding: 10px;">
    <p style="margin: 0;">Text content here</p>
  </td>
</tr>
```

**Section 2 (Text Left, Image Right - but Image FIRST in source):**
```html
<tr dir="rtl">
  <td class="stack-column" width="50%" style="padding: 10px;" dir="ltr">
    <img src="image-url" width="300" style="display: block; width: 100%; max-width: 300px;">
  </td>
  <td class="stack-column" width="50%" style="padding: 10px;" dir="ltr">
    <p style="margin: 0;">Text content here</p>
  </td>
</tr>
```

**Section 3+:** Continue pattern - odd sections normal, even sections with `dir="rtl"` on row and `dir="ltr"` on cells.

### How This Works

- **Mobile:** Images always stack first because they're first in source
- **Desktop:** `dir="rtl"` on the row reverses visual order, making text appear on left for even sections
- **`dir="ltr"` on cells:** Maintains proper text direction within each cell

### Mobile Stacking

Use `class="stack-column"` on cells that should stack vertically on mobile. The media query will make these display: block at 100% width.

**CRITICAL Mobile CSS (must be included in every email):**
```css
@media only screen and (max-width: 640px) {
  table[class="wrapper"] { width: 100% !important; }
  td[class="stack-column"] {
    display: block !important;
    width: 100% !important;
    text-align: center !important;
    padding-left: 0 !important;
    padding-right: 0 !important;
  }
  td[class="stack-column"] img {
    margin: 0 auto 15px !important;
    display: block !important;
    float: none !important;
    max-width: 100% !important;
  }
  td[class="stack-column"] h1,
  td[class="stack-column"] h2,
  td[class="stack-column"] h3 {
    text-align: center !important;
  }
  td[class="stack-column"] p {
    text-align: center !important;
  }
  tr[dir="rtl"] {
    direction: ltr !important;
  }
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

**Why This Works:**
- `text-align: center !important` with padding overrides ensures all content centers
- `margin: 0 auto 15px !important` centers images with spacing below
- `padding-left: 0 !important; padding-right: 0 !important` removes desktop padding interference
- `tr[dir="rtl"]` reset maintains proper stacking order

Since image cell is always first in source, it will always appear on top in mobile view, properly centered.

## ANVAYA Header Structure

### Pre-built Header GIF (Centered)

```html
<!-- Header GIF -->
<table role="presentation" width="640" align="center" style="margin: 0 auto;">
  <tr>
    <td>
      <img src="https://email-static.useinsider.com/f940abcdc21d4566ac97b97fb4e8650f/lib/pluginId_f940abcdc21d4566ac97b97fb4e8650f_anvayaprod_images/emailmarketingtheanvaya_01.gif" width="640" alt="ANVAYA Beach Resort Bali" style="display: block; width: 100%; max-width: 640px; height: auto; margin: 0 auto;">
    </td>
  </tr>
</table>
```

### Header Details
- **Wrapper table:** 640px width, centered with `align="center"` and `margin: 0 auto`
- **Image:** Pre-built GIF containing complete header design
- **URL:** https://email-static.useinsider.com/f940abcdc21d4566ac97b97fb4e8650f/lib/pluginId_f940abcdc21d4566ac97b97fb4e8650f_anvayaprod_images/emailmarketingtheanvaya_01.gif
- **Width:** 100% with max-width 640px
- **Height:** Auto
- **Display:** Block with `margin: 0 auto` for centering
- **Padding:** None
- **Background:** Transparent (matches email background)

## ANVAYA Hero Section Structure

### Hero with Title/Subtitle Below Image

```html
<!-- Hero Section -->
<table role="presentation" width="100%" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 32px 40px 0;">
      <!-- Hero Image with White Border -->
      <table role="presentation" width="100%" cellspacing="0" cellpadding="0" style="border: 1px solid #FFFFFF;">
        <tr>
          <td>
            <img src="hero-image-url" class="fluid" width="640" height="400" alt="Hero description" style="display: block; width: 100%; max-width: 640px; height: 400px; object-fit: cover;">
          </td>
        </tr>
      </table>
      <!-- Title and Subtitle -->
      <table role="presentation" width="100%" cellspacing="0" cellpadding="0" style="padding-top: 40px;">
        <tr>
          <td style="text-align: center;">
            <h1 style="margin: 0 0 15px; color: #333333; font-size: 28px; font-weight: 600; letter-spacing: 0.5px;">Promotion Title Here</h1>
            <p style="margin: 0; color: #666666; font-size: 16px; font-weight: 300; line-height: 1.6; max-width: 560px; margin-left: auto; margin-right: auto;">Engaging subtitle that describes the promotion and encourages action goes here with elegant formatting.</p>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>
```

### Hero Styling Details
- **Image dimensions:** 640px × 400px (desktop), 100% width × auto height (mobile)
- **Border:** 1px solid white (#FFFFFF)
- **Title:** Dark gray #333333, 28px, font-weight 600, centered
- **Subtitle:** Light gray #666666, 16px, font-weight 300, centered, line-height 1.6
- **Spacing:** 32px padding above image, 40px below subtitle
- **Max-width:** Title and subtitle constrained to maintain readability

## Image Sizing Guidelines

### Hero Image
- **Recommended:** 640px × 400px (landscape, optimized for desktop)
- **Desktop display:** Fixed height 400px, width 100% max-width 640px
- **Mobile display:** 100% width, auto height
- **Format:** JPG or PNG
- **Alt text:** Always include descriptive alt text
- **Border:** 1px solid white (#FFFFFF)
- **Object-fit:** Use `object-fit: cover` to maintain aspect ratio

### Section Images
- **Recommended:** 300px × 225px (3:4 ratio, half width)
- **Maximum:** 320px wide
- **Inline style:** `width: 100%; max-width: 300px; height: auto; display: block;`

## Image Sizing Guidelines

### Hero Image
- **Recommended:** 640px × 400px (landscape for desktop)
- **Desktop display:** Fixed height 400px, 100% width max-width 640px
- **Mobile display:** 100% width, auto height
- **Format:** JPG or PNG
- **Border:** 1px solid white (#FFFFFF)
- **Alt text:** Always include descriptive alt text
- **Inline style:** `width: 100%; max-width: 640px; height: 400px; object-fit: cover; display: block;`

### Section Images
- **Recommended:** 300px × 225px (same 3:4 ratio, half width)
- **Maximum:** 320px wide
- **Inline style:** `width: 100%; max-width: 300px; height: auto; display: block;`

## CTA Button Variations

### Button Structure

```html
<table role="presentation" cellspacing="0" cellpadding="0" border="0" style="margin: 20px auto;">
  <tr>
    <td style="background-color: #698995; border-radius: 4px; text-align: center;">
      <a href="#" style="display: inline-block; padding: 15px 30px; color: #ffffff; text-decoration: none; font-weight: bold; font-size: 16px;">Button Text</a>
    </td>
  </tr>
</table>
```

### CTA Text Options by Context

**Wellness/Spa:**
- "Book Your Spa Journey"
- "Discover Wellness"
- "Reserve Your Treatment"
- "Explore Spa Packages"

**Dining:**
- "Discover Dining"
- "View Menu"
- "Reserve Your Table"
- "Explore Restaurants"

**Activities:**
- "Learn More"
- "View Activities"
- "Book Your Experience"
- "Discover Activities"

**Accommodation:**
- "Book Now"
- "View Rooms"
- "Check Availability"
- "Explore Stays"

**General/Promotion:**
- "Book Now"
- "Learn More"
- "View Offer"
- "Get Started"

## Color Usage Guidelines

### Primary Color: #698995 (Teal)
Use for:
- CTA buttons (background)
- Primary links
- Headings and subheadings
- Key highlights

### Secondary Color: #9ABCC0 (Light Teal)
Use for:
- Section backgrounds
- Subtle highlights
- Decorative elements

### Accent Color: #80BC85 (Green)
Use for:
- Success messages
- Special offers
- Callout boxes
- Positive highlights

### Neutral Color: #D0D2D4 (Light Gray)
Use for:
- Disclaimer boxes
- Separator lines
- Background areas
- Footer sections

### Text Colors
- **Headings:** #000000 (black)
- **Body text:** #333333 (dark gray)
- **Links:** #698995 (primary teal)
- **Light text on dark:** #ffffff (white)

## Typography Implementation

### Google Fonts Import

Add in `<head>`:

```html
<link href="https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@400;600;700&display=swap" rel="stylesheet">
```

### Font Stack

Use inline font-family with fallback:

```html
style="font-family: 'Nunito Sans', Arial, sans-serif;"
```

### Font Weights
- **Regular:** 400 (body text)
- **Semi-bold:** 600 (subheadings, emphasis)
- **Bold:** 700 (headings, CTA buttons)

### Font Sizes
- **Headings:** 24-32px
- **Subheadings:** 18-20px
- **Body text:** 14-16px
- **Small text:** 12px (footers, disclaimers)

## Insider One Compatibility

### Supported Features
- Inline CSS styles
- Media queries for responsive design
- HTML5 doctype
- Standard table layouts
- Alt text on images

### Limitations to Avoid
- JavaScript (not supported)
- External stylesheets (use inline)
- Background images (limited support)
- CSS positioning (use tables)
- Forms (limited support)

### Testing Recommendations
Always test in:
- Gmail (desktop and mobile)
- Outlook (Windows and Mac)
- Apple Mail (iOS and macOS)
- Android email clients

## Complete Email Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="x-apple-disable-message-reformatting">
  <title>ANVAYA Beach Resort Bali</title>
  <link href="https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    @media only screen and (max-width: 640px) {
      table[class="wrapper"] { width: 100% !important; }
      td[class="stack-column"] { display: block !important; width: 100% !important; }
      img[class="fluid"] { width: 100% !important; height: auto !important; }
    }
  </style>
</head>
<body style="margin: 0; padding: 0; font-family: 'Nunito Sans', Arial, sans-serif;">

  <!-- Header GIF -->
  <table role="presentation" width="640" align="center" style="margin: 0 auto;">
    <tr>
      <td>
        <img src="https://email-static.useinsider.com/f940abcdc21d4566ac97b97fb4e8650f/lib/pluginId_f940abcdc21d4566ac97b97fb4e8650f_anvayaprod_images/emailmarketingtheanvaya_01.gif" width="640" alt="ANVAYA Beach Resort Bali" style="display: block; width: 100%; max-width: 640px; height: auto; margin: 0 auto;">
      </td>
    </tr>
  </table>

  <!-- Main Wrapper -->
  <table role="presentation" class="wrapper" width="640" align="center" style="margin: 0 auto; max-width: 640px;">

    <!-- Hero Section with Title/Subtitle -->
    <tr>
      <td style="padding: 32px 40px 0;">
        <!-- Hero Image with White Border -->
        <table role="presentation" width="100%" cellspacing="0" cellpadding="0" style="border: 1px solid #FFFFFF;">
          <tr>
            <td>
              <img src="hero-image-url" class="fluid" width="640" height="400" alt="Hero description" style="display: block; width: 100%; max-width: 640px; height: 400px; object-fit: cover;">
            </td>
          </tr>
        </table>
        <!-- Title and Subtitle -->
        <table role="presentation" width="100%" cellspacing="0" cellpadding="0" style="padding-top: 40px;">
          <tr>
            <td style="text-align: center;">
              <h1 style="margin: 0 0 15px; color: #333333; font-size: 28px; font-weight: 600; letter-spacing: 0.5px;">Promotion Title Here</h1>
              <p style="margin: 0; color: #666666; font-size: 16px; font-weight: 300; line-height: 1.6; max-width: 560px; margin-left: auto; margin-right: auto;">Engaging subtitle that describes the promotion and encourages action.</p>
            </td>
          </tr>
        </table>
      </td>
    </tr>

    <!-- Content Sections (repeat as needed) -->
    <!-- Section 1: Image Left, Text Right -->
    <tr>
      <td style="padding: 20px;">
        <table role="presentation" width="100%" cellspacing="0" cellpadding="0">
          <tr>
            <td class="stack-column" width="50%" style="padding: 10px; vertical-align: top;">
              <img src="section-image-1.jpg" width="300" alt="Description" style="display: block; width: 100%; max-width: 300px; height: auto;">
            </td>
            <td class="stack-column" width="50%" style="padding: 10px; vertical-align: top;">
              <h2 style="margin: 0 0 10px; color: #698995; font-size: 24px; font-weight: 600;">Section Title</h2>
              <p style="margin: 0; color: #333333; font-size: 14px; line-height: 1.6;">Section description text goes here.</p>
            </td>
          </tr>
        </table>
      </td>
    </tr>

    <!-- CTA Button -->
    <tr>
      <td style="text-align: center; padding: 30px 20px;">
        <table role="presentation" cellspacing="0" cellpadding="0" border="0" style="margin: 0 auto;">
          <tr>
            <td style="background-color: #698995; border-radius: 4px; text-align: center;">
              <a href="#" style="display: inline-block; padding: 15px 30px; color: #ffffff; text-decoration: none; font-weight: bold; font-size: 16px;">Book Now</a>
            </td>
          </tr>
        </table>
      </td>
    </tr>

  </table>

  <!-- Footer (insert footer-template.html content here) -->
</body>
</html>
```
