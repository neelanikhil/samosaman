# SamosaMan - Technical Documentation

## Overview
SamosaMan is a comprehensive inventory and sales management system for a samosa business with role-based access for Admin and Employee users.

---

## JSON Data Structures

### 1. User Authentication
```json
{
  "id": "uuid-string",
  "username": "john_smith",
  "password": "hashed_password",
  "role": "employee" // or "admin"
}
```

### 2. Inventory Setup
```json
{
  "id": "uuid-string",
  "weekOf": "2025-01-15T00:00:00Z",
  "samosaType": "SCH", // Spicy Chicken
  "quantity": 500
}
```

**Complete Inventory Record Example:**
```json
{
  "weekOf": "2025-01-15",
  "inventory": {
    "APP": 225,  // Apple
    "VEG": 300,  // Veggie
    "PJB": 400,  // Punjabi
    "VSP": 200,  // Vermont Spicy Potato
    "SCH": 500,  // Spicy Chicken
    "CHC": 500,  // Chicken & Cheese
    "STC": 500,  // Steak & Cheese
    "STP": 250,  // Steak & Potato
    "CCR": 300,  // Chicken Curry
    "CHP": 250   // Chickpea
  }
}
```

### 3. Daily Report (Draft - Start Report)
```json
{
  "id": "uuid-string",
  "marketName": "Davis Sq FM",
  "employeeName": "John Smith",
  "date": "2025-01-15T00:00:00Z",
  "status": "draft",
  "takenData": {
    "APP": 50,
    "VEG": 60,
    "PJB": 70,
    "VSP": 40,
    "SCH": 80,
    "CHC": 80,
    "STC": 80,
    "STP": 45,
    "CCR": 50,
    "CHP": 55
  }
}
```

### 4. Daily Report (Submitted - Update Report)
```json
{
  "id": "uuid-string",
  "marketName": "Davis Sq FM",
  "employeeName": "John Smith",
  "date": "2025-01-15T00:00:00Z",
  "status": "submitted",
  "takenData": {
    "APP": 50,
    "VEG": 60,
    "PJB": 70,
    "VSP": 40,
    "SCH": 80,
    "CHC": 80,
    "STC": 80,
    "STP": 45,
    "CCR": 50,
    "CHP": 55
  },
  "samosaData": {
    "APP": { "rnf": 5, "rf": 3, "giftWasteEat": 2, "sold": 40 },
    "VEG": { "rnf": 8, "rf": 4, "giftWasteEat": 3, "sold": 45 },
    "PJB": { "rnf": 6, "rf": 5, "giftWasteEat": 4, "sold": 55 },
    "VSP": { "rnf": 4, "rf": 2, "giftWasteEat": 1, "sold": 33 },
    "SCH": { "rnf": 10, "rf": 6, "giftWasteEat": 4, "sold": 60 },
    "CHC": { "rnf": 8, "rf": 5, "giftWasteEat": 3, "sold": 64 },
    "STC": { "rnf": 9, "rf": 6, "giftWasteEat": 5, "sold": 60 },
    "STP": { "rnf": 5, "rf": 3, "giftWasteEat": 2, "sold": 35 },
    "CCR": { "rnf": 6, "rf": 4, "giftWasteEat": 3, "sold": 37 },
    "CHP": { "rnf": 7, "rf": 4, "giftWasteEat": 2, "sold": 42 }
  },
  "meals": 15,
  "cashReceived": 1850.50,
  "tipsOnline": 45.00,
  "tipsOffline": 32.00,
  "createdAt": "2025-01-15T08:30:00Z",
  "updatedAt": "2025-01-15T16:45:00Z"
}
```

### 5. Calculated Totals (Auto-generated)
```json
{
  "samosasSold": 471,
  "samosaRevenue": 1884,  // samosasSold × $4
  "mealRevenue": 195,      // meals × $13
  "totalRevenue": 2079,    // samosaRevenue + mealRevenue
  "totalTips": 77,         // tipsOnline + tipsOffline
  "totalCash": 1850.50
}
```

---

## Firebase/Supabase Schema

### Collections/Tables

#### 1. `users`
```
- id (UUID, Primary Key)
- username (String, Unique)
- password (String, Hashed)
- role (Enum: 'admin' | 'employee')
- createdAt (Timestamp)
```

#### 2. `inventory`
```
- id (UUID, Primary Key)
- weekOf (Date)
- samosaType (String)
- quantity (Integer)
- createdAt (Timestamp)
- updatedAt (Timestamp)

Indexes:
- weekOf + samosaType (Composite, Unique)
```

#### 3. `daily_reports`
```
- id (UUID, Primary Key)
- marketName (String)
- employeeName (String)
- date (Date)
- status (Enum: 'draft' | 'submitted')
- takenData (JSON)
- samosaData (JSON, nullable)
- meals (Integer, default: 0)
- cashReceived (Decimal)
- tipsOnline (Decimal)
- tipsOffline (Decimal)
- createdAt (Timestamp)
- updatedAt (Timestamp)

Indexes:
- date (for quick date filtering)
- marketName (for market-wise queries)
- status (for filtering drafts/submitted)
- createdAt (for sorting)
```

#### 4. `analytics_cache` (Optional - for performance)
```
- id (UUID, Primary Key)
- type (Enum: 'weekly' | 'monthly')
- startDate (Date)
- endDate (Date)
- data (JSON)
- createdAt (Timestamp)
- expiresAt (Timestamp)

Indexes:
- type + startDate (Composite)
```

---

## Inventory Deduction Logic

### Formula
```
inventory_deducted = taken - rnf - rf - giftWasteEat - sold
```

### Example Calculation
```javascript
Initial Inventory: APP = 225

Employee takes: 50
Returns Not Fried (RNF): 5
Returns Fried (RF): 3
Gift/Waste/Eat: 2
Sold: 40

Deduction = 50 - 5 - 3 - 2 - 40 = 0
// This means everything taken was accounted for
// No "lost" samosas

Remaining Inventory: 225 - 50 = 175
```

---

## Optimization Recommendations

### 1. **Database Optimization**

#### Use Indexing
- Index `date`, `marketName`, `status` in `daily_reports`
- Composite index on `weekOf + samosaType` in `inventory`
- Index `createdAt` for time-based queries

#### Implement Caching
```javascript
// Cache weekly/monthly aggregations
const cacheKey = `analytics:weekly:${weekStart}`;
const cachedData = await redis.get(cacheKey);

if (cachedData) {
  return cachedData;
}

const freshData = await calculateWeeklyAnalytics();
await redis.set(cacheKey, freshData, 'EX', 3600); // 1 hour
```

### 2. **Frontend Optimization**

#### Implement Pagination
```javascript
// For reports table
const pageSize = 20;
const reports = await db.reports
  .where('date', '==', selectedDate)
  .limit(pageSize)
  .offset(page * pageSize);
```

#### Use React Query for Data Fetching
```javascript
// Already implemented in the app
const { data, isLoading } = useQuery({
  queryKey: ['/api/reports', selectedDate],
  staleTime: 5 * 60 * 1000, // 5 minutes
});
```

#### Lazy Load Charts
```javascript
import { lazy, Suspense } from 'react';

const SalesChart = lazy(() => import('./SalesChart'));

<Suspense fallback={<Skeleton />}>
  <SalesChart data={data} />
</Suspense>
```

### 3. **Backend Optimization**

#### Batch Inventory Updates
```javascript
// Instead of individual updates
const batchUpdate = db.batch();
Object.entries(inventory).forEach(([type, qty]) => {
  const ref = db.collection('inventory').doc(type);
  batchUpdate.set(ref, { quantity: qty, weekOf: date }, { merge: true });
});
await batchUpdate.commit();
```

#### Aggregate Queries
```javascript
// Pre-calculate daily totals
const dailyTotals = await db.collection('daily_reports')
  .where('date', '>=', weekStart)
  .where('date', '<=', weekEnd)
  .get()
  .then(snapshot => {
    return snapshot.docs.reduce((acc, doc) => {
      const data = doc.data();
      acc.totalRevenue += calculateRevenue(data);
      acc.totalSold += calculateSold(data);
      return acc;
    }, { totalRevenue: 0, totalSold: 0 });
  });
```

### 4. **Email Notification System**

#### Implement SendGrid/AWS SES
```javascript
// When employee submits report
const sendReportEmail = async (reportData) => {
  const emailContent = `
    New Report Submitted
    Market: ${reportData.marketName}
    Employee: ${reportData.employeeName}
    Date: ${reportData.date}
    Revenue: $${reportData.totalRevenue}
  `;
  
  await emailService.send({
    to: 'admin@samosaman.com',
    subject: `New Report - ${reportData.marketName}`,
    html: emailContent
  });
};
```

### 5. **Progressive Web App (PWA)**

#### Add Offline Support
```javascript
// service-worker.js
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

#### Enable "Add to Home Screen"
```json
// manifest.json
{
  "name": "SamosaMan",
  "short_name": "SamosaMan",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#FAF8F4",
  "theme_color": "#F39C12",
  "icons": [
    {
      "src": "/icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ]
}
```

### 6. **Security Enhancements**

#### JWT Authentication
```javascript
// Generate token on login
const token = jwt.sign(
  { userId, role },
  process.env.JWT_SECRET,
  { expiresIn: '24h' }
);
```

#### Role-Based Middleware
```javascript
const requireAdmin = (req, res, next) => {
  if (req.user.role !== 'admin') {
    return res.status(403).json({ error: 'Admin access required' });
  }
  next();
};

app.post('/api/inventory/add', requireAdmin, addInventory);
```

### 7. **Analytics & Monitoring**

#### Track Key Metrics
- Report submission time
- Inventory turnover rate
- Revenue per market
- Employee performance
- System errors and response times

#### Implement Error Tracking
```javascript
// Sentry or similar
Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
});
```

---

## API Endpoints Structure

### Employee Routes
```
POST   /api/auth/login
POST   /api/reports/start        // Create draft report
PUT    /api/reports/:id/update   // Update and submit report
GET    /api/reports/my-drafts    // Get employee's draft reports
```

### Admin Routes
```
POST   /api/inventory/add        // Add to existing inventory
GET    /api/inventory/current    // Get current inventory levels
GET    /api/reports              // Get all reports (with filters)
GET    /api/analytics/weekly     // Weekly analytics
GET    /api/analytics/monthly    // Monthly analytics
GET    /api/analytics/top-seller // Top selling samosa
GET    /api/analytics/top-market // Top performing market
```

---

## Mobile Optimization

### Responsive Design
- Mobile-first approach (already implemented with Tailwind)
- Touch-friendly buttons (min 44px tap targets)
- Optimized forms for mobile input
- Swipe gestures for navigation

### Performance
- Reduce image sizes
- Minimize bundle size
- Lazy load non-critical components
- Use service workers for offline support

---

## Future Enhancements

1. **Real-time Updates**: WebSocket for live dashboard
2. **Export Reports**: PDF/Excel export functionality
3. **Multi-language Support**: i18n implementation
4. **Voice Input**: For hands-free reporting at markets
5. **Barcode Scanning**: Quick inventory counting
6. **Automated Reordering**: Alert when inventory is low
7. **Employee Scheduling**: Assign employees to markets
8. **Customer Feedback**: Collect ratings per market
9. **Financial Integration**: QuickBooks/Xero sync
10. **Predictive Analytics**: ML-based demand forecasting
