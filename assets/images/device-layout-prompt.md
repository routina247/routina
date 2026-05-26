# 3-Device Hero Mockup Layout — Replication Prompt

Create a hero section with 3 device mockups (iPad, iPhone, Apple Watch) arranged in a specific overlapping composition. Here are the exact specs:

## HTML Structure

```html
<div class="hero-mockup">
  <img src="[YOUR_IPAD_IMAGE]" alt="App on iPad" class="hero-ipad">
  <img src="[YOUR_IPHONE_IMAGE]" alt="App on iPhone" class="hero-iphone">
  <img src="[YOUR_WATCH_IMAGE]" alt="App on Apple Watch" class="hero-watch">
</div>
```

## Layout Behavior

The container `.hero-mockup` uses `position: relative` and `display: inline-block` with `max-width` constraints. The iPad image is the base layer (static, fills the container width). The iPhone and Apple Watch are `position: absolute`, anchored to the bottom-right area, overlapping the iPad.

## CSS (3 Breakpoints)

```css
.hero-mockup {
  position: relative;
  width: 100%;
  max-width: 550px;
  display: inline-block;
}

.hero-mockup img {
  display: block;
  max-width: 100%;
  height: auto;
  background: transparent;
}

/* iPad — base layer, fills container */
.hero-ipad {
  width: 100%;
  display: block;
  z-index: 1;
  border-radius: 10px;
}

/* iPhone — overlaps bottom-right of iPad, ~32% width */
.hero-iphone {
  position: absolute;
  width: 32%;
  right: -4%;
  bottom: 0;
  z-index: 2;
  border-radius: 16px;
}

/* Apple Watch — smaller, sits to the left of iPhone, bottom-aligned */
.hero-watch {
  position: absolute;
  width: 16%;
  right: 6%;
  bottom: 0;
  z-index: 3;
  border-radius: 14px;
}

/* Tablet (768px+) */
@media (min-width: 768px) {
  .hero-mockup {
    max-width: 700px;
  }
  .hero-iphone {
    width: 30%;
    right: -3%;
  }
  .hero-watch {
    width: 15%;
    right: 25%;
  }
}

/* Desktop (1024px+) */
@media (min-width: 1024px) {
  .hero-mockup {
    max-width: 800px;
  }
  .hero-iphone {
    width: 28%;
    right: -2%;
  }
  .hero-watch {
    width: 14%;
    right: 24%;
  }
}
```

## Key Details About the Composition

1. The **iPad is the anchor** — it sits naturally in the flow and defines the container height.
2. The **iPhone** is positioned `absolute`, bottom-right, slightly overflowing the container to the right (negative `right` value). It's about **30–32%** of the container width.
3. The **Apple Watch** is also `absolute`, bottom-aligned, positioned between the iPad center and the iPhone. It's about **14–16%** of the container width.
4. All three devices are **bottom-aligned** (`bottom: 0`), creating a "standing on a shelf" look.
5. **Z-index stacking:** iPad (1) → iPhone (2) → Watch (3), so the Watch is on top visually.
6. As the viewport gets larger, the iPhone and Watch get slightly smaller proportionally and the iPhone tucks in closer to the edge.
7. The parent container of the hero uses `overflow: visible` so the iPhone's negative right position isn't clipped.
8. Device images should include their **device frames already baked into the image files** (not CSS-drawn frames).

## Parent Hero Context (for proper centering)

The mockup sits inside a flex container alongside text content. On mobile it stacks vertically (column), on tablet+ it goes side-by-side (row) with the mockup taking `flex: 1.5` (wider) and text taking `flex: 1`.

```css
.hero .container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
  overflow: visible;
}

@media (min-width: 768px) {
  .hero .container {
    flex-direction: row;
    text-align: left;
  }
  .hero-mockup { flex: 1.5; }
  .hero-content { flex: 1; }
}

@media (min-width: 1024px) {
  .hero-mockup { flex: 1.6; }
}
```
