# INTELLI - Complete System Architecture Documentation

**Last Updated:** April 9, 2026  
**Version:** 2.0  
**Product:** https://intellihq.net

---

## 📋 EXECUTIVE SUMMARY

Intelli is a comprehensive VAS (Value-Added Services) marketing performance analytics platform built on a microservices architecture. It enables telecom operators and digital service providers to:

- Track subscription lifecycles across multiple channels
- Monitor marketing campaign performance in real-time  
- Detect and prevent fraud with integrated anti-fraud platforms
- Analyze customer behavior and lifetime value
- Optimize marketing spend with conversion attribution
- Manage multi-service subscriptions with granular controls

---

## 🏗️ SYSTEM ARCHITECTURE OVERVIEW

### Microservices Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         INTELLI ECOSYSTEM                        │
└─────────────────────────────────────────────────────────────────┘

┌────────────────┐         ┌────────────────┐         ┌────────────────┐
│  intelli-      │         │  intelli-      │         │  intelli-      │
│  landing       │         │  assets        │         │  promo         │
│                │         │                │         │                │
│  Next.js 15    │         │  Next.js 14    │         │  FastAPI       │
│  (Pages)       │────────▶│  (App Router)  │◀────────│  Python        │
│                │         │                │         │                │
│  Marketing     │         │  Dashboard     │         │  Campaign      │
│  Website       │         │  Frontend      │         │  Tracking      │
└────────────────┘         └────────────────┘         └────────────────┘
        │                           │                          │
        │                           │                          │
        │                           ▼                          │
        │                  ┌──────────────────┐               │
        │                  │                  │               │
        └─────────────────▶│  intelli-service │◀──────────────┘
                          │                  │
                          │  Django/DRF      │
                          │  Python          │
                          │                  │
                          │  Core Engine     │
                          └──────────────────┘
                                   │
                    ┌──────────────┼──────────────┐
                    ▼              ▼              ▼
              ┌──────────┐  ┌──────────┐  ┌──────────┐
              │ Primary  │  │ Datasync │  │  Redis   │
              │   DB     │  │    DB    │  │  Cache   │
              │          │  │          │  │          │
              │PostgreSQL│  │PostgreSQL│  │  Celery  │
              └──────────┘  └──────────┘  └──────────┘
```

### Service Boundaries

| Service | Technology | Port | URL | Purpose |
|---------|-----------|------|-----|---------|
| **intelli-landing** | Next.js 15 Pages | 3000 | https://intellihq.net | Marketing landing page |
| **intelli-assets** | Next.js 14 App Router | 3001 | https://app.intellihq.net | Dashboard UI |
| **intelli-promo** | FastAPI | 8000 | https://marketing.intellihq.net | Campaign tracking |
| **intelli-service** | Django/DRF | 8080 | https://api.intellihq.net | Core backend API |

---

## 🔧 SERVICE DETAILS

### 1. intelli-landing (Marketing Website)

**Purpose:** Public-facing marketing website showcasing Intelli product features, benefits, and sign-up flows.

**Technology Stack:**
- Next.js 15.2.8 (Pages Router)
- React 19
- TypeScript
- Tailwind CSS 4
- Framer Motion (animations)
- React Hook Form + Yup validation

**Key Features:**
- Hero section with CTA
- Benefits showcase
- Products overview
- How It Works section
- Request Invite modal
- Contact/Inquiry modal
- Footer with links

**Components Structure:**
```
src/
├── pages/
│   ├── index.tsx          # Landing page
│   ├── _app.tsx           # App wrapper
│   └── _document.tsx      # HTML document
├── layout/
│   ├── hero.tsx
│   ├── benefits.tsx
│   ├── products.tsx
│   ├── how-it-works.tsx
│   ├── get-started.tsx
│   ├── footer.tsx
│   ├── invite.tsx         # Request invite modal
│   └── inquiry.tsx        # Contact modal
├── components/
│   ├── button/
│   ├── header/
│   ├── input/
│   ├── modal/
│   └── textarea/
└── styles/
```

**Integration Points:**
- Submits invite requests to intelli-service API
- Redirects to intelli-assets dashboard after sign-up

---

### 2. intelli-assets (Dashboard Frontend)

**Purpose:** Analytics dashboard for business users to monitor services, campaigns, subscriptions, and fraud.

**Technology Stack:**
- Next.js 14.2.35 (App Router)
- React 18
- TypeScript
- TanStack Query (data fetching)
- Zustand (global state)
- Tailwind CSS + shadcn/ui
- ApexCharts (visualization)
- Axios (API client)

**Architecture Patterns:**

#### A. State Management
```typescript
// Global State (Zustand + localStorage persistence)
useUserStore {
  user: NewUser                    // Current staff profile
  selectedInstitution: number      // Active org ID
  isLoggedIn: boolean
  setUserData()
  logout()
}

useServiceStore {
  activeIndex: number              // Selected service
  totalServices: number
}

// Async State (TanStack Query)
- 5-minute stale time
- Automatic background refetch
- Optimistic updates on mutations
- Query invalidation on data changes
```

#### B. Route Structure
```
app/
├── admin/                         # Protected routes
│   ├── home/                     # Main dashboard
│   ├── analytics/                # Service performance
│   │   └── view/[serviceId]/    # Service config & details
│   ├── insights/                 # Marketing analytics
│   │   └── [partnerId]/         # Partner drill-down
│   ├── customer-discovery/       # Customer intelligence
│   ├── profile-settings/         # User profile
│   ├── organization-settings/    # Org config
│   └── users/                    # Staff management
└── auth/                         # Public routes
    ├── login/
    ├── forgotpassword/
    ├── createnewpassword/
    └── onboarding/
```

#### C. API Integration Pattern
```typescript
// API client with JWT injection
const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_SERVER_URL,
  headers: { "Content-Type": "application/json" }
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem("intelliJWT");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});
```

#### D. Custom Hooks Pattern
```typescript
// Data fetching hooks (TanStack Query)
useBusiness()                      // Fetch all orgs
useBusinessServices(id)            // Fetch org services
useDashboardStats(id, params)      // Business KPIs
useServiceStats(id, params, apiKey) // Service KPIs
useProviderPerformance(...)        // Marketing metrics

// Mutation hooks
useUpdateBusiness()                // Update org
useInviteUser()                    // Invite staff
useLoginBusinnessRep()            // Login
```

#### E. Component Library (shadcn/ui + Custom)
- **UI Primitives:** Button, Input, Select, Checkbox, Dialog, Sheet, Table
- **Layout:** Sidebar, Topbar, InstitutionSwitcher
- **Analytics:** StatComponent, ServiceTable, KPICards
- **Charts:** DonutChart (ApexCharts), LineChart (ApexCharts)
- **Tables:** MarketingPartnerTable, PerformerTable, TrafficSourceTable
- **Forms:** AddProductModal, AddCampaignUrlModal, InviteUserModal

**Key Features:**

1. **Dashboard (Home)**
   - Institution-level KPIs (revenue, subscriptions, churn)
   - Service health monitoring
   - Business alerts aggregation
   - Multi-tab interface (Analytics, Alerts, Health Trends)

2. **Service Analytics**
   - Per-service performance metrics
   - Channel breakdown (donut chart)
   - Time-series trends (line chart)
   - Date range filtering (Today, 7d, 30d, Custom)
   - Service configuration (Products, Marketers, Campaign URLs)

3. **Marketing Insights**
   - Top/bottom performer rankings
   - Conversion rate analysis
   - Traffic quality assessment
   - Fraud detection integration
   - Provider-level drill-down
   - Revenue vs churn trends

4. **Customer Discovery**
   - MSISDN-level analytics
   - Subscription lifecycle tracking
   - Customer segmentation
   - Trend analysis (daily, weekly, monthly)
   - Pagination & export

5. **Organization Settings**
   - Business profile management
   - Staff role-based access control
   - Multi-institution switching
   - User invitation & management

**Design System:**
- **Primary Color:** `#0079FF` (Blue)
- **Font:** Euclid (serif), Inter (sans)
- **Responsive:** Mobile-first with breakpoints
- **Dark Mode:** CSS variable-based theming

---

### 3. intelli-promo (Marketing Service)

**Purpose:** High-performance campaign click tracking, MSISDN capture, fraud detection, and conversion attribution.

**Technology Stack:**
- FastAPI (async Python)
- SQLAlchemy ORM
- Celery (background tasks)
- Redis (state management, cache)
- PostgreSQL (dual databases)
- MaxMind GeoIP
- Sentry (error tracking)

**Architecture Patterns:**

#### A. Dual-Database Strategy
```python
PRIMARY_DB {
  # Service config, marketers, campaign URLs
  # Read-mostly, transactional integrity
  Service, CampaignURL, Marketer, MarketerPayout
}

DATASYNC_DB {
  # High-volume marketing tracking (millions of rows)
  # Write-heavy, analytics workload
  CampaignTracker, CampaignDuplicate, MsisdnArchive,
  Subscription, Billing, ActivationNotification
}
```

**Why:** Isolates high-volume tracking writes from critical service config reads. Prevents lock contention and index bloat.

#### B. State-Based MSISDN Capture Flow
```
1. User clicks campaign link
   ↓
2. Generate cryptographically-signed state (HMAC-SHA256)
   ↓
3. Store context in Redis (600s TTL)
   ↓
4. Redirect to telco MSISDN capture service
   ↓
5. Capture service extracts MSISDN (SIM-based)
   ↓
6. Callback to /msisdn-claim endpoint (Bearer token auth)
   ↓
7. Store MSISDN in Redis
   ↓
8. User returns to /return endpoint
   ↓
9. Poll Redis for MSISDN (~1.2s)
   ↓
10. Process campaign validation with full context
```

**Why:** Handles WiFi users without SIM data. Secure state prevents tampering. Redis TTL auto-cleans stale sessions.

#### C. Service Logic Comparison

**service.py (v1 - Legacy):**
- Basic campaign creation
- Daily caps
- Hard-coded URL parameters

**service_v2.py (v2 - Current):**
- ✅ Smart anti-fraud fallback (deterministic provider selection)
- ✅ Early fraud blocking (ENVINA integration)
- ✅ Prior MSISDN detection (repeat customer flagging)
- ✅ Dynamic parameter naming (configurable per campaign)
- ✅ Pure functions (caller controls commits)
- ✅ Comprehensive logging with request IDs

#### D. API Endpoints
| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/web/{service_id}/{provider}/` | GET | Campaign entry point |
| `/web2/{service_id}/intelli/{telco}/{pubid}/{antifraud}` | GET | Direct Intelli integration |
| `/msisdn-claim` | POST | MTN MSISDN capture callback |
| `/msisdn-claim-glo` | POST | GLO MSISDN capture callback |
| `/return` | GET | MTN return from capture |
| `/return-glo` | GET | GLO return from capture |
| `/intelli/postback/` | GET | Payout postback webhook |
| `/health` | GET | Health check |

#### E. Background Tasks (Celery)
```python
handle_occurence()         # Duplicate counter increment
handle_msisdn_archive()    # GeoIP enrichment + caching
handle_remarketing()       # Churn re-engagement scheduling
```

**Key Features:**

1. **Campaign Tracking**
   - Click-level attribution
   - Geo-enrichment (IP → Country, City, ASN)
   - Duplicate detection per MSISDN
   - Provider-level routing

2. **Anti-Fraud Integration**
   - ENVINA platform integration
   - SECURED platform integration
   - Early fraud blocking
   - Activation notification processing

3. **MSISDN Capture**
   - Multi-telco support (MTN, GLO, etc.)
   - WiFi user handling
   - State-based security
   - Redis-backed polling

4. **Conversion Attribution**
   - Subscription linking via campaign_tracker_id
   - Multi-touch attribution
   - Revenue tracking per marketer
   - CPA/CPL calculation

**Integration Points:**
- Receives clicks from intelli-landing & affiliates
- Tracks conversions from intelli-service (Datasync postbacks)
- Sends payout postbacks to marketers
- Stores tracking data in shared Datasync DB

---

### 4. intelli-service (Core Backend Engine)

**Purpose:** Central API engine managing services, subscriptions, billing, fraud analysis, user management, and organization orchestration.

**Technology Stack:**
- Django 4.2 + Django REST Framework
- PostgreSQL (dual databases)
- Celery + Celery Beat (task scheduling)
- Redis (broker, cache)
- TimescaleDB (future time-series optimization)
- Prometheus + Grafana (monitoring)
- ZeptoMail (email)
- DigitalOcean Spaces (file storage)

**Architecture Patterns:**

#### A. Django Apps (Modular Design)
```python
core/           # Shared utilities, base models, permissions, signals
ums/            # User Management System (email-based auth)
organization/   # Multi-tenant institution management
service/        # Service registration, health, API usage, analytics
marketing/      # Campaigns, subscriptions, billing, fraud analysis
datasync/       # Telco data ingestion (38.6M+ rows)
```

#### B. Dual-Database Routing
```python
class CustomRouter:
    """
    Routes marketing & datasync apps to datasync_db
    Routes all others to default DB
    """
    def db_for_read(self, model, **hints):
        if model._meta.app_label in ["marketing", "datasync"]:
            return "datasync_db"
        return "default"
```

**Why:** Isolates 38.6M+ high-volume records (23GB+) from core business logic. Prevents index bloat, enables independent scaling, and reduces lock contention.

#### C. Middleware Stack
```
PrometheusBeforeMiddleware (metrics start)
  ↓
SecurityMiddleware (HTTPS, HSTS)
  ↓
WhiteNoiseMiddleware (static files)
  ↓
SessionMiddleware, CORSMiddleware
  ↓
ServiceApiUsageMiddleware (per-request logging)
  ↓
PrometheusAfterMiddleware (metrics end)
```

#### D. Core Models

**Primary Database:**
```python
Institution               # Root org entity
  ├─ Staff                # Staff profiles with roles
  ├─ Service              # Registered services
  │   ├─ Product          # Service products/plans
  │   ├─ CampaignURL      # Marketing URLs
  │   └─ Marketer         # Partner marketers
  └─ ServiceApiRequestLog # API usage for billing
```

**Datasync Database (38.6M+ rows):**
```python
Datasync                  # Telco subscription/billing notifications
  └─ ActivationNotification  # Fraud platform responses

CampaignTracker           # Click tracking (geo-enriched)
  └─ CampaignDuplicate    # Duplicate detection

Subscribtion              # Subscription state per MSISDN
  └─ Billing              # Billing cycles
```

#### E. API Design (DRF ViewSets)
```
/api/v1/organization/
  GET    /                            # List orgs
  GET    /{id}/                       # Org details
  GET    /{id}/services/              # Org's services
  GET    /{id}/dashboard-stats/       # Business KPIs
  POST   /onboard-business/           # New org
  PATCH  /{id}/                       # Update org

/api/v1/service/
  GET    /{id}/stats/                 # Service KPIs
  GET    /{id}/performance-metrics/   # Service trends
  GET    /{id}/health/trends/         # Health monitoring
  GET    /{id}/marketing/provider-summary/
  GET    /{id}/marketing/fraud-analysis/stats/
  
/api/v1/service/customer-discovery/
  GET    /stats/                      # Customer stats
  GET    /customers/                  # Customer list

/api/v1/datasync/
  POST   /sync-notification/          # Telco data ingestion
  POST   /activation-notification/    # Fraud platform callback
```

#### F. Background Tasks (Celery Beat)
| Task | Schedule | Purpose |
|------|----------|---------|
| `nightly-rollup-all-services` | 00:10 daily | Aggregate daily stats |
| `health-check-all-services` | Every 30 min | Service reachability |
| `daily-performance-report` | 7:00 AM | Antifraud metrics |
| `cleanup-old-datasync-records` | 4:00 AM | 90-day retention |
| `reindex-datasync-indexes` | 2nd month, 2 AM | **REINDEX TABLE datasync** |
| `rollup-datasync-every-5-mins` | Every 5 min | Incremental rollup |

**Queue Assignment:**
```python
"notify": ["send_invite_email_task"]
"postback": ["process_provider_postback"]
"ingest": ["process_datasync"]           # High-volume
"maintenance": ["rollup_service_for_day"]
"otp": ["send_otp_sms"]                  # Time-sensitive
```

#### G. Circuit Breaker Pattern
```python
class CircuitBreaker:
    """
    Distributed circuit breaker using Redis cache
    States: CLOSED → OPEN (after N failures) → HALF_OPEN (testing)
    Prevents cascading failures in external API calls
    """
```

**Key Features:**

1. **Service Management**
   - Service registration & configuration
   - Multi-service acquisition controls
   - Health check monitoring with alerts
   - API key generation & usage tracking
   - Product/plan management

2. **Marketing Attribution**
   - Campaign click tracking
   - Subscription lifecycle management
   - Conversion attribution
   - Multi-touch attribution
   - Revenue tracking per channel/marketer

3. **Fraud Analysis**
   - Activation notification processing
   - Fraud rate aggregation by provider
   - Trend analysis (hourly, daily, weekly)
   - Fraudulent traffic identification
   - Marketer fraud scoring

4. **Data Synchronization (Datasync)**
   - Telco subscription ingestion (POST webhooks)
   - Billing cycle tracking
   - Campaign tracker linking
   - 90-day retention policy
   - Batch processing (50K rows/batch)

5. **User & Organization Management**
   - Multi-tenant institution model
   - Role-based access control (Owner, Manager, Analyst, Staff)
   - Email-based authentication (JWT)
   - Staff invitation & onboarding
   - Multi-institution support per staff

6. **Customer Discovery**
   - MSISDN-level analytics
   - Subscription trend analysis
   - Customer segmentation
   - Lifetime value calculation
   - Churn prediction

7. **API Usage Tracking**
   - Per-request logging
   - Price tier classification (Premium, Discounted, Standard)
   - Async Celery-based persistence
   - Business analytics & billing

**Integration Points:**
- Serves API to intelli-assets dashboard
- Receives data from intelli-promo tracking
- Ingests telco data via webhook
- Receives fraud signals from ENVINA/SECURED
- Sends postbacks to intelli-promo and external marketers

---

## 🔄 DATA FLOW ARCHITECTURE

### End-to-End User Journey

```
1. User discovers service via intelli-landing
   ↓
2. Clicks CTA → Redirects to intelli-promo campaign URL
   ↓
3. intelli-promo creates CampaignTracker (click record)
   ↓
4. MSISDN capture flow (WiFi handling via Redis state)
   ↓
5. Redirect to service landing page/subscription flow
   ↓
6. Telco activates subscription → Webhook to intelli-service
   ↓
7. intelli-service creates Datasync record (links to CampaignTracker)
   ↓
8. Marks CampaignTracker.converted = True
   ↓
9. Triggers Celery tasks:
   - process_datasync (rollups)
   - process_successful_subscription_sms (SMS notification)
   - process_provider_postback (marketer payout)
   ↓
10. Antifraud platform responds → ActivationNotification
    ↓
11. Fraud analysis aggregation (detection, scoring)
    ↓
12. Business users view analytics in intelli-assets dashboard
```

### Database Synchronization

```
┌─────────────────────────────────────────────────────────────┐
│                     DATABASE ARCHITECTURE                    │
└─────────────────────────────────────────────────────────────┘

intelli-service                    intelli-promo
┌──────────────┐                   ┌──────────────┐
│  Primary DB  │                   │  Primary DB  │
│              │                   │              │
│ Institution  │                   │  Service     │ (read replica)
│ Staff        │                   │  CampaignURL │
│ Service      │                   │  Marketer    │
└──────────────┘                   └──────────────┘
       │                                  │
       │                                  │
       └──────────┬───────────────────────┘
                  ▼
         ┌──────────────────┐
         │   Datasync DB    │ (Shared)
         │                  │
         │ CampaignTracker  │ ◀── intelli-promo writes
         │ Datasync         │ ◀── intelli-service writes
         │ Subscription     │
         │ Billing          │
         │ ActivationNotif  │
         └──────────────────┘
```

**Synchronization Strategy:**
- **Write Separation:** intelli-promo writes CampaignTracker, intelli-service writes Datasync
- **Foreign Key Linking:** Datasync.campaign_tracker_id → CampaignTracker.id
- **Eventual Consistency:** Background tasks reconcile data
- **Read-Heavy:** Both services read from Datasync DB for analytics

---

## 🔐 SECURITY ARCHITECTURE

### Authentication & Authorization

#### intelli-assets (Frontend)
```typescript
// JWT stored in localStorage
localStorage.setItem("intelliJWT", response.access);

// Axios interceptor injects token
api.interceptors.request.use((config) => {
  const token = localStorage.getItem("intelliJWT");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Route protection
if (isAdminRoute && !isLoggedIn) {
  router.push("/auth/login");
}
```

#### intelli-service (Backend)
```python
# JWT Authentication (SimpleJWT)
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
        'rest_framework.authentication.TokenAuthentication',
    ],
}

# JWT Config
SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(days=3),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=7),
    'ALGORITHM': 'HS512',
}

# Role-Based Permissions
class IsOrgStaffOrReadOnly(BasePermission):
    def has_permission(self, request, view):
        if request.method in SAFE_METHODS:
            return True
        return is_org_staff(request.user, request.institution_id)
```

#### intelli-promo (Marketing Service)
```python
# Bearer Token Authentication for MSISDN capture callbacks
MSISDN_CLAIM_TOKEN = os.getenv("MSISDN_CLAIM_TOKEN")

@app.post("/msisdn-claim")
async def msisdn_claim(
    authorization: str = Header(None),
):
    if not authorization or not authorization.startswith("Bearer "):
        raise HTTPException(status_code=401)
    token = authorization.split(" ")[1]
    if token != MSISDN_CLAIM_TOKEN:
        raise HTTPException(status_code=403)
```

### State Security (intelli-promo)
```python
# HMAC-SHA256 signed state for MSISDN capture flow
state_signature = hmac.new(
    MSISDN_STATE_SECRET.encode(),
    state_data.encode(),
    hashlib.sha256
).hexdigest()

# Verification on return
if not hmac.compare_digest(expected_sig, provided_sig):
    raise HTTPException(status_code=400, detail="Invalid state")
```

### Multi-Tenant Isolation
```python
# Institution-scoped queries
Service.objects.filter(institution_id=request.user.institution_id)

# Middleware injection
request.institution_id = get_institution_from_user(request.user)

# ViewSet-level enforcement
def get_queryset(self):
    return Service.objects.filter(
        institution_id=self.request.user.staff_profile.institution_id
    )
```

---

## 📊 MONITORING & OBSERVABILITY

### Metrics Collection

#### Prometheus Integration (intelli-service)
```python
MIDDLEWARE = [
    'django_prometheus.middleware.PrometheusBeforeMiddleware',
    # ... other middleware
    'django_prometheus.middleware.PrometheusAfterMiddleware',
]

# Metrics exposed at /metrics endpoint
```

#### Custom Metrics
```python
# Celery task metrics (auto-tracked via signals)
CeleryTaskMetric {
    task_name, status, execution_time,
    queue_time, worker, timestamp
}

# API usage metrics
ServiceApiRequestLog {
    service, path, method, status_code,
    response_time, price_tier
}
```

### Logging Architecture

#### Structured Logging (intelli-promo)
```python
# JSON structured logs
{
  "ts": "2026-04-09T14:23:45",
  "lvl": "INFO",
  "lg": "app.api",
  "msg": "campaign_entry",
  "service_id": 123,
  "provider": "mobplus",
  "request_id": "abc123"
}
```

#### Log Aggregation Stack
```
Application Logs
  ↓
Promtail (collector)
  ↓
Loki (aggregation)
  ↓
Grafana (visualization)
```

### Error Tracking

#### Sentry Integration
```python
# intelli-promo
sentry_sdk.init(
    dsn=os.getenv("SENTRY_DSN"),
    environment=os.getenv("APP_ENV", "dev"),
    traces_sample_rate=0.1,
)
```

#### Error Pages (intelli-promo)
```python
# User-friendly error pages with request ID
@app.exception_handler(Exception)
async def generic_exception_handler(request, exc):
    request_id = request.state.request_id
    return HTMLResponse(
        render_error_page(500, "Internal Server Error", request_id)
    )
```

---

## 🚀 DEPLOYMENT ARCHITECTURE

### Containerization (Docker)

#### intelli-service
```dockerfile
# Multi-stage build
FROM python:3.12-slim as builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

FROM python:3.12-slim
COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY app/ /app/
CMD ["gunicorn", "app.wsgi:application", "--bind", "0.0.0.0:8080"]
```

#### intelli-promo
```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app/ /app/
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

#### intelli-assets / intelli-landing
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json
CMD ["npm", "start"]
```

### Docker Compose Services

```yaml
services:
  intelli-service:
    build: ./intelli-service
    ports: ["8080:8080"]
    environment:
      - PGHOST=postgres
      - DATASYNC_PGHOST=postgres-datasync
      - REDIS_HOST=redis
    depends_on: [postgres, postgres-datasync, redis]

  intelli-promo:
    build: ./intelli-promo
    ports: ["8000:8000"]
    environment:
      - PGHOST=postgres
      - DATASYNC_PGHOST=postgres-datasync
      - CELERY_BROKER_URL=redis://redis:6379/0
    depends_on: [postgres, postgres-datasync, redis]

  intelli-assets:
    build: ./intelli-assets
    ports: ["3001:3000"]
    environment:
      - NEXT_PUBLIC_SERVER_URL=http://intelli-service:8080

  intelli-landing:
    build: ./intelli-landing
    ports: ["3000:3000"]

  postgres:
    image: postgres:16
    environment:
      - POSTGRES_DB=intelli_db

  postgres-datasync:
    image: postgres:16
    environment:
      - POSTGRES_DB=intelli_datasync

  redis:
    image: redis:7-alpine

  celery:
    build: ./intelli-service
    command: celery -A app worker -Q default,notify,postback,ingest,maintenance,otp -l info

  celery-beat:
    build: ./intelli-service
    command: celery -A app beat -l info
```

---

## 🗄️ DATABASE SCHEMA HIGHLIGHTS

### Primary Database (intelli-service)

```sql
-- Multi-tenant root
CREATE TABLE organization_institution (
    id SERIAL PRIMARY KEY,
    icode VARCHAR(10) UNIQUE,
    business_name VARCHAR(255),
    owner_id INTEGER REFERENCES ums_user(id),
    verified BOOLEAN DEFAULT FALSE,
    onboarded BOOLEAN DEFAULT FALSE,
    created TIMESTAMP DEFAULT NOW()
);

-- Service registration
CREATE TABLE service_service (
    id SERIAL PRIMARY KEY,
    institution_id INTEGER REFERENCES organization_institution(id),
    service_name VARCHAR(255),
    service_url TEXT,
    api_key VARCHAR(255) UNIQUE,
    manage_antifraud_resolve BOOLEAN DEFAULT FALSE,
    fraudulent_check BOOLEAN DEFAULT FALSE,
    created TIMESTAMP DEFAULT NOW()
);

-- API usage billing
CREATE TABLE service_serviceapirequestlog (
    id BIGSERIAL PRIMARY KEY,
    service_id INTEGER REFERENCES service_service(id),
    path TEXT,
    method VARCHAR(10),
    status_code INTEGER,
    response_time_ms FLOAT,
    price_tier VARCHAR(20),  -- PREMIUM, DISCOUNTED, STANDARD
    created TIMESTAMP DEFAULT NOW()
);
CREATE INDEX idx_service_created ON service_serviceapirequestlog(service_id, created);
```

### Datasync Database (Shared: intelli-service + intelli-promo)

```sql
-- Campaign click tracking (intelli-promo writes)
CREATE TABLE marketing_campaigntracker (
    id BIGSERIAL PRIMARY KEY,
    service_id INTEGER,
    click_id VARCHAR(255),
    provider VARCHAR(100),
    msisdn VARCHAR(20),
    telco VARCHAR(50),
    ip_address INET,
    country VARCHAR(100),
    city VARCHAR(100),
    asn INTEGER,
    converted BOOLEAN DEFAULT FALSE,
    converted_at TIMESTAMP,
    created TIMESTAMP DEFAULT NOW()
);
CREATE UNIQUE INDEX idx_campaign_unique ON marketing_campaigntracker(service_id, click_id, provider);
CREATE INDEX idx_campaign_service_created ON marketing_campaigntracker(service_id, created);
CREATE INDEX idx_campaign_msisdn ON marketing_campaigntracker(msisdn);

-- Telco data ingestion (intelli-service writes)
CREATE TABLE datasync_datasync (
    id BIGSERIAL PRIMARY KEY,
    service_id INTEGER,
    campaign_tracker_id BIGINT REFERENCES marketing_campaigntracker(id),
    phone VARCHAR(20),
    product_id VARCHAR(100),
    sub_date TIMESTAMP,
    sub_expiry TIMESTAMP,
    amount DECIMAL(10, 2),
    created TIMESTAMP DEFAULT NOW()
);
CREATE INDEX ds_service_created_idx ON datasync_datasync(service_id, created);
CREATE INDEX ds_campaign_tracker_idx ON datasync_datasync(campaign_tracker_id);
CREATE INDEX ds_phone_idx ON datasync_datasync(phone);
-- BRIN index for sequential scans
CREATE INDEX ds_created_brin_idx ON datasync_datasync USING BRIN(created);

-- Subscription state
CREATE TABLE marketing_subscribtion (
    id BIGSERIAL PRIMARY KEY,
    service_id INTEGER,
    msisdn VARCHAR(20),
    sub_active BOOLEAN DEFAULT TRUE,
    sub_status VARCHAR(50),
    traffic_source VARCHAR(100),
    starts_date TIMESTAMP,
    churn_date TIMESTAMP,
    created TIMESTAMP DEFAULT NOW()
);
CREATE INDEX idx_sub_service_msisdn ON marketing_subscribtion(service_id, msisdn);
CREATE INDEX idx_sub_status ON marketing_subscribtion(service_id, sub_status, starts_date);

-- Billing cycles
CREATE TABLE marketing_billing (
    id BIGSERIAL PRIMARY KEY,
    subscribtion_id BIGINT REFERENCES marketing_subscribtion(id),
    amount DECIMAL(10, 2),
    billing_type VARCHAR(50),
    product_id VARCHAR(100),
    created TIMESTAMP DEFAULT NOW()
);
```

---

## 🎯 KEY ARCHITECTURAL DECISIONS

### 1. Dual-Database Strategy

**Decision:** Separate high-volume tracking data (Datasync DB) from core business logic (Primary DB).

**Rationale:**
- **Scale:** Datasync DB handles 38.6M+ rows (~23GB) without impacting service queries
- **Performance:** Prevents index bloat on primary database
- **Isolation:** Allows independent backup/restore schedules
- **Optimization:** Different query patterns (OLTP vs OLAP)

**Trade-offs:**
- ✅ Improved query performance on primary DB
- ✅ Independent scaling of tracking infrastructure
- ❌ Foreign key constraints across databases not enforceable
- ❌ Requires careful transaction management

---

### 2. Microservices Architecture

**Decision:** Split into 4 services (landing, assets, promo, service) instead of monolith.

**Rationale:**
- **Technology Fit:** FastAPI for high-throughput tracking, Django for complex business logic
- **Team Autonomy:** Frontend/backend teams work independently
- **Deployment:** Independent release cycles
- **Scaling:** Scale promo service for traffic spikes without scaling backend

**Trade-offs:**
- ✅ Technology diversity (Next.js, FastAPI, Django)
- ✅ Independent deployments
- ✅ Fault isolation
- ❌ Increased operational complexity
- ❌ Distributed tracing required
- ❌ Network latency between services

---

### 3. State-Based MSISDN Capture (intelli-promo)

**Decision:** Use Redis-backed state management for WiFi user handling.

**Rationale:**
- **WiFi Users:** Cannot extract MSISDN client-side without SIM
- **Security:** HMAC-signed state prevents tampering
- **TTL:** Auto-cleanup prevents memory leaks
- **Polling:** Simple return endpoint waits for MSISDN

**Trade-offs:**
- ✅ Handles all user scenarios (WiFi + mobile)
- ✅ Secure state management
- ✅ No database writes for failed captures
- ❌ 1.2s polling delay on return
- ❌ Redis dependency for critical flow

---

### 4. Celery Task Queues

**Decision:** Dedicated queues (notify, postback, ingest, maintenance, otp) instead of single queue.

**Rationale:**
- **Priority:** OTP tasks must not block behind batch jobs
- **Resource Allocation:** Ingest queue handles high-volume writes
- **Failure Isolation:** Postback failures don't affect notifications
- **Monitoring:** Per-queue metrics for debugging

**Trade-offs:**
- ✅ Prevents head-of-line blocking
- ✅ Independent retry policies per queue
- ✅ Better observability
- ❌ More complex worker deployment
- ❌ Queue balancing required

---

### 5. TanStack Query + Zustand (intelli-assets)

**Decision:** TanStack Query for async state, Zustand for global state (not Redux).

**Rationale:**
- **Cache Management:** Auto-cache invalidation, background refetch
- **Optimistic Updates:** Fast UI without waiting for server
- **Simplicity:** Zustand has minimal boilerplate vs Redux
- **Persistence:** Zustand persists to localStorage seamlessly

**Trade-offs:**
- ✅ Less boilerplate than Redux
- ✅ Built-in loading/error states
- ✅ Auto-refetch on window focus
- ❌ Learning curve for developers used to Redux
- ❌ Zustand lacks Redux DevTools time-travel

---

### 6. 90-Day Datasync Retention

**Decision:** Auto-delete records older than 90 days via scheduled task.

**Rationale:**
- **Compliance:** GDPR-like data minimization
- **Performance:** Keeps indexes small
- **Cost:** Reduces storage costs
- **Analytics:** 90 days sufficient for business metrics

**Trade-offs:**
- ✅ Controlled database growth
- ✅ Faster queries on smaller dataset
- ❌ Historical data loss (mitigated by exports)
- ❌ Requires careful backup strategy

---

### 7. Circuit Breaker for External APIs

**Decision:** Implement distributed circuit breaker using Redis cache.

**Rationale:**
- **Cascading Failures:** Prevent failing external API from taking down service
- **Fast Fail:** Return error immediately when circuit open
- **Recovery Testing:** Half-open state tests recovery
- **Distributed:** Redis ensures all workers respect circuit state

**Trade-offs:**
- ✅ Prevents cascading failures
- ✅ Graceful degradation
- ❌ Adds complexity to API calls
- ❌ Redis dependency for critical path

---

## 📈 SCALABILITY CONSIDERATIONS

### Horizontal Scaling Points

```
Load Balancer
├─ intelli-landing (Vercel Edge / 3+ replicas)
├─ intelli-assets (Vercel Edge / 3+ replicas)
├─ intelli-promo (5+ FastAPI workers)
└─ intelli-service (3+ Gunicorn workers)
     ├─ Celery Worker Pool (10+ workers across queues)
     └─ Celery Beat (1 singleton)

Database Layer
├─ Primary DB (read replicas for analytics queries)
├─ Datasync DB (partitioning by created date)
└─ Redis (cluster mode for high availability)
```

### Performance Optimizations

#### Database
- **Indexes:** BRIN for time-series, partial indexes for 90-day retention
- **Connection Pooling:** 5 + 5 overflow per process
- **Query Timeout:** 600s for long-running analytics
- **Partitioning:** Future: partition Datasync by month

#### API
- **Pagination:** Cursor-based for large result sets
- **API Key Caching:** Redis cache for frequent lookups
- **Query Caching:** 5-minute cache for dashboard stats
- **Async Tasks:** Offload to Celery to keep requests fast

#### Frontend
- **Code Splitting:** Next.js automatic route-based splitting
- **Image Optimization:** Next.js Image component
- **Static Generation:** Landing page pre-rendered at build
- **CDN:** Assets served from CDN edge locations

---

## 🔧 DEVELOPMENT WORKFLOW

### Local Development Setup

```bash
# Clone repositories
git clone <intelli-landing-repo>
git clone <intelli-assets-repo>
git clone <intelli-promo-repo>
git clone <intelli-service-repo>

# Start infrastructure
docker-compose up -d postgres redis

# intelli-service
cd intelli-service/app
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver 8080

# intelli-promo
cd intelli-promo
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000

# intelli-assets
cd intelli-assets
npm install
npm run dev  # port 3001

# intelli-landing
cd intelli-landing
npm install
npm run dev  # port 3000
```

### Environment Variables

#### intelli-service
```env
# Primary DB
PGHOST=localhost
PGPORT=5432
PGUSER=postgres
PGPASSWORD=password
DB_NAME=intelli_db

# Datasync DB
DATASYNC_PGHOST=localhost
DATASYNC_PGPORT=5432
DATASYNC_PGUSER=postgres
DATASYNC_PGPASSWORD=password
DATASYNC_DB_NAME=intelli_datasync

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/1

# Django
SECRET_KEY=<generate-random>
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1

# Email
EMAIL_HOST_PASSWORD=<zeptomail-password>
AUTO_MAIL_FROM=noreply@intellihq.net

# SMS
TERMII_TOKEN=<termii-api-key>

# Storage
DO_SPACES_ENDPOINT_URL=<digitalocean-url>
DO_SPACES_ACCESS_KEY_ID=<access-key>
DO_SPACES_SECRET_ACCESS_KEY=<secret-key>
```

#### intelli-promo
```env
# Primary DB
PGHOST=localhost
PGPORT=5432
PGUSER=postgres
PGPASSWORD=password
DB_NAME=intelli_db
PRIMARY_DB_SSLMODE=disable

# Datasync DB
DATASYNC_PGHOST=localhost
DATASYNC_PGPORT=5432
DATASYNC_PGUSER=postgres
DATASYNC_PGPASS=password
DATASYNC_DB_NAME=intelli_datasync

# Celery
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/1

# Marketing
MSISDN_CLAIM_TOKEN=<secure-random-token>
MSISDN_STATE_SECRET=<secure-random-secret>
CLAIM_TTL_SEC=600
ENRICHED_CAPTURE_URL=https://capture.example.com/mtn
GLO_CAPTURE_URL=https://capture.example.com/glo
MARKETING_BASE_URL=https://marketing.intellihq.net

# GeoIP
GEO_CITY_DB=/app/GeoLite2-City.mmdb
GEO_ASN_DB=/app/GeoLite2-ASN.mmdb
GEO_FALLBACK_URL=https://ipapi.co/{ip}/json/

# App
APP_ENV=dev
```

#### intelli-assets
```env
NEXT_PUBLIC_SERVER_URL=http://localhost:8080
```

---

## 🧪 TESTING STRATEGY

### Backend Testing (intelli-service)

```bash
# Run tests
cd intelli-service/app
pytest

# With coverage
pytest --cov=app --cov-report=html

# Current coverage: ~35% (gradually increasing)
```

**Key Tests:**
- Model tests (factory_boy fixtures)
- ViewSet tests (DRF APIClient)
- Service layer tests (pure logic, no ORM)
- Task tests (Celery task execution)
- Permission tests (RBAC)

### Frontend Testing (intelli-assets)

```bash
# Run tests (when implemented)
npm test

# Type checking
npm run type-check
```

**Recommended Tests:**
- Component unit tests (React Testing Library)
- Hook tests (renderHook)
- Integration tests (MSW for API mocking)
- E2E tests (Playwright)

### API Testing (intelli-promo)

```bash
# Run tests
cd intelli-promo
pytest

# With coverage
pytest --cov=app --cov-report=html
```

**Key Tests:**
- Endpoint tests (TestClient)
- Service v2 function tests
- State generation/verification tests
- Geo-enrichment tests
- Database routing tests

---

## 📚 API DOCUMENTATION

### OpenAPI Schemas

#### intelli-service
- **URL:** https://api.intellihq.net/api/schema/
- **Swagger UI:** https://api.intellihq.net/api/schema/swagger-ui/
- **ReDoc:** https://api.intellihq.net/api/schema/redoc/
- **Generator:** drf-spectacular

#### intelli-promo
- **URL:** https://marketing.intellihq.net/docs
- **Generator:** FastAPI auto-generated

---

## 🔮 FUTURE ENHANCEMENTS

### Planned Features

1. **TimescaleDB Migration**
   - Migrate Datasync to TimescaleDB for time-series optimization
   - Hypertables for automatic partitioning
   - Continuous aggregates for real-time rollups
   - Compression for historical data

2. **Real-Time Analytics**
   - WebSocket connections for live dashboard updates
   - Server-Sent Events (SSE) for notifications
   - Redis Pub/Sub for cache invalidation

3. **Advanced Fraud Detection**
   - Machine learning models for fraud prediction
   - Behavioral analysis (click patterns, timing)
   - Device fingerprinting
   - Graph-based fraud detection

4. **Multi-Region Deployment**
   - Edge computing for campaign redirection
   - Regional database replicas
   - Geo-distributed Redis clusters
   - CDN integration for static assets

5. **Enhanced Customer Discovery**
   - Predictive churn modeling
   - Lifetime value prediction
   - Cohort analysis
   - A/B testing framework

6. **Payments Module**
   - Automated marketer payouts
   - Multi-currency support
   - Invoice generation
   - Payment reconciliation

7. **Content Management**
   - Dynamic landing page builder
   - A/B testing for creatives
   - Asset library management
   - Campaign template system

---

## 📞 SUPPORT & MAINTENANCE

### Monitoring Alerts

**Service Health:**
- Service down after 5 consecutive failures (Circuit breaker opens)
- Alert emails to service owners

**Database:**
- Datasync table size > 50GB (reindex recommended)
- Connection pool exhaustion
- Query timeout warnings

**Celery:**
- Task failure rate > 10%
- Queue backlog > 10,000 tasks
- Worker heartbeat missing

**Performance:**
- API response time > 2s (95th percentile)
- Dashboard load time > 5s
- Campaign redirection > 500ms

### Backup Strategy

```bash
# Primary DB daily backup
pg_dump -h $PGHOST -U $PGUSER intelli_db | gzip > backup_$(date +%Y%m%d).sql.gz

# Datasync DB weekly backup (large)
pg_dump -h $DATASYNC_PGHOST -U $DATASYNC_PGUSER intelli_datasync | gzip > datasync_backup_$(date +%Y%m%d).sql.gz

# Redis persistence (AOF + RDB)
redis-cli BGSAVE
```

### Maintenance Windows

**Monthly Reindex (2nd Sunday, 2:00 AM):**
```sql
REINDEX TABLE datasync_datasync;
REINDEX TABLE marketing_campaigntracker;
```

**Quarterly DB Vacuum:**
```sql
VACUUM ANALYZE datasync_datasync;
VACUUM ANALYZE marketing_subscribtion;
```

---

## 🎓 LEARNING RESOURCES

### New Developer Onboarding

1. **Read Architecture Docs** (this file)
2. **Read Technical Software Documentation** (INTELLI_TECHNICAL_DOCS.md)
3. **Read .copilot Development Guide** (.copilot-guide.md)
4. **Set up local environment** (see Development Workflow)
5. **Run test suite** to verify setup
6. **Review PROJECT_CONTEXT.md** (intelli-service)
7. **Study DEVELOPMENT.md** (intelli-service)
8. **Explore Swagger/ReDoc** API docs

### Key Files to Study

#### intelli-service
- `app/settings/base.py` - Core configuration
- `app/router.py` - Database routing
- `service/models.py` - Service models
- `marketing/models.py` - Marketing models
- `datasync/models.py` - Datasync models
- `service/views.py` - API endpoints
- `service/tasks.py` - Background tasks

#### intelli-promo
- `app/main.py` - FastAPI app
- `app/api.py` - Route handlers
- `app/service_v2.py` - Business logic (current)
- `app/models.py` - Database models
- `app/database.py` - Dual DB setup

#### intelli-assets
- `src/app/layout.tsx` - Root layout & auth guard
- `src/lib/api.ts` - API client
- `src/hooks/useService.ts` - Data fetching hooks
- `src/app/store/` - Zustand stores
- `src/app/admin/home/` - Dashboard components

---

## 📝 GLOSSARY

| Term | Definition |
|------|------------|
| **VAS** | Value-Added Services (telecom subscription services) |
| **MSISDN** | Mobile Station International Subscriber Directory Number (phone number) |
| **CPA** | Cost Per Acquisition |
| **CPL** | Cost Per Lead |
| **CR** | Conversion Rate |
| **LTV** | Lifetime Value |
| **CAC** | Customer Acquisition Cost |
| **Churn** | Customer cancellation/deactivation rate |
| **Datasync** | Telco subscription notification ingestion module |
| **Campaign Tracker** | Click-level tracking record |
| **Anti-fraud** | Fraud detection platform (ENVINA, SECURED) |
| **Marketer** | Partner marketing entity (affiliate) |
| **Provider** | Marketing traffic provider (e.g., mobplus, ads24) |
| **Telco** | Telecommunications carrier (MTN, GLO, Airtel, etc.) |
| **Institution** | Organization/business entity (multi-tenant root) |
| **Staff** | User account with org access |
| **Service** | Registered subscription service/product |
| **Product** | Pricing plan/tier within a service |

---

## 🏁 CONCLUSION

Intelli's architecture is designed for **scale**, **reliability**, and **flexibility**. The microservices approach allows independent scaling of high-traffic components (intelli-promo) while maintaining complex business logic in a robust backend (intelli-service). The dual-database strategy isolates high-volume tracking data from critical service configuration, ensuring consistent performance.

Key strengths:
- ✅ **High throughput:** FastAPI handles millions of clicks
- ✅ **Real-time analytics:** 5-minute incremental rollups
- ✅ **Fraud detection:** Integrated anti-fraud platforms
- ✅ **Multi-tenant:** Institution-based isolation
- ✅ **Observability:** Prometheus, Grafana, Loki, Sentry
- ✅ **Developer experience:** Type safety (TypeScript), comprehensive docs

This architecture positions Intelli to scale from thousands to millions of subscriptions while maintaining sub-second query times and reliable fraud detection.

---

**Document Version:** 2.0  
**Last Updated:** April 9, 2026  
**Maintained By:** Engineering Team  
**Contact:** dev@intellihq.net



