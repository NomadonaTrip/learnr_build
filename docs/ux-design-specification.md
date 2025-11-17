# LearnR UX Design Specification

_Created on November 17, 2025 by Developer_
_Generated using BMad Method - Create UX Design Workflow v1.0_

---

## Executive Summary

LearnR is an AI-powered adaptive learning platform for CBAP certification preparation, targeting working professionals aged 30-45 who need efficient, effective exam preparation despite limited study time. The UX design focuses on creating a **professional, focused learning environment** that balances Duolingo's proven engagement patterns with the seriousness and data-richness required for professional certification.

### Core UX Philosophy

**"Auto-Present, Never Interrupt, Always Inform"**

1. **Auto-Present**: The system intelligently determines what the user needs next (review, new content, mock exam) and presents it automatically—no decision fatigue
2. **Never Interrupt**: Quiz sessions are focused and flow-driven; comprehensive reading content appears only after session completion to avoid breaking concentration
3. **Always Inform**: All 6 CBAP knowledge areas progress in parallel with transparent, real-time competency scores visible on every dashboard visit

### Target Experience

Users should feel:
- **Guided**: Clear next steps without overwhelming choices
- **Informed**: Always aware of current competency and progress
- **Efficient**: Minimal friction between learning sessions
- **Confident**: Transparent progress tracking builds exam readiness confidence

---

## 1. Design System Foundation

### 1.1 Design System Choice

**Selected System: Material UI (MUI) v5+**

**Rationale:**

1. **Accessibility Built-In**: WCAG 2.1 Level AA compliance is a core requirement (EdTech domain, professional users). Material UI provides accessible components out-of-the-box with proper ARIA attributes, keyboard navigation, and focus management.

2. **Professional Component Library**: Includes robust data visualization, complex forms, and dashboard components needed for analytics and progress tracking. Components like DataGrid, Charts, and advanced navigation patterns reduce development time.

3. **Mature Theming System**: Excellent customization capabilities allow us to implement our minimal 3-color palette (#CDF348 green accent, background, text) while maintaining Material Design principles for consistency.

4. **Fast Development Timeline**: 30-day MVP timeline (from Product Brief) requires a battle-tested component library with extensive documentation and community support.

5. **React Ecosystem**: Aligns with tech stack (React SPA) and has excellent TypeScript support for maintainability.

**Material UI Customization:**
- Default Material Design with custom theme overrides
- **Framer-inspired visual design**: Smooth organic shapes, pill-shaped buttons, generous border radii
- **Inter font family**: Modern, highly legible sans-serif (replacing Roboto)
- Primary color: #CDF348 (bright lime green)
- Minimal color palette (3 colors + semantic)
- Custom component variants for quiz and learning components
- Both light and dark mode support
- **Vector icons only**: Material Icons (no emojis for consistent cross-platform rendering)

**Version:** Material UI v5.14+ (with Emotion for styling)

**Design System Documentation:** https://mui.com/material-ui/

**Visual Design Inspiration:** Framer website templates (framer.com/templates) - known for smooth, modern aesthetics with organic shapes, subtle animations, and professional typography

---

## 2. Core User Experience

### 2.1 Defining Experience

**The ONE thing that makes LearnR unique:**

> **"Parallel Mastery with Auto-Guidance"**
>
> Unlike sequential learning platforms (e.g., Duolingo's linear progression), LearnR shows users their competency across all 6 CBAP knowledge areas simultaneously and automatically determines the optimal next learning experience based on reviews due, competency gaps, and exam readiness. Users never have to choose what to study—the system guides them intelligently while maintaining full transparency of progress.

**Key Experience Principles:**

1. **Parallel Progression Transparency**
   - All 6 knowledge areas are visible at all times (horizontal bars on desktop, donut chart on mobile)
   - No artificial gating or sequential unlocking
   - Users see exactly where they're strong and where they need focus

2. **Intelligent Auto-Presentation**
   - Dashboard always shows "Your Next Learning Session" card
   - System decides: Review session (if reviews due) → Adaptive learning (target weak KAs) → Mock exam (if ready)
   - Eliminates choice paralysis while respecting user autonomy (they can override)

3. **Focus-Driven Learning Flow**
   - Quiz sessions are uninterrupted: no comprehensive reading mid-session
   - Brief feedback (≤300 chars) after each question maintains momentum
   - Deep learning happens after session in curated, expandable content

4. **Progress Confidence Building**
   - Real-time competency scores update after every session
   - Weekly growth indicators show trajectory
   - Exam readiness percentage provides clear milestone tracking

### 2.2 Novel UX Patterns

**Pattern 1: Two-Stage Reading Content**

**Problem Solved:** Traditional quiz apps either interrupt quiz flow with lengthy explanations (breaking focus) or provide no learning content at all (missing learning opportunity).

**Our Solution:**
- **During Quiz (Stage 1)**: Brief feedback (≤300 characters) after each answer
  - Correct/Incorrect indicator
  - One-sentence explanation
  - Immediate "Continue" button to maintain flow
- **After Session (Stage 2)**: Comprehensive reading content presented as expandable accordions
  - Filtered to concepts where user demonstrated weakness
  - Full BABOK chunks (2-3 paragraphs) from vector DB
  - User controls pacing (expand/read at own pace)

**User Benefit:** Maintains quiz momentum while still providing deep learning opportunities when user is ready to absorb them.

**Implementation:**
- Stage 1: Alert/Card component with strict 300-char limit
- Stage 2: Material UI Accordion component with BABOK content
- Backend: Flag concepts answered incorrectly, retrieve related chunks post-session

---

**Pattern 2: Parallel Knowledge Area Progress Visualization**

**Problem Solved:** Professional certification exams cover multiple domains, but most learning platforms show only aggregate progress or force sequential progression (unlock domain 2 after completing domain 1).

**Our Solution:**
- **Desktop**: 6 horizontal progress bars (one per knowledge area) stacked vertically
  - Each bar shows: KA name (left), progress bar (center), percentage (right)
  - Green fill (#CDF348) indicates current competency
  - Hover reveals detailed tooltip
- **Mobile**: Donut chart with 6 segments (one per KA)
  - Each segment fills based on competency percentage
  - Color-coded: Green (≥70%), Yellow (60-69%), Orange (<60%)
  - Center displays overall exam readiness percentage

**User Benefit:** Users immediately understand their strengths and weaknesses across all domains, enabling informed decisions about where to focus additional study time.

**Implementation:**
- Desktop: Custom styled Material UI LinearProgress components in vertical stack
- Mobile: Chart.js Doughnut chart or Recharts RadialChart
- Responsive breakpoint: Switch at 768px (md breakpoint)

---

**Pattern 3: Auto-Present Next Experience**

**Problem Solved:** Decision fatigue from too many choices (card-based dashboards with "Study KA1", "Study KA2", "Review", "Mock Exam" all as options).

**Our Solution:**
- Dashboard features single prominent "Your Next Learning Session" card
- System uses priority logic:
  1. IF reviews due → "Review Session" (X concepts ready)
  2. ELSE IF mock exam recommended → "Full Mock Exam" (if ready or 14+ days since last)
  3. ELSE → "Adaptive Learning Session" (target lowest-competency KA)
- User sees ONE clear recommendation with action button
- Alternative actions available as secondary buttons below (e.g., "Take Diagnostic Test", "Start Mock Exam")

**User Benefit:** Clear guidance reduces cognitive load; users trust the system to optimize their learning path.

**Implementation:**
- Backend: Next-experience algorithm evaluates reviews_due, last_mock_date, competency_scores
- Frontend: Prominent Card component with green accent border, description, primary CTA button
- User can override by clicking secondary action buttons (respects autonomy)

---

## 3. Visual Foundation

### 3.1 Color System

**Primary Accent: #CDF348 (Bright Lime Green)**

**Selection Process:**
- Created interactive color theme visualizer (docs/ux-color-themes.html) with 4 theme options
- User evaluated themes with live UI component examples
- Selected **Theme 2: Encouraging Progress** (inspired by Duolingo)
- Modified to use #CDF348 (brighter, more energetic lime green)

**Full Color Palette:**

**Light Mode:**
```
Primary: #CDF348 (bright lime green)
  - Light tint: #E0F77E
  - Dark shade: #B8E01F
  - Contrast text: #212121 (dark gray on green backgrounds)

Background: #FFFFFF (pure white)
Paper (cards, modals): #FFFFFF

Text:
  - Primary: #212121 (dark gray)
  - Secondary: #666666 (medium gray)

Divider/Borders: #E0E0E0 (light gray)

Semantic:
  - Error: #D32F2F (red)
  - Success: #2E7D32 (dark green, NOT accent green)
  - Warning: #ED6C02 (orange)
  - Info: #0277BD (blue)
```

**Dark Mode:**
```
Primary: #CDF348 (same green, works in both modes)
  - Contrast text: #212121 (dark gray on green)

Background: #1E1E1E (dark gray)
Paper: #2A2A2A (slightly lighter for depth)

Text:
  - Primary: #FFFFFF (white)
  - Secondary: #B0B0B0 (light gray)

Divider: #404040 (subtle borders)

Semantic:
  - Error: #F44336
  - Success: #4CAF50
  - Warning: #FF9800
  - Info: #29B6F6
```

**Color Usage Rules:**

1. **Green (#CDF348)** = Accent only
   - Use for: Primary buttons, progress bars, active navigation, focus indicators
   - NEVER for: Body text, headings (insufficient contrast: 1.25:1 on white)
   - Text on green: Always #212121 (dark gray) for 11.2:1 contrast ratio ✓

2. **Background** = Pure white (light) / Dark gray (dark)
   - No gradients, patterns, or textures
   - Maintains clean, professional aesthetic

3. **Text** = High contrast for readability
   - Light mode: #212121 on #FFFFFF = 16.1:1 ✓
   - Dark mode: #FFFFFF on #1E1E1E = 14.8:1 ✓

4. **Semantic colors** = Purpose-specific
   - Success: Dark green (not accent green) to avoid confusion
   - Error: Red for failures, destructive actions
   - Warning: Orange for cautions
   - Info: Blue for helpful tips

**Interactive Visualizations:**
- Color Theme Explorer: [ux-color-themes.html](./ux-color-themes.html)

**Accessibility:**
- All color combinations meet WCAG 2.1 Level AA (4.5:1 for text, 3:1 for UI components)
- Color is never the only indicator (icons + text for states)
- Dark mode provides equivalent contrast ratios

### 3.2 Typography

**Font Families:**
- **All text**: 'Inter', sans-serif
- **Monospace** (if needed): 'JetBrains Mono', monospace

**Rationale:** Inter is a highly legible sans-serif designed specifically for computer screens, with excellent readability at small sizes and a professional, modern appearance. Widely used in modern SaaS products (Framer, Linear, Notion).

**Font Loading:**
```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

**Type Scale:**
```
h1: 40px (2.5rem), weight 700, line-height 1.2
h2: 32px (2rem), weight 600, line-height 1.3
h3: 28px (1.75rem), weight 600, line-height 1.3
h4: 24px (1.5rem), weight 600, line-height 1.4
h5: 20px (1.25rem), weight 500, line-height 1.4
h6: 16px (1rem), weight 500, line-height 1.5

Body (primary): 16px (1rem), weight 400, line-height 1.5
Body (secondary): 14px (0.875rem), weight 400, line-height 1.43

Button: 14px (0.875rem), weight 500, sentence case (not uppercase)
Caption: 12px (0.75rem), weight 400, line-height 1.66
```

**Typography Rules:**
- Semantic HTML (h1-h6) for accessibility
- Max line length: 75 characters for readability
- Comfortable line-height for reading content: 1.6
- Button text: Sentence case, medium weight (Framer-inspired modern style)

### 3.3 Spacing & Layout

**Base Unit:** 8px (Material UI standard spacing)

**Spacing Scale:**
```
xs: 4px   (0.5 units)
sm: 8px   (1 unit)
md: 16px  (2 units)
lg: 24px  (3 units)
xl: 32px  (4 units)
xxl: 48px (6 units)
```

**Border Radius Hierarchy (Framer-Inspired):**
```
Main screen containers: 35px   (large, smooth curves)
Primary information cards: 22px (medium-large, soft corners)
Secondary cards: 14px          (medium, subtle rounding)
Icons: 8-12px                  (small, gentle rounding)
Buttons: pill-shaped           (fully rounded, 9999px)
```

**Rationale:** Larger border radii create a modern, soft, approachable aesthetic inspired by Framer website templates. The hierarchy establishes visual importance through progressive rounding—larger containers have more pronounced curves, creating depth and focus.

**Layout Grid:**
- **Container max-width**: 1280px (lg breakpoint)
- **Gutter**: 24px (lg) on desktop, 16px (md) on mobile
- **Columns**: 12-column grid (Material UI Grid system)

**Component Spacing:**
- Cards: 16px padding (md) default, 24px (lg) for spacious layouts
- Sections: 24px (lg) vertical spacing between major sections
- Related items: 16px (md) vertical spacing
- Form fields: 16px (md) between fields

---

## 4. Design Direction

### 4.1 Chosen Design Approach

**Selection Process:**
- Created interactive design direction mockups (docs/ux-design-directions.html) with 8 complete approaches
- Each mockup showed full-screen designs with light/dark mode toggle
- User evaluated all 8 directions and provided detailed feedback

**Approved Design Directions:**

**1. Quiz Focus Mode** (Direction #3) ✓
- **Layout**: Full-screen, distraction-free quiz interface
- **Elements**:
  - Thin progress bar at top (green fill, full-width)
  - Centered question card with generous padding
  - Vertical stack of answer options (4 options, full-width buttons)
  - Submit button at bottom (disabled until selection)
- **Philosophy**: Minimize cognitive load during quiz sessions
- **Use Case**: All quiz experiences (diagnostic, adaptive, review, mock exam)

**2. Side Navigation Dashboard** (Direction #4) ✓
- **Layout**: Fixed left sidebar (240px) + main content area
- **Sidebar Content**:
  - Logo + "LearnR" branding at top
  - Navigation items: Dashboard, Learn, Reviews, Mock Tests, Analytics, Profile
  - Settings at bottom
  - Active item: Green background (#CDF348), dark text
- **Main Content**: Course dashboard showing:
  - Header: "Your CBAP Competency Profile"
  - Parallel KA progress visualization (6 horizontal bars)
  - Auto-present next experience card
  - Action buttons: "Take Diagnostic Test", "Start Full Mock Exam" (secondary)
- **Philosophy**: Easy navigation to all major features, persistent context
- **Use Case**: Primary desktop navigation pattern
- **Responsive**: Collapses to 60px icon-only on tablet, hidden on mobile

**3. Mobile Bottom Navigation** (Direction #7) ✓
- **Layout**: Fixed bottom bar (56px height) with 5 icons
- **Icons**: Dashboard, Learn, Reviews, Profile, Menu
- **Active State**: Green fill (#CDF348), solid icon style
- **Philosophy**: Thumb-friendly navigation on mobile devices
- **Use Case**: Mobile navigation (replaces side nav below 768px)

**4. Data-Dense Dashboard** (Direction #8 - Modified) ✓
- **Layout**: Analytics-focused dashboard with rich data visualization
- **Sections**:
  1. **Overall Progress Header**:
     - Exam readiness percentage (progress bar)
     - Daily streak (flame vector icon + count, top-right)
  2. **6 Knowledge Area Cards** (2x3 grid on desktop, stacked on mobile):
     - Each card: KA name, current score (circular progress), weekly growth indicator ("+8% ↑" in green)
  3. **Study Activity Timeline**: Line chart showing all 6 KA scores over past 4 weeks
  4. **Learning Metrics**: Total questions, accuracy, concepts mastered, review completion rate
  5. **Exam Readiness Breakdown**: Table with per-KA readiness status
- **Philosophy**: Transparency and data-driven confidence building
- **Use Case**: Analytics page, accessed via side nav
- **User Modification**: Must show all 6 KA scores, weekly growth, and daily streak prominently

**Rejected/Modified Directions:**

**❌ Learning Path Journey** (Direction #1 - Modified)
- Original: Duolingo-style sequential node progression
- Feedback: "Progress along knowledge areas should be in parallel, as against Duolingo wherein they're in sequence"
- Solution: Replaced with parallel KA progress visualization (horizontal bars/donut chart)

**❌ Card-Based Dashboard** (Direction #2 - Rejected)
- Original: Dashboard with multiple action cards (Study KA1, Study KA2, Review, etc.)
- Feedback: "I don't believe the card-based dashboard is necessary. Like Duolingo, the next experience should be automatically presented to the user."
- Solution: Auto-present next experience pattern (single prominent card)

**❌ Minimal Single Column** (Direction #5 - Rejected)
- Original: Narrow single-column layout with extensive whitespace
- Feedback: "Single column is not approved. We should use the white space judiciously without cluttering it."
- Solution: Use multi-column layouts on desktop (2x3 grid for KA cards), full-width on mobile

**✓ Split Screen Learning** (Direction #6 - Partially Approved)
- Concept: Show reading content alongside quiz
- Feedback: "Reading content (correct answer and brief explanation max 300 characters) is approved. More comprehensive reading content will be presented after test sessions."
- Solution: Two-stage reading content pattern (brief during quiz, comprehensive after session)

**Interactive Mockups:**
- Design Direction Showcase: [ux-design-directions.html](./ux-design-directions.html)

### 4.2 Layout Pattern Summary

**Desktop (≥960px):**
- Side navigation: Visible, 240px width
- Main content: Grid layouts where appropriate (2x3 for KA cards, 2-column forms)
- Quiz: Full-screen focus mode (hides side nav during quiz)

**Tablet (768px - 959px):**
- Side navigation: Collapsed to 60px icon-only, expands on hover
- Main content: 2-column grids reduce to single column where needed
- Quiz: Full-screen focus mode

**Mobile (<768px):**
- Side navigation: Replaced with bottom navigation (5 icons)
- Main content: Single column, full-width
- Quiz: Full-screen focus mode
- Donut chart visualization replaces horizontal bars for parallel KA progress

---

## 5. User Journey Flows

### 5.1 Critical User Paths

**Comprehensive user journey documentation:** [user-journey-flows.md](./user-journey-flows.md)

**Summary of Critical Journeys:**

#### Journey 1: First-Time User Onboarding
**Goal:** Establish baseline competency across all 6 CBAP knowledge areas

**Flow:**
1. Welcome splash → "Start Diagnostic Test" CTA
2. Diagnostic introduction (explains 20 questions, ~15 minutes, no pressure)
3. Quiz focus mode: 20 questions with brief feedback after each (≤300 chars)
4. Processing: IRT model calculates initial competency scores (~1-2 seconds)
5. Dashboard reveal: Parallel KA progress visualization + auto-present next experience

**Key UX Decisions:**
- No comprehensive reading during diagnostic (keeps it fast, focused)
- All 6 KA scores immediately visible after completion (parallel progression transparency)
- Auto-present next experience guides user to first learning session

**Mermaid Diagram:** See user-journey-flows.md section

---

#### Journey 2: Daily Learning Session (Adaptive Quiz)
**Goal:** Present optimally-challenging questions to improve competency in targeted KAs

**Flow:**
1. Dashboard: User clicks "Start Learning" from auto-present card
2. Session introduction: "5 questions, ~8 minutes, targeting Strategy Analysis"
3. Quiz focus mode: 5 questions with adaptive difficulty adjustment
4. Brief feedback after each answer (≤300 chars): Correct/Incorrect + one-sentence explanation
5. Session complete summary: Score, competency deltas, daily streak update
6. Comprehensive reading content: Expandable accordions filtered to weak concepts
7. Return to dashboard: Updated auto-present next experience

**Key UX Decisions:**
- Quiz flow uninterrupted (brief feedback only, max 300 chars)
- Comprehensive reading appears AFTER session completion (two-stage pattern)
- Accordions filtered to only concepts user answered incorrectly (targeted learning)
- Auto-present logic re-evaluates after session (may suggest review session next)

**Mermaid Diagram:** See user-journey-flows.md section

---

#### Journey 3: Spaced Repetition Review Session
**Goal:** Reinforce previously learned concepts at optimal intervals (1, 3, 7, 14 days)

**Flow:**
1. Dashboard: Auto-present shows "You have 5 concepts ready for review"
2. Review introduction: Explains spaced repetition benefit
3. Quiz focus mode: 5 review questions (may be same or variations of previous)
4. Brief feedback after each answer
5. Review complete summary: Mastery updates (concepts advanced to longer intervals)
6. Return to dashboard

**Key UX Decisions:**
- System automatically determines when reviews are due (SM-2 algorithm)
- Reviews prioritized in auto-present logic (if due, they appear first)
- Success metrics: Advancing concepts to longer intervals (7-day, 14-day)

**Mermaid Diagram:** See user-journey-flows.md section

---

#### Journey 4: Progress Review & Analytics
**Goal:** Track progress, identify strengths/weaknesses, plan study strategy

**Flow:**
1. User clicks "Analytics" in side navigation
2. Data-dense dashboard loads:
   - Overall progress header (exam readiness, daily streak)
   - 6 KA cards with current scores + weekly growth indicators
   - Study activity timeline (line chart, 4 weeks)
   - Learning metrics (questions answered, accuracy, concepts mastered)
   - Exam readiness breakdown table
3. User reviews data, navigates back to dashboard or other section

**Key UX Decisions:**
- All 6 KAs visible simultaneously with weekly growth (parallel progression transparency)
- Daily streak prominently displayed (motivation)
- Line chart shows trajectory over time (progress confidence building)

**Mermaid Diagram:** See user-journey-flows.md section

---

#### Journey 5: Full Mock Exam Experience
**Goal:** Simulate real CBAP exam conditions, assess exam readiness

**Flow:**
1. Dashboard: User clicks "Start Full Mock Exam"
2. Mock exam introduction: 120 questions, 3.5 hours, no breaks warning
3. Full-screen quiz mode: 120 questions, timer counting down, NO feedback during exam
4. Processing: Grade exam (~2-3 seconds)
5. Mock exam results: Score, pass/fail status, per-KA breakdown, recommendations
6. Optional: Review incorrect questions with detailed explanations
7. Return to dashboard

**Key UX Decisions:**
- Full-screen mode with no side nav (exam conditions)
- No feedback during exam (matches real exam experience)
- Detailed results breakdown by KA (identifies focus areas)
- Optional question review (learning opportunity post-exam)

**Mermaid Diagram:** See user-journey-flows.md section

---

### 5.2 Auto-Present Logic Decision Tree

System determines "next experience" based on priority:

```
1. IF (reviews_due > 0)
   THEN present "Review Session" with count of concepts due

2. ELSE IF (mock_exam_not_taken OR days_since_mock > 14)
   THEN present "Full Mock Exam" recommendation

3. ELSE IF (lowest_KA_competency < 65%)
   THEN present "Adaptive Learning Session" targeting lowest KA

4. ELSE
   THEN present "Balanced Learning Session" across all KAs
```

**Rationale:**
- Reviews prioritized (spaced repetition is time-sensitive for retention)
- Mock exams suggested regularly (every 14 days) to track progress
- Adaptive learning targets weakest KA (efficient competency improvement)
- Balanced learning when competencies are relatively even

---

## 6. Component Library

### 6.1 Component Strategy

**Approach:** **Material UI base + custom components for novel patterns**

**Material UI Components (Used As-Is or Lightly Themed):**

**Navigation:**
- Drawer (side navigation)
- BottomNavigation (mobile)
- Tabs (sub-navigation)
- Breadcrumbs

**Inputs:**
- TextField (text input, password)
- Button (primary, secondary, tertiary variants)
- Checkbox, Radio, Switch
- Select (dropdown)

**Feedback:**
- Alert (success, error, warning, info)
- Snackbar (toast notifications)
- CircularProgress (loading spinner)
- LinearProgress (progress bar)
- Skeleton (loading placeholders)

**Surfaces:**
- Card (content containers)
- Paper (elevation/depth)
- Accordion (expandable content)
- Dialog (modals)

**Data Display:**
- Table (data tables)
- Chip (tags, badges)
- Tooltip
- Badge (notification counts)

**Custom Components (LearnR-Specific):**

**1. QuizQuestionCard**
- **Purpose:** Display quiz questions with answer options
- **Content:**
  - Question text (18px, medium weight)
  - 4 answer options (radio button style or custom styled buttons)
  - Progress indicator (question X of Y)
- **States:**
  - Default: Options selectable, submit button disabled
  - Selected: One option has green border + light green background
  - Submitting: Loading spinner on submit button
  - Disabled: After submission (prevents re-submission)
- **Variants:**
  - Standard (4 options)
  - True/False (2 options)
- **Accessibility:** Keyboard navigation (1-4 keys for options, Enter to submit), ARIA labels

**2. BriefFeedbackDisplay**
- **Purpose:** Show immediate feedback after quiz answer (≤300 chars)
- **Content:**
  - Icon: Checkmark vector icon (correct, green) or X vector icon (incorrect, red) - 24px (Material Icons: check_circle or cancel)
  - Result text: "Correct!" or "Incorrect. The answer is B." - medium weight
  - Explanation: One sentence (max 150 chars) - normal weight
  - CTA: "Continue" button (primary green, pill-shaped, full-width or right-aligned)
- **States:**
  - Correct: Green background (#E8F5E9 light mode), green text
  - Incorrect: Red background (#FFEBEE light mode), red text
- **Variants:** None (always same structure)
- **Behavior:** Appears below question card or as overlay, dismisses on "Continue" click

**3. ParallelKAProgressBars** (Desktop)
- **Purpose:** Visualize competency across all 6 knowledge areas simultaneously
- **Content:**
  - 6 horizontal progress bars (LinearProgress styled)
  - Each bar: KA name (left), progress bar (center, 100% width), percentage (right)
  - Bar fill: Green (#CDF348), background: light gray
- **States:**
  - Default: Static display
  - Hover: Slightly darker fill, tooltip with additional details
  - Animating: Smooth fill transition (500ms) on score update
- **Variants:** None
- **Behavior:** Click bar → navigate to detailed KA view
- **Accessibility:** Keyboard navigable, screen reader announces "Business Analysis Planning and Monitoring, 72%, increased by 8% this week"

**4. ParallelKAProgressDonut** (Mobile)
- **Purpose:** Compact visualization of all 6 KAs on mobile devices
- **Content:**
  - Donut chart (Chart.js Doughnut) with 6 segments
  - Each segment: Color-coded (green ≥70%, yellow 60-69%, orange <60%), fills based on competency %
  - Center: Overall exam readiness percentage
  - External labels: KA abbreviations + scores
- **States:**
  - Default: Static display
  - Tap segment: Navigate to detailed KA view
- **Variants:** None
- **Behavior:** Replaces horizontal bars below 768px breakpoint
- **Accessibility:** Tap targets ≥48px, ARIA labels for each segment

**5. AutoPresentCard**
- **Purpose:** Display system-recommended next learning experience
- **Content:**
  - Header: "Your Next Learning Session"
  - Description: Context-specific (e.g., "5 adaptive questions targeting Strategy Analysis")
  - CTA: Primary button ("Start Learning", "Start Review", or "Start Mock Exam")
  - Border: Green accent (#CDF348, 2px)
- **States:**
  - Default: Static display
  - Hover: Slight elevation increase (shadow)
  - Loading: Skeleton placeholder while next experience calculates
- **Variants:**
  - Review session (if reviews due)
  - Adaptive learning (target weak KA)
  - Mock exam (if ready or 14+ days since last)
- **Behavior:** Backend determines variant via auto-present logic
- **Accessibility:** Full keyboard navigation, CTA button focus-first

**6. ComprehensiveReadingAccordion**
- **Purpose:** Display targeted BABOK content after quiz sessions
- **Content:**
  - Accordion items (Material UI Accordion)
  - Each item: KA icon + concept name (header), BABOK chunk 2-3 paragraphs (body)
  - Filtered to concepts user answered incorrectly
- **States:**
  - Collapsed: Header only, chevron down icon
  - Expanded: Header + body content, chevron up icon
  - Animating: Smooth 300ms expand/collapse transition
- **Variants:** None
- **Behavior:**
  - Click header → toggle expand/collapse
  - Keyboard: Enter/Space to toggle
  - Multiple accordions can be open simultaneously
- **Accessibility:** ARIA expanded/collapsed attributes, focus management

**7. DailyStreakBadge**
- **Purpose:** Motivational display of consecutive learning days
- **Content:**
  - Flame vector icon (24px, orange - Material Icons: local_fire_department)
  - Text: "12 day streak"
  - Pill-shaped badge (border-radius 9999px, fully rounded)
- **States:**
  - Active streak: Light orange background (#FFF3E0), orange border (#FF9800)
  - Broken streak (if applicable): Gray background, gray border, broken chain vector icon
- **Variants:**
  - Compact (dashboard header): Inline badge
  - Prominent (session complete): Large centered element with animation
- **Behavior:** Updates after each learning session
- **Accessibility:** Announced by screen reader as "Daily streak: 12 days"

**8. CompetencyScoreCard**
- **Purpose:** Display individual KA competency with growth indicators
- **Content:**
  - KA name (heading, 18px bold)
  - Circular progress indicator (120px diameter, green fill)
  - Center text: Score percentage (e.g., "72%")
  - Below circle: Weekly growth ("+8% this week ↑" in green or "-2% this week ↓" in red)
  - Bottom: "View Details" link (tertiary button)
- **States:**
  - Default: Static display
  - Hover: Elevation increase, "View Details" underlined
  - Loading: Skeleton placeholder
- **Variants:** None (same structure for all 6 KAs)
- **Behavior:** Click card or "View Details" → navigate to KA deep-dive
- **Accessibility:** Card focusable, Enter to navigate

**Comprehensive component specifications:** [ux-patterns-components.md](./ux-patterns-components.md)

---

## 7. UX Pattern Decisions

### 7.1 Consistency Rules

**Comprehensive UX pattern documentation:** [ux-patterns-components.md](./ux-patterns-components.md)

**Summary of Key Patterns:**

#### Button Hierarchy
1. **Primary Button**: Main CTA (one per screen)
   - Style: Contained, green (#CDF348) background, dark text (#212121)
   - Usage: "Start Learning", "Submit Answer", "Continue"
   - State: Hover darkens, active scales down (0.98)

2. **Secondary Button**: Alternative actions
   - Style: Outlined, green border, transparent background
   - Usage: "Cancel", "Return to Dashboard", "Take Diagnostic Test"

3. **Tertiary Button**: Low-emphasis actions
   - Style: Text-only, green text, no border
   - Usage: "Skip", "Learn More", "View Details"

4. **Destructive Button**: Irreversible actions
   - Style: Contained, red background, white text
   - Usage: "Delete Account", "End Exam Early"
   - Pattern: Always require confirmation modal

#### Feedback Patterns

**Success:**
- Inline: Green Alert with checkmark, 3-4 second auto-dismiss
- Modal: Large checkmark or celebration emoji, summary, primary CTA

**Error:**
- Inline: Red FormHelperText below field, warning icon
- Alert: Red Alert with error icon, persistent (requires dismiss), retry action

**Warning:**
- Alert: Orange background, warning icon
- Usage: Cautions, non-critical issues

**Info:**
- Alert: Blue background, info icon
- Usage: Helpful tips, neutral information

**Loading:**
- Spinner: CircularProgress (green accent, 40px default)
- Skeleton: Animated gradient for large content areas
- Progress bar: LinearProgress (determinate, green fill)

#### Form Patterns

**Validation Timing:**
- Real-time: Validate on blur (when user leaves field)
- Submit: Validate all fields on form submit
- No validation while typing

**Error Display:**
- Field-level: Error text below field (FormHelperText, 12px, red)
- Form-level: Alert box at top summarizing errors
- Focus management: Focus first invalid field on submit error

**Field Styling:**
- Label: Floating (Material UI standard)
- Border: 1px solid gray (default), 2px solid green (focus)
- Error state: Red border, red error text below
- Disabled: Gray background, cursor not-allowed

#### Modal Patterns

**Sizes:**
- Small: 400px (confirmations)
- Medium: 600px (default)
- Large: 900px (data-heavy)
- Full-screen: Mobile (<768px) or special cases

**Structure:**
- Header: Title (h5/h6, bold) + close button (X, top-right)
- Body: Scrollable if exceeds viewport, 24px padding
- Footer: Actions right-aligned, Cancel (left) + Confirm (right)

**Dismiss Actions:**
- Close button (X)
- "Cancel" button
- Escape key
- Click backdrop (non-critical modals only)

#### Navigation Patterns

**Active State:**
- Side nav: Green background (#CDF348), dark text, bold
- Bottom nav: Green icon fill, solid style
- Tabs: Green underline (2px), green text, bold

**Hover State:**
- Side nav: Light gray background
- Buttons: Darken or add background opacity
- Links: Green color (#CDF348)

**Breadcrumbs:**
- Separator: > or /
- Links: Clickable, green on hover
- Current page: Not clickable, darker color

#### Empty State Patterns

**First Use:**
- Illustration/icon (200px)
- Heading: "Get Started with [Feature]"
- Description: 1-2 sentences
- CTA: Primary button to initiate action

**No Results:**
- Magnifying glass with X icon
- Heading: "No results found"
- Description: "Try adjusting filters or search terms"
- CTA: "Clear Filters" (secondary)

**Cleared Content:**
- Checkmark or empty box icon
- Heading: "All done!" or "Nothing here"
- Description: Context-specific
- CTA: Action to add new content

---

## 8. Responsive Design & Accessibility

### 8.1 Responsive Strategy

**Breakpoints:**
```
xs: 0px - 600px   (Mobile phones)
sm: 600px - 960px (Tablets portrait)
md: 960px - 1280px (Tablets landscape, small laptops)
lg: 1280px - 1920px (Desktops)
xl: 1920px+ (Large desktops)
```

**Layout Adaptations:**

**Desktop (lg+):**
- Side navigation: Visible, 240px width
- Main content: Multi-column grids (2x3 for KA cards)
- Quiz: Full-screen focus mode
- Modals: Medium size (600px), centered
- Parallel KA progress: Horizontal bars

**Tablet (md):**
- Side navigation: Collapsed to 60px icon-only, expands on hover
- Main content: 2-column grids
- Modals: Medium size, centered
- Parallel KA progress: Horizontal bars (sufficient space)

**Mobile (xs-sm):**
- Side navigation: Hidden, replaced with bottom navigation (5 icons)
- Main content: Single column, full-width
- Quiz: Full-screen focus mode
- Modals: Full screen or bottom sheet
- Parallel KA progress: Donut chart (compact)
- Touch targets: Minimum 48x48px (WCAG 2.1 AA)
- Font sizes: Slightly larger (18px body) for readability

**Component Responsive Behavior:**
- Tables → Scrollable or stacked on mobile
- Charts → Simplified or alternative visualizations
- Forms → Full-width fields, larger touch targets
- Navigation → Bottom nav with 5 essential icons

### 8.2 Accessibility

**WCAG 2.1 Level AA Compliance**

**Color Contrast:**
- Text on background: Min 4.5:1 (normal text), 3:1 (large text ≥18px)
- UI components: Min 3:1 (borders, icons)

**Verified Combinations:**
- #212121 on #FFFFFF: 16.1:1 ✓
- #FFFFFF on #1E1E1E: 14.8:1 ✓
- #212121 on #CDF348: 11.2:1 ✓
- #CDF348 on #FFFFFF: 1.25:1 ✗ (Never use for text)

**Keyboard Navigation:**
- All interactive elements: Tab-accessible
- Focus indicators: 2px solid green (#CDF348) outline, 2px offset
- Logical tab order: Left-to-right, top-to-bottom
- Skip links: "Skip to main content" at top
- Shortcuts:
  - Quiz: 1-4 for answer options, Enter to submit
  - Modals: Escape to close
  - Navigation: Arrow keys for menu items

**Screen Reader Support:**
- Semantic HTML: h1-h6, nav, main, article, section
- ARIA labels: All icons have aria-label
- ARIA live regions: Announcements for dynamic content ("Answer submitted", "Score updated")
- Alt text: All images have descriptive alt attributes
- Form labels: All inputs associated with labels (htmlFor + id)

**Focus Management:**
- Modal open: Focus trapped inside modal, focus first interactive element
- Modal close: Focus returns to trigger element
- Dynamic content: Announce changes with aria-live
- Loading states: Announce "Loading" and "Loaded"

**Touch Targets:**
- Minimum size: 48x48px (WCAG 2.1 Level AA)
- Spacing: 8px minimum between targets
- Mobile buttons: Larger padding for easier tapping

**Motion & Animation:**
- Respect prefers-reduced-motion media query
- All animations: Max 300ms duration
- Critical information: Never conveyed by motion alone

---

## 9. Framer-Inspired Design Principles

LearnR's visual design draws heavy inspiration from **Framer website templates** (framer.com/templates), adapting their modern, professional aesthetic for an educational technology platform.

### 9.1 Core Framer Design Patterns

**1. Organic, Generous Border Radii**
- Framer templates use large, smooth border radii that create approachable, modern interfaces
- LearnR applies a hierarchical border radius system:
  - Main containers: 35px (prominent, smooth curves)
  - Primary cards: 22px (soft, welcoming corners)
  - Secondary cards: 14px (subtle rounding)
  - Icons: 8-12px (gentle polish)
  - Buttons: Pill-shaped (9999px, fully rounded)
- Creates visual depth through progressive rounding

**2. Pill-Shaped Buttons**
- Fully rounded buttons (border-radius: 9999px) instead of sharp 4px corners
- Creates organic, friendly interaction points
- Encourages user engagement through inviting, touchable shapes
- Applied consistently across all button variants (primary, secondary, tertiary)

**3. Inter Typography**
- Framer heavily uses Inter font family for its screen-optimized legibility
- LearnR adopts Inter throughout the interface (replacing Material UI's default Roboto)
- Sentence case for buttons (not uppercase) for modern, approachable tone
- Excellent readability at all sizes: 12px captions to 40px headings

**4. Subtle, Sophisticated Micro-Interactions**
- Hover states use subtle scale (1.02) instead of only color changes
- Press states scale down (0.98) for tactile feedback
- Smooth easing curves: `cubic-bezier(0.4, 0, 0.2, 1)` for natural motion
- Animations feel responsive and organic, not mechanical

**5. Soft, Diffused Shadows**
- Elevation creates depth without harsh edges
- Shadows are subtle and blend naturally with light/dark modes
- Avoid Material Design's sharp, pronounced shadow layers
- Support for both light mode (subtle gray shadows) and dark mode (deeper, softer shadows)

**6. Generous White Space**
- Clean, uncluttered layouts with breathing room
- Strategic use of space to guide attention
- Avoid cramming information—let components breathe
- Balance between data density (professional requirement) and visual comfort

**7. Vector Icons Only (No Emojis)**
- Emojis render inconsistently across platforms (iOS, Android, Windows, macOS)
- Material Icons provide consistent, professional appearance
- Scalable and accessible (proper ARIA labels, color contrast)
- Examples:
  - Checkmark: `check_circle` (not ✓)
  - Error: `cancel` or `error` (not ✗)
  - Warning: `warning_amber` (not ⚠)
  - Info: `info` or `info_outline` (not ℹ)
  - Flame (streak): `local_fire_department` (not 🔥)

### 9.2 Material UI Theming for Framer Style

**Implementation Example:**

```javascript
import { createTheme } from '@mui/material/styles';

const theme = createTheme({
  typography: {
    fontFamily: "'Inter', sans-serif",
    button: {
      textTransform: 'none', // Sentence case, Framer style
      fontWeight: 500,
    },
    h1: { fontWeight: 700 },
    h2: { fontWeight: 600 },
    h3: { fontWeight: 600 },
    h4: { fontWeight: 600 },
    h5: { fontWeight: 500 },
    h6: { fontWeight: 500 },
  },

  palette: {
    mode: 'light',
    primary: {
      main: '#CDF348',
      contrastText: '#212121',
    },
    background: {
      default: '#FFFFFF',
      paper: '#FFFFFF',
    },
    text: {
      primary: '#212121',
      secondary: '#666666',
    },
  },

  shape: {
    borderRadius: 14, // Default for most components (secondary cards)
  },

  components: {
    MuiButton: {
      styleOverrides: {
        root: {
          borderRadius: 9999, // Pill-shaped
          padding: '10px 24px',
          '&:hover': {
            transform: 'scale(1.02)', // Framer-style subtle scale
            transition: 'transform 150ms cubic-bezier(0.4, 0, 0.2, 1)',
          },
          '&:active': {
            transform: 'scale(0.98)',
          },
        },
      },
    },

    MuiCard: {
      styleOverrides: {
        root: {
          borderRadius: 22, // Primary information cards
          boxShadow: '0 2px 12px rgba(0, 0, 0, 0.08)', // Soft shadow
        },
      },
    },

    MuiPaper: {
      styleOverrides: {
        rounded: {
          borderRadius: 22, // Modals, dialogs
        },
        elevation1: {
          boxShadow: '0 2px 8px rgba(0, 0, 0, 0.06)',
        },
        elevation2: {
          boxShadow: '0 2px 12px rgba(0, 0, 0, 0.08)',
        },
        elevation3: {
          boxShadow: '0 4px 16px rgba(0, 0, 0, 0.1)',
        },
      },
    },

    MuiLinearProgress: {
      styleOverrides: {
        root: {
          borderRadius: 9999, // Pill-shaped progress bars
          height: 8,
        },
      },
    },

    MuiTextField: {
      styleOverrides: {
        root: {
          '& .MuiOutlinedInput-root': {
            borderRadius: 12, // Smooth corners for inputs
          },
        },
      },
    },
  },
});

export default theme;
```

### 9.3 Design Decision Rationale

**Why Framer-Inspired Design?**

1. **Professional + Approachable**: Framer templates balance sophistication with warmth—perfect for professional learners who need a serious tool that doesn't feel intimidating

2. **Modern, Not Trendy**: Framer's design language is contemporary but timeless—won't feel dated in 2-3 years like overly trendy designs

3. **Proven Engagement**: Framer's templates are used by successful SaaS products (Linear, Notion, etc.) with high user satisfaction

4. **Accessibility Compatible**: Soft shapes and generous spacing support accessibility (larger touch targets, better focus indicators, comfortable reading)

5. **Differentiation**: Most learning platforms use Material Design defaults or Bootstrap—Framer-inspired design makes LearnR visually distinctive

6. **Implementation Feasibility**: Material UI's theming system allows Framer aesthetics while maintaining component functionality—best of both worlds

**Why Inter Font?**
- Screen-optimized legibility (designed for digital interfaces)
- Wide character set (supports 200+ languages for future expansion)
- Excellent performance (variable font reduces load times)
- Professional association (used by GitHub, Figma, Stripe)

**Why Pill-Shaped Buttons?**
- Reduces visual weight (no sharp corners competing for attention)
- Encourages clicks (rounded shapes feel more touchable)
- Modern standard (Apple, Google, Microsoft all use rounded buttons in latest designs)
- Consistency (same shape for primary, secondary, tertiary maintains hierarchy through color/border only)

**Why Vector Icons Instead of Emojis?**
- Cross-platform consistency (emojis look different on iOS vs Android vs Windows)
- Professional appearance (vector icons feel intentional, emojis can feel casual/unpredictable)
- Accessibility (vector icons have proper ARIA labels, predictable sizing)
- Design control (can adjust color, size, weight—emojis are fixed)

---

## 10. Implementation Guidance

### 10.1 Completion Summary

**Workflow Completion Status:**

✅ **Design System Selected**: Material UI v5+ with custom theming
✅ **Color System Defined**: #CDF348 green accent with light/dark mode palettes
✅ **Design Directions Chosen**: Quiz focus mode, side nav dashboard, mobile bottom nav, data-dense analytics
✅ **User Journey Flows Documented**: 5 critical paths with Mermaid diagrams and flow specifications
✅ **UX Patterns Defined**: Button hierarchy, feedback, forms, modals, navigation, empty states
✅ **Component Strategy Established**: Material UI base + 8 custom components for novel patterns
✅ **Responsive Strategy Defined**: Breakpoints and adaptation patterns for mobile/tablet/desktop
✅ **Accessibility Requirements**: WCAG 2.1 Level AA with keyboard navigation, screen reader support, color contrast

**Implementation Readiness:**

**High Priority (MVP - Phase 1):**
1. Authentication screens (login, signup, password reset)
2. Diagnostic quiz flow (onboarding)
3. Dashboard with parallel KA progress (horizontal bars)
4. Adaptive learning quiz session
5. Brief feedback display (≤300 chars)
6. Session complete summary
7. Side navigation (desktop) + bottom navigation (mobile)
8. Basic responsive layouts

**Medium Priority (Enhanced Experience - Phase 2):**
9. Comprehensive reading accordions
10. Spaced repetition review flow
11. Analytics dashboard (data-dense with charts)
12. Daily streak display
13. Auto-present next experience logic refinement
14. Empty state variations
15. Dark mode implementation
16. Advanced responsive patterns

**Lower Priority (Polish - Phase 3):**
17. Mock exam full experience
18. Advanced data visualizations (timeline charts)
19. In-app notification center
20. Onboarding tooltips
21. Keyboard shortcut hints
22. Celebration animations (confetti)

**Developer Handoff Checklist:**

- [ ] Material UI v5+ installed and configured
- [ ] Custom theme object created with color palette, typography, spacing
- [ ] Dark mode toggle implemented (ThemeProvider with mode switching)
- [ ] Core layout components: AppShell (side nav + main content), BottomNav (mobile)
- [ ] Quiz components: QuizQuestionCard, BriefFeedbackDisplay
- [ ] Progress components: ParallelKAProgressBars (desktop), ParallelKAProgressDonut (mobile)
- [ ] Dashboard components: AutoPresentCard, CompetencyScoreCard, DailyStreakBadge
- [ ] Responsive breakpoints configured (Material UI breakpoints)
- [ ] Accessibility: Focus indicators styled, ARIA labels added, keyboard navigation tested
- [ ] Color contrast audits passed (Lighthouse or axe DevTools)
- [ ] Auto-present logic API endpoint implemented (backend)
- [ ] Two-stage reading content: Brief feedback endpoint + comprehensive reading endpoint

**Design Files Deliverables:**

1. **ux-color-themes.html**: Interactive color theme visualizer (4 themes, user selected Theme 2 modified)
2. **ux-design-directions.html**: Interactive design direction mockups (8 directions, user approved 4)
3. **user-journey-flows.md**: Detailed user journey specifications with Mermaid diagrams (5 critical paths)
4. **ux-patterns-components.md**: Comprehensive UX pattern and component specifications (20 sections, 300+ lines)
5. **ux-design-specification.md**: This document (comprehensive UX design spec, 1000+ lines)

**Integration with Planning Documents:**

- **PRD Alignment**: All functional requirements (FR1-FR17) have corresponding UX flows
- **Product Brief Alignment**: Core differentiation (adaptive learning loop) reflected in auto-present and two-stage reading patterns
- **Technical Stack Alignment**: Material UI supports React SPA, theming supports PostgreSQL-driven dark mode preferences
- **Timeline Alignment**: MVP prioritization supports 30-day timeline from Product Brief

---

## Appendix

### Related Documents

- **Product Requirements**: [prd.md](./prd.md)
- **Product Brief**: [product-brief.md](./product-brief.md)

### Core Interactive Deliverables

This UX Design Specification was created through visual collaboration:

- **Color Theme Visualizer**: [ux-color-themes.html](./ux-color-themes.html)
  - Interactive HTML showing all color theme options explored
  - Live UI component examples in each theme (buttons, forms, cards, alerts)
  - Side-by-side comparison and semantic color usage
  - User selected Theme 2 with #CDF348 green modification

- **Design Direction Mockups**: [ux-design-directions.html](./ux-design-directions.html)
  - Interactive HTML with 8 complete design approaches
  - Full-screen mockups of key screens with light/dark mode toggle
  - Design philosophy and rationale for each direction
  - User approved: Quiz focus mode (#3), Side nav dashboard (#4), Mobile nav (#7), Data-dense dashboard (#8)
  - User rejected: Card-based dashboard (#2), Minimal single column (#5)
  - User modified: Learning path (#1) → Parallel KA progress, Split screen (#6) → Two-stage reading

- **User Journey Flows**: [user-journey-flows.md](./user-journey-flows.md)
  - Detailed flow documentation for 5 critical user journeys
  - Mermaid diagrams showing screen-by-screen progression
  - Decision points, branching logic, and state changes documented
  - Incorporates all user feedback: parallel KA progression, auto-present logic, two-stage reading

- **UX Patterns & Components**: [ux-patterns-components.md](./ux-patterns-components.md)
  - Comprehensive pattern specifications for consistency (20 sections)
  - Button hierarchy, feedback, forms, modals, navigation, empty states
  - 8 custom component specifications (QuizQuestionCard, BriefFeedbackDisplay, etc.)
  - Accessibility requirements (WCAG 2.1 AA compliance guidelines)
  - Responsive patterns and animation guidelines

### Optional Enhancement Deliverables

_This section will be populated if additional UX artifacts are generated through follow-up workflows._

<!-- Future enhancements:
- Wireframe Generation Workflow → Detailed wireframes from user flows
- Figma Design Workflow → Figma files via MCP integration (if available)
- Interactive Prototype Workflow → Clickable HTML prototypes
- Component Showcase Workflow → Interactive component library (Storybook-style)
- AI Frontend Prompt Workflow → Prompts for v0.dev, Lovable.dev, Bolt.new
-->

### Next Steps & Follow-Up Workflows

This UX Design Specification can serve as input to:

1. **Solution Architecture Workflow** - Define technical architecture with UX context (recommended next step)
   - Input: UX component requirements inform frontend architecture decisions
   - Input: User journey flows inform API endpoint design
   - Input: Responsive strategy informs deployment targets (web responsive vs. separate mobile app)

2. **Implementation Workflow** - Begin frontend development with clear UX specifications
   - Input: Component specifications → Component library implementation
   - Input: UX patterns → Style guide and theming configuration
   - Input: User journeys → Screen implementations

3. **Wireframe Generation Workflow** (Optional) - Create detailed wireframes from user flows
4. **Interactive Prototype Workflow** (Optional) - Build clickable HTML prototypes for user testing
5. **AI Frontend Prompt Workflow** (Optional) - Generate prompts for v0.dev, Lovable.dev, Bolt.new

**Recommended Immediate Next Step:** Run **Solution Architecture Workflow** to define technical architecture informed by UX requirements.

### Design Decision Rationale Summary

**Why Material UI?**
- WCAG 2.1 AA accessibility built-in (EdTech requirement)
- Professional component library (dashboards, data viz)
- Fast development (30-day MVP timeline)
- Excellent theming support (custom #CDF348 green)

**Why #CDF348 Green?**
- Inspired by Duolingo (proven engagement in learning apps)
- Energetic, encouraging, progress-oriented emotional tone
- High contrast on both light and dark backgrounds (11.2:1 with #212121)
- Distinctive brand identity

**Why Parallel KA Progression?**
- Professional exams cover multiple domains simultaneously (not sequential like language learning)
- Users need full transparency to prioritize study time efficiently
- Eliminates artificial gating (users control their learning path)

**Why Auto-Present Logic?**
- Reduces decision fatigue (busy professionals want guidance, not choices)
- Optimizes learning path based on data (reviews due, competency gaps, exam readiness)
- Maintains user autonomy (override available via secondary action buttons)

**Why Two-Stage Reading Content?**
- Maintains quiz flow momentum (brief feedback ≤300 chars doesn't interrupt)
- Provides deep learning opportunity when user is ready (comprehensive reading after session)
- Targets weak areas efficiently (accordions filtered to incorrect concepts)

**Why Side Nav + Bottom Nav (Not Hamburger Menu)?**
- Side nav provides persistent context on desktop (users always know where they are)
- Bottom nav optimizes mobile thumb reach (5 essential icons always accessible)
- Hamburger menu hides navigation (increases cognitive load, requires extra tap)

**Why Data-Dense Dashboard?**
- Professional users want comprehensive data (not simplified, "gamified" progress bars)
- Competency scores + weekly growth + daily streak provide complete picture
- Builds exam confidence through transparent, data-driven progress tracking

### Version History

| Date | Version | Changes | Author |
|------|---------|---------|--------|
| November 17, 2025 | 1.0 | Initial UX Design Specification (collaborative design facilitation, all decisions with user input) | Developer |

---

_This UX Design Specification was created through collaborative design facilitation, not template generation. All decisions were made with user input through visual exploration (color theme visualizer, design direction mockups) and are documented with rationale. The specification is implementation-ready and aligns with PRD functional requirements, Product Brief strategic goals, and 30-day MVP timeline._
