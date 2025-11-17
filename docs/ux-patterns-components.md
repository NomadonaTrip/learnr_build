# LearnR UX Patterns & Component Specifications

_Created on 2025-11-17_
_Material UI Design System with Custom Adaptations_

---

## Overview

This document defines consistency rules and component specifications for LearnR's user interface. All components follow Material UI patterns with custom theming using the approved color system.

**Design System**: Material UI (MUI) v5+
**Primary Accent**: #CDF348 (bright lime green)
**Color Palette**: Minimal 3-color approach (green, background, text)

---

## 1. Color System

### Light Mode Theme
```javascript
{
  palette: {
    mode: 'light',
    primary: {
      main: '#CDF348',      // Bright lime green
      light: '#E0F77E',     // Lighter tint
      dark: '#B8E01F',      // Darker shade
      contrastText: '#212121' // Dark text on green buttons
    },
    background: {
      default: '#FFFFFF',   // Pure white
      paper: '#FFFFFF'      // Cards, modals
    },
    text: {
      primary: '#212121',   // Main text (dark gray)
      secondary: '#666666'  // Secondary text
    },
    divider: '#E0E0E0',     // Borders, dividers
    error: {
      main: '#D32F2F'       // Error red
    },
    success: {
      main: '#2E7D32'       // Success green (darker than accent)
    },
    warning: {
      main: '#ED6C02'       // Warning orange
    }
  }
}
```

### Dark Mode Theme
```javascript
{
  palette: {
    mode: 'dark',
    primary: {
      main: '#CDF348',
      light: '#E0F77E',
      dark: '#B8E01F',
      contrastText: '#212121' // Dark text on green backgrounds
    },
    background: {
      default: '#1E1E1E',   // Dark gray background
      paper: '#2A2A2A'      // Slightly lighter for cards
    },
    text: {
      primary: '#FFFFFF',   // White text
      secondary: '#B0B0B0'  // Light gray for secondary
    },
    divider: '#404040',     // Subtle borders
    error: {
      main: '#F44336'
    },
    success: {
      main: '#4CAF50'
    },
    warning: {
      main: '#FF9800'
    }
  }
}
```

### Color Usage Rules

1. **Primary Green (#CDF348)**:
   - Use for: Primary buttons, progress bars, active states, focus indicators
   - Do NOT use for: Body text, headings (insufficient contrast)
   - Text on green: Always use #212121 (dark gray) for readability

2. **Background**:
   - Light mode: Pure white (#FFFFFF)
   - Dark mode: Dark gray (#1E1E1E)
   - No gradients or patterns

3. **Text**:
   - Light mode: #212121 (primary), #666666 (secondary)
   - Dark mode: #FFFFFF (primary), #B0B0B0 (secondary)

4. **Semantic Colors**:
   - Error: Red (#D32F2F light, #F44336 dark)
   - Success: Dark green (#2E7D32 light, #4CAF50 dark) - NOT accent green
   - Warning: Orange (#ED6C02 light, #FF9800 dark)

---

## 2. Typography

### Font Families
- **Headings**: 'Roboto', sans-serif (Material UI default)
- **Body**: 'Roboto', sans-serif
- **Monospace**: 'Roboto Mono', monospace (for code, if needed)

### Type Scale

```css
h1: 2.5rem (40px), font-weight: 700, line-height: 1.2
h2: 2rem (32px), font-weight: 600, line-height: 1.3
h3: 1.75rem (28px), font-weight: 600, line-height: 1.3
h4: 1.5rem (24px), font-weight: 600, line-height: 1.4
h5: 1.25rem (20px), font-weight: 500, line-height: 1.4
h6: 1rem (16px), font-weight: 500, line-height: 1.5

body1: 1rem (16px), font-weight: 400, line-height: 1.5
body2: 0.875rem (14px), font-weight: 400, line-height: 1.43

button: 0.875rem (14px), font-weight: 500, text-transform: uppercase, letter-spacing: 0.02em

caption: 0.75rem (12px), font-weight: 400, line-height: 1.66
```

### Typography Rules

1. **Headings**: Use semantic HTML (h1-h6) for accessibility
2. **Body text**: Use body1 for primary content, body2 for secondary
3. **Button text**: Always uppercase, medium weight (Material UI standard)
4. **Line length**: Max 75 characters per line for readability
5. **Hierarchy**: Maintain clear visual hierarchy (don't skip heading levels)

---

## 3. Spacing System

**Base Unit**: 8px (Material UI standard)

### Spacing Scale
```
xs: 4px   (0.5 * base)
sm: 8px   (1 * base)
md: 16px  (2 * base)
lg: 24px  (3 * base)
xl: 32px  (4 * base)
xxl: 48px (6 * base)
```

### Application
- **Component padding**: md (16px) for most cards, lg (24px) for spacious layouts
- **Vertical spacing**: lg (24px) between sections, md (16px) between related items
- **Form fields**: md (16px) vertical spacing between fields
- **Button padding**: sm (8px) vertical, md (16px) horizontal

---

## 4. Button Patterns

### Button Hierarchy

#### Primary Button
- **Usage**: Main call-to-action (one per screen/section)
- **Component**: `<Button variant="contained" color="primary">`
- **Style**:
  - Background: #CDF348 (green)
  - Text: #212121 (dark gray)
  - Text transform: Uppercase
  - Border radius: 4px
  - Padding: 8px 16px
- **States**:
  - Hover: Background darkens to #B8E01F
  - Active: Slight scale down (transform: scale(0.98))
  - Disabled: Gray background, light gray text, cursor not-allowed
  - Loading: Spinner inside button, text "Loading..."
- **Examples**: "Start Learning", "Submit Answer", "Begin Diagnostic"

#### Secondary Button
- **Usage**: Alternative actions, less emphasis
- **Component**: `<Button variant="outlined" color="primary">`
- **Style**:
  - Background: Transparent
  - Border: 2px solid #CDF348
  - Text: #CDF348 (light mode), #CDF348 (dark mode)
  - Text transform: Uppercase
- **States**:
  - Hover: Background #CDF348 with 10% opacity, text remains green
  - Active: Border darker, slight scale
  - Disabled: Gray border, gray text
- **Examples**: "Cancel", "Return to Dashboard", "Take Diagnostic Test"

#### Tertiary Button (Text Button)
- **Usage**: Low-emphasis actions, navigation
- **Component**: `<Button variant="text" color="primary">`
- **Style**:
  - Background: Transparent
  - Text: #CDF348
  - No border
- **States**:
  - Hover: Background green with 5% opacity
  - Active: Slight scale
  - Disabled: Gray text
- **Examples**: "Skip", "Learn More", "View Details"

#### Destructive Button
- **Usage**: Delete, discard, irreversible actions
- **Component**: `<Button variant="contained" color="error">`
- **Style**:
  - Background: #D32F2F (error red)
  - Text: #FFFFFF
- **States**: Same as primary
- **Examples**: "Delete Account", "End Exam Early"
- **Pattern**: Always require confirmation modal

### Button Sizing
- **Small**: Height 32px, padding 6px 12px, font-size 13px
- **Medium** (default): Height 40px, padding 8px 16px, font-size 14px
- **Large**: Height 48px, padding 10px 20px, font-size 15px

### Button Icons
- **Icon position**: Left of text (leading icon) or right (trailing icon)
- **Icon size**: 20px for medium buttons, scale proportionally
- **Icon + text spacing**: 8px gap
- **Example**: `<Button startIcon={<PlayArrowIcon />}>Start Learning</Button>`

---

## 5. Feedback Patterns

### Success Feedback

#### Inline Success (Brief)
- **Component**: Alert or Snackbar
- **Usage**: After successful action (answer submission, save)
- **Style**:
  - Icon: ✓ (checkmark)
  - Background: Light green (#E8F5E9 light mode, #1B5E20 dark mode)
  - Text: #2E7D32 (success green, light mode), #FFFFFF (dark mode)
  - Border: None or subtle green border
- **Duration**: 3-4 seconds auto-dismiss
- **Example**: "✓ Answer submitted successfully"

#### Success Modal (Major)
- **Usage**: After completing significant milestone (diagnostic, session complete)
- **Component**: Dialog (modal)
- **Style**:
  - Icon: Large checkmark or celebration emoji 🎉
  - Heading: "Session Complete!" or "Congratulations!"
  - Body: Summary of achievement
  - CTA: Primary button to continue
- **Example**: Session complete summary screen

### Error Feedback

#### Inline Error (Form Validation)
- **Component**: FormHelperText with error prop
- **Usage**: Below form fields with validation issues
- **Style**:
  - Text: #D32F2F (error red)
  - Icon: ⚠ or ✗
  - Font size: 12px
- **Example**: "⚠ Password must be at least 8 characters"

#### Error Alert (System)
- **Component**: Alert severity="error"
- **Usage**: Network errors, system failures
- **Style**:
  - Background: Light red (#FFEBEE light mode, #B71C1C dark mode)
  - Text: #C62828 (dark red, light mode), #FFFFFF (dark mode)
  - Icon: ✗ or ⚠
- **Duration**: Persistent (requires user dismiss)
- **Action**: Retry button if applicable
- **Example**: "✗ Failed to load question. [Retry]"

### Warning Feedback
- **Component**: Alert severity="warning"
- **Usage**: Caution notices, non-critical issues
- **Style**:
  - Background: Light orange (#FFF3E0 light mode, #E65100 dark mode)
  - Text: #E65100 (warning orange, light mode), #FFFFFF (dark mode)
  - Icon: ⚠
- **Example**: "⚠ Mock exam will take 3.5 hours. Ensure you have time."

### Info Feedback
- **Component**: Alert severity="info"
- **Usage**: Helpful tips, neutral information
- **Style**:
  - Background: Light blue (#E3F2FD light mode, #01579B dark mode)
  - Text: #0277BD (info blue, light mode), #FFFFFF (dark mode)
  - Icon: ℹ
- **Example**: "ℹ Reviews scheduled based on spaced repetition for optimal retention"

### Loading States

#### Spinner (Inline)
- **Component**: CircularProgress
- **Usage**: Waiting for data (questions, results)
- **Style**:
  - Color: #CDF348 (green)
  - Size: 40px (default), 24px (small)
- **Placement**: Center of loading area
- **Text**: Optional "Loading..." below spinner

#### Skeleton Loading
- **Component**: Skeleton (MUI)
- **Usage**: Large content areas (dashboard, analytics)
- **Style**:
  - Background: Animated gradient (gray to light gray)
  - Shape matches content (rectangles for text, circles for avatars)
- **Example**: Dashboard KA cards show skeleton rectangles while loading

#### Progress Bar (Determinate)
- **Component**: LinearProgress variant="determinate"
- **Usage**: File uploads, multi-step processes
- **Style**:
  - Bar color: #CDF348
  - Background: Light gray (#E0E0E0 light mode, #404040 dark mode)
  - Height: 8px
  - Border radius: 4px
- **Example**: Quiz progress (Question 5 of 20 = 25% progress)

---

## 6. Form Patterns

### Text Input Fields

#### Standard TextField
- **Component**: `<TextField variant="outlined">`
- **Style**:
  - Label: Floating label (Material UI standard)
  - Border: 1px solid #E0E0E0 (light mode), #404040 (dark mode)
  - Border radius: 4px
  - Padding: 12px 16px
  - Font size: 16px
- **States**:
  - Focus: Border #CDF348 (green), 2px width
  - Error: Border #D32F2F (red), error text below
  - Disabled: Gray background, gray text, cursor not-allowed
  - Filled: Border color remains default (not green)

#### Password Field
- **Component**: TextField with InputAdornment
- **Features**:
  - Toggle visibility icon (eye icon) on right
  - Click to show/hide password
  - Type switches between "password" and "text"
- **Example**:
  ```jsx
  <TextField
    type={showPassword ? 'text' : 'password'}
    InputProps={{
      endAdornment: (
        <IconButton onClick={togglePassword}>
          {showPassword ? <VisibilityOff /> : <Visibility />}
        </IconButton>
      )
    }}
  />
  ```

### Form Layout
- **Vertical stacking**: Default, all fields full-width
- **Field spacing**: 16px vertical gap (md)
- **Label position**: Above field (floating) or inline for checkboxes/radios
- **Required indicator**: Red asterisk (*) after label
- **Help text**: Below field, 12px font, gray color

### Form Validation

#### Validation Timing
- **Real-time**: Validate on blur (when user leaves field)
- **Submit**: Validate all fields on form submit
- **No validation on type**: Don't show errors while user is typing

#### Error Display
- **Field-level**: Error text below field (FormHelperText)
- **Form-level**: Alert box at top of form summarizing errors
- **Focus management**: On submit error, focus first invalid field

#### Error Messages
- **Specific**: "Email format is invalid" (not "Invalid input")
- **Actionable**: "Password must contain at least one number"
- **Friendly tone**: "Oops! Email format is invalid" (if brand appropriate)

---

## 7. Modal Patterns

### Modal Sizes
- **Small**: Max width 400px (confirmations, simple forms)
- **Medium**: Max width 600px (default, detailed content)
- **Large**: Max width 900px (data-heavy, complex forms)
- **Full-screen**: Mobile devices (<768px), special cases (mock exam results)

### Modal Structure

#### Header
- **Title**: h5 or h6, bold
- **Close button**: X icon, top-right corner
- **Divider**: Subtle line below header (optional)

#### Body
- **Padding**: 24px all sides
- **Content**: Forms, text, images, etc.
- **Scrollable**: If content exceeds viewport height

#### Footer
- **Actions**: Buttons aligned right
- **Order**: Cancel (secondary) on left, Confirm (primary) on right
- **Padding**: 16px all sides
- **Divider**: Subtle line above footer (optional)

### Modal Types

#### Confirmation Modal
- **Usage**: Before destructive actions
- **Title**: "Are you sure?"
- **Body**: Clear explanation of consequence
- **Actions**: "Cancel" (secondary), "Confirm" (destructive/primary)
- **Example**:
  ```
  Title: "End Exam Early?"
  Body: "You've answered 45 of 120 questions. Ending now will submit your exam as-is and cannot be undone."
  Actions: [Cancel] [End Exam]
  ```

#### Information Modal
- **Usage**: Help, tips, detailed explanations
- **Title**: Descriptive (e.g., "About Competency Scores")
- **Body**: Detailed content, can include images/lists
- **Actions**: "Got it" (primary) or just close button
- **Example**: Session introduction overlays

#### Form Modal
- **Usage**: Quick actions without page navigation (add item, edit settings)
- **Title**: Action-oriented (e.g., "Edit Profile")
- **Body**: Form fields
- **Actions**: "Cancel" (secondary), "Save" (primary)

### Modal Behavior

#### Dismiss Actions
1. Click close button (X)
2. Click "Cancel" button
3. Press Escape key
4. Click backdrop (outside modal) - only for non-critical modals

#### Focus Management
- On open: Focus first interactive element (input field or primary button)
- Trap focus: Tab cycles within modal, doesn't escape to page behind
- On close: Return focus to element that opened modal

#### Accessibility
- Role: `dialog` or `alertdialog` for critical modals
- ARIA labels: `aria-labelledby` (title), `aria-describedby` (body)
- Keyboard navigation: Tab, Shift+Tab, Escape

---

## 8. Navigation Patterns

### Side Navigation (Desktop)

#### Structure
- **Width**: 240px (expanded), 60px (collapsed icon-only)
- **Position**: Fixed left side
- **Background**:
  - Light mode: #FAFAFA (very light gray)
  - Dark mode: #252525 (slightly lighter than page background)
- **Items**:
  - Logo + "LearnR" at top
  - Navigation links (Dashboard, Learn, Reviews, Mock Tests, Analytics, Profile)
  - Settings at bottom

#### Navigation Item States
- **Default**:
  - Icon + label
  - Text color: #666666 (light mode), #B0B0B0 (dark mode)
- **Hover**:
  - Background: #F0F0F0 (light mode), #303030 (dark mode)
  - Cursor: pointer
- **Active**:
  - Background: #CDF348 (green)
  - Text: #212121 (dark)
  - Bold font weight
  - Slight left border or indicator

#### Collapse Behavior
- **Trigger**: Icon button at top of nav
- **Transition**: Smooth 300ms animation
- **Collapsed state**: Shows icons only (60px width)
- **Hover in collapsed**: Tooltip appears with label

### Bottom Navigation (Mobile)

#### Structure
- **Position**: Fixed bottom of screen
- **Height**: 56px
- **Background**: White (light mode), #2A2A2A (dark mode)
- **Shadow**: Subtle shadow upward
- **Items**: 5 icons (Dashboard, Learn, Reviews, Profile, Menu)

#### Navigation Item States
- **Default**:
  - Icon only
  - Color: #666666 (light mode), #B0B0B0 (dark mode)
- **Active**:
  - Icon color: #CDF348 (green)
  - Fill style (solid icon instead of outline)
  - Optional label below icon (small font)
- **Tap**: Ripple effect (Material UI standard)

### Breadcrumbs

#### Usage
- Deep navigation paths (e.g., Dashboard > Analytics > Strategy Analysis Details)
- Top of main content area, above page heading

#### Style
- **Separator**: > or / symbol
- **Links**: Clickable, green on hover
- **Current page**: Not clickable, darker color
- **Font size**: 14px
- **Example**: `Home > Analytics > Strategy Analysis`

### Tab Navigation

#### Usage
- Switching between related views within same page (e.g., Overview / Performance / History)
- Horizontal tabs below page heading

#### Component
- **MUI Tabs**: `<Tabs>` and `<Tab>` components
- **Style**:
  - Underline indicator: Green (#CDF348), 2px height
  - Active tab: Green text, bold
  - Inactive tabs: Gray text, normal weight
  - Hover: Light gray background

---

## 9. Quiz & Learning Components

### Question Card

#### Structure
- **Component**: Card (Material UI)
- **Padding**: 24px
- **Border radius**: 8px
- **Shadow**: elevation={2} (subtle)
- **Background**: White (light mode), #2A2A2A (dark mode)

#### Elements
1. **Question Text**:
   - Font size: 18px (body1 scaled up)
   - Font weight: 500 (medium)
   - Color: Primary text color
   - Margin bottom: 16px

2. **Answer Options**:
   - Layout: Vertical stack, full-width
   - Spacing: 12px between options
   - Component: Radio button group or custom styled buttons

3. **Option Button**:
   - **Default state**:
     - Background: White (light mode), #2A2A2A (dark mode)
     - Border: 2px solid #E0E0E0 (light mode), #404040 (dark mode)
     - Padding: 16px
     - Border radius: 8px
     - Text: Left-aligned
   - **Selected state**:
     - Border: 2px solid #CDF348 (green)
     - Background: #CDF348 with 10% opacity
   - **Hover state**:
     - Border: 2px solid #B0B0B0
     - Cursor: pointer

### Progress Bar (Quiz)

#### Structure
- **Position**: Top of screen, full width
- **Height**: 6px
- **Background**: Light gray (#E0E0E0 light mode, #404040 dark mode)
- **Fill**: Green (#CDF348)
- **Animation**: Smooth transition (300ms) on progress change

#### Variants

**A. Determinate Progress**
- Shows exact progress (e.g., 25% for question 5 of 20)
- Width of green fill = (current / total) * 100%

**B. Segmented Progress**
- Shows individual question indicators (20 small segments)
- Completed: Green fill
- Current: Green outline
- Upcoming: Gray

### Feedback Display (≤300 chars)

#### Structure
- **Component**: Alert (MUI) or custom Card
- **Position**: Below question card or as overlay
- **Padding**: 16px
- **Border radius**: 8px

#### Content
1. **Icon** (left side):
   - Correct: ✓ (green circle)
   - Incorrect: ✗ (red circle)
   - Size: 24px

2. **Result text**:
   - "Correct!" (green text, #2E7D32)
   - "Incorrect. The answer is B." (red text, #D32F2F)
   - Font weight: 500 (medium)

3. **Explanation**:
   - One clear sentence (max 150 chars)
   - Normal font weight (400)
   - Primary text color

4. **CTA Button**:
   - "Continue" or "Next Question"
   - Primary button (green)
   - Full width or right-aligned

#### Example (Correct)
```
[✓] Correct!
Facilitated workshops are most effective for gathering diverse stakeholder input and achieving consensus on requirements.
[Continue Button]
```

#### Example (Incorrect)
```
[✗] Incorrect. The answer is B.
While interviews are valuable, facilitated workshops bring stakeholders together to collaboratively define requirements in real-time.
[Continue Button]
```

---

## 10. Progress Visualization Components

### Parallel KA Progress - Horizontal Bars

#### Structure
- **Layout**: Vertical stack of 6 horizontal bars
- **Spacing**: 16px between bars
- **Each bar**:
  - Label (left): Knowledge area name, 14px font
  - Progress bar (center): Width 100%, height 24px
  - Score (right): Percentage, 16px font, bold

#### Bar Styling
- **Background**: Light gray (#E0E0E0 light mode, #404040 dark mode)
- **Fill**: Green (#CDF348)
- **Border radius**: 12px (rounded ends)
- **Animation**: Progress bar fills from 0 to score% over 500ms

#### Interaction
- **Hover**: Slightly darker fill color
- **Click**: Navigate to detailed view for that KA

#### Example Layout
```
Business Analysis Planning & Monitoring  [███████████░░░░░░░░] 72%
Elicitation & Collaboration              [████████████░░░░░░░░] 68%
Requirements Life Cycle Management       [███████████░░░░░░░░░] 65%
Strategy Analysis                        [█████████░░░░░░░░░░░] 58%
Requirements Analysis & Design Def.      [█████████████░░░░░░░] 70%
Solution Evaluation                      [█████████░░░░░░░░░░░] 55%
```

### Parallel KA Progress - Donut Chart (Mobile)

#### Structure
- **Component**: Chart.js Doughnut chart or MUI custom component
- **Size**: 200px diameter (mobile), 250px (tablet)
- **Segments**: 6 equal-sized arcs (one per KA)
- **Center**: Overall readiness score (68%)

#### Segment Styling
- **Color coding**:
  - Above 70%: Green (#CDF348)
  - 60-69%: Yellow (#FDD835)
  - Below 60%: Orange (#FF9800)
- **Segment fill**: Percentage-based (58% = 58% of segment filled)
- **Gap**: 4px between segments

#### Labels
- **External labels**: KA abbreviations with scores
- **Hover**: Tooltip shows full KA name + score

#### Interaction
- **Tap segment**: Navigate to detailed view for that KA

### Competency Score Card

#### Structure
- **Component**: Card (Material UI)
- **Dimensions**: 300px width, 200px height (desktop), full-width (mobile)
- **Padding**: 20px
- **Shadow**: elevation={3}

#### Elements
1. **KA Name** (heading):
   - Font size: 18px
   - Font weight: 600
   - Margin bottom: 8px

2. **Circular Progress Indicator**:
   - Center of card
   - Size: 120px diameter
   - Track: Light gray
   - Fill: Green (#CDF348) arc
   - Center text: Score percentage (e.g., "72%")

3. **Weekly Growth** (below circle):
   - Text: "+8% this week" or "-2% this week"
   - Color: Green (positive), red (negative)
   - Icon: ↑ or ↓ arrow
   - Font size: 14px

4. **Action Link** (bottom):
   - "View Details" link (tertiary button style)
   - Navigates to KA deep-dive

### Daily Streak Display

#### Compact Version (Dashboard)
- **Layout**: Inline badge
- **Icon**: 🔥 (fire emoji)
- **Text**: "12 day streak"
- **Background**: Light orange gradient (#FFF3E0 to #FFE0B2)
- **Border**: 2px solid #FF9800 (orange)
- **Padding**: 8px 16px
- **Border radius**: 20px (pill shape)

#### Prominent Version (Session Complete)
- **Layout**: Large centered element
- **Icon**: Large 🔥 (48px)
- **Text**: "7 day streak!" (24px, bold)
- **Background**: Animated gradient (gold to orange)
- **Celebration**: Confetti animation (optional)

---

## 11. Reading Content Components

### Brief Explanation (During Quiz)

See **Feedback Display** section above (Section 9).

### Comprehensive Reading Accordion

#### Structure
- **Component**: Accordion (Material UI)
- **Layout**: Vertical stack of accordion items
- **Spacing**: 8px between items

#### Accordion Item (Collapsed)
- **Header**:
  - Icon (left): Knowledge area icon or colored dot
  - Title: Concept name (e.g., "Business Case Development")
  - Subtitle: Knowledge area (e.g., "Strategy Analysis")
  - Expand icon (right): ▼ (chevron down)
- **Background**: White (light mode), #2A2A2A (dark mode)
- **Border**: 1px solid #E0E0E0 (light mode), #404040 (dark mode)
- **Padding**: 16px

#### Accordion Item (Expanded)
- **Header**: Same, expand icon rotates to ▲ (chevron up)
- **Body**:
  - **Content**: BABOK chunk (2-3 paragraphs)
  - **Formatting**:
    - Paragraph spacing: 12px
    - Font size: 16px (body1)
    - Line height: 1.6 (comfortable reading)
    - Max width: 700px (optimal line length)
  - **Padding**: 16px (continuation of header padding)

#### Interaction
- **Click header**: Toggle expand/collapse
- **Animation**: Smooth 300ms expand/collapse transition
- **Keyboard**: Enter or Space to toggle

#### Filtering Logic
- Only show accordions for concepts user struggled with (answered incorrectly) during session
- If user got all correct, show: "Great job! No additional reading needed."

---

## 12. Empty State Patterns

### First Use (No Data Yet)

#### Structure
- **Layout**: Centered vertically and horizontally in content area
- **Elements**:
  1. **Illustration**: Simple icon or graphic (200px)
  2. **Heading**: "Get Started with [Feature]" (h4)
  3. **Description**: 1-2 sentences explaining what will appear here
  4. **CTA**: Primary button to initiate action

#### Example - Dashboard (Before Diagnostic)
```
[Illustration: Chart icon]
"Complete Your Diagnostic Test"
"We'll assess your current CBAP knowledge across all 6 areas to personalize your learning journey."
[Start Diagnostic Button]
```

### No Results (Searched/Filtered)

#### Structure
- **Layout**: Centered in search results area
- **Elements**:
  1. **Icon**: Magnifying glass with X (48px)
  2. **Heading**: "No results found" (h5)
  3. **Description**: "Try adjusting your filters or search terms"
  4. **CTA**: "Clear Filters" button (secondary)

### Cleared Content

#### Structure
- **Layout**: Centered in previously filled area
- **Elements**:
  1. **Icon**: Checkmark or empty box (48px)
  2. **Heading**: "All done!" or "Nothing here" (h5)
  3. **Description**: Context-specific message
  4. **CTA**: Action to add new content

#### Example - No Reviews Due
```
[Icon: ✓]
"All caught up!"
"No reviews due today. Come back tomorrow for your next spaced repetition session."
[Continue Learning Button]
```

---

## 13. Data Visualization Components

### Analytics Charts

#### Line Chart (Progress Over Time)
- **Library**: Chart.js or Recharts
- **Dimensions**: Full width, 300px height
- **X-axis**: Time (weeks)
- **Y-axis**: Competency percentage (0-100%)
- **Lines**: 6 lines (one per KA), color-coded
- **Legend**: Below chart, shows KA names with color swatches
- **Grid**: Subtle horizontal lines (light gray)
- **Tooltip**: On hover, shows exact values for all KAs at that time point

#### Bar Chart (Comparison)
- **Usage**: Compare performance across KAs
- **Dimensions**: Full width, 400px height
- **X-axis**: Knowledge areas (abbreviated)
- **Y-axis**: Score or count
- **Bars**: Green (#CDF348) fill, 40px width, 8px spacing
- **Labels**: Score value on top of each bar

#### Gauge/Radial Chart (Single Metric)
- **Usage**: Exam readiness, overall competency
- **Size**: 200px diameter
- **Arc**: 270° (starts at bottom-left)
- **Fill**: Gradient from orange (low) to green (high)
- **Center text**: Percentage or "Ready" / "Not Ready"
- **Threshold marker**: Line at 75% (exam readiness threshold)

---

## 14. Notification Patterns

### Toast Notifications (Snackbar)

#### Position
- **Desktop**: Bottom-left corner
- **Mobile**: Bottom center

#### Structure
- **Background**: Dark gray (#2A2A2A, semi-transparent)
- **Text**: White (#FFFFFF)
- **Padding**: 12px 16px
- **Border radius**: 4px
- **Max width**: 400px
- **Shadow**: elevation={6}

#### Variants

**Success Toast**
- Icon: ✓ (green)
- Message: "Changes saved successfully"
- Duration: 3 seconds auto-dismiss

**Error Toast**
- Icon: ✗ (red)
- Message: "Failed to save. [Retry]"
- Duration: Persistent (requires dismiss or action)

**Info Toast**
- Icon: ℹ (blue)
- Message: "New review session available"
- Duration: 4 seconds auto-dismiss

#### Action Button
- Optional: "Undo", "Retry", "View"
- Style: Text button (uppercase, white text)
- Position: Right side of message

#### Stacking
- Multiple toasts: Stack vertically with 8px gap
- Max visible: 3 toasts (older ones fade out)

### In-App Notifications (Bell Icon)

#### Badge
- **Position**: Top-right of bell icon in header/nav
- **Style**:
  - Background: Red (#D32F2F)
  - Text: White, count of unread notifications
  - Size: 20px diameter
  - Border: 2px white (creates separation from icon)

#### Dropdown Panel
- **Trigger**: Click bell icon
- **Position**: Below bell icon, right-aligned
- **Dimensions**: 360px width, max 500px height
- **Content**:
  - Header: "Notifications" (bold, with "Mark all read" link)
  - List: Each notification is a card
  - Footer: "View all" link

#### Notification Item
- **Layout**: Horizontal
  - Icon (left): Type-specific (checkmark, alert, info)
  - Content (center):
    - Title: Bold, 14px
    - Message: Normal, 13px, gray
    - Time: Small, 12px, light gray ("2 hours ago")
  - Dot (right): Blue dot if unread
- **Interaction**: Click to navigate to related content
- **Background**: Hover state = light gray

---

## 15. Confirmation Patterns

### Destructive Action Confirmation

#### Usage
- Before: Delete account, end exam early, discard progress

#### Modal Structure
- **Size**: Small (400px max width)
- **Title**: "Are you sure?" (h5, red text)
- **Body**:
  - Clear explanation of action
  - Consequences (e.g., "This cannot be undone")
  - Optional: "Type DELETE to confirm" input field (extra safety)
- **Actions**:
  - "Cancel" (secondary button, left)
  - "Delete" / "End" / "Discard" (destructive button, right)

#### Example
```
Title: "End Exam Early?"
Body: "You've answered 45 of 120 questions. Ending now will submit your exam as-is and cannot be undone."
Actions: [Cancel] [End Exam]
```

### Passive Confirmation (Less Critical)

#### Usage
- Before: Navigate away from unsaved form, log out

#### Modal Structure
- **Size**: Small
- **Title**: "Unsaved Changes" (h5, warning orange text)
- **Body**: "You have unsaved changes. Do you want to save before leaving?"
- **Actions**:
  - "Discard" (secondary, left)
  - "Cancel" (secondary, center)
  - "Save" (primary green, right)

### No Confirmation Needed

#### Auto-save patterns
- Forms auto-save as user types (debounced)
- Show subtle "Saved" indicator (checkmark + text, fades in/out)

#### Easily reversible actions
- "Undo" action available immediately after
- Toast notification with "Undo" button

---

## 16. Responsive Patterns

### Breakpoints
```
xs: 0px - 600px   (Mobile phones)
sm: 600px - 960px (Tablets portrait)
md: 960px - 1280px (Tablets landscape, small laptops)
lg: 1280px - 1920px (Desktops)
xl: 1920px+ (Large desktops)
```

### Layout Adaptations

#### Desktop (lg+)
- Side navigation: Visible, 240px width
- Main content: Grid layouts (2-3 columns)
- Cards: 300-400px width, arranged in rows
- Modals: Medium size (600px), centered

#### Tablet (md)
- Side navigation: Collapsed to 60px (icon-only), expands on hover
- Main content: 2-column grid
- Cards: Full width in single column or 2 columns
- Modals: Medium size, centered

#### Mobile (xs-sm)
- Side navigation: Hidden, replaced with bottom navigation (5 icons)
- Main content: Single column, full width
- Cards: Full width, stacked vertically
- Modals: Full screen or slide-up sheet
- Font sizes: Slightly larger for readability (18px body)
- Touch targets: Minimum 48x48px (WCAG 2.1 AA)

### Component Responsive Behavior

**Parallel KA Progress**:
- Desktop: Horizontal bars
- Mobile: Donut chart + list view toggle

**Quiz Options**:
- Desktop: 2-column grid (if short options)
- Mobile: Single column, full width

**Analytics Dashboard**:
- Desktop: 2x3 grid of KA cards
- Tablet: 2-column grid
- Mobile: Single column, cards stacked

---

## 17. Accessibility Requirements

### WCAG 2.1 Level AA Compliance

#### Color Contrast
- **Text on background**:
  - Normal text: Min 4.5:1 ratio
  - Large text (18px+ or 14px bold): Min 3:1 ratio
- **UI components**: Min 3:1 ratio for borders, icons

**Passing Combinations**:
- #212121 on #FFFFFF: 16.1:1 ✓
- #FFFFFF on #1E1E1E: 14.8:1 ✓
- #212121 on #CDF348: 11.2:1 ✓ (button text on green)
- #CDF348 on #FFFFFF: 1.25:1 ✗ (NEVER use for text)

#### Keyboard Navigation
- **All interactive elements**: Focusable via Tab key
- **Focus indicators**: 2px solid #CDF348 outline, 2px offset
- **Logical tab order**: Left to right, top to bottom
- **Skip links**: "Skip to main content" link at top
- **Keyboard shortcuts**:
  - Quiz: 1-4 for options, Enter to submit
  - Modals: Escape to close
  - Navigation: Arrow keys for menu items

#### Screen Reader Support
- **Semantic HTML**: Use h1-h6, nav, main, article, section
- **ARIA labels**: All icons have aria-label
- **ARIA live regions**: Announcements for dynamic content (e.g., "Answer submitted")
- **Alt text**: All images have descriptive alt attributes
- **Form labels**: All inputs associated with labels (htmlFor + id)

#### Focus Management
- **Modal open**: Focus trapped inside modal
- **Modal close**: Focus returns to trigger element
- **Dynamic content**: Announce changes with aria-live
- **Loading states**: Announce "Loading" and "Loaded"

---

## 18. Animation & Motion

### Principles
- **Subtle**: Animations should feel natural, not distracting
- **Fast**: 150-300ms for most transitions
- **Purposeful**: Every animation should have a reason (guide attention, show relationship)
- **Respect preferences**: Honor `prefers-reduced-motion` media query

### Common Animations

#### Page Transitions
- **Duration**: 300ms
- **Easing**: ease-in-out
- **Type**: Fade in/out (opacity 0 to 1)

#### Button Press
- **Duration**: 100ms
- **Type**: Scale down (transform: scale(0.98))
- **Trigger**: On active state (mouse down or touch)

#### Modal Open/Close
- **Duration**: 250ms
- **Type**:
  - Open: Fade in + scale up (0.9 to 1)
  - Close: Fade out + scale down (1 to 0.9)

#### Progress Bar Fill
- **Duration**: 500ms
- **Easing**: ease-out
- **Type**: Width change (animated from previous to new value)

#### Accordion Expand/Collapse
- **Duration**: 300ms
- **Easing**: ease-in-out
- **Type**: Height change (0 to auto)

#### Toast Notification
- **Enter**: 200ms slide up + fade in
- **Exit**: 150ms fade out
- **Auto-dismiss**: After 3-4 seconds (unless error)

### Reduced Motion
```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## 19. Implementation Checklist

### For Each Component

- [ ] **Visual design matches spec** (colors, spacing, typography)
- [ ] **All states implemented** (default, hover, active, disabled, loading, error)
- [ ] **Responsive behavior** (mobile, tablet, desktop)
- [ ] **Dark mode support** (colors, contrast maintained)
- [ ] **Keyboard accessible** (focusable, keyboard shortcuts work)
- [ ] **Screen reader friendly** (ARIA labels, semantic HTML)
- [ ] **Color contrast meets WCAG 2.1 AA** (4.5:1 for text, 3:1 for UI)
- [ ] **Animations respect reduced motion** (prefers-reduced-motion query)
- [ ] **Loading states** (skeleton, spinner, or progress indicator)
- [ ] **Error states** (clear messaging, retry action if applicable)
- [ ] **Empty states** (helpful guidance, CTA to populate)

---

## 20. Component Priority Matrix

### Phase 1: MVP (Critical Path)

**Must Have**:
1. Button (primary, secondary, tertiary)
2. Text input fields (with validation)
3. Question card + answer options
4. Progress bar (quiz)
5. Brief feedback display (≤300 chars)
6. Side navigation (desktop) + bottom navigation (mobile)
7. Parallel KA progress (horizontal bars OR donut chart)
8. Competency score cards
9. Modal (confirmation, information)
10. Loading spinner
11. Toast notifications (success, error)

### Phase 2: Enhanced Experience

**Should Have**:
12. Accordion (comprehensive reading)
13. Analytics line chart (progress over time)
14. Daily streak display
15. Empty states (all variants)
16. Form validation (real-time)
17. Skeleton loading states
18. Tab navigation
19. Breadcrumbs
20. Gauge chart (exam readiness)

### Phase 3: Polish

**Nice to Have**:
21. Confetti animation (celebrations)
22. Advanced data visualizations
23. In-app notification center
24. Onboarding tooltips
25. Keyboard shortcut hints

---

_This UX patterns document ensures consistent, accessible, and professional implementation of all LearnR components using Material UI with custom theming._
