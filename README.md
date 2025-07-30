# 🧩 Introduction to Web Accessibility

**Web Accessibility** means designing websites and applications so that everyone — including people with disabilities — can perceive, navigate, and interact with them.


## 👨‍🦯 Common Disabilities

| Type      | Example Challenges                        |
| --------- | ----------------------------------------- |
| Visual    | Blindness, low vision, color blindness    |
| Motor     | Cannot use a mouse, limited fine movement |
| Auditory  | Hearing loss, deafness                    |

---



# 👁️‍🗨️ Focus Area 1: **Key Accessibility Topics for Low Vision Users**

Low vision users may:

* Zoom in up to 200–400%
* Use **high contrast** themes or color inversion
* Struggle with **small text**, **blurry fonts**, or **low contrast**
* Navigate with **screen magnifiers**
* Prefer **larger, scalable UI elements**

---

## ✅ 1. **Color Contrast**

### 🔹 What to Do:

Ensure sufficient contrast between **foreground text** and **background**.

| Text Type                       | Minimum Contrast Ratio |
| ------------------------------- | ---------------------- |
| Regular text                    | 4.5:1                  |
| Large text (18px bold or 24px+) | 3:1                    |

### 🧪 Tools:

* [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
* Browser extensions like **axe**, **WAVE**

---

## ✅ 2. **Scalable Text / Zoom Support**

### 🔹 What to Do:

* Use **relative units** like `em`, `rem`, `%` — **avoid pixels (`px`)** for text sizing
* Allow browser zoom to **200% or more** without breaking layout or causing horizontal scroll

```css
body {
  font-size: 1rem; /* Scales with user preferences */
}
```

### ❌ Don’t:

* Disable zoom via `viewport` meta tag:

```html
<!-- BAD -->
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
```

✅ **Remove `maximum-scale` and `user-scalable=no`** so zoom remains usable.

---

## ✅ 3. **Text Spacing and Readability**

* Allow increased **line-height**, **letter spacing**, and **word spacing**
* Test your site using [Cognitive Accessibility guidelines (WCAG SC 1.4.12)](https://www.w3.org/WAI/WCAG21/Understanding/text-spacing.html)

```css
body {
  line-height: 1.5;
  letter-spacing: 0.12em;
}
```

---

## ✅ 4. **Avoid Reliance on Color Alone**

### 🔹 Why:

Users with **color blindness** or poor color perception may miss information that’s only color-coded.

### ✅ Use:

* Icons or text labels **along with color**
* Patterns or shapes (e.g. error icon + red)

```html
<!-- GOOD -->
<span class="error" role="alert">
  ❌ Payment failed. Please try again.
</span>
```

---


## ✅ 5. **Focus Visibility**

* All focusable elements **must show a visible outline** when focused by keyboard
* Avoid removing focus styles (`outline: none`) unless you replace them with something equally visible

```css
button:focus {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
}
```

---


## ✅ 6. **Meaningful Link and Button Text**

* Ensure link/button text is **clear**, **descriptive**, and readable when zoomed
* Avoid vague text like “Click here” or “More”

---

# 🧠 Summary: Quick Checklist

| Area                     | Must Do                                                        |
| ------------------------ | -------------------------------------------------------------- |
| Color contrast           | Minimum 4.5:1 for normal text, 3:1 for large text              |
| Scalable text            | Use `rem`, `em`, `%`, not `px`                                 |
| Zoom support             | Ensure layout works at 200%+ zoom without horizontal scrolling |
| No color-only info       | Combine color with text, icons, or patterns                    |
| Focus visibility         | Don’t hide focus outlines — style them clearly                 |
| High contrast mode       | Respect system settings, test with forced-colors media query   |
| Text spacing readability | Allow overrides; test with browser extensions or custom styles |

---


# 🧭 Focus Area 2: **Key Topics in Keyboard Accessibility**

## ⌨️ 1. **Keyboard Navigation Basics**

### 🔑 What It Means:

All interactive elements must be **fully usable with the keyboard alone**, without requiring a mouse.

### 🔹 Default Keyboard Interactions:

| Action               | Key(s) Used      |
| -------------------- | ---------------- |
| Navigate forward     | `Tab`            |
| Navigate backward    | `Shift + Tab`    |
| Activate button/link | `Enter`, `Space` |
| Close modal/menu     | `Esc`            |
| Navigate dropdowns   | Arrow keys       |

### 🔍 Why It Matters:

Users with motor disabilities, blindness, or temporary injuries may rely entirely on the keyboard or assistive tech.

---

## 🧱 2. **Tab Order and `tabindex`**

### 🔹 Natural Tab Order:

Follows the **DOM order** of focusable elements (`<a>`, `<button>`, `<input>`, etc.)

### 🔹 Customizing Tab Order with `tabindex`:

| Value           | Behavior                                                       |
| --------------- | -------------------------------------------------------------- |
| `tabindex="0"`  | Makes non-focusable elements keyboard focusable (in DOM order) |
| `tabindex="-1"` | Makes element focusable **only programmatically** (`.focus()`) |
| `tabindex="1+"` | Changes tab order priority (⚠️ Avoid — not recommended)        |

#### ✅ Example:

```html
<div tabindex="0">Focusable custom div</div>
```

### ⚠️ Don't overuse `tabindex="1"` or higher — it **breaks natural tab flow**.

---

## 🎭 3. **Roles in Accessibility (`role` attribute)**

### 🔹 What Is a Role?

Defines the **semantic purpose** of an element to assistive technologies.

### 🔹 Common Roles:

| Role                | Purpose / Use Case                           |
| ------------------- | -------------------------------------------- |
| `role="button"`     | When using a `<div>` or `<span>` as a button |
| `role="dialog"`     | Marks modals or popups                       |
| `role="navigation"` | Defines a nav menu                           |
| `role="main"`       | Primary page content                         |
| `role="alert"`      | For error or success messages                |
| `role="tabpanel"`   | For tabbed interfaces                        |

#### ✅ Example:

```html
<div role="button" tabindex="0">Click me</div>
```

🛑 **Tip:** Always **prefer native elements** (`<button>`, `<nav>`, etc.) before adding roles to generic elements.

---

## 🪄 4. **Focus Management**

### 🔹 Why It Matters:

Users need to **see and know** where they are on the page.

### 🔑 Best Practices:

* Use **visible focus indicators** (browser default or custom outlines)
* Trap focus **inside modals**, and return focus when closed
* Use `tabindex="-1"` to move focus programmatically:

```js
document.getElementById("modal").focus();
```

---

## 🚀 5. **Skip Links and Landmarks**

### 🔹 Skip Links:

Allow users to skip repeated content (like nav) and go straight to the main section.

```html
<a href="#main-content" class="skip-link">Skip to main content</a>
```

### 🔹 Landmarks:

Enable screen reader users to jump to different page areas.

| HTML Element | Implied Role           |
| ------------ | ---------------------- |
| `<header>`   | `role="banner"`        |
| `<nav>`      | `role="navigation"`    |
| `<main>`     | `role="main"`          |
| `<footer>`   | `role="contentinfo"`   |
| `<aside>`    | `role="complementary"` |

---

# 🧠 Summary: Quick Reference Table

| Concept              | What to Remember                                         |
| -------------------- | -------------------------------------------------------- |
| **Tab Order**        | Should follow logical, DOM-based order                   |
| **Tabindex**         | Use `0` and `-1`, avoid positive numbers                 |
| **Roles**            | Add semantics where native elements aren’t used          |
| **Focus Management** | Move focus programmatically for modals or custom widgets |
| **Skip Links**       | Allow skipping to main content                           |
| **Landmark Roles**   | Help screen reader navigation                            |
| **Keyboard Testing** | Always test without a mouse                              |

---




# 🗣️ Focus Area 3: **Key Topics for Screen Reader Accessibility**

Screen readers (like **NVDA**, **JAWS**, **VoiceOver**) help users **navigate and understand web content by reading it aloud** or displaying it via Braille.

---

## ✅ 1. **Semantic HTML Is Your First Tool**

### 🔹 Use Native Elements:

| Task           | Use This Element                      |
| -------------- | ------------------------------------- |
| Button         | `<button>`                            |
| Form input     | `<input>`, `<select>`, `<textarea>`   |
| Navigation     | `<nav>`                               |
| Headings       | `<h1>` to `<h6>`                      |
| Landmark roles | `<main>`, `<header>`, `<footer>` etc. |

**⚠️ Tip:** Always prefer **native HTML** before falling back on ARIA.

---

## ✅ 2. **Correct Heading Structure**

* Use **only one `<h1>`** per page
* Nest headings in order (`<h2>` under `<h1>`, etc.)
* Don’t skip levels (e.g., don’t go from `<h2>` to `<h4>`)

Helps screen reader users **understand the content hierarchy**.

---

## ✅ 3. **Accessible Image Descriptions**

### 📸 Use `alt` Attribute:

| Image Type       | Use This                                                   |
| ---------------- | ---------------------------------------------------------- |
| Informative      | `alt="Description"`                                        |
| Decorative       | `alt=""` (empty alt)                                       |
| Complex (charts) | Use `aria-describedby` or a `<figcaption>` for more detail |

```html
<img src="icon.png" alt="Search icon" />
```

---

## ✅ 4. **Forms: Labels and Descriptions**

### 🔹 Every input must have a **label**:

```html
<label for="email">Email Address</label>
<input id="email" type="email" />
```

### 🔹 Use `aria-describedby` for hints or errors:

```html
<input id="username" aria-describedby="usernameHint">
<div id="usernameHint">Must be at least 6 characters.</div>
```

---

## ✅ 5. **Keyboard & Focus Management**

* Ensure all interactive elements are **focusable and operable** via keyboard
* Use `tabindex="0"` to make custom components focusable
* Use `aria-hidden="true"` to hide things from screen readers
* Move focus with `.focus()` when opening modals, tooltips, etc.

---

## ✅ 6. **Live Regions and Dynamic Content**

Use ARIA to let screen readers know when **content updates without a page reload**.

| Attribute               | Use For                           |
| ----------------------- | --------------------------------- |
| `aria-live="polite"`    | Updates that can wait             |
| `aria-live="assertive"` | Urgent updates (errors, warnings) |

```html
<div aria-live="polite" id="statusMsg">Loading complete.</div>
```

---

# 🔑 **Most Common ARIA Attributes for Accessibility**

## 📌 1. `aria-label`

* **What it does:** Gives an **invisible label** to an element.
* **Use when:** There’s **no visible text** to describe the element (e.g., icon buttons).

```html
<button aria-label="Close modal">
  <svg>...</svg>
</button>
```

---

## 📌 2. `aria-labelledby`

* **What it does:** Uses **existing visible text** elsewhere as the label.
* **Use when:** You want to **reuse visible content** for screen readers.

```html
<h2 id="section-title">Pricing Plans</h2>
<div aria-labelledby="section-title">
  ...
</div>
```

---

## 📌 3. `aria-describedby`

* **What it does:** Points to **additional text** that describes the element in more detail.
* **Use for:** Tooltips, input hints, or form error explanations.

```html
<input id="email" aria-describedby="email-help">
<span id="email-help">We'll never share your email.</span>
```

---

## 📌 4. `aria-hidden="true"`

* **What it does:** Hides content **from screen readers**, but not visually.
* **Use for:** Decorative icons, duplicate text, animation artifacts.

```html
<span aria-hidden="true">✓</span>
```

---

## 📌 5. `role`

* **What it does:** Defines the **semantic role** of an element (especially useful for non-semantic HTML).
* **Use for:** Custom components or widgets made with `<div>` or `<span>`.

```html
<div role="button" tabindex="0">Click me</div>
```

🛑 **Prefer native elements** (`<button>`, `<nav>`) over using `role`.

---

## 📌 6. `aria-live`

* **What it does:** Announces **dynamic updates** to screen readers automatically.
* **Use for:** Alerts, validation messages, live status updates.

```html
<div aria-live="polite">
  New message received.
</div>
```

Options:

* `aria-live="polite"` → waits for current reading to finish
* `aria-live="assertive"` → interrupts immediately

---

## 📌 7. `aria-expanded`

* **What it does:** Indicates whether a collapsible section is open.
* **Use for:** Dropdowns, accordions, toggles.

```html
<button aria-expanded="false" aria-controls="menu">Menu</button>
<div id="menu">...</div>
```

---

## 📌 8. `aria-checked`, `aria-selected`, `aria-pressed`

* **What they do:** Indicate **toggle states**.
* **Use for:** Custom checkboxes, radio buttons, tabs, or toggle buttons.

```html
<div role="checkbox" aria-checked="true" tabindex="0">Subscribe</div>
```

---

## 📌 9. `aria-controls`

* **What it does:** Points to the **element being controlled**.
* **Use for:** Buttons or toggles that open/hide another element.

```html
<button aria-controls="accordion1">Toggle Details</button>
<div id="accordion1">...</div>
```

---

## 📌 10. `aria-role="alert"`

* **What it does:** Announces an important message **immediately**.
* **Use for:** Errors, warnings, or important changes.

```html
<div role="alert">
  Please enter a valid email address.
</div>
```

---

# 🧭 Landmark Roles (Semantic Navigation Aids)

Use these **with or without ARIA** to help screen reader users navigate by page regions:

| Role                   | HTML Equivalent        | Purpose                  |
| ---------------------- | ---------------------- | ------------------------ |
| `role="banner"`        | `<header>`             | Site-wide header         |
| `role="navigation"`    | `<nav>`                | Primary/secondary menus  |
| `role="main"`          | `<main>`               | Main content of the page |
| `role="contentinfo"`   | `<footer>`             | Page or site footer      |
| `role="search"`        | `<form role="search">` | Site-wide search         |
| `role="complementary"` | `<aside>`              | Sidebars or related info |

---

## ✅ Best Practices for Using ARIA

* **Use native HTML** (e.g. `<button>`, `<input>`, `<nav>`) before falling back to ARIA.
* **Do not misuse ARIA**—improper use can **reduce accessibility**.
* Always **test with screen readers** (like NVDA, VoiceOver) and automated tools (like axe DevTools).

---

