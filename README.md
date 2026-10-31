# Westend Signature — website

A standalone website for Westend Signature, a private chauffeur and concierge service in Lagos and Abuja. `index.html` holds the page, with its CSS and JavaScript embedded. It needs no build step: open the file directly or serve the folder from any static host.

```
index.html          the site (CSS + JS embedded)
privacy.html        privacy notice TEMPLATE (needs completing and review)
terms.html          terms TEMPLATE (needs completing and review)
assets/images/      demonstration images (placeholders, see below)
case-study/         UX/UI case study of this website (open case-study/index.html)
```

The case study is a single static page. It uses real screenshots of the site, taken on 31 October 2026, in `case-study/assets/`. No interview, survey or conversion data is invented, and audience profiles and wireframes are labelled as hypotheses and reconstructions. If the site changes, retake the screenshots.

## Enquiries go to WhatsApp

Enquiries are sent to **+234 703 651 7790** on WhatsApp (`whatsapp` in `WESTEND_CONFIG`). The form's main button is "Send to WhatsApp". When tapped, it checks the required fields and opens WhatsApp with the visitor's journey details already written in a message to that number; the enquiry reaches you when they press send. The site never claims it was sent. The number also appears as "WhatsApp · Message us" next to the form and in the footer.

## Booking form suggestions

- **Pickup and destination:** suggestions appear as you type.
  - A built-in list of Lagos and Abuja airports, hotels and districts (`PLACES` in `index.html`) shows instantly, favouring the selected city.
  - Live address results come from Photon (OpenStreetMap, free, no key needed), limited to Nigeria.
  - Turn live results off with `placeSearch: false` in `WESTEND_CONFIG`; the built-in list still works.
  - The destination also offers "As directed (hourly service)".
- **Email:** common domains (gmail.com and so on) are suggested after "@".
- **Requirements:** tap-to-add chips for common requests.



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
   whatsapp: "2347036517790", // +234 703 651 7790 — enquiries are sent here via WhatsApp
   enquiryEndpoint: "",  // URL accepting a JSON POST of the enquiry
   smoothScroll: true
   ```

   Any value left empty stays hidden; the site never shows invented details.

3. **Connect the form (optional).** With `enquiryEndpoint` empty, submitting a valid enquiry shows a summary. The summary states plainly that the enquiry has **not been sent** and offers copy, download (.txt) and, where configured, email and WhatsApp hand-off. With an endpoint set, the form POSTs JSON. On success the visitor sees "sent, not yet confirmed" (never a booking confirmation). On failure they get the not-sent summary.

4. **Complete `privacy.html` and `terms.html`** and have them legally reviewed.

## Hero sequence

The hero pins and plays one video of all six cars, exported as still frames (`assets/hero/sequence/002.jpg` to `297.jpg`, 1280×720). The frames are drawn to a canvas as you scroll, so scrolling up plays it in reverse.

- **Loading:** frame 002 loads first; the rest load in order, six at a time.
- **Phones:** use every second frame (about 3.4 MB instead of 6.7 MB).
- **Copy per car:** each car has its own headline and supporting line (`.hero__headline` and `.hero__support-item`, one per `data-slide`). They switch, along with the index number, model name and Explore link, at the dark moments between cars. Those moments are set in `SEQ_CARS` in `index.html` (frames 61, 101, 144, 212 and 269); update them if the video changes.
- **Reduced motion, or scripts failing:** the lit `028.jpg` is shown instead.
- `001.jpg` is not used.
- The still keyframes in `assets/hero/frame-*.webp` are still used in the vehicle view and the "Every vehicle, our own." band.

## Editing vehicles

Each fleet card is an `<article class="vehicle">`. Its `data-vehicle` id must match an `<option value>` in the enquiry form's "Preferred vehicle" select.

"Explore this vehicle" opens a dashboard-style vehicle view:

- **Desktop:** a sidebar lists the whole collection (with a concierge prompt). The main area shows the model name, four "at a glance" tiles with round icons, the car in the arrival hall, a glass "Plan with this vehicle" card, and an expandable features list.
- **Phones:** the collection becomes a swipeable row and the page scrolls.
- **Switching cars:** the sidebar, previous/next, the arrow keys or a swipe on the car, with a fade through darkness.
- **Plan card:** sends city, service and vehicle straight into the booking form.

Content is in the `VEHICLE DETAILS` script near the end of `index.html`:

- `WESTEND_VEHICLES`: name, collection, image, one-sentence description.
- `tiles`: the four tiles (icon, short value, label). Icons: drive, frame, seat, star, engine, gear, wave, screen, design, lock, light, voice.
- `stats`: the features list (feature, category, one-line detail). These are published model features checked against public sources in October 2026. Trims and markets vary, so confirm each against Westend's own vehicles.
- `WESTEND_SERVICE_STATS`: the fallback features for a vehicle without `stats`.

No prices, capacities, model years, specifications or availability are stated, by design.

## Motion and accessibility

- Scroll storytelling: a thin gold progress line along the bottom of the screen; the header dims to 60% while scrolling down and returns on scroll up, hover or focus; section headlines rise word by word; Experiences and Standard photographs open with a clip-path wipe; outlined buttons draw their border clockwise on first view; links draw their underline in from the left and retract to the right.
- The manifesto after the enquiry strip brightens word by word as you scroll, with three photographs drifting behind at different speeds. The "Every vehicle, our own." band opens from a narrow strip to full width before its caption rises. "Back to top" is in the footer.

- GSAP 3.12.5 + ScrollTrigger (cdnjs) and Lenis 1.1.13 (jsDelivr), pinned versions. Everything runs on GSAP's single ticker.
- Hero entrance runs about 1.5 s: masked headline lines, staggered copy, and the image easing from 1.06 to 1. No loading screen.
- Desktop (≥1024 px wide, ≥620 px tall): the fleet section pins for about 210 vh total and moves horizontally through three collections, with synced labels and progress. Tabbing into an off-screen card scrolls to its panel.
- Smaller screens get stacked cards with no pinning. Lenis runs only on fine-pointer devices.
- `prefers-reduced-motion`: no pinning, parallax, smooth scrolling or reveals; all content shows immediately.
- If the CDN scripts fail or JavaScript is off, all content stays visible. Menus, dialog and form need JavaScript only for enhancement (the fleet's "Enquire" links fall back to plain anchors).
- Native `<dialog>` handles the vehicle details (focus containment, Escape, focus restored). The mobile menu traps focus, closes on Escape and makes the page behind it inert.
