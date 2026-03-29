# Design System Document: Meditative Breath

## 1. Overview & Creative North Star
The Creative North Star for this design system is **"The Weightless Ethereal."** 

Unlike standard "Dark Mode" apps that feel heavy or technical, this system is designed to feel like a soft, nocturnal atmosphere. We move beyond the "template" look by rejecting rigid borders and harsh grids in favor of **Tonal Layering** and **Asymmetric Breathing Room**. The interface should not feel like a tool, but like a digital sanctuary. We achieve this through "Atmospheric Depth"—where elements don't just sit on a screen; they float within a volumetric space, defined by soft glows and subtle shifts in surface luminosity.

## 2. Colors & Atmospheric Depth
Our palette is rooted in the deep obsidian of a night sky, punctuated by the rhythmic pulse of calm blues and warm oranges.

### Color Tokens
- **Background:** `#10141a` (The base void)
- **Primary (Inhale):** `#a2c9ff` (A soft, luminous blue)
- **Tertiary (Exhale):** `#ffb77c` (A warm, sunset orange)
- **Surface Tiers:** Use `surface_container_lowest` (#0a0e14) to `surface_container_highest` (#31353c) to create depth.

### The "No-Line" Rule
**Explicit Instruction:** Do not use 1px solid borders to define sections. High-end design is felt, not outlined. 
- Separate content blocks using background color shifts. For example, a `surface_container_low` card should sit directly on a `surface` background. The subtle 2-3% difference in HEX value provides all the separation a sophisticated eye needs.

### Surface Hierarchy & Nesting
Treat the UI as a series of nested, translucent layers. 
- **The Base:** `background` (#10141a).
- **The Content Area:** `surface_container_low` (#181c22).
- **The Interactive Elements:** `surface_container_high` (#262a31).
This nesting creates a "soft-landing" effect for the user's eyes, reducing cognitive load and visual noise.

### The "Glass & Glow" Rule
For primary actions (like the main breathing trigger), use **Glassmorphism**. Apply `primary_container` at 12% opacity with a `backdrop-blur` of 20px. This creates a "frosted sapphire" effect that feels premium and tactile.

## 3. Typography: Editorial Rhythm
We use 'Inter' not as a system font, but as a sophisticated editorial tool. By utilizing extreme scale contrasts, we create a sense of importance without needing bold, heavy weights.

- **Display (L/M/S):** Used for the breath countdown or primary metrics. These should be set with `-0.02em` letter spacing to feel tight and custom.
- **Headline (M/S):** Used for session titles. 
- **Body (L/M):** Reserved for instructions. Ensure a line-height of `1.6` to allow the text to "breathe" as much as the user.
- **Label (M/S):** Small, all-caps with `+0.05em` letter spacing for a technical, high-end feel in secondary metadata.

The hierarchy is driven by **space**, not weight. A `display-lg` timer sitting in the center of a vast `surface_container_lowest` area communicates focus better than any bold font ever could.

## 4. Elevation & Depth: Tonal Layering
Traditional drop shadows are forbidden. We use **Ambient Light** and **Tonal Lifts**.

- **The Layering Principle:** To lift a card, move it up one tier in the `surface_container` scale. A card on `surface_dim` should be `surface_container_low`.
- **Ambient Shadows:** For floating modals, use an extra-diffused shadow: `0px 24px 48px rgba(0, 0, 0, 0.4)`. The shadow color should never be pure black; it should be a deep tint of our background color to maintain color harmony.
- **The "Ghost Border" Fallback:** If a boundary is required for accessibility, use the `outline_variant` token at **15% opacity**. It should be a whisper of a line, barely felt.

## 5. Components & UI Elements

### The Breathing Orb (Signature Component)
The core of the app. Use a radial gradient transitioning from `primary` (#a2c9ff) to `primary_container` (#58a6ff). Apply a soft outer glow using a box-shadow with the same color at 20% opacity.

### Buttons
- **Primary:** Rounded `full` (9999px). No solid fill; use a subtle gradient of `primary` to `primary_container`. 
- **Secondary:** Transparent background with a `Ghost Border` (15% opacity `outline_variant`).
- **Interaction:** On press, the element should "sink" into the background (shift color to a lower surface tier), mimicking physical haptics.

### Cards & Lists
- **Rule:** Forbid divider lines.
- **Implementation:** Use `spacing-8` (2.75rem) to separate list items. If grouping is needed, use a `surface_container_low` background with a `xl` (3rem) corner radius.

### Input Fields
- Avoid boxes. Use a simple `surface_variant` underline or a slightly darker `surface_container_lowest` rounded pill. 

### Progress Bars
- Use a "Glow-Track" approach. The background track is `surface_container_highest`, and the active progress is `primary` with a 4px blur to simulate a light-pipe effect.

## 6. Do's and Don'ts

### Do
- **Embrace Asymmetry:** Place the main session title slightly off-center to create a modern, editorial feel.
- **Use Negative Space:** If you think there is enough space, double it. Space is the "oxygen" of this design system.
- **Smooth Transitions:** Every state change (hover, active, transition) must have a duration of at least 400ms with a `cubic-bezier(0.4, 0, 0.2, 1)` easing to feel organic.

### Don't
- **Don't use pure white:** The `on_surface` color (`#dfe2eb`) is a soft grey. Pure white (#FFFFFF) will cause eye strain in a dark meditative environment.
- **Don't use 90-degree corners:** Use the `lg` (2rem) or `xl` (3rem) tokens for almost everything. Sharp corners feel aggressive; we want "soft and pillowy."
- **Don't use standard icons:** Use "Thin" or "Light" weight stroke icons (1px or 1.5px) to match the Inter typography weight. Bold icons will break the ethereal vibe.