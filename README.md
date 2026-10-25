# Westend Signature — website

A standalone website for Westend Signature, a private chauffeur and concierge service in Lagos and Abuja. `index.html` holds the page, with its CSS and JavaScript embedded. It needs no build step: open the file directly or serve the folder from any static host.

```
index.html          the site (CSS + JS embedded)
privacy.html        privacy notice TEMPLATE (needs completing and review)
terms.html          terms TEMPLATE (needs completing and review)
assets/images/      demonstration images (placeholders, see below)
```

## Before launch

1. **Replace the photographs.** Every image in `assets/images/` is a labelled placeholder. Overwrite each file with a real photograph under the same name and at roughly the same proportions; no code changes are needed. Then update the matching `alt` text in `index.html`. Each `<img>` sits beside an `<!-- IMAGE: ... -->` comment.

   | File | Use | Size |
   |---|---|---|
   | `hero-g-class.jpg` | Hero: black G-Class at a hotel entrance at dusk | 2400×1350 |
   | `fleet-prado.jpg` | Toyota Prado | 1600×1200 |
   | `fleet-land-cruiser.jpg` | Toyota Land Cruiser | 1600×1200 |
   | `fleet-range-rover.jpg` | Range Rover | 1600×1200 |
   | `fleet-g-class.jpg` | Mercedes-Benz G-Class | 1600×1200 |
   | `fleet-mercedes-suv.jpg` | Mercedes-Benz SUV | 1600×1200 |
   | `fleet-mercedes-saloon.jpg` | Mercedes-Benz luxury saloon | 1600×1200 |
   | `exp-airport.jpg` | Airport arrivals | 1600×2000 |
   | `exp-executive.jpg` | Executive chauffeur | 2000×1333 |
   | `exp-weddings.jpg` | Weddings & occasions | 1600×2000 |
   | `exp-city.jpg` | Private city travel | 2000×1333 |
   | `exp-concierge.jpg` | Concierge assistance | 1600×2000 |
   | `standard-interior.jpg` | Luxury vehicle interior | 1600×2000 |

   Use Westend's own vehicles where possible (around 2023-era models), or licensed photography. Compress the JPGs to about 300 KB or less.

2. **Add contact details.** Edit `window.WESTEND_CONFIG` near the top of `<body>` in `index.html`:

   ```js
   email: "",            // shown under Contact; enables "Email this enquiry"
   phone: "",            // shown as a tap-to-call link
   whatsapp: "",         // digits only, e.g. 2348000000000; enables "Send via WhatsApp"
   enquiryEndpoint: "",  // URL accepting a JSON POST of the enquiry
   smoothScroll: true
   ```

   Any value left empty stays hidden; the site never shows invented details.

3. **Connect the form (optional).** With `enquiryEndpoint` empty, submitting a valid enquiry shows a summary. The summary states plainly that the enquiry has **not been sent** and offers copy, download (.txt) and, where configured, email and WhatsApp hand-off. With an endpoint set, the form POSTs JSON. On success the visitor sees "sent, not yet confirmed" (never a booking confirmation). On failure they get the not-sent summary.

4. **Complete `privacy.html` and `terms.html`** and have them legally reviewed.

## Editing vehicles

Each vehicle is an `<article class="vehicle">` in the Fleet section. The card text is written in the markup. The vehicle detail dialog reads `data-name`, `data-collection` and `data-detail`. The id in `data-vehicle` must match an `<option value>` in the enquiry form's "Preferred vehicle" select. No prices, capacities, model years or availability are stated, by design.

## Motion and accessibility

- GSAP 3.12.5 + ScrollTrigger (cdnjs) and Lenis 1.1.13 (jsDelivr), pinned versions. Everything runs on GSAP's single ticker.
- Hero entrance runs about 1.5 s: masked headline lines, staggered copy, and the image easing from 1.06 to 1. No loading screen.
- Hero showcase (all screen sizes, motion allowed): the hero pins for about 4.5 screens of scrolling. The copy gives way to a 3D stage where each vehicle's feature callouts pop up, then the card flips to the next vehicle. Edit vehicles and callouts in the `.showcase` markup; each callout's `--x`/`--y` is a position on the 4:3 photo, and `feat--left` puts its label on the left. It reuses the `fleet-*.jpg` photos and loads them after the hero image.
- Desktop (≥1024 px wide, ≥620 px tall): the fleet section pins for about 210 vh total and moves horizontally through three collections, with synced labels and progress. Tabbing into an off-screen card scrolls to its panel.
- Smaller screens get stacked cards with no pinning. Lenis runs only on fine-pointer devices.
- `prefers-reduced-motion`: no pinning, parallax, smooth scrolling or reveals; all content shows immediately.
- If the CDN scripts fail or JavaScript is off, all content stays visible. Menus, dialog and form need JavaScript only for enhancement (the fleet's "Enquire" links fall back to plain anchors).
- Native `<dialog>` handles the vehicle details (focus containment, Escape, focus restored). The mobile menu traps focus, closes on Escape and makes the page behind it inert.
