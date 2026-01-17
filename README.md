# Quantum Computing Study Lab ⚛️

A modern, clean, and interactive study application for quantum computing education. This web-based tool features a beautiful gradient-based UI with dark/light mode support and helps students master quantum computing concepts through multiple study modes.

## ✨ Features

### 📚 Multiple Study Modes

- **Flashcards**: Interactive 3D flip cards with smooth animations to test recall
- **Quiz (Multiple Choice)**: Test your knowledge with comprehensive MCQ questions
- **Written Answers**: Practice expressing quantum concepts in your own words with expandable answers
- **True/False**: Quick assessment of fundamental quantum computing principles
- **Notes**: Review comprehensive study material with LaTeX math rendering

### 🎨 Modern UI Design

- **Clean & Accessible**: High contrast, readable design with proper opacity levels
- **Quantum-Inspired Theme**: Purple and cyan gradient accents throughout
- **Fixed Navigation**: Always-visible navigation bar for easy mode switching
- **Dark/Light Mode**: Seamless theme toggle integrated into the navigation bar
- **Smooth Animations**: Subtle fade-ins and transitions for a polished experience
- **Mobile Responsive**: Includes swipe gestures for navigation on touch devices

### 🔄 Interactive Learning Tools

- **Card Counter**: Track your progress through flashcards (e.g., "5 / 50")
- **Color-Coded Feedback**: Instant visual feedback with green for correct, red for incorrect answers
- **Progress Tracking**: Real-time score tracking in quiz mode
- **Expandable Answers**: Smooth animated reveals for written answer verification
- **Theme Persistence**: Your preferred theme is saved between sessions

### 🤖 AI Integration

- **Ask ChatGPT**: Get instant explanations for quantum computing concepts from any mode
- **Context-Aware**: AI queries automatically include the question and correct answer for better explanations
- **Text Selection Support**: Select any text in notes to ask for a specific explanation
- **Mobile-Optimized**: Floating button positioning works seamlessly on all devices

### 🔬 Quantum Computing Support

- **LaTeX Math Rendering**: MathJax integration for displaying quantum equations and notation
- **Code Highlighting**: Syntax highlighting for Python/Qiskit code examples
- **Markdown Notes**: Full markdown support with quantum-specific formatting

## 🚀 Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- Internet connection (for CDN resources: highlight.js, markdown-it, MathJax)
- No installation or build process required - runs entirely in the browser

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/dromaniv/quizzer3.git
   ```

2. Open `index.html` in your browser:
   ```bash
   cd quizzer3
   open index.html
   ```

Alternatively, you can:
- Simply double-click `index.html` to open it in your default browser
- Host the files on any web server or static hosting service (GitHub Pages, Netlify, Vercel, etc.)

## 📖 Usage

### Navigation

- Use the **fixed navigation bar** at the top to switch between study modes
- The nav bar stays visible at all times - no need to scroll to the top
- Toggle between **dark and light mode** using the theme buttons in the nav bar
- Use **Prev/Next** buttons to navigate through questions in each mode
- On mobile devices, **swipe left or right** to navigate

### Study Modes

1. **Flashcards**: 
   - Click anywhere on a card to flip between question and answer
   - View your progress with the card counter
   - Navigate with buttons or swipe gestures

2. **Quiz (Multiple Choice)**: 
   - Select an option to see if it's correct
   - Correct answers turn green, incorrect turn red
   - Your score updates automatically
   - Each question has an "Ask ChatGPT" button for deeper explanations

3. **Written Answers**: 
   - Read the question and type your answer in the text area
   - Click "Reveal Answer" to see the correct response
   - Compare your answer with the model answer

4. **True/False**: 
   - Select True or False to test your understanding
   - Instant feedback on correctness
   - Navigate through statements with buttons or swipes

5. **Notes**: 
   - Comprehensive study material with LaTeX equations and code examples
   - Select any text to get a floating "Ask ChatGPT" button for explanations
   - Clean, readable formatting with gradient headers

### AI Assistance

- Every study mode includes an "Ask ChatGPT" button
- Opens a new ChatGPT tab with context about the quantum computing concept
- In Notes mode, select text to get explanations about specific topics

## 🛠️ Customization

The application loads study content from two files:

### `data.json`
Contains all questions and answers structured as:
```json
{
  "flashcards": [{"q": "...", "a": "..."}],
  "multipleChoice": [{"question": "...", "choices": [...], "answer": "..."}],
  "writtenAnswer": [{"q": "...", "a": "..."}],
  "trueFalse": [{"q": "...", "a": true/false}]
}
```

### `notes.md`
Contains comprehensive study notes in Markdown format with:
- LaTeX math notation (inline `$...$` and display `$$...$$`)
- Code blocks with syntax highlighting
- Tables, lists, and rich formatting

**To customize:** Simply edit these files to replace the quantum computing content with your own study material.

## 🎨 Design Principles

- **Clean & Modern**: No unnecessary animations or visual clutter
- **Accessible**: Solid backgrounds with good contrast ratios
- **Performance**: No laggy effects, optimized for long study sessions
- **Responsive**: Works beautifully on desktop, tablet, and mobile devices

## 💻 Technical Details

- **Pure Vanilla JavaScript**: No frameworks or build tools required
- **Modern CSS**: CSS custom properties for theming, flexbox/grid layouts
- **CDN Dependencies**:
  - [highlight.js](https://highlightjs.org/) for code syntax highlighting
  - [markdown-it](https://github.com/markdown-it/markdown-it) for Markdown parsing
  - [MathJax](https://www.mathjax.org/) for LaTeX math rendering
- **Inter Font**: Clean, modern typography via Google Fonts
- **LocalStorage**: Theme preference persistence
- **Touch Events**: Mobile swipe gesture support

## 📱 Browser Support

- Chrome/Edge (latest)
- Firefox (latest)
- Safari (latest)
- Mobile browsers (iOS Safari, Chrome Mobile)

## 📄 License

This project is licensed under the terms of the license included in the repository.

## 🙏 Acknowledgments

- Designed for quantum computing education
- Built with modern web standards for optimal learning experience
- Inspired by the need for clean, distraction-free study tools
