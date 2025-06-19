# B.I.A.M Study Suite

A modern, interactive study application for Biologically-Inspired Algorithms and Methods (B.I.A.M). This web-based tool helps students learn and test their knowledge through multiple study modes.

## Features

### 📚 Multiple Study Modes

- **Flashcards**: Traditional two-sided cards to test recall
- **Multiple Choice**: Test your knowledge with MCQ format questions
- **Written Answers**: Practice expressing concepts in your own words
- **True/False**: Quick assessment of fundamental concepts
- **Notes**: Review comprehensive study material in a clean format

### 🔄 Interactive Learning Tools

- **Expandable Answers**: Smooth animated reveals for answers
- **Color-Coded Feedback**: Instant visual feedback for correct/incorrect answers
- **Progress Tracking**: Score tracking in multiple-choice mode
- **Dark/Light Mode**: Study comfortably in any lighting condition

### 🤖 AI Integration

- **Ask ChatGPT**: Ask for explanations of concepts directly from any mode
- **Context-Aware Questions**: AI queries include the correct answer for better explanations
- **Text Selection Support**: Select any text in notes to ask for a specific explanation

## Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- No installation required - it runs entirely in the browser

### Installation

1. Clone this repository:
   ```
   git clone https://github.com/dromaniv/quizzer3.git
   ```

2. Open `index.html` in your browser:
   ```
   cd quizzer3
   open index.html
   ```

Alternatively, you can simply host the files on any web server or static hosting service.

## Usage

### Navigation

- Use the top navigation bar to switch between different study modes
- Use Prev/Next buttons to navigate through questions
- Click the "Ask ChatGPT" button when you need additional explanations

### Study Modes

1. **Flashcards**: Click on a card to flip between question and answer
2. **Multiple Choice**: Select an option to see if it's correct
3. **Written Answers**: Type your answer and then reveal the model answer for comparison
4. **True/False**: Select True or False to test your understanding
5. **Notes**: Read through comprehensive study material and select text to get AI explanations

## Customization

The application loads study content from two files:

- `data.json`: Contains all questions and answers for the different modes
- `notes.md`: Contains the study notes in Markdown format

Edit these files to customize the content for your specific study needs.

## Technical Details

- Built with vanilla JavaScript - no frameworks required
- Responsive design works on desktop and mobile devices
- Uses the marked.js library for Markdown rendering
- Local theme preference storage using localStorage

## License

This project is licensed under the terms of the license included in the repository.

## Acknowledgments

- Built for students studying Biologically-Inspired Algorithms and Methods
- Designed to facilitate effective learning through multiple modalities
