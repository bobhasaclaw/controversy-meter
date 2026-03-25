# Controversy Meter — Specification

## Concept & Vision

A premium editorial tool that transforms binary debates into nuanced visual landscapes. The experience should feel like opening a sophisticated analysis piece in a premium publication — something worth screenshotting and sharing. The central metaphor is a "lens" that reveals the hidden complexity beneath seemingly simple arguments.

**Tagline:** "In a world of binary arguments, see the full landscape."

---

## Design Language

### Aesthetic Direction
Dark editorial sophistication — inspired by The Economist's data visualizations meets Bloomberg Terminal aesthetics. Deep, rich backgrounds with vibrant accent colors that pop.

### Color Palette
- **Background:** `#0d1117` (deep space black)
- **Surface:** `#161b22` (elevated cards)
- **Border:** `#30363d` (subtle separators)
- **Text Primary:** `#f0f6fc` (bright white)
- **Text Secondary:** `#8b949e` (muted gray)
- **Spectrum Gradient:** `#ff6b6b` → `#feca57` → `#48dbfb` → `#1dd1a1` → `#5f27cd` (emotional journey from red through purple)
- **Accent:** `#58a6ff` (electric blue for CTAs)

### Typography
- **Headings:** Inter (700 weight) — clean, authoritative
- **Body:** Inter (400/500 weight)
- **Monospace accents:** JetBrains Mono for stats/data
- **Scale:** 14px base, 1.5 line-height

### Spatial System
- Base unit: 8px
- Card padding: 24px
- Section gaps: 48px
- Max content width: 1200px

### Motion Philosophy
- Spectrum bar: sequential position marker animation on load (staggered 150ms each)
- Cards: slide-up + fade-in, 400ms ease-out
- Hover states: subtle scale (1.02) + glow effect
- Loading: pulsing gradient shimmer on spectrum bar
- All transitions use cubic-bezier(0.4, 0, 0.2, 1)

---

## Layout & Structure

### Page Flow
1. **Header** — Logo/title + tagline, minimal
2. **Hero Input Section** — Large topic input, centered, with trending chips below
3. **Spectrum Visualization** — The hero element, full-width gradient bar with 5-7 clickable position markers
4. **Viewpoint Cards Grid** — 2-3 column responsive grid, cards expand on click
5. **Spectrum Map** — Visual connection diagram showing position relationships
6. **Share Section** — Generate shareable link/summary
7. **Footer** — Minimal attribution

### Responsive Strategy
- Desktop: 3-column card grid, full spectrum bar
- Tablet: 2-column grid
- Mobile: Single column, horizontal scroll for spectrum markers if needed

---

## Features & Interactions

### Topic Input
- Large input field (500px max-width, 56px height)
- Placeholder: "Enter any topic to map its controversy..."
- Submit via Enter key or "Analyze" button
- On submit: show loading state, disable input, animate spectrum bar segments

### Visual Spectrum Meter (HERO)
- Full-width gradient bar (8px height) with smooth color transitions
- 5-7 circular markers positioned along the bar
- Each marker: 40px diameter, white fill, colored border matching spectrum position
- Markers pulse gently when loading
- On load complete: markers animate in sequentially with bounce
- Hover on marker: expand to 48px, show position name tooltip
- Click on marker: highlight corresponding card, scroll into view

### Viewpoint Cards
- Each card contains:
  - Position number badge (colored)
  - Position name (bold heading)
  - Strongest argument (italic quote)
  - Who holds this view (paragraph)
  - Key stat (monospace, highlighted)
  - Connection tags: "Agrees with X" / "Opposes Y"
- Default state: collapsed to show only position name + number
- Expanded state: full content visible with smooth height animation
- Active state (when corresponding spectrum marker clicked): glowing border

### Spectrum Map
- Horizontal node graph showing positions as circles
- Lines connect nodes showing relationships
- Solid lines = agreement, dashed lines = opposition
- Line thickness indicates strength of relationship

### Share Button
- Click generates URL with topic encoded as query param
- Copies summary text to clipboard
- Toast notification confirms copy

### Trending Topics
- 8 chip buttons below input
- Chips: "AI Art", "Crypto Investing", "5:2 Fasting", "Homesteading", "Tesla FSD", "Remote Work", "Nuclear Energy", "Universal Basic Income"
- Click loads topic immediately

---

## Component Inventory

### Input Field
- States: default, focused (blue glow), loading (disabled, spinner), error (red border)
- Dimensions: 100% width up to 500px, 56px height
- Border-radius: 12px

### Analyze Button
- States: default (blue), hover (lighter blue + lift), loading (spinner + "Analyzing..."), disabled (gray)
- Dimensions: auto width, 48px height, 24px horizontal padding

### Spectrum Bar
- Container: full width, 120px height (includes markers + labels)
- Gradient track: 8px height, rounded, subtle shadow
- Markers: 40px circles, white fill, 3px border in spectrum color
- Labels: 12px text below each marker

### Viewpoint Card
- Dimensions: fluid width (grid cell), min-height 120px collapsed
- States: collapsed, expanded, active (glowing border)
- Animation: max-height transition 400ms, opacity 200ms

### Chip Button
- Padding: 8px 16px
- Border-radius: 20px (pill shape)
- States: default (outline), hover (filled background)

### Share Button
- Icon + text layout
- States: default, hover, active (copied state shows checkmark)

---

## Technical Approach

### Stack
- Single HTML file
- Inline CSS (no external stylesheets except Tailwind CDN)
- Vanilla JavaScript (no framework)
- Google Fonts: Inter, JetBrains Mono
- Tailwind CSS via CDN

### API Integration
- Endpoint: `https://api.minimax.io/anthropic/v1/messages`
- Method: POST
- Headers: `x-api-key`, `anthropic-version`, `content-type`
- Model: MiniMax-M2.7
- Max tokens: 4096

### State Management
- Simple object storing: currentTopic, positions[], activePosition
- DOM updates via direct manipulation

### Error Handling
- API failure: show error state in cards area with retry button
- Invalid response: graceful fallback message
- Network timeout: 30 second limit with timeout message

### Output Path
`/Users/pmanopen/Documents/controversy-meter/index.html`
