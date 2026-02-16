# prodev-mobile-app-0x03 — Core Components: Login Screen

## Overview

This project implements a complete **Sign In screen** for a mobile application, demonstrating a wide range of React Native core components in a real-world context. The screen includes a text input form, password field with visibility toggle, social login buttons, and navigation prompts — all styled to match a professional mobile UI.

---

## Prerequisites

- Node.js LTS installed
- Expo Go installed on a physical device
- Completed Tasks 0–2
- Required assets: `logo.png`, `google.png`, `facebook.png`, `splash.png`

---

## Setup Steps

### Step 1 — Create the Expo Project

```bash
cd prodev-mobile-setup
npx create-expo-app@latest prodev-mobile-app-0x03
cd prodev-mobile-app-0x03
```

### Step 2 — Reset to a Clean Slate

```bash
npm run reset-project
```

### Step 3 — Add Assets

Place all required images in `assets/images/`:

```
assets/
└── images/
    ├── logo.png
    ├── google.png
    ├── facebook.png
    └── splash.png
```

### Step 4 — Install Dependencies

The `@expo/vector-icons` package (which includes FontAwesome and Ionicons) comes pre-installed with Expo. If you encounter import errors, run:

```bash
npx expo install @expo/vector-icons
npx expo install react-native-safe-area-context
```

### Step 5 — Create the Styles File

Create the `styles/` directory at the project root and add `styles/index.tsx`:

```bash
mkdir styles
touch styles/index.tsx
```

Type (do not copy-paste) the full styles object into `styles/index.tsx`. This manual typing practice reinforces familiarity with React Native styling properties.

### Step 6 — Update `app/_layout.tsx`

Replace the root layout to hide the default header:

```tsx
import { Stack } from "expo-router";

export default function RootLayout() {
  return <Stack screenOptions={{ headerShown: false }} />;
}
```

### Step 7 — Update `app/index.tsx`

Type out the full login screen implementation into `app/index.tsx`. See the source file in this repository.

### Step 8 — Run the App

```bash
npx expo start
```

Scan with Expo Go and confirm all screen elements render correctly.

---

## Concepts Covered

### 1. TextInput

`TextInput` is the core component for accepting user text input. It maps to `UITextField` on iOS and `EditText` on Android.

```tsx
import { TextInput } from "react-native";

// Email input with correct keyboard type
<TextInput
  keyboardType="email-address"
  style={styles.inputField}
/>

// Password input inside a flex row (for icon overlay)
<TextInput style={{ flex: 1 }} secureTextEntry={true} />
```

**Key props:**
| Prop | Purpose |
|------|---------|
| `keyboardType` | Shows appropriate keyboard (e.g., `"email-address"`, `"numeric"`, `"phone-pad"`) |
| `secureTextEntry` | Hides characters — use for passwords |
| `placeholder` | Hint text shown when input is empty |
| `onChangeText` | Callback with updated text value as user types |
| `value` | Controlled value for the input |

### 2. Vector Icons (@expo/vector-icons)

Expo bundles several popular icon libraries through `@expo/vector-icons`, removing the need to install them separately.

```tsx
import { FontAwesome, Ionicons } from "@expo/vector-icons";

// Back navigation arrow
<Ionicons name="arrow-back" size={25} />

// Password visibility toggle
<FontAwesome name="eye-slash" size={24} color="#7E7B7B" />
```

**Available icon sets in `@expo/vector-icons`:**
- `Ionicons` — Material-style icons (great for navigation)
- `FontAwesome` / `FontAwesome5` — Classic icon set
- `MaterialIcons` — Google Material Design icons
- `AntDesign`, `Feather`, `Entypo`, and more

Browse all icons at: [icons.expo.fyi](https://icons.expo.fyi)

### 3. Organizing Styles in a Separate File

Rather than keeping styles inline in the component file, this project extracts all styles into `styles/index.tsx` and imports them:

```tsx
// styles/index.tsx
import { StyleSheet } from "react-native";

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20, backgroundColor: '#fff' },
  // ... more styles
});

export { styles };
```

```tsx
// app/index.tsx
import { styles } from "@/styles";
```

**Benefits of this approach:**
- Keeps component files focused on logic and structure
- Styles are reusable across multiple screens
- Easier to maintain and update a consistent design system
- The `@/` path alias points to the project root (configured in `tsconfig.json`)

### 4. Expo Router — `Stack` Navigator

`expo-router` provides a `Stack` navigator that manages screen history with push/pop navigation (like a browser's forward/back history):

```tsx
import { Stack } from "expo-router";

export default function RootLayout() {
  return <Stack screenOptions={{ headerShown: false }} />;
}
```

`headerShown: false` hides the default navigation header bar, giving full control over the screen's top area.

### 5. Flexbox Layout Patterns Used

**Horizontal row with space between (nav group):**
```tsx
<View style={{ flexDirection: 'row', justifyContent: 'space-between' }}>
  <Ionicons name="arrow-back" size={25} />
  <Image source={require('@/assets/images/logo.png')} />
</View>
```

**Password input with icon on the right:**
```tsx
<View style={{ flexDirection: 'row', alignItems: 'center' }}>
  <TextInput style={{ flex: 1 }} />   {/* Takes all remaining space */}
  <FontAwesome name="eye-slash" size={24} />
</View>
```

**Divider with centered text:**
```tsx
<View style={{ flexDirection: 'row', alignItems: 'center', gap: 10 }}>
  <View style={{ flex: 1, borderWidth: 0.5 }} />
  <Text>OR</Text>
  <View style={{ flex: 1, borderWidth: 0.5 }} />
</View>
```

**Absolute-positioned bottom text:**
```tsx
<View style={{ position: 'absolute', bottom: 33, left: 77, right: 76 }}>
  <Text>Don't have an account?</Text>
  <Text>Join now</Text>
</View>
```

---

## File Structure

```
prodev-mobile-app-0x03/
├── app/
│   ├── _layout.tsx          ← Stack navigator (header hidden)
│   └── index.tsx            ← Login screen
├── styles/
│   └── index.tsx            ← Centralized StyleSheet
├── assets/
│   └── images/
│       ├── logo.png
│       ├── google.png
│       ├── facebook.png
│       └── splash.png
├── app.json
├── package.json
└── tsconfig.json
```

---

## Screen Layout Breakdown

```
┌─────────────────────────────────┐
│ ← (back)          [Logo]        │  ← navGroup: row, space-between
│                                 │
│ Sign in to your                 │  ← largeText (2 lines)
│ Account                         │
│ Enter your email and password   │  ← smallText
│                                 │
│ Email                           │  ← placeholderText label
│ ┌─────────────────────────────┐ │  ← inputField (TextInput)
│ └─────────────────────────────┘ │
│                                 │
│ Password                        │  ← placeholderText label
│ ┌───────────────────── 👁‍🗨 ─────┐ │  ← passwordGroup (row)
│ └─────────────────────────────┘ │
│                    Forgot password? │
│                                 │
│ ┌─────────────────────────────┐ │
│ │          Sign in            │ │  ← button (TouchableOpacity)
│ └─────────────────────────────┘ │
│                                 │
│ ────────────── OR ────────────  │  ← dividerGroup
│                                 │
│ ┌─────────────────────────────┐ │
│ │ [G] Continue with Google    │ │  ← socialMediaButton
│ └─────────────────────────────┘ │
│ ┌─────────────────────────────┐ │
│ │ [f] Continue with Facebook  │ │  ← socialMediaButton
│ └─────────────────────────────┘ │
│                                 │
│      Don't have an account? Join now  │  ← subTextGroup (absolute bottom)
└─────────────────────────────────┘
```

---

## Challenges Faced

### Challenge 1 — Icon Import Errors
**Problem:** `@expo/vector-icons` was not recognized after running reset-project.  
**Solution:** Ran `npx expo install @expo/vector-icons` to ensure the package was properly linked.

### Challenge 2 — `position: "absolute"` Overflow
**Problem:** The bottom "Don't have an account?" text appeared cut off on smaller screens.  
**Solution:** Used explicit `left` and `right` values in the `subTextGroup` style to keep the text within the safe horizontal boundary.

### Challenge 3 — TypeScript `fontWeight` Type Error
**Problem:** TypeScript threw an error when `fontWeight` was set to a plain number like `700` instead of the string `"700"`.  
**Solution:** Always use string values for `fontWeight` in React Native TypeScript: `"400"`, `"500"`, `"600"`, `"700"`, etc.

### Challenge 4 — `@/` Path Alias Not Resolving
**Problem:** VS Code showed an error for `import { styles } from "@/styles"`.  
**Solution:** The `tsconfig.json` in an Expo project already configures `"@/*"` as an alias for the project root. Restarting VS Code and the TypeScript server resolved the editor warning.

---

## Key Learnings

- `TextInput` is highly configurable — `keyboardType` and `secureTextEntry` are essential props for form inputs
- `@expo/vector-icons` provides thousands of icons with zero additional setup in an Expo project
- Separating styles into a dedicated file is a scalable architecture choice that keeps component code clean
- `position: "absolute"` with `left` / `right` / `bottom` is the standard way to pin UI elements to a fixed position in the viewport
- The `Stack` navigator from Expo Router manages screen-level navigation history automatically

---

## References

- [React Native TextInput](https://reactnative.dev/docs/textinput)
- [Expo Vector Icons](https://docs.expo.dev/guides/icons/)
- [Expo Router - Stack](https://docs.expo.dev/router/advanced/stack/)
- [React Native StyleSheet](https://reactnative.dev/docs/stylesheet)
- [React Native SafeAreaView](https://docs.expo.dev/versions/latest/sdk/safe-area-context/)
- [Browse Expo Icons](https://icons.expo.fyi)