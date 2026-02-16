# prodev-mobile-app-0x00 — First Mobile App

## Overview

This project documents the process of creating a first React Native mobile application using the **Expo Router** template. It covers the scaffolding process, file structure of a new Expo project, modifying the home screen, and the behavior of the `reset-project` command.

---

## Prerequisites

Ensure the following are in place before starting:

- Node.js LTS installed (`node --version` ≥ v18)
- Expo Go installed on a physical device
- Git installed and configured
- A terminal / command prompt open in your project parent directory

---

## Scaffolding Steps

### Step 1 — Navigate to the Project Directory

Open a terminal and navigate to the parent project directory:

```bash
cd prodev-mobile-setup
```

### Step 2 — Initialize the Expo Project

Create a new Expo project using the latest Expo Router template inside the `prodev-mobile-app-0x00` directory:

```bash
mkdir prodev-mobile-app-0x00
cd prodev-mobile-app-0x00
npx create-expo-app@latest .
```

When prompted to select a template, choose the **default (Expo Router)** option.  
The CLI will scaffold the project and install all required dependencies automatically.

**Expected output:**
```
Your project is ready!
```

### Step 3 — Explore the Generated File Structure

After scaffolding, the project directory looks like this:

```
prodev-mobile-app-0x00/
├── app/
│   ├── (tabs)/
│   │   ├── index.tsx        ← Home screen
│   │   ├── explore.tsx      ← Explore screen
│   │   └── _layout.tsx      ← Tab navigator layout
│   ├── _layout.tsx          ← Root layout
│   └── +not-found.tsx       ← 404 screen
├── assets/
│   ├── fonts/
│   └── images/
├── components/              ← Reusable components
├── constants/
│   └── Colors.tsx           ← Theme color definitions
├── hooks/                   ← Custom React hooks
├── scripts/
│   └── reset-project.js     ← Reset script
├── app.json                 ← Expo configuration
├── package.json
└── tsconfig.json
```

**Key directories explained:**

- `app/` — All screens and layouts using Expo Router's file-based routing
- `app/(tabs)/` — The tab navigation group. Each file becomes a tab
- `components/` — Shared, reusable UI components (e.g., ThemedText, ParallaxScrollView)
- `constants/Colors.tsx` — Centralized color palette for light and dark themes
- `assets/` — Static assets: fonts, images, icons

### Step 4 — Modify the Home Screen

Open `app/(tabs)/index.tsx` in VS Code.

Locate the default welcome text:

```tsx
<ThemedText type="title">Welcome!</ThemedText>
```

Change it to:

```tsx
<ThemedText type="title">First App Created</ThemedText>
```

Save the file.

### Step 5 — Run the Application

Start the Expo development server:

```bash
npx expo start
```

A QR code will appear in the terminal.

- **Android:** Open **Expo Go** → tap "Scan QR code" → scan the terminal QR code
- **iOS:** Open the **Camera app** → point at the QR code → tap the banner notification

The app will load on your physical device. Verify the home screen displays **"First App Created"**.

---

## The `reset-project` Command

### What It Does

The reset command is run using:

```bash
npm run reset-project
```

This executes the script located at `scripts/reset-project.js`.

### Observed Behavior

After running the command, the following changes occurred:

1. **The entire `app/` directory was moved** to a new folder called `app-example/` at the project root
2. **The `components/` directory was moved** to `components-example/`
3. **A new minimal `app/` directory was created** containing only a single file: `app/index.tsx`
4. **The new `app/index.tsx`** contains a bare-bones screen with placeholder text — a clean slate to begin development

**Before reset:**
```
app/
├── (tabs)/
│   ├── index.tsx
│   ├── explore.tsx
│   └── _layout.tsx
├── _layout.tsx
└── +not-found.tsx
components/
└── (many pre-built components)
```

**After reset:**
```
app/
└── index.tsx         ← New minimal entry point
app-example/
└── (tabs)/           ← Original template files preserved here
components-example/
└── (original components preserved here)
```

### Why This Is Useful

The default Expo Router template includes many pre-built components, screens, and example code that are helpful for reference but not needed in your own app. The `reset-project` command gives you a **clean starting point** while preserving the original template in `app-example/` so you can still refer back to it.

This workflow is common in professional projects: scaffold a template to understand the structure, then strip it down and build your own features from scratch.

---

## Key Learnings

- **Expo Router** uses a **file-based routing** system — each file in `app/` automatically becomes a route/screen, similar to how Next.js works for web
- The `(tabs)` folder uses parentheses to create a **route group** — files inside share the tab layout without the group name appearing in the URL/path
- `_layout.tsx` files define the **navigation wrapper** for their directory level
- `constants/Colors.tsx` provides a scalable way to manage theme colors across the entire app

---

## References

- [Expo Router Docs](https://docs.expo.dev/router/introduction/)
- [React Native Core Components](https://reactnative.dev/docs/components-and-apis)
- [create-expo-app](https://docs.expo.dev/more/create-expo/#--template)