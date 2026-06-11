# Product Requirement Document (PRD)

## Project Name: MishtiJwelWorld V1
**Author:** Antigravity (AI Assistant)  
**Target Audience:** Customers (Web Front-end), Jewelry Store Admin (Android App)  
**Hosting Constraint:** 100% Free Tier Stack

---

## 1. Project Overview
MishtiJwelWorld V1 is an e-commerce platform designed for selling jewelry. The system consists of two primary applications sharing a common cloud-based database backend:
1. **Public Customer Web App**: A visually stunning, highly responsive website for customers to browse, search, and purchase jewelry.
2. **Admin Android App**: A secure native mobile application used by the business owner/staff to manage inventory, catalog (products, pricing, details), and process orders in real time.

---

## 2. Key Objectives & Scope
- **Customer Experience**: Offer a premium, smooth shopping experience with rich media (high-quality jewelry images), filters, and an easy checkout process.
- **Merchant Convenience**: Enable store management on-the-go via a dedicated Android App (no need for a desktop system).
- **Cost Effectiveness**: Utilize hosting platforms and backend services that offer robust free tiers so that running costs are zero (excluding domain name purchase).

---

## 3. Technology Stack Selection (Zero-Cost Hosting)

To meet the requirement of zero hosting costs, we will use the following production-grade free-tier services:

| Component | Technology / Platform | Why Selected | Free Tier Limits |
| :--- | :--- | :--- | :--- |
| **Frontend Web** | **Next.js** or **Vite (React)** | Outstanding performance, SEO optimization (Next.js), and easy components. | N/A (Runs on Client/Hosting) |
| **Web Hosting** | **Vercel** or **Netlify** | Auto-deployment from Git, fast CDN, custom domain support, SSL certificates. | **Vercel Hobby**: 100GB bandwidth/month, continuous deployment.<br>**Netlify**: 100GB bandwidth/month. |
| **Backend & DB** | **Firebase** (Firestore, Storage, Auth) | No-SQL real-time database, native SDKs for Web and Android, Firebase Cloud Messaging (FCM) for instant push notifications. | **Firestore**: 50k reads, 20k writes/day, 1GB storage.<br>**Auth**: Unlimited users.<br>**Storage**: 5GB files. |
| **Alternative DB** | **Supabase** | Relational PostgreSQL database, real-time subscriptions, secure auth. | **Supabase Free**: 500MB database, 1GB file storage, 50k monthly active users. |
| **Admin App** | **Android (Kotlin + Jetpack Compose)** | Native Android framework, fast UI development, seamless integration with Firebase. | 100% Free development |

### Recommended Stack Recommendation:
* **Frontend**: Next.js (React) + TailwindCSS (hosted on Vercel)
* **Backend/DB**: Firebase (Authentication, Cloud Firestore, Cloud Storage)
* **Admin App**: Native Android App (Kotlin & Jetpack Compose) using Firebase Android SDK

---

## 4. User Personas & Use Cases

### 4.1 The Online Customer (Web)
- **Browse Catalog**: View categories (e.g., Rings, Necklaces, Earrings, Bracelets).
- **Product Details**: Look at high-res images, read metal purity details (e.g., 22kt Gold, 18kt Gold, Diamonds), weight, dimensions, and price.
- **Search & Filter**: Filter by price range, metal type, category, or search by keywords.
- **Cart & Checkout**: Add items, adjust quantities, input delivery address, and complete orders (e.g., cash on delivery, or integrating a free payment gateway link/checkout).

### 4.2 The Store Admin (Android App)
- **Dashboard**: View daily sales, pending orders, and low-stock alerts.
- **Catalog Management**: Add new products, upload images from the phone's gallery/camera, edit descriptions, adjust prices, and toggle visibility.
- **Inventory Control**: Update stock quantities. Real-time updates push directly to the customer website.
- **Order Processing**: Receive instant push notifications on new orders, view customer details, and update order status (Pending -> Shipped -> Delivered).

---

## 5. Functional Requirements

### 5.1 E-Commerce Website (Next.js)
- **Responsive Layout**: Works perfectly on mobile, tablet, and desktop screens.
- **Visuals**: Premium jewelry-themed aesthetics (luxurious dark/light modes, gold/champagne accents).
- **Search Engine Optimization (SEO)**: Server-side rendering (SSR) for product pages so Google can index individual jewelry items.
- **Order Placement**: Cart persistence (Local Storage), form validation for shipping details, and a clear order confirmation page.

### 5.2 Admin Android Application
- **Admin Authentication**: Secure login to prevent unauthorized access.
- **Add/Edit Product Form**:
  - Fields: Name, Category, Metal Purity, Weight, Stock Quantity, Price, Description, Images.
  - Image Upload: Directly upload to Firebase Storage from camera or photo library.
- **Order Notification Service**: Background service utilizing Firebase Cloud Messaging (FCM) to trigger sound/vibration alerts when a customer places an order.
- **Status Updater**: Simple dropdown or toggle to change order status.

---

## 6. Database Schema (Firebase Firestore)

### `products` Collection
```json
{
  "id": "prod_101",
  "name": "22kt Gold Bridal Necklace",
  "description": "Exquisite handcrafted gold necklace with traditional design.",
  "category": "Necklaces",
  "price": 2500.00,
  "weight": "24.5g",
  "purity": "22kt",
  "images": [
    "https://firebasestorage.googleapis.com/.../necklace_1.jpg"
  ],
  "stock": 3,
  "createdAt": "2026-06-12T00:00:00Z"
}
```

### `orders` Collection
```json
{
  "id": "ord_999",
  "customerId": "guest_user_123",
  "customerName": "John Doe",
  "shippingAddress": "123 Main St, New York, NY 10001",
  "phone": "+1 555-0199",
  "items": [
    {
      "productId": "prod_101",
      "name": "22kt Gold Bridal Necklace",
      "quantity": 1,
      "price": 2500.00
    }
  ],
  "totalAmount": 2500.00,
  "status": "Pending", // Pending, Shipped, Delivered, Cancelled
  "createdAt": "2026-06-12T00:15:30Z"
}
```

---

## 7. Verification & Testing Strategy

### Phase 1: Local Development
- Run Next.js locally and simulate adding/updating products in Firebase Console to check if the website displays updates instantly.
- Test Android App on Emulator / Physical device to add a new product and ensure the image uploads successfully to Cloud Storage.

### Phase 2: Integration Testing
- Place a test order on the local website.
- Verify that a push notification is delivered to the Android App.
- Open the Android App, check order details, change status to "Shipped", and confirm the website reflects the status update.

### Phase 3: Free Deployment
- Deploy the frontend to Vercel/Netlify.
- Attach the custom domain.
- Verify SSL certificate activation and production performance.
