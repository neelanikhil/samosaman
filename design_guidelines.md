# Samosa Man - Design Guidelines

## Design Approach: Material Design System
**Rationale:** As a utility-focused business application with information-dense displays, forms, and analytics dashboards, Material Design provides optimal component patterns for data entry efficiency and chart visualization clarity. The system's mobile-first principles align perfectly with field employee needs.

---

## Core Design Elements

### A. Color Palette

**Light Mode:**
- Primary: 27 65% 47% (Deep Teal - professional, food-industry appropriate)
- Primary Variant: 27 70% 35% (Darker teal for hover states)
- Secondary: 35 85% 55% (Warm Amber - samosa-inspired accent)
- Background: 0 0% 98% (Off-white)
- Surface: 0 0% 100% (Pure white cards)
- Error: 0 65% 51% (Material red for validation)
- Success: 145 63% 42% (Material green for confirmations)

**Dark Mode:**
- Primary: 27 55% 65% (Lighter teal for contrast)
- Background: 220 15% 10% (Deep charcoal)
- Surface: 220 15% 15% (Elevated charcoal)
- Text Primary: 0 0% 95%
- Text Secondary: 0 0% 70%

**Role Indicators:**
- Admin Badge: 266 60% 55% (Purple)
- Employee Badge: 210 70% 50% (Blue)

### B. Typography

**Font Families:**
- Primary: Inter (via Google Fonts CDN) - clean, legible for data
- Monospace: 'JetBrains Mono' - for numeric displays and tables

**Type Scale:**
- Display (Admin Headers): text-4xl font-bold (36px)
- Headline (Section Titles): text-2xl font-semibold (24px)
- Body (Forms/Content): text-base font-normal (16px)
- Caption (Metadata): text-sm font-medium (14px)
- Micro (Labels): text-xs uppercase tracking-wide (12px)

### C. Layout System

**Spacing Primitives:** Tailwind units of 2, 4, 8, 12, 16, 20
- Micro spacing: p-2, gap-2 (form field groups)
- Standard spacing: p-4, gap-4 (cards, sections)
- Large spacing: p-8, gap-8 (page sections)
- Extra spacing: p-12, p-16 (dashboard layouts)

**Grid System:**
- Mobile (base): Single column, full-width cards
- Tablet (md:): 2-column grids for forms and summaries
- Desktop (lg:): 3-4 column dashboards, 2-column forms

**Container Strategy:**
- Employee Forms: max-w-2xl mx-auto (focused, mobile-optimized)
- Admin Dashboards: max-w-7xl mx-auto (wide for data density)
- Report Tables: w-full with horizontal scroll on mobile

### D. Component Library

**Navigation:**
- Top App Bar: Fixed header with role indicator, user avatar, logout
- Admin Sidebar: Persistent navigation for dashboard sections (hidden on mobile, drawer on md+)
- Bottom Navigation: Mobile-only for employees (Report, History, Profile)
- Breadcrumbs: Admin only for deep navigation context

**Forms (Employee Priority):**
- Material-style elevated cards with shadow-lg
- Floating labels for all text inputs
- Dropdown selects with search for market/employee names
- Number steppers with +/- buttons for samosa quantities
- Auto-calculation chips showing real-time totals
- Sticky footer with "Submit Report" CTA (primary button)
- Form validation with inline error messages (text-error)

**Data Display:**
- Cards: Elevated surface with rounded-xl corners, p-6 padding
- Tables: Alternating row colors, sticky headers, mobile-responsive collapse
- Stat Cards: Large number displays with trend indicators (↑↓)
- Progress Bars: Linear indicators for inventory levels (Material style)
- Chips/Badges: Rounded-full for samosa types, market names, status indicators

**Charts & Analytics:**
- Use Chart.js or Recharts library
- Bar Charts: Market comparison (vertical bars, color-coded by market)
- Pie Charts: Samosa distribution (8-slice with samosa type colors)
- Line Charts: Weekly/monthly trends (smooth curves, grid background)
- Chart cards: white/surface background with p-6, title + subtitle

**Interactive Elements:**
- Primary Button: bg-primary text-white rounded-lg h-12 px-6 (Material elevation on hover)
- Secondary Button: border-2 border-primary text-primary rounded-lg h-12 px-6
- Icon Buttons: w-10 h-10 rounded-full for actions (edit, delete, filter)
- FAB (Floating Action Button): Admin only - fixed bottom-right, rounded-full, shadow-2xl for "Add Inventory"

**Overlays:**
- Modals: Centered with backdrop-blur-sm, max-w-lg, slide-up animation
- Drawers: Slide from right (admin sidebar on mobile)
- Snackbars: Bottom toast notifications for success/error (Material style)
- Confirmation Dialogs: Elevated cards with two-action layout

### E. Animations

**Minimal, Purposeful Only:**
- Form submission: Button loading spinner (1s max)
- Data refresh: Skeleton loaders for tables/charts
- Navigation: 200ms ease-in-out transitions for drawer/modal
- Success states: Subtle check-mark fade-in on form submit
- No scroll animations, no decorative motion

---

## Role-Specific Design Patterns

**Employee View:**
- Clean, distraction-free form interface
- Large touch targets (min 44px) for field use
- Auto-save drafts to prevent data loss
- Minimal navigation - focus on daily report task
- Confirmation screen with summary before final submit

**Admin Dashboard:**
- Information-dense multi-column layouts
- Tabbed navigation for different report views
- Filter panels (collapsible on mobile)
- Export buttons (PDF, CSV) prominently placed
- Data visualization priority: charts above tables

---

## Accessibility & Responsiveness

- WCAG AA contrast ratios maintained (4.5:1 text, 3:1 UI)
- Keyboard navigation for all forms
- Screen reader labels for data tables and charts
- Touch-friendly 44px minimum tap targets
- Responsive breakpoints: 640px (sm), 768px (md), 1024px (lg), 1280px (xl)
- Dark mode toggle in user profile (persistent preference)

---

## Images

**No Hero Images Required** - This is a utility application focused on data entry and analysis, not marketing content. All visual interest comes from data visualization (charts, cards, structured layouts) rather than photography.