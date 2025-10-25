# 🍽️ SamosaMan - Inventory & Sales Management System

A comprehensive role-based management system for tracking samosa sales, inventory, and market analytics.

## ✨ Features

### 🔐 Login Screen
- Clean, professional design with SamosaMan branding
- Two-button login system: Admin and Employee
- Soft beige background (#FAF8F4)
- Color-coded buttons:
  - Admin: Muted red (#C0392B)
  - Employee: Muted yellow (#F39C12)

### 👨‍💼 Admin Dashboard

#### Dashboard Tab
- **Total Revenue Card** - Toggle between Weekly/Monthly view
- **Total Sold** - Dynamic display based on selected period
- **Top Seller of the Week** - Highlights best-selling samosa type
- **Top Market of the Week** - Shows highest revenue market

#### Charts & Analytics
- **Sales by Samosa Type** - Pie chart with Weekly/Monthly toggle
- **Sales by Market** - Bar chart with Weekly/Monthly toggle
- All charts are interactive and responsive

#### Add Inventory Tab
- Add quantities to existing inventory for all 10 samosa types:
  - APP (Apple)
  - VEG (Veggie)
  - PJB (Punjabi)
  - VSP (Vermont Spicy Potato)
  - SCH (Spicy Chicken)
  - CHC (Chicken & Cheese)
  - STC (Steak & Cheese)
  - STP (Steak & Potato)
  - CCR (Chicken Curry)
  - CHP (Chickpea)
- Real-time total calculation

#### Reports Tab
- Calendar picker to filter reports by date
- Table display showing:
  - Date
  - Market
  - Employee
  - View Report button
- Easy report access and review

### 👷 Employee Portal

#### Two-Step Reporting Flow

**Step 1: Start Report**
- Select market from dropdown or enter custom market
- Enter employee name
- Auto-populated date
- Input "Taken" quantities for each samosa type
- Save as draft

**Step 2: Update Report**
- View saved draft details (market, employee, date, taken quantities)
- Enter for each samosa:
  - RNF (Return Not Fried)
  - RF (Return Fried)
  - Gift/Waste/Eat
  - Sold
- Enter:
  - Meals sold
  - Cash received
  - Tips (online and offline)
- Auto-calculated totals:
  - Total samosas sold
  - Samosa revenue ($4 each)
  - Meal revenue ($13 each)
  - Total revenue
- Submit report (sends email to admin)

## 🎨 Design Features

- **Responsive Design** - Works on desktop, tablet, and mobile
- **Dark Mode Support** - Toggle between light and dark themes
- **Clean Typography** - Inter font family for professional look
- **Material Design Cards** - Elevated surfaces with shadows
- **Color-Coded Data** - Easy visual distinction
- **Interactive Charts** - Recharts library for data visualization

## 🏗️ Technical Stack

### Frontend
- React 18 with TypeScript
- Tailwind CSS for styling
- Shadcn UI components
- Recharts for analytics
- Wouter for routing
- TanStack Query for data fetching

### Backend (Ready to Implement)
- Express.js REST API
- PostgreSQL or Firebase/Supabase
- JWT Authentication
- Email notifications (SendGrid/AWS SES)

## 📊 Data Models

### Samosa Types
```
APP: Apple
VEG: Veggie
PJB: Punjabi
VSP: Vermont Spicy Potato
SCH: Spicy Chicken
CHC: Chicken & Cheese
STC: Steak & Cheese
STP: Steak & Potato
CCR: Chicken Curry
CHP: Chickpea
```

### Pricing
- Samosa: $4 each
- Meal: $13 each

### Inventory Deduction Formula
```
deducted = taken - rnf - rf - giftWasteEat - sold
remaining_inventory = initial - deducted
```

## 🚀 Getting Started

1. **Install Dependencies**
   ```bash
   npm install
   ```

2. **Run Development Server**
   ```bash
   npm run dev
   ```

3. **Access Application**
   - Open browser to `http://localhost:5000`
   - Login as Admin or Employee

## 📱 Mobile Optimization

- Touch-friendly interface
- Optimized forms for field data entry
- Responsive charts and tables
- Mobile-first design approach
- PWA ready (can be installed on mobile devices)

## 🔒 Security Features

- Role-based access control
- Secure authentication
- Password hashing
- JWT tokens
- Protected API routes

## 📈 Analytics & Reporting

### Weekly View
- Revenue trends
- Sales by samosa type
- Market-wise comparison
- Top performers

### Monthly View
- Long-term revenue analysis
- Seasonal trends
- Inventory planning insights
- Performance metrics

## 🌟 Future Enhancements

- Real-time dashboard updates
- PDF/Excel report export
- Multi-language support
- Voice input for reporting
- Barcode inventory scanning
- Automated low-stock alerts
- Employee scheduling
- Customer feedback integration
- Financial software integration
- Predictive analytics with ML

## 📄 Documentation

See [DOCUMENTATION.md](./DOCUMENTATION.md) for:
- Complete JSON data structures
- Firebase/Supabase schema
- API endpoints
- Optimization recommendations
- Security best practices

## 🎯 Key Improvements from Original

### Removed
- ❌ Non-inventory items (CLM, CPM, RICE, SSS)
- ❌ Shift start time (simplified flow)
- ❌ Mobile/cash payment tracking (simplified to just cash)
- ❌ Total Waste display
- ❌ Daily Best Seller (kept weekly only)

### Added
- ✅ Two new samosa types (Chicken Curry, Chickpea)
- ✅ Two-step employee workflow (Start → Update)
- ✅ Custom market input option
- ✅ Weekly/Monthly toggle for all analytics
- ✅ Top Seller and Top Market cards
- ✅ Calendar-based report filtering
- ✅ Email notification on report submission
- ✅ Clean, professional login screen
- ✅ Improved UX with clear workflows

## 📞 Support

For questions or support, contact: admin@samosaman.com

---

© 2025 SamosaMan, Inc. All rights reserved.
