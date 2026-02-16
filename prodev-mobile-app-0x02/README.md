# prodev-mobile-app-0x02 — Safe Areas, Images & Touchable Components

## Overview

This project builds a visually polished **landing/welcome screen** for a travel application. It introduces essential React Native components for real-world app development: `SafeAreaView` for device-safe rendering, `ImageBackground` for full-screen backgrounds, `Image` for logos, and `TouchableOpacity` for interactive buttons.

---

## Prerequisites

- Node.js LTS installed
- Expo Go installed on a physical device
- Completed Tasks 0 and 1
- Required assets: `Logo.png` and `background-image.png` (downloaded from course resources)

---

## Setup Steps

### Step 1 — Create the Expo Project

```bash
cd prodev-mobile-setup
npx create-expo-app@latest prodev-mobile-app-0x02
cd prodev-mobile-app-0x02
```

### Step 2 — Reset to a Clean Slate

```bash
npm run reset-project
```

### Step 3 — Add Assets

Create/confirm the `assets/images/` directory exists, then place the following files there:

```
assets/
└── images/
    ├── Logo.png
    └── background-image.png
```

### Step 4 — Install SafeArea Context (if not included)

```bash
npx expo install react-native-safe-area-context
```

### Step 5 — Update `app/index.tsx`

Replace the contents of `app/index.tsx` with the full implementation (see the source file in this repo).

### Step 6 — Run the App

```bash
npx expo start
```

Scan with Expo Go and confirm the landing screen renders with the background image, logo, text, and buttons.

---

## Concepts Covered

### 1. SafeAreaView & SafeAreaProvider

Modern smartphones have hardware intrusions — notches, punch-holes, curved corners, and home indicator bars — that can overlap UI elements if not accounted for.

`SafeAreaProvider` and `SafeAreaView` from `react-native-safe-area-context` automatically detect these insets and add appropriate padding so your content is never hidden behind hardware elements.

```tsx
import { SafeAreaView, SafeAreaProvider } from "react-native-safe-area-context";

<SafeAreaProvider>
  <SafeAreaView style={{ flex: 1 }}>
    {/* Content here is always visible, regardless of device notch */}
  </SafeAreaView>
</SafeAreaProvider>
```

**Why wrap with both?**
- `SafeAreaProvider` measures the safe area insets and makes them available to child components via context
- `SafeAreaView` consumes those measurements and applies padding automatically

### 2. ImageBackground

`ImageBackground` renders an image that fills its container and allows child components to be layered on top of it — ideal for hero/splash screens.

```tsx
import { ImageBackground, Dimensions } from "react-native";

<ImageBackground
  source={require("@/assets/images/background-image.png")}
  style={{ width: "100%", height: Dimensions.get("window").height }}
  resizeMode="cover"
>
  {/* Children render on top of the image */}
</ImageBackground>
```

**`resizeMode` options:**
| Value | Behavior |
|-------|----------|
| `"cover"` | Scales image to fill container, may crop edges |
| `"contain"` | Scales image to fit inside container, may show empty space |
| `"stretch"` | Stretches image to exactly fill container (may distort) |
| `"center"` | Centers image without scaling |

### 3. Dimensions API

`Dimensions.get("window")` returns the current screen's width and height in density-independent pixels. This is used to make the background image cover the full screen height reliably across all device sizes.

```tsx
import { Dimensions } from "react-native";

const { width, height } = Dimensions.get("window");
```

This is more reliable than `"100%"` for height in certain layout scenarios, because percentage heights in React Native require all parent views to have an explicit height or `flex: 1`.

### 4. Image Component

The `Image` component displays static image files from the local assets folder or remote URLs.

```tsx
import { Image } from "react-native";

// Local asset
<Image source={require("@/assets/images/Logo.png")} />

// Remote URL
<Image source={{ uri: "https://example.com/image.png" }} style={{ width: 100, height: 100 }} />
```

**Important:** For remote images, you must always specify `width` and `height` explicitly — React Native cannot infer dimensions for remote sources.

### 5. TouchableOpacity

`TouchableOpacity` is the standard way to create pressable/tappable UI elements in React Native. When pressed, it reduces the opacity of its children, giving visual feedback.

```tsx
import { TouchableOpacity, Text } from "react-native";

<TouchableOpacity onPress={() => console.log("Pressed!")}>
  <Text>Tap Me</Text>
</TouchableOpacity>
```

**Other touchable components:**
| Component | Behavior |
|-----------|----------|
| `TouchableOpacity` | Fades content on press (most common) |
| `TouchableHighlight` | Darkens background on press |
| `TouchableWithoutFeedback` | No visual feedback (use sparingly) |
| `Pressable` | Modern replacement with more control over press states |

### 6. Absolute Positioning

The button group is pinned to the bottom of the screen using `position: "absolute"`:

```tsx
<View style={{ position: "absolute", bottom: 0, width: "100%" }}>
  {/* Buttons always appear at the bottom regardless of content above */}
</View>
```

This is equivalent to CSS `position: fixed` / `position: absolute` with `bottom: 0`.

---

## Screen Layout Breakdown

```
┌─────────────────────────────────┐
│ SafeAreaView (device-safe area)  │
│ ┌─────────────────────────────┐  │
│ │ ImageBackground (full screen)│  │
│ │                              │  │
│ │   [Company Logo - centered]  │  │
│ │                              │  │
│ │   "Find your favorite        │  │
│ │    place here"               │  │
│ │   "The best prices for       │  │
│ │    over 2 million..."        │  │
│ │                              │  │
│ │ ┌──────────┬──────────────┐  │  │
│ │ │ Join here│   Sign In    │  │  │
│ │ └──────────┴──────────────┘  │  │
│ │   "Continue to home"         │  │
│ └─────────────────────────────┘  │
└─────────────────────────────────┘
```

---

## Styles Reference

| Style Key | Purpose |
|-----------|---------|
| `container` | `flex: 1` — fills the `ImageBackground` |
| `background` | Full-screen background image with exact window height |
| `companyLogo` | Centers the logo with padding and bottom margin |
| `textGroup` | Centers the title and subtitle text |
| `textLarge` | White, bold, 40dp title text |
| `textSmall` | White, light, 18dp subtitle text |
| `buttonGroup` | Horizontal row of two equal-width buttons with a gap |
| `button` | Solid white button with rounded corners |
| `transparentButton` | Outlined white button with transparent background |

---

## Challenges Faced

### Challenge 1 — Background Image Not Covering Full Height
**Problem:** The background image left whitespace at the bottom on some devices.  
**Solution:** Used `Dimensions.get("window").height` instead of `"100%"` for the image height, ensuring it always fills the screen regardless of parent layout constraints.

### Challenge 2 — Buttons Overlapping Content
**Problem:** Absolute-positioned buttons appeared on top of the text on smaller screens.  
**Solution:** The content above the buttons uses `marginBottom` and the parent `View` container uses `flex: 1`, allowing the background image to handle scrolling if needed. The button position is fixed at `bottom: 0`.

### Challenge 3 — Safe Area on Android vs iOS
**Problem:** Padding behavior differed between Android and iOS devices.  
**Solution:** `SafeAreaProvider` + `SafeAreaView` from `react-native-safe-area-context` handled the cross-platform inset differences automatically.

---

## Key Learnings

- `SafeAreaView` is essential for modern devices — always wrap top-level screens with it
- `ImageBackground` is the idiomatic way to create full-screen background images in React Native
- `Dimensions.get("window").height` is more reliable than percentage heights for full-screen layouts
- `TouchableOpacity` is the most common touchable component and provides good default press feedback
- `position: "absolute"` with `bottom: 0` is how you pin UI elements to the bottom of the screen

---

## References

- [SafeAreaView Docs](https://docs.expo.dev/versions/latest/sdk/safe-area-context/)
- [React Native Image](https://reactnative.dev/docs/image)
- [React Native ImageBackground](https://reactnative.dev/docs/imagebackground)
- [React Native TouchableOpacity](https://reactnative.dev/docs/touchableopacity)
- [React Native Dimensions](https://reactnative.dev/docs/dimensions)
- [React Native Layout with Flexbox](https://reactnative.dev/docs/flexbox)