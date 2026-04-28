---
name: anvaya-email-generator
description: This skill should be used when the user asks to "create an email", "generate a marketing email", "build an email template", "create a promotional email for ANVAYA", "create ANVAYA email", or mentions ANVAYA Beach Resort Bali email marketing. Generates responsive HTML emails with consistent ANVAYA branding for wellness, dining, activities, and accommodation promotions.
version: 1.0.0
---

# ANVAYA Email Generator

Generate responsive HTML email templates for ANVAYA Beach Resort Bali marketing campaigns. This skill produces professionally designed emails consistent with ANVAYA's brand guidelines, suitable for use with Insider One email service provider.

## Purpose

Transform user prompts into production-ready HTML email templates. Parse offerings from natural language requests, apply consistent ANVAYA branding, and generate responsive layouts that work on both mobile and desktop devices.

## Brand Assets

**Logo:** https://www.theanvayabali.com/wp-content/uploads/2023/01/anvaya-logo.png

**Color Palette:**
- Primary: #698995 (teal) - headings, CTAs, links
- Secondary: #9ABCC0 (light teal) - backgrounds, highlights
- Accent: #80BC85 (green) - offers, success messages
- Neutral: #D0D2D4 (light gray) - separators, footer areas

**Typography:** Nunito Sans (Google Fonts)
- Import: `https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@400;600;700&display=swap`
- Font stack: `'Nunito Sans', Arial, sans-serif`

## Input Parsing

Parse the user prompt to extract:

**1. Offerings:** Identify distinct offerings mentioned in the prompt
- Examples: "sakanti spa", "boxing class", "yoga", "beachfront restaurant", "sunset bar"
- Create one content section per offering
- If multiple offerings are mentioned, create alternating side-by-side sections

**2. Images:** Extract image URLs provided by the user OR select from ANVAYA image library
- If user provides image URLs: Use those URLs
- If no images provided: Select appropriate images from `references/image-reference.md` based on email theme
  - Wellness emails: Use Sakanti Spa, Yoga, or Gym images
  - Dining emails: Use Kunyit, Sands Restaurant, or Buffet images
  - Activities emails: Use Cooking Class, Cycling, or Pool images
  - Events emails: Use Ballroom, Meeting Room, or Foyer images
  - Accommodation emails: Use Guest Room, Lobby, or Lounge images
- First image becomes hero image
- Remaining images pair with content sections
- If still no images available, use placeholder: `https://placehold.co/600x450/9ABCC0/ffffff?text=ANVAYA`

**3. Email Type:** Determine the campaign theme
- Wellness: spa, massage, yoga, fitness, meditation
- Dining: restaurant, food, cuisine, bar, breakfast, lunch, dinner
- Activities: boxing, cooking class, pool, beach, water sports
- Accommodation: room, suite, villa, stay, booking, packages
- Events: wedding, celebration, party, conference

**4. CTA Details:** Extract call-to-action information
- If CTA text is specified (e.g., "CTA: Book Now"), use that text
- If CTA URL is specified (e.g., "link: https://example.com"), use that URL
- If CTA text not specified, choose appropriate text based on email type:
  - Wellness: "Book Your Spa Journey" or "Discover Wellness"
  - Dining: "Discover Dining" or "Reserve Your Table"
  - Activities: "Book Your Experience" or "View Activities"
  - Accommodation: "Book Now" or "Check Availability"
  - General: "Learn More" or "View Offer"

## HTML Generation Workflow

Generate the complete HTML email following these steps:

### Step 1: Create HTML Boilerplate

Use HTML5 doctype with proper meta tags for email compatibility:
- Viewport meta tag for responsiveness
- X-UA-Compatible for IE
- x-apple-disable-message-reformatting for Apple Mail
- Link to Google Fonts for Nunito Sans

### Step 2: Add Inline CSS

Include media query for mobile responsiveness in `<head>`:
```html
<style>
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
</style>
```

**IMPORTANT:** This CSS ensures all images and text are centered in mobile view. Use `!important` flags to override inline styles. Padding is added to h3 and p tags within stack-column cells to prevent text from touching the edges on mobile.

### Step 3: Generate Header

Use the pre-built ANVAYA header GIF:
- Header image URL: https://email-static.useinsider.com/f940abcdc21d4566ac97b97fb4e8650f/lib/pluginId_f940abcdc21d4566ac97b97fb4e8650f_anvayaprod_images/emailmarketingtheanvaya_01.gif
- Display: Centered with wrapper table, max-width 640px
- Wrapper table: `width="640" align="center" style="margin: 0 auto;"`
- Image style: `width: 100%; max-width: 640px; height: auto; display: block; margin: 0 auto;`
- No additional header elements needed - the GIF contains the complete header design

### Step 4: Add Hero Section

Place hero image with title and subtitle overlay BELOW the image:
- Full-width hero image with 1px white border
- Desktop: Fixed height 400px, width 100% max-width 640px
- Mobile: 100% width, auto height
- Use first image from provided list or placeholder
- Below image (not overlaid), add centered title and subtitle:
  - Title: Dark gray #333333, 28px, font-weight 600, centered
  - Subtitle: Light gray #666666, 16px, font-weight 300, centered, line-height 1.5
- Padding: 32px above image, 40px below subtitle

### Step 5: Create Headline

Generate main promotion title:
- Extract theme from offerings (e.g., "Wellness Retreat", "Dining Experience", "Adventure Activities")
- Style: centered, color #698995, 32px, bold
- Padding: 20px

### Step 6: Build Content Sections

For each offering identified in the prompt, create a section with alternating layout:

**CRITICAL MOBILE-FIRST RULE:** Always place image cell FIRST in HTML source. This ensures image appears on top in mobile view.

**Section 1, 3, 5... (odd):** Image left, text right
- Image cell: first in source, normal table row
- Text cell: second in source, normal table row
- Desktop result: Image ← | Text →

**Section 2, 4, 6... (even):** Text left, image right (but image FIRST in source)
- Image cell: first in source, table row has `dir="rtl"`
- Text cell: second in source, both cells have `dir="ltr"`
- Desktop result: Text ← | Image → (reversed by dir="rtl")
- Mobile result: Image stacks first (source order)

Each section includes:
- Table with two 50% width cells
- Use `class="stack-column"` on cells for mobile stacking
- Image cell: 300px wide, 10px padding, ALWAYS first in source
- Text cell: 10px padding, vertical-align top, ALWAYS second in source
- Section title: 24px, color #698995, semi-bold
- Section description: 14px, color #333333, line-height 1.6
- Generate 2-3 sentences describing the offering based on the name

This mobile-first approach ensures image always appears on top for mobile users (most important), while maintaining visual variety on desktop.

### Step 7: Add CTA Button

Create centered button with:
- Table wrapper, 30px padding
- Background color: #698995
- Border radius: 4px
- Link text: white, bold, 16px
- Padding: 15px × 30px
- No border, no text decoration
- Use extracted CTA text and URL, or defaults based on email type

### Step 8: Include Footer

Append the pre-built footer template from `assets/footer-template.html`
Read the file and insert its content at the end of the email body, after the main wrapper table closes.

### Step 9: Return Complete HTML

Save the generated HTML to a file named using the email theme (e.g., `wellness-promo.html`, `dining-experience.html`)

## Responsive Layout Rules

**Desktop (640px+):**
- Main wrapper: 640px max-width, centered
- Content sections: side-by-side with alternating image positions
- Images: 300px max-width per section
- Full hero image: 600px max-width

**Mobile (<640px):**
- Main wrapper: 100% width
- Content sections: stacked vertically (single column)
- Images: 100% width, fluid scaling
- Cells with `class="stack-column"` become block-level

## Example Usage

**Example 1: Simple wellness email**
```
User: "Create an email for wellness promotions with sakanti spa, boxing class, and yoga"

Parse:
- Offerings: sakanti spa, boxing class, yoga
- Images: none provided → use placeholder
- Type: wellness
- CTA: auto-select "Book Your Spa Journey"

Generate:
- Hero with placeholder image
- Section 1 (left-right): Sakanti Spa description
- Section 2 (right-left): Boxing Class description
- Section 3 (left-right): Yoga description
- CTA: "Book Your Spa Journey"
- Footer: pre-built template
```

**Example 2: Dining email with images**
```
User: "Create a dining email featuring our beachfront restaurant and sunset bar. Images: https://example.com/restaurant.jpg, https://example.com/bar.jpg"

Parse:
- Offerings: beachfront restaurant, sunset bar
- Images: 2 URLs provided
- Type: dining
- CTA: auto-select "Discover Dining"

Generate:
- Hero: restaurant.jpg
- Section 1 (left-right): Beachfront Restaurant with restaurant.jpg
- Section 2 (right-left): Sunset Bar with bar.jpg
- CTA: "Discover Dining"
- Footer: pre-built template
```

**Example 3: Custom CTA**
```
User: "Create a promotional email for room packages including breakfast and spa credit. CTA: Book Now, link: https://theanvayabali.com/book"

Parse:
- Offerings: room packages (breakfast, spa credit)
- Images: none → use placeholder
- Type: accommodation
- CTA: "Book Now" (user-specified), URL: https://theanvayabali.com/book

Generate:
- Hero with placeholder
- Section 1: Room Packages description highlighting breakfast and spa credit
- CTA: "Book Now" with custom link
- Footer: pre-built template
```

## Additional Resources

### Reference Files
For detailed technical specifications and complete email structure:
- **`references/email-structure.md`** - Complete HTML email best practices, responsive patterns, color usage guidelines, typography implementation, Insider One compatibility, and full email template code
- **`references/mobile-css-template.md`** - CRITICAL mobile CSS template with all centering rules. Copy this CSS to ensure proper mobile responsiveness with centered images and text
- **`references/image-reference.md`** - Complete library of approved ANVAYA property images organized by category (wellness, dining, activities, events, accommodation). Use these images when generating emails to match content with appropriate visuals.

### Asset Files
- **`assets/footer-template.html`** - Pre-built footer with social links, contact details, disclaimer, and mysantika/MYVALUE branding. Read and insert this file directly into generated emails.

## Output Format

Generate complete HTML5 email files that:
- Include inline CSS for compatibility
- Use table-based layouts for maximum client support
- Implement responsive design with media queries
- Contain proper alt text on all images
- Include the complete ANVAYA footer
- Are ready to import into Insider One
- Can be tested in browser and email clients

Save output as `.html` files with descriptive names based on the email theme.
