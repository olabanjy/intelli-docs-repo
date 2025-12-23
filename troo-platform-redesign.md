

# **Troo Platform Architecture & Software Design Overview**

## 1. Executive Summary (For Business & Product)

Troo is designed as a **shared commerce platform** that powers multiple products — **TrooX** (in-house dining & hospitality) and **Gogrub** (online delivery & storefronts).

Instead of building separate systems for each product, Troo uses:

* **One shared core platform** for businesses, branches, staff, menus, and permissions
* **A shared order engine** with **product-specific flows**
* **Independent but connected services** for scalability and long-term growth

This approach:

* Reduces duplicated development
* Keeps operational rules consistent
* Allows each product to evolve independently
* Supports future expansion without re-architecture

---

## 2. The Two Products at a Glance

### **TrooX**

Used by **hotels and restaurants for in-house dining**.

Key characteristics:

* Customers are **physically present**
* Orders are created via **QR codes** (rooms, tables, kiosks)
* Orders flow to **Kitchen Display System (KDS)** and **Order Display System (ODS)**
* Fulfillment is internal (room service, table service, pickup)
* Payment rules vary by business (pay first, pay later, room charge)

### **Gogrub**

Used for **online ordering and delivery**.

Key characteristics:

* Customers order **from home**
* Each merchant has a **public storefront URL**
* Delivery address and fees are mandatory
* Orders are fulfilled via delivery logistics
* No physical QR assets involved

---

## 3. Shared Platform Philosophy

Although TrooX and Gogrub feel different to users, **they share the same business foundations**:

### Shared Concepts

* Businesses & branches
* Staff, roles, and permissions
* Menu structure and pricing
* Orders (tickets)
* Payments
* Fulfillment logic

### Key Design Decision

> **Orders are fundamentally the same, but how they are initiated and fulfilled differs by product.**

This is solved by:

* One **Order Core**
* Multiple **Order Flows**

---

## 4. High-Level System Architecture

### Core Services Overview

| Layer                          | Responsibility                                                   |
| ------------------------------ | ---------------------------------------------------------------- |
| **Troo Core (Django)**         | Identity, businesses, branches, staff, permissions, assets       |
| **Orders Service (FastAPI)**   | Menu, orders, fulfillment, pricing, product-specific order flows |
| **Payment Service (Existing)** | Payments, verification, provider webhooks                        |
| **Realtime Layer**             | KDS / ODS live updates                                           |
| **Integrations Layer**         | POS sync, printers, future notifications                         |

---

## 5. Troo Core Platform (Shared Across All Products)

### Purpose

Troo Core is the **source of truth for organizational structure and access control**.

### What It Manages

* Businesses / organizations
* Branches
* Staff members
* Roles & permissions (per branch)
* Assets (QR codes for rooms, tables, kiosks)
* Merchant storefront identifiers (for Gogrub)
* Business and branch operational settings

### Why This Matters (Business View)

* One place to manage staff and access
* Flexible permissions per branch
* Supports hotels, restaurants, and multi-location businesses
* Easy administration via backend dashboards

---

## 6. Menu System (Shared Across Products)

The menu system is **universal** across TrooX and Gogrub.

### Menu Capabilities

* Categories and subcategories
* Menu items
* Variants (e.g. sizes)
* Modifier groups (e.g. toppings, add-ons)
* Individual modifiers
* Tags (e.g. spicy, vegan)
* Branch-level availability and pricing

### Why Shared Menus Matter

* Same kitchen, same items
* Reduces configuration duplication
* Pricing and modifiers remain consistent
* Supports both dine-in and delivery menus with rules

---

## 7. Orders & Tickets – One Core, Two Experiences

### Order Core (Shared)

Every order includes:

* Items, variants, modifiers
* Pricing totals
* Status and timestamps
* Payment status
* Business and branch context

### Product-Specific Order Flows

#### **TrooX Order Flow**

* Order starts from a **QR asset** (room/table/kiosk)
* Customer sees branch-specific menu
* Order is created and routed to kitchen
* Appears on KDS and optionally ODS
* Fulfillment is internal (room delivery, table service, pickup)

#### **Gogrub Order Flow**

* Order starts from a **merchant storefront URL**
* Customer selects delivery or pickup
* Delivery address and fees are required
* Order goes to vendor kitchen
* Fulfillment is external delivery

### Key Insight

> **Same order engine, different orchestration rules.**

---

## 8. Assets Explained (Critical for TrooX)

Assets represent **physical or logical ordering points**.

Examples:

* Hotel room QR code
* Restaurant table QR code
* Self-service kiosk
* Kitchen screen
* Order display screen (ODS)

Assets:

* Are linked to branches
* Control which menus are visible
* Define order context (room number, table number)

This allows TrooX to operate without customers needing accounts.

---

## 9. Payments (Already Existing Service)

Troo already has a **standalone payment service**, which remains independent.

### Payment Flow Summary

1. Orders Service initiates payment
2. Payment Service handles provider interaction
3. Payment Service receives provider webhooks
4. Payment Service confirms payment to Orders
5. Orders updates order state and triggers fulfillment

### Why This Is Good

* Payments remain isolated and secure
* Orders don’t need to understand payment providers
* Supports multiple payment methods
* Easy reconciliation and retries

---

## 10. Payment Policy as a Business Setting

Different businesses operate differently.
Therefore, **payment behavior is configurable**.

### Supported Payment Policies

* **Pay First** – kitchen starts only after payment
* **Pay at Pickup** – kitchen starts immediately, but release is blocked until payment
* **Postpaid / Room Charge** (optional)

### Where It Lives

* Configured at **business level**
* Can be overridden per **branch**

### Why This Matters

* Hotels ≠ fast food ≠ fine dining
* Supports real operational realities
* No hardcoded rules

---

## 11. Kitchen & Order Displays (TrooX)

### KDS (Kitchen Display System)

* Shows active orders to kitchen staff
* Orders update in real time
* Staff can update order status

### ODS (Order Display System)

* Public-facing order readiness screen
* Shows “Now Serving” or “Ready” orders

### How It Works

* Orders emit events when created or updated
* WebSockets push updates instantly to screens
* No polling, no refresh delays

---

## 12. Fulfillment Model

Each order has a fulfillment record:

### Fulfillment Types

* Table service
* Room delivery
* Customer pickup
* Third-party delivery

This allows:

* TrooX to manage internal delivery
* Gogrub to manage external logistics
* Future integrations with riders or partners

---

## 13. Communication Between Services

| Purpose                                         | Technology                 |
| ----------------------------------------------- | -------------------------- |
| Immediate actions (create order, update status) | HTTP APIs                  |
| Real-time screen updates                        | WebSockets                 |
| Side effects (printing, POS sync)               | Background events/messages |

This ensures:

* Fast user experience
* Reliable retries
* No single point of failure

---

## 14. Why This Architecture Is Good for the Business

### Business Benefits

* Faster product launches
* Lower long-term maintenance cost
* Consistent behavior across products
* Easier onboarding of new business types
* Supports scale without rebuild

### Product Benefits

* Clear separation of responsibilities
* Feature flags per product
* Easy experimentation
* Predictable roadmap expansion

### Engineering Benefits

* Clean boundaries
* Minimal duplication
* Safe evolution into microservices
* Strong foundation for growth

---

## 15. Final Summary

Troo is built as a **platform**, not just an app.

* TrooX and Gogrub share the same foundation
* Differences are handled through **flow logic**, not duplicated systems
* Payments remain decoupled and secure
* Real-world business operations are configurable, not hardcoded

This architecture ensures Troo can:

* Serve hotels, restaurants, and delivery merchants
* Scale to multiple products
* Adapt to future business models without rework

---

