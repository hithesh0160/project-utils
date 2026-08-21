# Inspira UI

[Inspira UI](https://github.com/unovue/inspira-ui) (by [Unovue](https://github.com/unovue)) is a free, open-source (MIT licensed) collection of UI components designed specifically for **Vue 3** and **Nuxt 3**.

It ports popular React UI design systems like **Aceternity UI** and **Magic UI** into the Vue/Nuxt ecosystem, offering animated, visual effect-heavy, and copy-paste components.

---

## Key Features

- **Vue 3 & Nuxt 3 Native:** Built specifically for Vue/Nuxt projects.
- **Aceternity & Magic UI Port:** Brings modern visual effects, gradients, text animations, and 3D interactions to Vue.
- **shadcn-vue Integration:** Compatible with `shadcn-vue` CLI for copy-paste component installation directly into your codebase.
- **Powered by Modern Vue Stack:** Built with Tailwind CSS, Motion Vue (`@vueuse/motion` / Motion), VueUse, and Radix Vue.

---

## Installation & Setup

### 1. Prerequisites
Ensure your Vue / Nuxt project has Tailwind CSS and required icon/animation utilities:

```bash
# Install core dependencies for Vue 3 / Nuxt 3
npm install @vueuse/core @vueuse/motion clsx tailwind-merge lucide-vue-next
```

### 2. CLI Installation
Use the CLI to add components directly to your Vue/Nuxt project:

```bash
npx inspira-ui@latest add <component-name>
```

---

## Example Usage (Vue 3 / Nuxt 3)

```vue
<script setup lang="ts">
import SparklesText from '@/components/ui/sparkles-text/SparklesText.vue'
</script>

<template>
  <div class="flex items-center justify-center min-h-screen bg-black">
    <SparklesText text="Inspira UI for Vue & Nuxt" />
  </div>
</template>
```

---

## Resources

- **GitHub Repository:** [unovue/inspira-ui](https://github.com/unovue/inspira-ui)
- **Documentation:** [inspira-ui.com](https://inspira-ui.com)
- **License:** MIT License (Free for personal and commercial use)
