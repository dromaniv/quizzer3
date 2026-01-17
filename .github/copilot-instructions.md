# GitHub Copilot Instructions for Quantum Computing Study Lab

## Project Overview

This is a **vanilla JavaScript web application** for studying quantum computing concepts. It features multiple study modes (flashcards, quiz, written answers, true/false, notes) with a modern, clean UI using quantum-inspired purple/cyan gradients.

## Core Principles

1. **Pure Vanilla JavaScript**: No frameworks, no build tools, no npm dependencies
2. **Clean & Accessible Design**: High contrast, solid backgrounds, no excessive transparency
3. **Performance First**: No laggy animations, optimized for long study sessions
4. **Mobile-First Responsive**: Touch gestures and responsive layouts

## Architecture

### File Structure
- `index.html` - Single-page application with embedded CSS and JavaScript
- `data.json` - Study content (questions, answers, choices)
- `notes.md` - Markdown study notes with LaTeX support
- `favicon.ico` - App icon

### Key Technologies
- **No frameworks**: Pure DOM manipulation
- **CSS Custom Properties**: For theming (dark/light mode)
- **CDN Libraries**: highlight.js, markdown-it, MathJax
- **LocalStorage**: Theme preference persistence

## Code Style Guidelines

### JavaScript
- Use modern ES6+ features (arrow functions, const/let, async/await)
- Prefer functional approaches where appropriate
- Keep functions small and focused
- Use descriptive variable names
- Avoid global scope pollution - use IIFEs for modules

```javascript
// Good: Use arrow functions and const
const render = () => {
  const card = $('#fcCard');
  card.classList.remove('flipped');
};

// Good: Use async/await for data loading
(async()=>{
  const arr = (await dataPromise).flashcards;
  // ... rest of code
})();
```

### CSS
- Use CSS custom properties for all colors and sizes
- Mobile-first responsive design
- Transition/animation durations should be fast (0.2-0.3s)
- Avoid heavy effects (blur, shadows should be subtle)
- Use semantic naming (--brand, --bg, --text)

```css
/* Good: Use custom properties */
.btn {
  background: var(--brand);
  color: #fff;
  transition: all 0.2s ease;
}

/* Avoid: Magic numbers and hard-coded colors */
.btn {
  background: #8b5cf6;
  transition: all 300ms;
}
```

### HTML
- Semantic HTML5 elements
- Accessible (aria-labels where needed)
- Keep structure flat and simple
- Avoid unnecessary wrapper divs

## Component Patterns

### Navigation
- Fixed position at top (z-index: 200)
- Contains both nav buttons and theme toggle
- Active state shows with brand color background
- Mobile-responsive with proper touch targets

### Study Modes
Each mode follows this pattern:
```javascript
(async()=>{
  const arr = (await dataPromise).modeName;
  shuffle(arr); // Randomize order
  let i = 0; // Current index
  
  const render = () => {
    // Update DOM with current item
  };
  
  const prev = () => { i = (i-1+arr.length)%arr.length; render(); };
  const next = () => { i = (i+1)%arr.length; render(); };
  
  // Attach event handlers
  addSwipe(element, next, prev); // Mobile support
})();
```

### Theme System
- Two themes: dark (default) and light
- Stored in localStorage
- Applied via class on `<html>` element
- CSS variables change based on theme

## Common Tasks

### Adding a New Study Mode
1. Add button to nav with `data-view="modename"`
2. Add section with `id="modename"` class="view"
3. Add data structure to `data.json`
4. Create async IIFE to handle the mode logic
5. Implement shuffle, navigation, and rendering

### Modifying Styles
- Always use CSS custom properties, never hard-code colors
- Test in both dark and light mode
- Ensure sufficient contrast for accessibility
- Keep animations subtle and fast

### Adding Features
- Prefer vanilla JavaScript solutions over libraries
- Keep bundle size minimal (no new CDN dependencies if possible)
- Test on mobile devices (touch events, responsive layout)
- Maintain performance (no memory leaks, efficient DOM updates)

## LaTeX & Markdown

### LaTeX Rendering
- Inline math: `$...$` or `\(...\)`
- Display math: `$$...$$` or `\[...\]`
- MathJax handles rendering asynchronously
- Always call `MathJax.typesetPromise()` after updating content

### Markdown Rendering
- Use markdown-it with `html: true` for flexibility
- Render with `renderMD(element, markdownString)`
- Support tables, lists, code blocks, and LaTeX

## Quantum Computing Context

When working with quantum computing content:
- Use proper notation: kets `|ψ⟩`, bras `⟨ψ|`
- LaTeX for math: `$|0\rangle$`, `$\hat{H}$`, etc.
- Reference IBM Qiskit when discussing code
- Use atom emoji ⚛ for quantum-related UI elements

## Testing Checklist

Before committing changes:
- [ ] Test in both dark and light mode
- [ ] Test on mobile (responsive layout, touch gestures)
- [ ] Verify no console errors
- [ ] Check theme persistence (localStorage)
- [ ] Ensure all study modes still work
- [ ] Verify LaTeX renders correctly
- [ ] Test "Ask ChatGPT" functionality
- [ ] Check fixed nav stays at top

## Performance Guidelines

- Avoid unnecessary re-renders
- Use event delegation where appropriate
- Minimize DOM queries (cache selectors)
- Avoid memory leaks (remove event listeners if needed)
- Keep CSS animations on compositor properties (transform, opacity)

## Accessibility

- Maintain contrast ratios (WCAG AA minimum)
- Include aria-labels for icon buttons
- Ensure keyboard navigation works
- Use semantic HTML
- Test with screen readers when possible

## Don't Do

❌ Add npm dependencies or build tools
❌ Use frameworks (React, Vue, etc.)
❌ Add heavy animations or effects
❌ Use excessive transparency (readability issues)
❌ Break mobile responsiveness
❌ Add features that hurt performance
❌ Hard-code colors instead of using CSS variables
❌ Create memory leaks with unremoved listeners

## Do

✅ Keep code simple and readable
✅ Use vanilla JavaScript idiomatically
✅ Maintain the clean, modern aesthetic
✅ Test on mobile devices
✅ Optimize for performance
✅ Use semantic HTML and CSS
✅ Follow established patterns
✅ Keep the quantum computing theme consistent
