## Contact form with EmailJS

This project includes a modal contact form (no Vue Router) powered by EmailJS.

### Setup

1. Create a free account on EmailJS and add a service and template.
2. In your template, expect these variables: `from_name`, `from_email`, `message`.
3. Create a `.env` file in the project root (same level as `package.json`) with:

```
VITE_EMAILJS_SERVICE_ID=your_service_id
VITE_EMAILJS_TEMPLATE_ID=your_template_id
VITE_EMAILJS_PUBLIC_KEY=your_public_key
```

Restart the dev server after editing env.

### Usage

- Click the floating "Contact" button to open the form.
- The form validates required fields and shows success or error messages.

# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

Learn more about IDE Support for Vue in the [Vue Docs Scaling up Guide](https://vuejs.org/guide/scaling-up/tooling.html#ide-support).
