# Product Requirement Document (PRD)

## Project Name: MishtiJwelWorld V1
**Author:** Antigravity (AI Assistant)  
**Target Audience:** Customers (Web Front-end), Jewelry Store Admin (Android App)  
**Hosting Constraint:** 100% Free Tier Stack (No Credit Card Required)
**Architecture Style:** WhatsApp-First Conversational Commerce

---

## 1. Project Overview
MishtiJwelWorld V1 is an e-commerce platform designed for selling jewelry with a streamlined, zero-cost communication layer. The system consists of two primary applications sharing a common cloud-based database backend:
1. **Public Customer Web App**: A visually stunning website for customers to browse jewelry, manage their cart, and initiate checkout via WhatsApp.
2. **Admin Android App**: A secure native mobile application used by the business owner to manage inventory, verify payments (via generated UPI QR codes), and trigger WhatsApp status updates for orders.

---

## 2. Key Objectives & Scope
- **Customer Experience**: Offer a premium shopping experience with high-quality jewelry images, followed by a frictionless WhatsApp-based checkout.
- **Merchant Convenience**: Manage everything on-the-go via a dedicated Android App. Generate payment links/QRs and send updates instantly.
- **Cost Effectiveness**: Utilize hosting platforms and backend services with robust free tiers that **do not require credit card details**.

---

## 3. Technology Stack Selection (Zero-Cost Hosting)

| Component | Technology / Platform | Why Selected | Free Tier Limits |
| :--- | :--- | :--- | :--- |
| **Frontend Web** | **Next.js** or **Vite (React)** | Outstanding performance, SEO optimization (Next.js), and easy components. | N/A (Runs on Client/Hosting) |
| **Web Hosting** | **Vercel** or **Netlify** | Auto-deployment from Git, fast CDN, custom domain support, SSL certificates. | **Vercel Hobby**: 100GB bandwidth/month (No CC required). |
| **Backend & DB** | **Supabase** (PostgreSQL Database & Storage) | Open-source Firebase alternative. Excellent relational database, native SDKs for Web and Android. | **Supabase Free**: 500MB database, 1GB file storage, 50k monthly active users. **(No CC required)** |
| **Admin App** | **Android (Kotlin + Jetpack Compose)** | Native Android framework, fast UI development, seamless integration with Supabase. | 100% Free development |

### Recommended Stack Recommendation:
* **Frontend**: Next.js (React) + TailwindCSS (hosted on Vercel)
* **Backend/DB**: Supabase (PostgreSQL, Supabase Storage)
* **Admin App**: Native Android App (Kotlin & Jetpack Compose)

---

## 4. User Personas & Use Cases

### 4.1 The Online Customer (Web)
- **Browse Catalog**: View categories (e.g., Rings, Necklaces, Earrings, Bracelets).
- **Cart Management**: Add items, adjust quantities, input delivery address.
- **WhatsApp Checkout**: 
  - Instead of a traditional payment gateway, clicking "Checkout" saves the order to Supabase and generates a unique `Order ID`.
  - The customer is redirected to WhatsApp with a pre-filled message (e.g., *"Hello! I would like to place an order. My Order ID is ORD-102. Total: ₹2500."*).

### 4.2 The Store Admin (Android App)
- **Catalog Management**: Add new products, upload images, adjust prices, and toggle visibility.
- **Order Synchronization**: When the customer sends the WhatsApp message, the admin checks the `Order ID` in the App (which synced instantly via Supabase).
- **Payment Collection**: The Admin App generates a UPI Payment QR Code for the exact order amount. The admin clicks "Share to WhatsApp" to send this QR to the customer.
- **Status Updates**: After verifying the UPI payment, the admin updates the status to "Shipped". A "Notify Customer" button opens WhatsApp pre-filled with the shipping update to send to the customer.

---

## 5. Functional Requirements

### 5.1 E-Commerce Website
- **Responsive Layout**: Works perfectly on mobile, tablet, and desktop screens.
- **Visuals**: Premium jewelry-themed aesthetics (luxurious dark/light modes).
- **Order Processing Logic**: 
  - Save cart data to Supabase `orders` table.
  - Generate `wa.me/<number>?text=...` URI.
  - Redirect user to WhatsApp Web or App.

### 5.2 Admin Android Application
- **Add/Edit Product Form**: Fields for Name, Category, Metal Purity, Price, Description, Images. Uploads images directly to Supabase Storage.
- **Order Dashboard**: List of orders synced from Supabase.
- **UPI QR Generator**: Converts standard UPI strings (`upi://pay?pa=...&am=...`) into shareable QR images.
- **WhatsApp Intent Integrations**: Native Android sharing intents to send QR codes and pre-filled text updates without needing paid APIs.

---

## 6. Database Schema (Supabase PostgreSQL)

### `products` Table
```sql
CREATE TABLE products (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  name TEXT NOT NULL,
  category TEXT,
  price DECIMAL(10, 2) NOT NULL,
  stock INTEGER DEFAULT 0,
  images TEXT[] DEFAULT '{}',
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### `orders` Table
```sql
CREATE TABLE orders (
  id UUID DEFAULT uuid_generate_v4() PRIMARY KEY,
  order_number TEXT UNIQUE NOT NULL, -- e.g., 'ORD-102'
  customer_name TEXT,
  shipping_address TEXT,
  items JSONB NOT NULL, -- Array of product IDs, quantities, prices
  total_amount DECIMAL(10, 2) NOT NULL,
  status TEXT DEFAULT 'Pending', -- Pending, Paid, Shipped, Delivered
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

---

## 7. Verification & Testing Strategy

- **Checkout Flow Testing**: Build a dummy cart on the local web app, click checkout, ensure the order appears in Supabase, and verify the WhatsApp link formatting.
- **QR Code Generation**: Test the Android App's ability to generate valid UPI QRs that can be scanned by standard payment apps (GPay, PhonePe).
- **Intent Sharing**: Ensure the Android App successfully opens WhatsApp to share the QR code image and text status updates.
