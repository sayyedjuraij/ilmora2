# ILMORA EDUCATION - Technical Specification

## Component Inventory

### shadcn/ui Components (Built-in)
| Component | Purpose | Customization |
|-----------|---------|---------------|
| Button | CTAs, form submit | Custom gold variant, rounded-full |
| Card | University cards, feature cards | Custom hover effects |
| Input | Contact form fields | Floating labels, gold focus |
| Textarea | Message field | Matching input styling |
| Select | Program dropdown | Custom styling |
| Label | Form labels | Floating animation |
| Sheet | Mobile navigation | Slide from right |
| Separator | Visual dividers | Gold accent variant |

### Third-Party Registry Components
| Component | Registry | Purpose |
|-----------|----------|---------|
| None required | - | Custom implementations preferred for premium feel |

### Custom Components
| Component | Location | Purpose |
|-----------|----------|---------|
| FlipCard | `components/FlipCard.tsx` | 3D card flip for study options |
| FloatingLabel | `components/FloatingLabel.tsx` | Animated form labels |
| ScrollReveal | `components/ScrollReveal.tsx` | GSAP scroll-triggered animations |
| AnimatedCheckmark | `components/AnimatedCheckmark.tsx` | SVG draw animation |
| GlassNavbar | `components/GlassNavbar.tsx` | Glassmorphism navigation |
| WhatsAppButton | `components/WhatsAppButton.tsx` | Floating WhatsApp CTA |

---

## Animation Implementation Table

| Animation | Library | Implementation Approach | Complexity |
|-----------|---------|------------------------|------------|
| Page load sequence | GSAP | Timeline with staggered children | High |
| Hero logo fade + scale | GSAP | FromTo with opacity and scale | Medium |
| Headline stagger reveal | GSAP | SplitText-like manual spans with stagger | High |
| Scroll indicator bounce | CSS | Keyframe animation infinite | Low |
| Navbar appear on scroll | GSAP ScrollTrigger | Toggle class/animation at 100px | Medium |
| Logo shrink to navbar | GSAP ScrollTrigger | Scrub animation tied to scroll | High |
| Section reveal (fade up) | GSAP ScrollTrigger | Batch animation for elements | Medium |
| Card stagger reveal | GSAP ScrollTrigger | Stagger with 100ms delay | Medium |
| 3D card flip | CSS + React | Perspective + rotateY on hover | High |
| Card lift on hover | CSS | Transform + shadow transition | Low |
| Icon scale on hover | CSS | Transform scale | Low |
| Checkmark draw | GSAP | SVG stroke-dashoffset animation | Medium |
| Floating labels | React + CSS | State-based translateY animation | Medium |
| Input focus glow | CSS | Box-shadow transition | Low |
| Button magnetic hover | CSS | Scale + shadow transition | Low |
| WhatsApp pulse | CSS | Keyframe scale animation | Low |
| Background gradient shift | CSS | Background-position animation | Low |
| Nav underline grow | CSS | Pseudo-element width transition | Low |

---

## Animation Library Choices

### Primary: GSAP + ScrollTrigger
**Rationale:**
- Industry-standard for complex scroll animations
- Excellent performance with transform/opacity
- ScrollTrigger for viewport-based triggers
- Timeline for sequenced animations

**Usage:**
- Page load sequence
- Scroll-triggered reveals
- Logo transition
- Checkmark draw animation

### Secondary: CSS Animations/Transitions
**Rationale:**
- Better performance for simple hover states
- No JS overhead for basic interactions
- Hardware accelerated

**Usage:**
- Button hovers
- Card lifts
- Icon scales
- Scroll indicator bounce
- WhatsApp pulse

### Smooth Scroll: Lenis
**Rationale:**
- Lightweight smooth scrolling
- Works well with GSAP ScrollTrigger
- Mobile-friendly

---

## Project File Structure

```
/mnt/okcomputer/output/app/
├── public/
│   ├── logos/
│   │   ├── logo-white.png
│   │   ├── logo-black.png
│   │   ├── arni-university.png
│   │   ├── rntu.png
│   │   ├── osgu.png
│   │   ├── rgu.png
│   │   ├── lingayas.png
│   │   └── jamia-urdu.png
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── ui/                    # shadcn components
│   │   ├── FlipCard.tsx
│   │   ├── FloatingLabel.tsx
│   │   ├── ScrollReveal.tsx
│   │   ├── AnimatedCheckmark.tsx
│   │   ├── GlassNavbar.tsx
│   │   └── WhatsAppButton.tsx
│   ├── sections/
│   │   ├── Hero.tsx
│   │   ├── About.tsx
│   │   ├── StudyOptions.tsx
│   │   ├── PostCompletion.tsx
│   │   ├── Universities.tsx
│   │   ├── SchoolCompletion.tsx
│   │   ├── Programs.tsx
│   │   ├── Contact.tsx
│   │   └── Footer.tsx
│   ├── hooks/
│   │   ├── useScrollPosition.ts
│   │   └── useLenis.ts
│   ├── lib/
│   │   └── utils.ts
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── index.html
├── package.json
├── tailwind.config.js
├── tsconfig.json
└── vite.config.ts
```

---

## Dependencies

### Core
```json
{
  "react": "^18.2.0",
  "react-dom": "^18.2.0",
  "typescript": "^5.0.0"
}
```

### Animation
```json
{
  "gsap": "^3.12.5",
  "@studio-freight/lenis": "^1.0.42"
}
```

### UI
```json
{
  "tailwindcss": "^3.4.0",
  "class-variance-authority": "^0.7.0",
  "clsx": "^2.1.0",
  "tailwind-merge": "^2.2.0",
  "lucide-react": "^0.344.0"
}
```

### Fonts
```html
<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;500;600;700&family=Inter:wght@400;500;600&family=Cinzel:wght@400;500&display=swap" rel="stylesheet">
```

---

## Key Implementation Details

### GSAP ScrollTrigger Setup
```typescript
// Register ScrollTrigger
gsap.registerPlugin(ScrollTrigger);

// Create batch for card reveals
ScrollTrigger.batch(".reveal-card", {
  onEnter: batch => gsap.to(batch, {
    opacity: 1,
    y: 0,
    stagger: 0.1,
    duration: 0.8,
    ease: "power3.out"
  }),
  start: "top 80%"
});
```

### 3D Card Flip CSS
```css
.flip-card {
  perspective: 1000px;
}

.flip-card-inner {
  transition: transform 0.6s;
  transform-style: preserve-3d;
}

.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

.flip-card-front,
.flip-card-back {
  backface-visibility: hidden;
}

.flip-card-back {
  transform: rotateY(180deg);
}
```

### Floating Label Pattern
```typescript
// State-based approach
const [isFocused, setIsFocused] = useState(false);
const [hasValue, setHasValue] = useState(false);

// Label class
`transition-all duration-200 ${
  isFocused || hasValue 
    ? '-translate-y-6 text-sm text-[#d4af37]' 
    : 'translate-y-0 text-base text-gray-500'
}`
```

### Lenis Smooth Scroll
```typescript
const lenis = new Lenis({
  lerp: 0.1,
  wheelMultiplier: 0.8,
  gestureOrientation: 'vertical'
});

// Connect to GSAP ScrollTrigger
lenis.on('scroll', ScrollTrigger.update);

gsap.ticker.add((time) => {
  lenis.raf(time * 1000);
});
```

---

## Performance Optimizations

1. **Image Optimization**
   - Use WebP with PNG fallback
   - Lazy load below-fold images
   - Proper sizing with srcset

2. **Animation Performance**
   - Only animate transform and opacity
   - Use will-change sparingly
   - Respect prefers-reduced-motion

3. **Code Splitting**
   - Lazy load sections if needed
   - Tree-shake unused components

4. **Font Loading**
   - Use font-display: swap
   - Preload critical fonts

---

## Accessibility

1. **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

2. **Focus States**
   - Visible focus rings on all interactive elements
   - Skip to main content link

3. **ARIA Labels**
   - Proper labels on icons
   - Role attributes where needed

4. **Color Contrast**
   - All text meets WCAG 4.5:1 ratio
   - Gold on navy: 7.2:1 ✓
   - Navy on white: 12.5:1 ✓
