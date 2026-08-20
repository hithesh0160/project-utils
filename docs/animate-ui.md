# Animate UI

[Animate UI](https://github.com/imskyleen/animate-ui) (by [imskyleen](https://github.com/imskyleen)) is a free, open-source (MIT licensed) component distribution of fully animated, reusable UI components built with React, TypeScript, Tailwind CSS, and Motion (Framer Motion).

It is designed with a **shadcn/ui** copy-paste philosophy where components live directly in your codebase rather than hidden in `node_modules`, providing complete customization freedom.

---

## Key Features

- **shadcn/ui Compatible:** Seamless integration with `shadcn` CLI for easy component addition.
- **Copy & Paste Component Model:** Source code is placed into your project repository.
- **Modern Tech Stack:** React, TypeScript, Tailwind CSS, Framer Motion.
- **Comprehensive UI Collection:** Buttons, text animations, interactive cards, headless UI elements, and motion transitions.

---

## Installation & Setup

### 1. Prerequisites
Ensure your project has Tailwind CSS and Motion installed:

```bash
npm install motion clsx tailwind-merge lucide-react
```

### 2. Adding Components via CLI
Use the `shadcn` CLI to add components directly:

```bash
npx shadcn@latest add "https://animate-ui.com/r/button"
```

---

## Example Usage

```tsx
import { AnimatedButton } from '@/components/ui/animated-button';

export default function HeroSection() {
  return (
    <div className="flex items-center justify-center p-8">
      <AnimatedButton variant="outline" className="px-6 py-3">
        Explore Components
      </AnimatedButton>
    </div>
  );
}
```

---

## Resources

- **GitHub Repository:** [imskyleen/animate-ui](https://github.com/imskyleen/animate-ui)
- **Website & Documentation:** [animate-ui.com](https://animate-ui.com)
- **License:** MIT License (Free for personal and commercial use)
