# 🎨 Chromatic Folds

### An Interactive Study in Color, Motion & CSS Geometry

Chromatic Folds is a creative, interactive color accordion built using **HTML5, CSS3, and Vanilla JavaScript**. It explores the relationship between color, geometry, and motion through an elegant folded-paper-inspired interface.

Users can interact with individual color panels, dynamically add or remove colors, and experience smooth accordion transitions with responsive fold angles.

The project demonstrates modern CSS techniques, dynamic DOM manipulation, the ResizeObserver API, and accessible interactive UI design without relying on external JavaScript frameworks.

---

## ✨ Features

### 🎨 Interactive Color Accordion
- Beautiful folded-paper-inspired color panels.
- Smooth expansion animations on hover, click, and keyboard focus.
- Dynamic accordion layout using CSS Flexbox.
- Unique fold geometry created with `clip-path`.
- Gradient-based lighting and shadow effects.
- Each color maintains its predefined identity.

### ➕ Dynamic Color Management
- Add and remove colors interactively.
- Starts with **8 color panels**.
- Supports a minimum of **3 colors** and a maximum of **12 colors**.
- Automatically disables controls when limits are reached.
- Updates the color counter in real time.
- Preserves existing DOM elements when changing the color count.

### 📐 Dynamic Fold Geometry
- Uses the JavaScript `ResizeObserver` API.
- Automatically calculates fold angles based on panel dimensions.
- Applies alternating fold directions.
- Updates CSS custom properties dynamically.
- Maintains realistic folded-paper geometry during resizing.

### 📱 Responsive Design
- Responsive layout for desktop, tablet, and mobile screens.
- Fluid typography using CSS `clamp()`.
- Flexible accordion dimensions.
- Responsive controls and spacing.
- Uses modern viewport units for improved layout behavior.

### ♿ Accessibility
- Semantic HTML structure.
- Accessible buttons with descriptive ARIA labels.
- Keyboard-accessible color selection.
- Visible keyboard focus indicators.
- `aria-pressed` state for selected panels.
- Live color count announcements using `aria-live`.
- Reduced-motion support through `prefers-reduced-motion`.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| HTML5 | Semantic structure and accessibility |
| CSS3 | Styling, animations, gradients, and responsive layouts |
| JavaScript (ES6+) | Dynamic interactions and DOM manipulation |
| CSS Flexbox | Accordion layout and expansion |
| CSS Clip Path | Folded-paper geometry |
| ResizeObserver API | Responsive fold-angle calculations |
| CSS Custom Properties | Dynamic styling and configuration |
| Google Fonts | Rubik typography |

**No external JavaScript libraries or frameworks required.**

---

## 📁 Project Structure

```text
chromatic-folds/
│
├── index.html       # Main HTML structure
├── style.css        # Styling, geometry and animations
├── script.js        # Interactive functionality
└── README.md        # Project documentation
```

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/chromatic-folds.git
```

### 2. Navigate to the Project

```bash
cd chromatic-folds
```

### 3. Run the Application

Open `index.html` directly in your browser.

Alternatively, use the **Live Server** extension in Visual Studio Code.

No installation, build tools, or environment variables are required.

---

## 🎮 How to Use

### Explore Colors

Hover over a color panel to expand it.

Click a panel to select it and keep it expanded.

Click the selected panel again to deselect it.

Only one panel can remain selected at a time.

### Add Colors

Click the **Add color** button to introduce another color panel.

Colors are added sequentially from the predefined palette.

Maximum supported colors: **12**

### Remove Colors

Click the **Remove color** button to remove the last color panel.

Minimum supported colors: **3**

### Monitor Color Count

The footer displays the current number of active color panels.

---

## 🎨 Color Palette

The application includes 12 predefined colors.

| # | Color Name | Hex Code |
|---|---|---|
| 01 | Scarlet | `#ed4b50` |
| 02 | Vermilion | `#e7875a` |
| 03 | Apricot | `#e7cd5a` |
| 04 | Citron | `#bae75a` |
| 05 | Acid | `#64e75a` |
| 06 | Mint | `#5ae7a5` |
| 07 | Lagoon | `#5ad3e7` |
| 08 | Iris | `#ae5ae7` |
| 09 | Orchid | `#d76cda` |
| 10 | Rose | `#e77ca5` |
| 11 | Dusk | `#a18bbd` |
| 12 | Plum | `#ad6b9f` |

The initial interface displays the first eight colors.

---

## ⚙️ How It Works

### 1. Dynamic Swatch Creation

The `createSwatch()` function generates interactive color panels.

Each swatch includes:

- A predefined background color.
- A descriptive color label.
- An accessible button element.
- Click-based selection behavior.
- Dynamic CSS custom properties.

### 2. Accordion Expansion

CSS Flexbox controls the expansion of individual panels.

```css
.swatch {
  flex: 1;
  transition: flex 0.65s
    cubic-bezier(0.2, 0.8, 0.2, 1);
}

.swatch.chosen,
.swatch:focus-visible {
  flex: 3;
}
```

When a panel is selected or receives keyboard focus, its flex-grow value increases from `1` to `3`.

Hover-based expansion is enabled on devices that support hovering.

### 3. Fold Geometry

The folded-paper appearance is created using CSS `clip-path`.

```css
.swatch {
  clip-path: polygon(
    0 0,
    100% 5%,
    100% 100%,
    0 95%
  );
}
```

Alternating panels use reversed polygon coordinates to create the accordion's zigzag geometry.

### 4. ResizeObserver Integration

The `ResizeObserver` API monitors changes in panel dimensions.

```javascript
const foldObserver = new ResizeObserver(
  (entries) => {
    for (const { target } of entries) {
      const direction =
        Number(target.dataset.num) % 2
          ? 1
          : -1;

      const angle =
        direction *
        Math.atan2(
          target.offsetHeight * 0.05,
          Math.max(1, target.offsetWidth)
        ) *
        (180 / Math.PI);

      target.style.setProperty(
        "--fold-angle",
        `${angle}deg`
      );
    }
  }
);
```

This ensures the text and numerical labels adjust their skew angles according to the changing panel dimensions.

### 5. Efficient DOM Updates

The `updateSwatches()` function adds or removes only the necessary panels instead of rebuilding the entire accordion.

This approach helps preserve existing elements and their event listeners.

### 6. Modern CSS Sibling Functions

The project uses progressive enhancement with:

```css
@supports (width: calc(sibling-index() * 1px)) {
  .swatch {
    --i: sibling-index();
    --n: sibling-count();
  }
}
```

These functions calculate the sibling index and sibling count for stacking behavior in supporting browsers.

JavaScript-provided fallback values ensure the layout does not depend entirely on these newer CSS features.

---

## 🎛️ Customization

You can customize the appearance using CSS custom properties defined in `:root`.

```css
:root {
  --title-size: clamp(28px, 4vw, 55px);
  --accordion-angle: -3deg;
  --label-angle: 90deg;
  --label-size: 20px;
  --number-size: 22px;
  --control-size: 18px;

  --background: #e9e4db;
  --ink: #262723;
}
```

You can modify:

- Background and text colors.
- Accordion rotation.
- Typography sizes.
- Label orientation.
- Number styling.
- Control appearance.

**Note:** Some properties are overridden later in the stylesheet, so the final computed styles also depend on the CSS cascade.

### Customize the Color Palette

Edit the `colors` array inside `script.js`.

```javascript
const colors = [
  { name: "SCARLET", color: "#ed4b50" },
  { name: "VERMILION", color: "#e7875a" },
  { name: "APRICOT", color: "#e7cd5a" }
];
```

You can replace existing colors with your own palette.

---

## 🧠 Key Learning Outcomes

This project demonstrates practical experience with:

1. Advanced CSS Flexbox layouts.
2. Interactive accordion UI development.
3. CSS `clip-path` geometry.
4. Dynamic CSS custom properties.
5. JavaScript DOM manipulation.
6. Browser event handling.
7. ResizeObserver-based responsive calculations.
8. Accessible interactive components.
9. Responsive typography and layouts.
10. Progressive enhancement with modern CSS features.
11. Efficient DOM updates.
12. CSS transitions and cubic-bezier easing.

---

## 🌐 Browser Compatibility

The project uses modern browser features including:

- CSS Flexbox
- CSS `clip-path`
- CSS custom properties
- JavaScript ResizeObserver
- CSS `clamp()`
- Modern viewport units
- CSS sibling functions with fallback support

A recent version of Chrome, Edge, Firefox, or Safari is recommended.

Some advanced CSS features may vary depending on browser support.

---

## 🔮 Future Enhancements

Potential improvements include:

- [ ] Custom color picker.
- [ ] Random color palette generator.
- [ ] Copy HEX color values to clipboard.
- [ ] Export color palettes.
- [ ] Dark and light theme toggle.
- [ ] Save favorite color combinations.
- [ ] Additional accordion animation styles.
- [ ] Enhanced touch interactions.

---

## 👨‍💻 Author

**Manoj Kumar**

Web Developer | JavaScript Enthusiast | Creative UI Developer

🌐 **Portfolio:** https://manojkumar281023.netlify.app/

---

## ⭐ Support

If you find this project interesting or useful, consider giving the repository a ⭐ on GitHub.

Feel free to explore the code, experiment with the color palette, and build your own creative variations.

---

**Built with HTML, CSS, and JavaScript by Manoj Kumar.**