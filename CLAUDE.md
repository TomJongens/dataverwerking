# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Data Detective** is a browser-based educational game for MBO level 4 students at Da Vinci College, teaching data processing concepts ("Dataverwerking"). The entire application is a single-file React app (`index.html`) that runs directly in the browser without a build pipeline.

**Tech Stack:**
- React 18 (via CDN)
- Babel Standalone (for in-browser JSX transformation)
- Vanilla CSS with CSS variables for theming
- Canvas Confetti library
- Web Audio API for procedural sound generation

**No build process:** All code, styles, and game data live in `index.html`. Changes are immediately visible on page refresh.

## Running the Application

### Development
```bash
# Open directly in browser
start index.html  # Windows
open index.html   # macOS

# Or use a local server (recommended for testing)
# Any static server works - Python, VS Code Live Server, etc.
python -m http.server 8080
```

### Debugging
- Use `.vscode/launch.json` to debug in Chrome
- Configure to run on `localhost:8080`
- React DevTools work normally despite CDN delivery
- Check browser console for Babel transformation errors

## Code Architecture

### Single-File Structure
The `index.html` file contains (in order):
1. **HTML head** (lines 1-10): React/Babel/Confetti CDN imports
2. **CSS styles** (lines 11-1157): Complete styling with CSS variables, animations, dark mode
3. **JavaScript/React** (lines 1158-3222):
   - `gameData` object (lines 1340-2219): All 60 questions across 3 weeks, 4 levels each
   - Sound management system (lines 1165-1218)
   - Achievement and history utilities (lines 2184-2222)
   - Main `DataDetectiveGame` component (lines 2224-3213)
   - ReactDOM render call (lines 3215-3222)

### Game Data Structure
Located at line 1340, the `gameData` object follows this pattern:
```javascript
gameData = {
  week1: {
    level1: { title: "...", questions: [...] },
    level2: { title: "...", questions: [...] },
    level3: { title: "...", questions: [...] },
    level4: { title: "...", questions: [...] }
  },
  week2: { /* same structure */ },
  week3: { /* same structure */ }
}
```

### Question Types
The game supports five question types (check the `question.type` switch in the rendering logic):

1. **`multiple-choice`**: Standard single-answer questions
   - Structure: `{ id, type, question, options: [], correct: number, explanation }`

2. **`matching`**: Drag items to categories
   - Structure: `{ id, type, question, items: [{text, category}], categories: [], explanation }`

3. **`drag-drop-classification`**: Same as matching (different UI label)

4. **`scenario`**: Practical situation questions (same structure as multiple-choice)

5. **`scenarios-multiple`**: Select all correct answers
   - Structure: `{ id, type, question, scenarios: [{text, isWrong: boolean}], explanation }`

### React Component Structure
The main `DataDetectiveGame` component (line 2224) uses React Hooks exclusively:
- **State management**: 15+ `useState` hooks for game state, UI state, achievements, theme
- **Side effects**: `useEffect` for theme updates, sound toggling, keyboard navigation
- **Refs**: `useRef` for question card animations
- **No class components**: Everything is functional

## Key Features

### Theming System
- CSS variables defined in `:root` and `[data-theme="dark"]` (lines 14-55)
- Toggle handled by `theme` state and `useEffect` that sets `data-theme` attribute
- All colors, shadows, and transitions use CSS custom properties

### Sound System
`SoundManager` object (lines 1165-1218) uses Web Audio API:
- **Procedural generation**: No audio files, all sounds created with oscillators
- **Sound types**: correct (ascending melody), incorrect (descending), click, level-up, achievement
- **Toggle**: `soundEnabled` state controls `SoundManager.enabled`

### Achievements System
Five achievements tracked in `unlockedAchievements` state:
- `first_correct`: First question answered correctly
- `perfect_level`: All questions in a level correct
- `streak_3`: 3 correct answers in a row
- `streak_5`: 5 correct answers in a row
- `completionist`: All levels completed

Logic in `unlockAchievement` function (line 2264) and various check points throughout the component.

### Progress Tracking
- **LocalStorage**: Test history saved via `localStorage.getItem/setItem('dataDetectiveHistory')`
- **History format**: Array of objects with `{ week, level, score, percentage, date }`
- **Access functions**: `getTestHistory()`, `saveTestResult()`, `clearTestHistory()` (lines 2184-2222)

## Modifying Content

### Adding Questions
1. Locate the appropriate week/level in `gameData` (starting line 1340)
2. Add question object to the `questions` array
3. Ensure `id` is unique across all questions
4. Use correct structure for the question type (see Question Types above)
5. Always include an `explanation` for educational value

### Adding a New Week
1. Add `week4` object to `gameData` with same structure as weeks 1-3
2. Update week selector buttons in the JSX rendering logic
3. Update learning objectives display (search for "selectedWeek === 1" conditionals)
4. Test that level navigation works correctly

### Adding Question Types
1. Add new case to the question type switch statement (search for `question.type === 'multiple-choice'`)
2. Define question structure in this CLAUDE.md
3. Implement rendering logic and answer validation
4. Test keyboard navigation compatibility

### Styling Changes
- All styles are in the `<style>` block (lines 11-1157)
- Use CSS variables for colors/shadows to maintain theme consistency
- Dark mode: Define overrides in `[data-theme="dark"]` selector
- Animations: Many use `@keyframes`, check existing patterns before adding new ones

## Educational Context

### Curriculum Alignment
Content aligns with the "Dataverwerking" curriculum from `source/Leerlijn dataverwerking.pdf`:
- **Week 1**: Basic data concepts (DIKW pyramid), structured/unstructured data, GDPR/AVG, ethical data handling
- **Week 2**: Administrative documents (orders, invoices), data flow, financial processing (VAT, debtor/creditor)
- **Week 3**: Reporting types, Excel functions, pivot tables, data analysis and interpretation

### Learning Objectives
Each level must connect to specific competencies:
- Analyzing data structures
- Information processing
- Communication (through explanations)
- Responsible data handling (AVG/GDPR scenarios)
- Quality-focused work (data quality questions)

### Content Guidelines
When adding/modifying questions:
- Keep language at MBO level 4 (accessible but professional)
- Use realistic workplace scenarios (CRM, HR, finance systems)
- Include practical "What would you do?" questions for ethical/AVG topics
- Explanations should be concise and refer to real-world applications
- Link to relevant learning objectives from the curriculum

## Common Pitfalls

### Babel Transformation
- JSX must be in `<script type="text/babel">` blocks
- Syntax errors show in browser console as Babel errors
- Arrow functions and modern JS work fine
- Don't mix `type="text/babel"` with `type="module"`

### State Management
- All state updates are asynchronous
- Use functional updates when new state depends on old: `setState(prev => prev + 1)`
- Batch updates within same event handler render once

### React Hooks Rules
- Hooks must be at top level (not in conditionals/loops)
- Hook dependencies in `useEffect` must be complete or you'll get stale closures
- Custom hooks not used in this codebase

### CSS Variables
- Must be accessed with `var(--variable-name)` in CSS
- JavaScript access via `getComputedStyle(element).getPropertyValue('--variable-name')`
- Theme switching changes `:root` vs `[data-theme="dark"]` context

## Testing

### Manual Testing Checklist
- Test all question types render correctly
- Verify drag-and-drop works on both mouse and touch
- Check keyboard navigation (numbers 1-4 for answers, Enter for next)
- Test dark mode toggle (check all screens)
- Verify sound toggle (each sound type)
- Test all achievements unlock correctly
- Check progress saving (clear localStorage and retest)
- Mobile responsive: test on narrow viewports (<600px)
- Accessibility: tab navigation, screen reader announcements

### Browser Requirements
- Modern browsers (Chrome, Edge, Firefox, Safari)
- Internet connection required (CDN dependencies)
- JavaScript must be enabled
- LocalStorage must be available (private browsing may affect history)

## File Organization

```
.
├── index.html                 # Complete application (3222 lines)
├── da-vinci-college.webp     # Logo displayed in UI header
├── README.md                  # Dutch documentation for students/teachers
├── LICENSE                    # Project license
├── .vscode/
│   └── launch.json           # Chrome debugging configuration
├── docs/                      # Empty placeholder directory
└── source/                    # Educational materials (PDFs)
    ├── Leerlijn dataverwerking.pdf         # Core curriculum document
    ├── Planning BSP W3 25-26.pdf           # Course planning
    ├── W1 - Les 1 - Dataverwerking - Intro & Data (2).pdf
    ├── W1 - Les 2 - Dataverwerking - Structuur (1).pdf
    └── W1 - Les 3 - Dataverwerking - AVG (1).pdf
```

## Version Information

Current version: **v2.0** (as of November 17, 2025)
- 60 questions across 3 weeks (20 per week)
- Complete gamification (achievements, streaks, confetti)
- Full accessibility support
- Dark mode with smooth transitions
- Procedural sound effects
- Mobile responsive

## Git Workflow

Current branch: `main`
- No separate development branch
- Commits should reference version numbers or feature changes
- Keep commit history clean (reference README's version history section)
