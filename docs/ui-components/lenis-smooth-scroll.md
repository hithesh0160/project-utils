# Lenis Smooth Scroll

[Lenis](https://github.com/darkroomengineering/lenis) (by [Darkroom Engineering](https://darkroom.engineering/)) is a free, open-source (MIT licensed), lightweight smooth scroll library designed for modern web applications. It offers smooth, physics-based momentum scrolling without breaking native web browser behaviors or accessibility.

---

## Key Features

- **Robust Performance:** High FPS momentum scrolling with negligible overhead.
- **Framework Ecosystem:** Official adapters for React (`@lenis/react`), Snap (`@lenis/snap`), and Webflow.
- **GSAP & ScrollTrigger Ready:** Seamless integration with GSAP ScrollTrigger animation pipelines.
- **Accessibility Friendly:** Preserves standard keyboard navigation, screen reader access, and touch interactions.
- **Zero Heavy Dependencies:** Clean, modern TypeScript codebase.

---

## Installation

```bash
# Core package (Vanilla JS / Framework agnostic)
npm install lenis

# React wrapper
npm install @lenis/react
```

---

## Usage Examples

### 1. Vanilla JavaScript / HTML

```javascript
import Lenis from 'lenis';

// Initialize Lenis
const lenis = new Lenis({
  duration: 1.2,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true,
});

// Setup RequestAnimationFrame loop
function raf(time) {
  lenis.raf(time);
  requestAnimationFrame(raf);
}

requestAnimationFrame(raf);
```

### 2. React Setup (`@lenis/react`)

```tsx
import { ReactLenis, useLenis } from '@lenis/react';

function App() {
  const lenis = useLenis(({ scroll }) => {
    // Called on every scroll event
  });

  return (
    <ReactLenis root options={{ lerp: 0.1, duration: 1.5, smoothWheel: true }}>
      <main>
        <h1>My Smooth Scrolling App</h1>
      </main>
    </ReactLenis>
  );
}

export default App;
```

### 3. Integration with GSAP ScrollTrigger

```javascript
import Lenis from 'lenis';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

const lenis = new Lenis();

// Synchronize Lenis scroll position with GSAP ScrollTrigger
lenis.on('scroll', ScrollTrigger.update);

// Synchronize GSAP ticker with Lenis requestAnimationFrame
gsap.ticker.add((time) => {
  lenis.raf(time * 1000);
});

// Disable GSAP lag smoothing to prevent jitter
gsap.ticker.lagSmoothing(0);
```

---

## Useful Configuration Options

| Option | Type | Default | Description |
|---|---|---|---|
| `duration` | `number` | `1.2` | Scroll animation duration in seconds |
| `easing` | `function` | `(t) => Math.min(1, 1.001 - Math.pow(2, -10 * t))` | Easing function curve |
| `orientation` | `'vertical' \| 'horizontal'` | `'vertical'` | Scroll orientation direction |
| `smoothWheel` | `boolean` | `true` | Enable smooth scrolling for mouse wheel events |
| `wheelMultiplier` | `number` | `1` | Sensitivity multiplier for mouse wheel |
| `touchMultiplier` | `number` | `2` | Sensitivity multiplier for touch drag |

---

## Essential API Methods

```javascript
// Programmatic scroll to element, selector, or position (px)
lenis.scrollTo('#section-2', { offset: -50, duration: 1.5 });

// Pause smooth scroll processing
lenis.stop();

// Resume smooth scroll processing
lenis.start();

// Cleanup instance listener and bindings
lenis.destroy();
```

---

## Additional Resources

- **GitHub Repository:** [darkroomengineering/lenis](https://github.com/darkroomengineering/lenis)
- **Documentation & Demos:** [lenis.darkroom.engineering](https://lenis.darkroom.engineering/)
- **License:** MIT License (Free for personal and commercial use)
