# SamosaMan - Inventory & Sales Management System

## Overview

SamosaMan is a comprehensive inventory and sales management system designed for a samosa business. It provides role-based access for Admin and Employee users to efficiently track inventory, manage daily sales reports, and analyze business performance across multiple market locations. The system handles 8 distinct samosa types and additional items, offering tools for inventory management, sales reporting, and market analysis. Its primary purpose is to streamline operations and provide valuable business insights.

## Recent Changes

- **Low Stock Alert Thresholds (Oct 2025)**: Configured item-specific minimum cooler stock thresholds for low stock email alerts. CCM and CPM set to 4 units minimum, samosa types range from 150-300 units based on popularity. System monitors cooler stock levels and sends automated alerts to admin when stock falls below configured thresholds.

- **Cooler Stock Deduction System (Oct 2025)**: Daily report submissions now deduct inventory from cooler_stock instead of initialQuantity. When employees submit reports, the system calculates total deduction (sold + gift + waste + eat) and deducts from cooler stock with validation to prevent negative values. Low stock alerts monitor cooler levels with item-specific thresholds. Complete flow: Admin adds to refrigerator → Transfer to cooler → Employee takes from cooler for market → Submit report deducts from cooler.

- **Refrigerator-to-Cooler Stock Flow (Oct 2025)**: Implemented automated stock transfer system where new inventory additions go directly to refrigerator storage. Both admin and employees can transfer stock from refrigerator to cooler. Admin sets target cooler levels (system calculates and transfers needed amount from refrigerator). Employees specify transfer amounts (system validates against refrigerator availability and adds to current cooler stock). Backend validates all transfers to prevent overdrawing refrigerator stock. UI labels clearly distinguish between setting target amounts (admin) and transferring amounts (employee). All stock operations maintain decimal precision (NUMERIC 10,2).

- **Cooler & Refrigerator Stock Management (Oct 2025)**: Implemented separate stock tracking for cooler and refrigerator inventory. Database schema updated with cooler_stock and refrigerator_stock columns (NUMERIC 10,2). Admin can manage both stock types independently through dedicated Stock Management tab. Employees can transfer stock from refrigerator to cooler through Stock tab. Automated low cooler stock email alerts sent to admin via Resend with item-specific thresholds.

- **Fractional Inventory Support (Oct 2025)**: Enhanced inventory system to support decimal/fractional quantities for CCM and CPM items. Database schema updated to use NUMERIC(10,2) for all inventory fields. Admin can add fractional quantities (step=0.25), employees can select fractions (¼, ½, ¾) in daily reports, and inventory deduction calculates precisely: used = taken - returned. UI displays decimals with proper formatting (up to 2 decimal places).

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

The frontend is built with React 18+ and TypeScript, utilizing Vite for development and bundling. UI components are developed using `shadcn/ui` based on Radix UI primitives, styled with Tailwind CSS following Material Design principles, and managed with Class Variance Authority (CVA). State management and data fetching are handled by TanStack Query for server state and React's `useState` for UI state. The design system includes light/dark theme support, a custom color palette (Deep Teal primary, Warm Amber secondary), and specific typography (Inter for UI, JetBrains Mono for numeric displays).

### Backend Architecture

The backend uses Express.js to provide REST API endpoints, with middleware for JSON parsing and URL encoding. Data is persistently stored in a PostgreSQL database (Neon serverless driver) and managed using Drizzle ORM for type-safe operations. The system includes JWT-based authentication with role verification, bcrypt password hashing, and an employee approval workflow. API design includes endpoints for report management, analytics, inventory, and markets, with structured error handling.

### Data Models

- **Users**: Role-based access ("admin" or "employee") with username/password authentication and secure hashing.
- **Inventory**: Tracks 10 items (8 samosa types + CCM + CPM) with decimal precision (NUMERIC 10,2) to support fractional quantities. CCM and CPM support fractional inputs (¼, ½, ¾) for curry portions. Quantities automatically deducted upon report submission using formula: used = taken - returned.
- **Daily Reports**: Captures detailed sales data including market, employee, date, shift times, samosa quantities (taken, RNF, RF, gift, waste, eat), additional items (CCM, CPM, RICE, SSS with taken/return supporting fractional values), financial data (cash, mobile, Venmo sales, expenses, tips), and an optional note field. Reports have a 'draft' or 'submitted' status. Auto-calculates total work hours from shift start/end times. Database fields use snake_case.

### Technical Implementations

- **Database Migration**: Full migration to PostgreSQL using Drizzle ORM with auto-creation of default users.
- **Role-Based Authentication**: JWT-based authentication with 7-day expiration, bcrypt hashing, and admin-controlled employee approval workflow.
- **Email Notifications**: Integration with SendGrid for welcome, approval, low inventory, and report submission/draft saved notifications, supporting dynamic templates with fallback to HTML.
- **Draft Report Editing**: Employees can edit saved draft reports; submitted reports are read-only.
- **Profile Management**: Users can update name, username, email, and change passwords with validation.
- **Admin Dashboard**: Features a date range picker for analytics, displaying total revenue, samosas sold, top market, and visual charts (pie chart for samosa distribution, bar chart for revenue by market).
- **Inventory History & Tracking**: Comprehensive inventory tracking with date/day logging, history storage, and filtering by date range and samosa type.
- **Weekly Employee Work Hours**: Automatically calculates employee work hours from shift times (handles overnight shifts), stores in total_hours field (decimal 5,2), and displays weekly summary in dedicated Admin portal tab showing employee name, total markets worked, and total hours for current week (Monday-Sunday). Only includes submitted reports.

## External Dependencies

### Core Runtime Dependencies

- **Database & ORM**: `drizzle-orm`, `@neondatabase/serverless`, `drizzle-zod`.
- **UI Framework & Components**: React, `@radix-ui/*`, `recharts`, `lucide-react`, `cmdk`.
- **Forms & Validation**: `react-hook-form`, `@hookform/resolvers`, `zod`.
- **Styling & Utilities**: `tailwindcss`, `tailwind-merge`, `clsx`, `class-variance-authority`, `date-fns`.
- **State Management**: `@tanstack/react-query`.
- **Development & Build Tools**: `vite`, `@vitejs/plugin-react`, `typescript`, `esbuild`, `postcss`, `autoprefixer`.

### Third-Party Services & APIs

- **Database Infrastructure**: Neon Serverless PostgreSQL for production data hosting.
- **Email Service**: SendGrid for transactional email notifications.

### Font Resources

- Google Fonts CDN: Inter, DM Sans, Fira Code / Geist Mono, Architects Daughter.