# INTELLI - Technical Software Documentation

**Last Updated:** April 9, 2026  
**Version:** 2.0  
**For:** Software Engineers & Technical Staff

---

## 📑 TABLE OF CONTENTS

1. [Code Organization](#code-organization)
2. [Coding Standards & Patterns](#coding-standards--patterns)
3. [API Reference](#api-reference)
4. [Database Schema Reference](#database-schema-reference)
5. [Component Reference](#component-reference)
6. [Task & Background Jobs](#task--background-jobs)
7. [Testing Guidelines](#testing-guidelines)
8. [Deployment Procedures](#deployment-procedures)
9. [Troubleshooting Guide](#troubleshooting-guide)
10. [Performance Optimization](#performance-optimization)

---

## 🗂️ CODE ORGANIZATION

### intelli-service (Django Backend)

```
intelli-service/app/
├── app/                          # Project root
│   ├── settings/
│   │   ├── base.py              # Base settings
│   │   ├── dev.py               # Development overrides
│   │   └── prod.py              # Production overrides
│   ├── celery.py                # Celery app configuration
│   ├── router.py                # Database routing logic
│   ├── urls.py                  # URL routing
│   └── wsgi.py                  # WSGI entry point
├── core/                        # Shared utilities
│   ├── models.py                # BaseAbstractModel
│   ├── permissions.py           # Custom DRF permissions
│   ├── signals.py               # Global signals
│   ├── mail.py                  # ZeptoMail wrapper
│   └── utils/                   # Helper functions
├── ums/                         # User Management System
│   ├── models.py                # User model (email-based)
│   ├── views.py                 # Auth endpoints
│   ├── serializers.py           # User serializers
│   └── admin.py                 # Admin config
├── organization/                # Multi-tenant management
│   ├── models.py                # Institution, Staff
│   ├── views.py                 # Org CRUD APIs
│   ├── serializers.py           # Org serializers
│   └── permissions.py           # Org-level permissions
├── service/                     # Service management
│   ├── models.py                # Service, Product, CampaignURL, etc.
│   ├── views.py                 # Service APIs
│   ├── serializers.py           # Service serializers
│   ├── tasks.py                 # Service-related Celery tasks
│   ├── middleware.py            # API usage tracking
│   ├── signals.py               # Task metrics tracking
│   ├── services/                # Business logic layer
│   │   ├── health_service.py   # Health check logic
│   │   └── analytics_service.py
│   └── utils/
│       ├── circuit_breaker.py  # Circuit breaker pattern
│       └── date_utils.py
├── marketing/                   # Campaign tracking
│   ├── models.py                # CampaignTracker, Subscription, Billing
│   ├── views.py                 # Marketing APIs
│   ├── serializers.py           # Marketing serializers
│   ├── tasks.py                 # Marketing Celery tasks
│   ├── fraud_analysis.py        # Fraud detection logic
│   └── urls.py                  # Marketing URL patterns
├── datasync/                    # Telco data ingestion
│   ├── models.py                # Datasync, ActivationNotification
│   ├── views.py                 # Ingestion endpoints
│   ├── serializers.py           # Datasync serializers
│   └── tasks.py                 # Cleanup & rollup tasks
├── manage.py                    # Django management script
└── requirements.txt             # Python dependencies
```

---

### intelli-promo (FastAPI Marketing Service)

```
intelli-promo/app/
├── main.py                      # FastAPI app definition
├── api.py                       # Route handlers (~500 lines)
├── service.py                   # Business logic v1 (legacy)
├── service_v2.py                # Business logic v2 (current)
├── models.py                    # SQLAlchemy models (18+ models)
├── schemas.py                   # Pydantic request/response schemas
├── database.py                  # Dual database connections
├── config.py                    # Settings & environment vars
├── tasks.py                     # Celery background tasks
├── logging_setup.py             # JSON structured logging
├── error_pages.py               # HTML error templates
├── middleware/
│   └── request_id.py            # Request ID injection
├── utils/
│   ├── ip.py                    # IP & geo-enrichment
│   ├── state.py                 # HMAC state signing
│   └── validators.py            # Input validation
├── alembic/                     # Database migrations
│   ├── versions/                # Migration files
│   └── env.py                   # Alembic config
├── requirements.txt             # Python dependencies
└── alembic.ini                  # Alembic settings
```

---

### intelli-assets (Next.js Dashboard)

```
intelli-assets/src/
├── app/                         # Next.js App Router
│   ├── layout.tsx               # Root layout (auth guard)
│   ├── admin/                   # Protected routes
│   │   ├── home/                # Dashboard
│   │   │   ├── page.tsx
│   │   │   └── components/      # Dashboard components
│   │   ├── analytics/           # Service analytics
│   │   │   ├── page.tsx
│   │   │   └── view/[serviceId]/  # Service detail
│   │   ├── insights/            # Marketing insights
│   │   │   ├── page.tsx
│   │   │   ├── [partnerId]/
│   │   │   └── components/
│   │   ├── customer-discovery/  # Customer intelligence
│   │   │   ├── page.tsx
│   │   │   └── [customerId]/
│   │   ├── profile-settings/    # User profile
│   │   ├── organization-settings/ # Org config
│   │   └── users/               # Staff management
│   │       ├── page.tsx
│   │       └── components/
│   ├── auth/                    # Public routes
│   │   ├── login/
│   │   ├── forgotpassword/
│   │   └── onboarding/
│   ├── store/                   # Zustand stores
│   │   ├── useUserStore.tsx     # User & institution state
│   │   └── useServiceStore.tsx  # Service selection
│   ├── helpers/                 # Utility functions
│   └── globals.css              # Global styles
├── components/
│   ├── ui/                      # shadcn/ui primitives
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── select.tsx
│   │   ├── table.tsx
│   │   ├── dialog.tsx
│   │   └── ...
│   ├── layout/                  # Layout components
│   │   ├── AppSidebar.tsx
│   │   ├── AppTopbar.tsx
│   │   └── InstitutionSwitcher.tsx
│   └── pages/                   # Page-specific components
│       ├── ServiceTabs.tsx
│       ├── ProductsTab.tsx
│       └── ...
├── hooks/
│   ├── useService.ts            # Data fetching hooks
│   ├── useAuth.ts               # Auth hooks
│   ├── useBusiness.ts           # Business hooks
│   ├── useUsers.ts              # User management hooks
│   └── use-mobile.tsx           # Responsive hook
├── lib/
│   ├── api.ts                   # Axios instance
│   ├── apiMethods.ts            # API method definitions
│   ├── queryClient.ts           # TanStack Query config
│   └── utils.ts                 # Utility functions (cn)
├── sections/                    # Feature sections
│   ├── chart.tsx                # Donut chart component
│   └── graph.tsx                # Line chart component
├── types/
│   └── insights.ts              # TypeScript interfaces
└── assets/                      # Static assets
```

---

### intelli-landing (Next.js Marketing Site)

```
intelli-landing/src/
├── pages/                       # Next.js Pages Router
│   ├── _app.tsx                 # App wrapper
│   ├── _document.tsx            # HTML document
│   └── index.tsx                # Landing page
├── layout/                      # Page sections
│   ├── hero.tsx
│   ├── benefits.tsx
│   ├── products.tsx
│   ├── how-it-works.tsx
│   ├── get-started.tsx
│   ├── footer.tsx
│   ├── invite.tsx               # Request invite modal
│   └── inquiry.tsx              # Contact modal
├── components/                  # Reusable components
│   ├── button/
│   ├── header/
│   ├── input/
│   ├── modal/
│   └── textarea/
├── hooks/                       # Custom hooks
├── service/                     # API services
└── styles/                      # CSS files
```

---

## 📐 CODING STANDARDS & PATTERNS

### Python (Django/FastAPI)

#### Naming Conventions
```python
# Models: Singular, CamelCase
class Service(models.Model):
    pass

class CampaignTracker(models.Model):
    pass

# Functions: snake_case
def process_datasync(datasync_id: int) -> None:
    pass

# Constants: UPPER_SNAKE_CASE
MAX_RETRY_COUNT = 3
DEFAULT_PAGE_SIZE = 100

# Private methods: _leading_underscore
def _validate_internal_state(self) -> bool:
    pass
```

#### Model Pattern (Django)
```python
from django.db import models
from core.models import BaseAbstractModel
import reversion

@reversion.register()  # Automatic audit trail
class MyModel(BaseAbstractModel):
    """
    Docstring describing model purpose.
    
    Relationships:
    - ForeignKey to Service
    - ManyToMany to Products
    """
    institution = models.ForeignKey(
        "organization.Institution",
        on_delete=models.CASCADE,
        related_name="my_models"
    )
    
    name = models.CharField(max_length=255)
    description = models.TextField(blank=True)
    is_active = models.BooleanField(default=True)
    
    class Meta:
        db_table = "myapp_mymodel"
        ordering = ["-created"]
        indexes = [
            models.Index(fields=["institution", "created"]),
        ]
    
    def __str__(self):
        return f"{self.name} ({self.institution.business_name})"
```

#### ViewSet Pattern (DRF)
```python
from rest_framework import viewsets
from rest_framework.decorators import action
from rest_framework.response import Response
from drf_spectacular.utils import extend_schema, extend_schema_view

@extend_schema_view(
    list=extend_schema(summary="List all items"),
    retrieve=extend_schema(summary="Get item detail"),
)
class MyViewSet(viewsets.ModelViewSet):
    """
    API endpoint for MyModel CRUD operations.
    """
    queryset = MyModel.objects.all()
    serializer_class = MySerializer
    permission_classes = [IsAuthenticated, IsOrgStaff]
    filterset_fields = ["is_active", "institution"]
    
    def get_queryset(self):
        """Filter by user's institution."""
        user = self.request.user
        institution_id = user.staff_profile.institution_id
        return MyModel.objects.filter(institution_id=institution_id)
    
    @action(detail=True, methods=["post"])
    @extend_schema(
        summary="Custom action",
        request=CustomRequestSerializer,
        responses={200: CustomResponseSerializer}
    )
    def custom_action(self, request, pk=None):
        instance = self.get_object()
        # Process custom logic
        serializer = CustomResponseSerializer(data)
        return Response(serializer.data)
```

#### Celery Task Pattern
```python
from celery import shared_task
import logging

logger = logging.getLogger(__name__)

@shared_task(
    bind=True,
    max_retries=3,
    default_retry_delay=60,
    queue="default"
)
def my_task(self, arg1: int, arg2: str) -> dict:
    """
    Task description.
    
    Args:
        arg1: Description of arg1
        arg2: Description of arg2
    
    Returns:
        Dict with task result
    """
    try:
        # Task logic
        logger.info(f"Processing task with {arg1}, {arg2}")
        result = process_something(arg1, arg2)
        return {"status": "success", "result": result}
    
    except Exception as exc:
        logger.error(f"Task failed: {exc}")
        raise self.retry(exc=exc)
```

#### FastAPI Route Pattern
```python
from fastapi import APIRouter, HTTPException, Depends
from pydantic import BaseModel
import logging

logger = logging.getLogger(__name__)
router = APIRouter()

class MyRequest(BaseModel):
    field1: str
    field2: int

class MyResponse(BaseModel):
    status: str
    data: dict

@router.post("/my-endpoint", response_model=MyResponse)
async def my_endpoint(
    request: MyRequest,
    db: Session = Depends(get_db)
):
    """
    Endpoint description.
    
    Args:
        request: Request body
        db: Database session
    
    Returns:
        Response with processed data
    """
    try:
        logger.info(f"Processing request: {request.field1}")
        
        # Business logic
        result = await process_async(request, db)
        
        return MyResponse(
            status="success",
            data=result
        )
    
    except ValueError as e:
        logger.error(f"Validation error: {e}")
        raise HTTPException(status_code=400, detail=str(e))
    
    except Exception as e:
        logger.exception("Unexpected error")
        raise HTTPException(status_code=500, detail="Internal server error")
```

---

### TypeScript/React (Frontend)

#### Naming Conventions
```typescript
// Interfaces/Types: PascalCase
interface User {
  id: number;
  firstName: string;
}

type ServiceStats = {
  revenue: number;
  subscriptions: number;
};

// Components: PascalCase
const MyComponent: React.FC<Props> = () => { };

// Functions: camelCase
const fetchUserData = async () => { };

// Constants: UPPER_SNAKE_CASE
const MAX_PAGE_SIZE = 100;

// Private variables: _leadingUnderscore (discouraged, use closures)
```

#### Component Pattern
```typescript
"use client";

import React, { useState, useEffect } from "react";
import { Button } from "@/components/ui/button";
import { useMyData } from "@/hooks/useMyData";

interface MyComponentProps {
  id: string;
  onSuccess?: () => void;
}

export const MyComponent: React.FC<MyComponentProps> = ({ id, onSuccess }) => {
  const [isLoading, setIsLoading] = useState(false);
  
  const { data, isLoading: queryLoading, error } = useMyData(id);
  
  useEffect(() => {
    // Side effect logic
  }, [id]);
  
  const handleClick = async () => {
    setIsLoading(true);
    try {
      await performAction();
      onSuccess?.();
    } catch (err) {
      console.error(err);
    } finally {
      setIsLoading(false);
    }
  };
  
  if (queryLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <div className="flex flex-col gap-4">
      <h2 className="text-xl font-semibold">{data?.title}</h2>
      <Button onClick={handleClick} disabled={isLoading}>
        {isLoading ? "Processing..." : "Submit"}
      </Button>
    </div>
  );
};
```

#### Custom Hook Pattern (TanStack Query)
```typescript
import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";
import { api } from "@/lib/api";

interface MyData {
  id: number;
  name: string;
}

// Query hook
export const useMyData = (id: string) => {
  return useQuery<MyData>({
    queryKey: ["myData", id],
    queryFn: async () => {
      const response = await api.get(`/my-endpoint/${id}`);
      return response.data;
    },
    enabled: !!id && id.trim().length > 0,
    staleTime: 1000 * 60 * 5, // 5 minutes
    retry: 1,
  });
};

// Mutation hook
export const useUpdateMyData = () => {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: async (data: Partial<MyData>) => {
      const response = await api.patch(`/my-endpoint/${data.id}`, data);
      return response.data;
    },
    onSuccess: (data) => {
      // Invalidate related queries
      queryClient.invalidateQueries({ queryKey: ["myData", data.id] });
    },
  });
};
```

#### Zustand Store Pattern
```typescript
import { create } from "zustand";
import { persist } from "zustand/middleware";

interface MyStore {
  value: string;
  count: number;
  setValue: (value: string) => void;
  increment: () => void;
  reset: () => void;
}

export const useMyStore = create<MyStore>()(
  persist(
    (set) => ({
      value: "",
      count: 0,
      
      setValue: (value) => set({ value }),
      
      increment: () => set((state) => ({ count: state.count + 1 })),
      
      reset: () => set({ value: "", count: 0 }),
    }),
    {
      name: "my-store", // localStorage key
    }
  )
);
```

---

## 🔌 API REFERENCE

### Authentication

#### POST /login/
**Request:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123"
}
```

**Response:**
```json
{
  "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "staff_profile": {
      "id": 1,
      "first_name": "John",
      "last_name": "Doe",
      "staff_email": "user@example.com",
      "institutions": [
        {
          "institution_id": 1,
          "institution_name": "ACME Corp",
          "role": "Admin"
        }
      ]
    }
  }
}
```

**Headers:**
- `Content-Type: application/json`

---

#### POST /api/v1/ums/reset-password-request/
**Request:**
```json
{
  "email": "user@example.com"
}
```

**Response:**
```json
{
  "message": "Password reset email sent."
}
```

---

### Organization Management

#### GET /api/v1/organization/
**Description:** List all organizations for authenticated user.

**Query Parameters:**
- `page` (int): Page number (default: 1)
- `limit` (int): Items per page (default: 100)

**Response:**
```json
{
  "count": 2,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": 1,
      "icode": "ACME001",
      "business_name": "ACME Corp",
      "verified": true,
      "onboarded": true,
      "owner": {
        "id": 1,
        "email": "owner@acme.com"
      },
      "created": "2026-01-01T00:00:00Z"
    }
  ]
}
```

---

#### GET /api/v1/organization/{id}/dashboard-stats/
**Description:** Get institution-level KPIs.

**Query Parameters:**
- `date_range` (str): `today`, `yesterday`, `7days`, `30days`, `custom`
- `start` (ISO date): Start date (required if `custom`)
- `end` (ISO date): End date (required if `custom`)
- `api_key` (str, optional): Service API key (for service-scoped access)

**Response:**
```json
{
  "revenue": 150000.50,
  "new_subscriptions": 1250,
  "active_subscriptions": 5400,
  "deactivations": 180,
  "renewals": 2100,
  "conversion_rate": 12.5,
  "churn_rate": 3.2
}
```

---

### Service Management

#### GET /api/v1/service/{id}/stats/
**Description:** Get service-level KPIs.

**Query Parameters:**
- `date_range` (str): `today`, `yesterday`, `7days`, `30days`, `custom`
- `start` (ISO date): Start date (required if `custom`)
- `end` (ISO date): End date (required if `custom`)
- `api_key` (str, optional): Service API key

**Response:**
```json
{
  "service_stats": {
    "revenue": 45000.00,
    "new_subscriptions": 320,
    "active_subscriptions": 1800,
    "deactivations": 45,
    "renewals": 680,
    "channel_breakdown": {
      "Organic": {
        "count": 150,
        "revenue": 12000.00
      },
      "Paid": {
        "count": 170,
        "revenue": 33000.00
      }
    }
  },
  "graph": [
    {
      "hour": "00:00",
      "value": 12
    },
    {
      "hour": "01:00",
      "value": 8
    }
  ]
}
```

---

#### GET /api/v1/service/{id}/performance-metrics/
**Description:** Get service performance trends over time.

**Query Parameters:**
- `date_range`, `start`, `end` (as above)
- `granularity` (str): `hour`, `day`, `week`, `month`
- `api_key` (str, optional)

**Response:**
```json
{
  "metrics": [
    {
      "date": "2026-04-01",
      "revenue": 1500.00,
      "subscriptions": 45,
      "renewals": 120,
      "churn": 8
    },
    {
      "date": "2026-04-02",
      "revenue": 1800.00,
      "subscriptions": 52,
      "renewals": 135,
      "churn": 6
    }
  ]
}
```

---

### Marketing Analytics

#### GET /api/v1/service/{id}/marketing/provider-summary/
**Description:** Get marketing provider performance summary.

**Query Parameters:**
- `date_range`, `start`, `end`, `api_key` (as above)
- `provider` (str, optional): Filter by specific provider

**Response:**
```json
{
  "providers": [
    {
      "provider": "mobplus",
      "partner_name": "MobPlus Marketing",
      "hits": 15000,
      "conversions": 1850,
      "conversion_rate": 12.3,
      "revenue": 92500.00,
      "spend": 45000.00,
      "cpa": 24.32,
      "profit": 47500.00
    }
  ]
}
```

---

#### GET /api/v1/service/{id}/marketing/fraud-analysis/stats/
**Description:** Get fraud detection statistics by provider.

**Query Parameters:**
- `date_range`, `start`, `end`, `api_key` (as above)

**Response:**
```json
{
  "fraud_stats": [
    {
      "provider": "mobplus",
      "fraudulent_count": 45,
      "legitimate_count": 1805,
      "fraud_rate": 2.43,
      "total_activations": 1850
    }
  ]
}
```

---

### Customer Discovery

#### GET /api/v1/service/customer-discovery/customers/
**Description:** Get customer list with subscription details.

**Query Parameters:**
- `institution_id` (int, required)
- `page` (int): Page number
- `limit` (int): Items per page (10, 20, 50, 100, 200)
- `search` (str): MSISDN search
- `api_key` (str, optional)

**Response:**
```json
{
  "count": 1500,
  "next": "https://api.intellihq.net/api/v1/service/customer-discovery/customers/?page=2",
  "previous": null,
  "results": [
    {
      "msisdn": "2348012345678",
      "total_revenue": 1200.00,
      "subscription_count": 4,
      "first_subscription_date": "2025-12-01T10:00:00Z",
      "last_subscription_date": "2026-03-15T14:30:00Z",
      "status": "active"
    }
  ]
}
```

---

#### GET /api/v1/service/customer-discovery/stats/trends/
**Description:** Get customer trends over time.

**Query Parameters:**
- `institution_id` (int, required)
- `date_range`, `start`, `end` (as above)
- `granularity` (str): `day`, `week`, `month`
- `api_key` (str, optional)

**Response:**
```json
{
  "trends": [
    {
      "date": "2026-04-01",
      "total_msisdns": 1500,
      "active_msisdns": 1350,
      "inactive_msisdns": 150
    },
    {
      "date": "2026-04-02",
      "total_msisdns": 1520,
      "active_msisdns": 1370,
      "inactive_msisdns": 150
    }
  ]
}
```

---

### Data Sync (Telco Ingestion)

#### POST /api/v1/datasync/sync-notification/
**Description:** Ingest telco subscription/billing notification.

**Authentication:** API Key (query param or header)

**Request:**
```json
{
  "service_id": 1,
  "phone": "2348012345678",
  "product_id": "PREMIUM",
  "sub_date": "2026-04-09T10:00:00Z",
  "sub_expiry": "2026-05-09T10:00:00Z",
  "amount": 500.00,
  "campaign_tracker_id": 123456,
  "telco": "MTN",
  "transaction_type": "subscription"
}
```

**Response:**
```json
{
  "id": 789012,
  "status": "created",
  "message": "Subscription notification received"
}
```

**Side Effects:**
- Creates `Datasync` record
- Links to `CampaignTracker` if `campaign_tracker_id` provided
- Marks `CampaignTracker.converted = True`
- Triggers Celery tasks: `process_datasync`, `process_successful_subscription_sms`, `process_provider_postback`

---

### intelli-promo Endpoints

#### GET /web/{service_id}/{provider}/
**Description:** Campaign entry point (creates click tracking).

**Query Parameters:**
- `pubid` (str): Publisher/marketer ID
- `click_id` (str): Unique click identifier
- `telco` (str): Telco operator (MTN, GLO, etc.)
- `antifraud` (str): Anti-fraud provider (envina, secured)

**Response:** HTTP 302 Redirect to MSISDN capture or service landing page

**Side Effects:**
- Creates `CampaignTracker` record
- Geo-enrichment (IP → country, city, ASN)
- Duplicate detection (per MSISDN)

---

#### POST /msisdn-claim
**Description:** MSISDN capture callback (Bearer token auth).

**Headers:**
- `Authorization: Bearer <MSISDN_CLAIM_TOKEN>`

**Request:**
```json
{
  "state": "abc123def456",
  "msisdn": "2348012345678"
}
```

**Response:**
```json
{
  "status": "success",
  "message": "MSISDN claimed"
}
```

**Side Effects:**
- Stores MSISDN in Redis (600s TTL)

---

#### GET /return
**Description:** Return from MSISDN capture (polls Redis for MSISDN).

**Query Parameters:**
- `state` (str): Signed state from campaign entry

**Response:** HTTP 302 Redirect to service landing page or error page

**Logic:**
1. Verify state signature (HMAC-SHA256)
2. Poll Redis for MSISDN (~1.2s)
3. Process campaign validation with full context
4. Redirect to service landing page

---

## 🗃️ DATABASE SCHEMA REFERENCE

### Primary Database Models

#### Institution
```sql
CREATE TABLE organization_institution (
    id SERIAL PRIMARY KEY,
    icode VARCHAR(10) UNIQUE NOT NULL,
    business_name VARCHAR(255) NOT NULL,
    business_email VARCHAR(255),
    phone_number VARCHAR(20),
    cac_number VARCHAR(50),
    industry VARCHAR(100),
    website VARCHAR(255),
    logo TEXT,
    owner_id INTEGER REFERENCES ums_user(id),
    verified BOOLEAN DEFAULT FALSE,
    onboarded BOOLEAN DEFAULT FALSE,
    created TIMESTAMP DEFAULT NOW(),
    modified TIMESTAMP DEFAULT NOW()
);
```

**Indexes:**
- `UNIQUE (icode)`
- `idx_institution_owner (owner_id)`

**Relationships:**
- Owner → `ums_user` (ForeignKey)
- Services → `service_service` (Reverse FK)
- Staff → `organization_staff` (Reverse FK)

---

#### Service
```sql
CREATE TABLE service_service (
    id SERIAL PRIMARY KEY,
    institution_id INTEGER NOT NULL REFERENCES organization_institution(id),
    service_name VARCHAR(255) NOT NULL,
    service_url TEXT,
    api_key VARCHAR(255) UNIQUE,
    service_id_key VARCHAR(100),
    service_secret VARCHAR(255),
    manage_antifraud_resolve BOOLEAN DEFAULT FALSE,
    fraudulent_check BOOLEAN DEFAULT FALSE,
    is_active BOOLEAN DEFAULT TRUE,
    health_check_url TEXT,
    health_check_interval INTEGER DEFAULT 1800,  -- 30 min
    alert_emails TEXT,
    created TIMESTAMP DEFAULT NOW(),
    modified TIMESTAMP DEFAULT NOW()
);
```

**Indexes:**
- `UNIQUE (api_key)`
- `idx_service_institution (institution_id, created)`
- `idx_service_active (is_active, created)`

**Relationships:**
- Institution → `organization_institution` (ForeignKey)
- Products → `service_product` (Reverse FK)
- CampaignURLs → `service_campaignurl` (Reverse FK)
- Marketers → `service_marketer` (Reverse FK)

---

#### User (UMS)
```sql
CREATE TABLE ums_user (
    id SERIAL PRIMARY KEY,
    uuid UUID UNIQUE DEFAULT uuid_generate_v4(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(128) NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    phone VARCHAR(20),
    photo TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    is_staff BOOLEAN DEFAULT FALSE,
    is_superuser BOOLEAN DEFAULT FALSE,
    email_verified BOOLEAN DEFAULT FALSE,
    phone_verified BOOLEAN DEFAULT FALSE,
    password_change_required BOOLEAN DEFAULT FALSE,
    last_login TIMESTAMP,
    created TIMESTAMP DEFAULT NOW(),
    modified TIMESTAMP DEFAULT NOW()
);
```

**Indexes:**
- `UNIQUE (email)`
- `UNIQUE (uuid)`
- `idx_user_active (is_active, created)`

---

### Datasync Database Models

#### CampaignTracker
```sql
CREATE TABLE marketing_campaigntracker (
    id BIGSERIAL PRIMARY KEY,
    service_id INTEGER NOT NULL,
    click_id VARCHAR(255),
    provider VARCHAR(100),
    msisdn VARCHAR(20),
    telco VARCHAR(50),
    ip_address INET,
    country VARCHAR(100),
    region VARCHAR(100),
    city VARCHAR(100),
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8),
    timezone VARCHAR(100),
    asn INTEGER,
    as_org VARCHAR(255),
    converted BOOLEAN DEFAULT FALSE,
    converted_at TIMESTAMP,
    antifraud_result VARCHAR(50),
    redirected_tranx_id VARCHAR(255),
    created TIMESTAMP DEFAULT NOW(),
    modified TIMESTAMP DEFAULT NOW()
);
```

**Indexes:**
- `UNIQUE (service_id, click_id, provider)`
- `idx_campaign_service_created (service_id, created)`
- `idx_campaign_msisdn (msisdn)`
- `idx_campaign_converted (converted, converted_at)`

**Constraints:**
- Unique per (service_id, click_id, provider)

---

#### Datasync
```sql
CREATE TABLE datasync_datasync (
    id BIGSERIAL PRIMARY KEY,
    service_id INTEGER NOT NULL,
    campaign_tracker_id BIGINT REFERENCES marketing_campaigntracker(id),
    phone VARCHAR(20),
    product_id VARCHAR(100),
    sub_date TIMESTAMP,
    sub_expiry TIMESTAMP,
    amount DECIMAL(10, 2),
    telco VARCHAR(50),
    transaction_type VARCHAR(50),
    billing_type VARCHAR(50),
    status VARCHAR(50),
    created TIMESTAMP DEFAULT NOW(),
    modified TIMESTAMP DEFAULT NOW()
);
```

**Indexes:**
- `ds_service_created_idx (service_id, created)`
- `ds_campaign_tracker_idx (campaign_tracker_id)`
- `ds_phone_idx (phone)`
- `ds_created_brin_idx USING BRIN (created)` (range index)
- Partial index: `(service_id, created) WHERE created > NOW() - INTERVAL '90 days'`

**Retention Policy:** 90 days (auto-deleted by scheduled task)

---

#### Subscribtion
```sql
CREATE TABLE marketing_subscribtion (
    id BIGSERIAL PRIMARY KEY,
    service_id INTEGER NOT NULL,
    msisdn VARCHAR(20) NOT NULL,
    sub_active BOOLEAN DEFAULT TRUE,
    sub_status VARCHAR(50),
    traffic_source VARCHAR(100),
    telco VARCHAR(50),
    starts_date TIMESTAMP,
    churn_date TIMESTAMP,
    auto_renewal BOOLEAN DEFAULT TRUE,
    created TIMESTAMP DEFAULT NOW(),
    modified TIMESTAMP DEFAULT NOW()
);
```

**Indexes:**
- `idx_sub_service_msisdn (service_id, msisdn)`
- `idx_sub_status (service_id, sub_status, starts_date)`
- `idx_sub_active (sub_active, churn_date)`

---

#### ActivationNotification
```sql
CREATE TABLE datasync_activationnotification (
    id BIGSERIAL PRIMARY KEY,
    datasync_id BIGINT REFERENCES datasync_datasync(id),
    msisdn VARCHAR(20),
    subscription_id VARCHAR(255),
    activation VARCHAR(50),
    description TEXT,
    ren_flag VARCHAR(10),
    auto_renew VARCHAR(10),
    sdp_success VARCHAR(10),
    sequence_no VARCHAR(50),
    antifraud_platform VARCHAR(50),  -- ENVINA, MFILTER
    created TIMESTAMP DEFAULT NOW(),
    modified TIMESTAMP DEFAULT NOW()
);
```

**Indexes:**
- `idx_activation_datasync (datasync_id)`
- `idx_activation_msisdn (msisdn)`
- `idx_activation_description (description) WHERE description LIKE '%fraud_detected%'`

---

## 🧩 COMPONENT REFERENCE

### UI Components (intelli-assets)

#### Button
```typescript
import { Button } from "@/components/ui/button";

<Button variant="default" size="default" onClick={handleClick}>
  Click Me
</Button>

// Variants: default, destructive, outline, ghost, link
// Sizes: default, sm, lg, icon
```

---

#### Input
```typescript
import { Input } from "@/components/ui/input";

<Input
  type="text"
  placeholder="Enter text"
  value={value}
  onChange={(e) => setValue(e.target.value)}
/>
```

---

#### Select
```typescript
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";

<Select value={selected} onValueChange={setSelected}>
  <SelectTrigger>
    <SelectValue placeholder="Select option" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="option1">Option 1</SelectItem>
    <SelectItem value="option2">Option 2</SelectItem>
  </SelectContent>
</Select>
```

---

#### Table
```typescript
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table";

<Table>
  <TableHeader>
    <TableRow>
      <TableHead>Name</TableHead>
      <TableHead>Status</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    {data.map((row) => (
      <TableRow key={row.id}>
        <TableCell>{row.name}</TableCell>
        <TableCell>{row.status}</TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

---

#### Dialog (Modal)
```typescript
import { Dialog, DialogContent, DialogDescription, DialogHeader, DialogTitle, DialogTrigger } from "@/components/ui/dialog";

<Dialog open={isOpen} onOpenChange={setIsOpen}>
  <DialogTrigger asChild>
    <Button>Open Modal</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Modal Title</DialogTitle>
      <DialogDescription>Modal description</DialogDescription>
    </DialogHeader>
    {/* Modal content */}
  </DialogContent>
</Dialog>
```

---

#### Date Range Picker
```typescript
import { DateRange } from "@/components/ui/date-range";

<DateRange
  date={dateRange}
  setDate={setDateRange}
/>

// dateRange shape: { from: Date, to: Date }
```

---

### Custom Components

#### StatComponent
```typescript
import { StatComponent } from "@/app/admin/home/components/StatComponent";

<StatComponent
  title="Revenue"
  value={150000}
  trend={12.5}
  trendDirection="up"
  loading={false}
/>
```

---

#### KPICards
```typescript
import { KPICards } from "@/app/admin/insights/components/KPICards";

<KPICards
  hits={15000}
  conversions={1850}
  cr={12.3}
  cpa={24.32}
  revenue={92500}
  spend={45000}
  churnCount={180}
  churnRate={3.2}
/>
```

---

## ⚙️ TASK & BACKGROUND JOBS

### Celery Tasks (intelli-service)

#### Scheduled Tasks (Celery Beat)
```python
# app/settings/base.py
CELERY_BEAT_SCHEDULE = {
    "nightly-rollup-all-services": {
        "task": "service.tasks.rollup_all_services_for_yesterday",
        "schedule": crontab(hour=0, minute=10),
    },
    "health-check-all-services": {
        "task": "service.tasks.check_all_services_health_task",
        "schedule": crontab(minute="*/30"),
    },
    "cleanup-old-datasync-records": {
        "task": "datasync.tasks.cleanup_old_datasync_records",
        "schedule": crontab(hour=4, minute=0),
    },
    "reindex-datasync-indexes": {
        "task": "datasync.tasks.reindex_datasync_table",
        "schedule": crontab(day_of_month=2, hour=2, minute=0),
    },
    "rollup-datasync-every-5-mins": {
        "task": "datasync.tasks.rollup_last_3_days_incremental",
        "schedule": crontab(minute="*/5"),
    },
}
```

---

#### Task: `log_service_api_request`
**File:** `service/tasks.py`

```python
@shared_task(queue="default")
def log_service_api_request(
    api_key: str,
    path: str,
    method: str,
    status_code: int,
    response_time_ms: float
):
    """
    Log API request for billing/analytics.
    
    Determines price tier from URL pattern.
    """
    try:
        service = Service.objects.get(api_key=api_key)
        price_tier = determine_price_tier(path)
        
        ServiceApiRequestLog.objects.create(
            service=service,
            path=path,
            method=method,
            status_code=status_code,
            response_time_ms=response_time_ms,
            price_tier=price_tier,
        )
    except Service.DoesNotExist:
        logger.error(f"Service with API key {api_key} not found")
```

---

#### Task: `process_datasync`
**File:** `datasync/tasks.py`

```python
@shared_task(queue="ingest", max_retries=3)
def process_datasync(datasync_id: int):
    """
    Process datasync record: rollups, updates, linking.
    """
    try:
        datasync = Datasync.objects.using('datasync_db').get(id=datasync_id)
        
        # Link to campaign tracker if campaign_tracker_id provided
        if datasync.campaign_tracker_id:
            tracker = CampaignTracker.objects.using('datasync_db').get(
                id=datasync.campaign_tracker_id
            )
            tracker.converted = True
            tracker.converted_at = timezone.now()
            tracker.save()
        
        # Update subscription state
        update_subscription_state(datasync)
        
        # Trigger rollup
        rollup_service_incremental(datasync.service_id)
        
    except Datasync.DoesNotExist:
        logger.error(f"Datasync {datasync_id} not found")
```

---

#### Task: `cleanup_old_datasync_records`
**File:** `datasync/tasks.py`

```python
@shared_task(queue="maintenance")
def cleanup_old_datasync_records():
    """
    Delete Datasync records older than 90 days.
    Batch processing (50K rows/batch).
    """
    cutoff_date = timezone.now() - timedelta(days=90)
    
    while True:
        ids = Datasync.objects.using('datasync_db').filter(
            created__lt=cutoff_date
        ).values_list('id', flat=True)[:50000]
        
        ids_list = list(ids)
        if not ids_list:
            break
        
        Datasync.objects.using('datasync_db').filter(
            id__in=ids_list
        ).delete()
        
        logger.info(f"Deleted {len(ids_list)} old Datasync records")
```

---

### Celery Tasks (intelli-promo)

#### Task: `handle_msisdn_archive`
**File:** `tasks.py`

```python
@shared_task
def handle_msisdn_archive(campaign_tracker_id: int):
    """
    Geo-enrich IP address using MaxMind or fallback API.
    Cache in Redis for 7 days.
    """
    try:
        tracker = CampaignTracker.query.get(campaign_tracker_id)
        
        # Check Redis cache
        cache_key = f"geo:{tracker.ip_address}"
        cached = redis_client.get(cache_key)
        if cached:
            geo_data = json.loads(cached)
        else:
            # GeoIP lookup
            geo_data = lookup_geo(tracker.ip_address)
            redis_client.setex(cache_key, 7 * 24 * 3600, json.dumps(geo_data))
        
        # Update tracker
        tracker.country = geo_data.get("country")
        tracker.city = geo_data.get("city")
        tracker.asn = geo_data.get("asn")
        db.session.commit()
        
    except Exception as exc:
        logger.error(f"Geo-enrichment failed: {exc}")
```

---

## 🧪 TESTING GUIDELINES

### Unit Tests (Django)

```python
# tests/test_service_views.py
from django.test import TestCase
from rest_framework.test import APIClient
from service.models import Service
from organization.models import Institution
from ums.models import User

class ServiceViewSetTest(TestCase):
    def setUp(self):
        self.client = APIClient()
        
        # Create test user
        self.user = User.objects.create_user(
            email="test@example.com",
            password="testpass123"
        )
        
        # Create institution
        self.institution = Institution.objects.create(
            business_name="Test Org",
            owner=self.user
        )
        
        # Create service
        self.service = Service.objects.create(
            institution=self.institution,
            service_name="Test Service"
        )
        
        # Authenticate
        response = self.client.post("/login/", {
            "email": "test@example.com",
            "password": "testpass123"
        })
        self.token = response.data["access"]
        self.client.credentials(HTTP_AUTHORIZATION=f"Bearer {self.token}")
    
    def test_list_services(self):
        response = self.client.get("/api/v1/service/")
        self.assertEqual(response.status_code, 200)
        self.assertEqual(len(response.data["results"]), 1)
    
    def test_get_service_stats(self):
        response = self.client.get(
            f"/api/v1/service/{self.service.id}/stats/",
            {"date_range": "7days"}
        )
        self.assertEqual(response.status_code, 200)
        self.assertIn("revenue", response.data)
```

---

### Integration Tests (FastAPI)

```python
# tests/test_api.py
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_campaign_entry():
    response = client.get(
        "/web/1/mobplus/",
        params={
            "pubid": "123",
            "click_id": "abc123",
            "telco": "MTN",
            "antifraud": "envina"
        },
        follow_redirects=False
    )
    assert response.status_code == 302
    assert "Location" in response.headers

def test_msisdn_claim():
    response = client.post(
        "/msisdn-claim",
        json={
            "state": "valid_state_123",
            "msisdn": "2348012345678"
        },
        headers={"Authorization": f"Bearer {os.getenv('MSISDN_CLAIM_TOKEN')}"}
    )
    assert response.status_code == 200
    assert response.json()["status"] == "success"
```

---

### Frontend Tests (React Testing Library)

```typescript
// __tests__/components/StatComponent.test.tsx
import { render, screen } from "@testing-library/react";
import { StatComponent } from "@/app/admin/home/components/StatComponent";

describe("StatComponent", () => {
  it("renders stat value", () => {
    render(
      <StatComponent
        title="Revenue"
        value={150000}
        trend={12.5}
        trendDirection="up"
      />
    );
    
    expect(screen.getByText("Revenue")).toBeInTheDocument();
    expect(screen.getByText("150,000")).toBeInTheDocument();
    expect(screen.getByText("↑ 12.5%")).toBeInTheDocument();
  });
  
  it("shows loading skeleton", () => {
    render(
      <StatComponent
        title="Revenue"
        value={0}
        loading={true}
      />
    );
    
    expect(screen.queryByText("Revenue")).not.toBeInTheDocument();
    // Check for skeleton class
  });
});
```

---

## 🚀 DEPLOYMENT PROCEDURES

### Environment Setup

#### Production Environment Variables
```bash
# intelli-service
export DJANGO_SETTINGS_MODULE=app.settings.prod
export DEBUG=False
export ALLOWED_HOSTS=api.intellihq.net
export SECRET_KEY=<strong-random-key>

# Database
export PGHOST=db-primary.example.com
export DATASYNC_PGHOST=db-datasync.example.com
export REDIS_HOST=redis.example.com

# Email & SMS
export EMAIL_HOST_PASSWORD=<zeptomail-password>
export TERMII_TOKEN=<termii-token>

# Storage
export DO_SPACES_ACCESS_KEY_ID=<key>
export DO_SPACES_SECRET_ACCESS_KEY=<secret>
```

---

### Docker Deployment

#### Build Images
```bash
# intelli-service
cd intelli-service
docker build -t intelli-service:latest -f Dockerfile .

# intelli-promo
cd intelli-promo
docker build -t intelli-promo:latest -f Dockerfile .

# intelli-assets
cd intelli-assets
docker build -t intelli-assets:latest .

# intelli-landing
cd intelli-landing
docker build -t intelli-landing:latest .
```

---

#### Deploy with Docker Compose
```yaml
# docker-compose.deploy.yml
version: '3.8'

services:
  intelli-service:
    image: intelli-service:latest
    ports: ["8080:8080"]
    environment:
      - DJANGO_SETTINGS_MODULE=app.settings.prod
      - PGHOST=${PGHOST}
      - DATASYNC_PGHOST=${DATASYNC_PGHOST}
    depends_on:
      - postgres
      - redis
    restart: unless-stopped

  celery:
    image: intelli-service:latest
    command: celery -A app worker -Q default,notify,postback,ingest,maintenance,otp -l info
    environment:
      - DJANGO_SETTINGS_MODULE=app.settings.prod
    depends_on:
      - redis
      - postgres
    restart: unless-stopped

  celery-beat:
    image: intelli-service:latest
    command: celery -A app beat -l info
    environment:
      - DJANGO_SETTINGS_MODULE=app.settings.prod
    depends_on:
      - redis
      - postgres
    restart: unless-stopped

  intelli-promo:
    image: intelli-promo:latest
    ports: ["8000:8000"]
    environment:
      - APP_ENV=prod
    depends_on:
      - postgres
      - redis
    restart: unless-stopped

  intelli-assets:
    image: intelli-assets:latest
    ports: ["3001:3000"]
    environment:
      - NEXT_PUBLIC_SERVER_URL=https://api.intellihq.net
    restart: unless-stopped

  intelli-landing:
    image: intelli-landing:latest
    ports: ["3000:3000"]
    restart: unless-stopped

  postgres:
    image: postgres:16
    environment:
      - POSTGRES_DB=intelli_db
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
```

---

### Database Migrations

#### Django (intelli-service)
```bash
# Create migration
python manage.py makemigrations

# Apply migration
python manage.py migrate

# Apply to datasync_db
python manage.py migrate --database=datasync_db
```

#### Alembic (intelli-promo)
```bash
# Create migration
alembic revision --autogenerate -m "Description"

# Apply migration
alembic upgrade head
```

---

## 🔍 TROUBLESHOOTING GUIDE

### Common Issues

#### Issue: JWT Token Expired
**Symptoms:** 401 Unauthorized responses on intelli-assets

**Solution:**
```typescript
// Check token expiration
const token = localStorage.getItem("intelliJWT");
if (!token) {
  router.push("/auth/login");
}

// Refresh token flow
const refreshToken = localStorage.getItem("intelliRefreshJWT");
const response = await api.post("/api/v1/ums/token/refresh/", {
  refresh: refreshToken
});
localStorage.setItem("intelliJWT", response.data.access);
```

---

#### Issue: Celery Tasks Not Processing
**Symptoms:** Tasks stuck in queue, no logs

**Debug:**
```bash
# Check Celery workers
celery -A app inspect active

# Check queues
celery -A app inspect active_queues

# Restart workers
docker-compose restart celery celery-beat
```

---

#### Issue: Datasync Record Not Linking to CampaignTracker
**Symptoms:** `converted = False` despite subscription

**Debug:**
```python
# Check if campaign_tracker_id exists in Datasync
datasync = Datasync.objects.using('datasync_db').get(id=123)
print(datasync.campaign_tracker_id)

# Check if CampaignTracker exists
tracker = CampaignTracker.objects.using('datasync_db').get(
    id=datasync.campaign_tracker_id
)
print(tracker.converted, tracker.converted_at)

# Manually trigger conversion
tracker.converted = True
tracker.converted_at = timezone.now()
tracker.save()
```

---

#### Issue: Slow Dashboard Loading
**Symptoms:** intelli-assets dashboard takes >5s to load

**Debug:**
```bash
# Check API response times
curl -w "@curl-format.txt" -o /dev/null -s "https://api.intellihq.net/api/v1/organization/1/dashboard-stats/?date_range=7days"

# Check PostgreSQL slow queries
SELECT query, mean_exec_time, calls
FROM pg_stat_statements
ORDER BY mean_exec_time DESC
LIMIT 10;

# Check TanStack Query cache
// In browser console
console.log(queryClient.getQueryCache().getAll());
```

**Solution:**
- Add database indexes
- Increase TanStack Query `staleTime`
- Implement query result caching in Django

---

#### Issue: MSISDN Capture Flow Fails
**Symptoms:** Users redirected to error page after MSISDN capture

**Debug:**
```python
# Check Redis state
redis_client = redis.Redis(host='localhost', port=6379, db=0)
state_data = redis_client.get(f"msisdn_state:{state}")
print(state_data)

# Check state signature
expected_sig = hmac.new(
    MSISDN_STATE_SECRET.encode(),
    state.encode(),
    hashlib.sha256
).hexdigest()
print(expected_sig == provided_sig)
```

**Solution:**
- Verify `MSISDN_STATE_SECRET` matches across deployments
- Check Redis connectivity
- Increase `CLAIM_TTL_SEC` if users are slow

---

## ⚡ PERFORMANCE OPTIMIZATION

### Database Optimization

#### Index Management
```sql
-- Check index usage
SELECT
    schemaname,
    tablename,
    indexname,
    idx_scan,
    idx_tup_read,
    idx_tup_fetch
FROM pg_stat_user_indexes
WHERE idx_scan = 0
  AND indexname NOT LIKE '%_pkey';

-- Reindex large tables (monthly)
REINDEX TABLE datasync_datasync;
REINDEX TABLE marketing_campaigntracker;
```

---

#### Query Optimization
```python
# Bad: N+1 queries
services = Service.objects.all()
for service in services:
    print(service.institution.business_name)  # Extra query per service

# Good: select_related
services = Service.objects.select_related('institution').all()
for service in services:
    print(service.institution.business_name)  # No extra queries

# Bad: Multiple reverse FK queries
service = Service.objects.get(id=1)
products = service.products.all()  # Query
campaigns = service.campaign_urls.all()  # Query

# Good: prefetch_related
service = Service.objects.prefetch_related('products', 'campaign_urls').get(id=1)
products = service.products.all()  # No query (cached)
campaigns = service.campaign_urls.all()  # No query (cached)
```

---

### Frontend Optimization

#### Code Splitting
```typescript
// Dynamic import for heavy components
const HeavyChart = dynamic(() => import("@/components/HeavyChart"), {
  loading: () => <Skeleton />,
  ssr: false,
});
```

---

#### Memoization
```typescript
// Memoize expensive computations
const processedData = useMemo(() => {
  return data?.map(item => ({
    ...item,
    computed: expensiveComputation(item)
  }));
}, [data]);

// Memoize callbacks
const handleClick = useCallback(() => {
  performAction(id);
}, [id]);
```

---

#### TanStack Query Optimization
```typescript
// Prefetch on hover
const prefetchService = useQueryClient();

<Link
  href={`/analytics/view/${id}`}
  onMouseEnter={() => {
    prefetchService.prefetchQuery({
      queryKey: ["service", id],
      queryFn: () => getService(id),
    });
  }}
>
  View Service
</Link>
```

---

### API Optimization

#### Response Compression
```python
# Django settings
MIDDLEWARE = [
    'django.middleware.gzip.GZipMiddleware',  # Add compression
    # ... other middleware
]
```

---

#### Pagination & Filtering
```python
# Use cursor pagination for large datasets
from rest_framework.pagination import CursorPagination

class CustomerDiscoveryPagination(CursorPagination):
    page_size = 100
    ordering = "-created"
```

---

#### Caching
```python
from django.core.cache import cache

def get_service_stats(service_id, date_range):
    cache_key = f"service_stats:{service_id}:{date_range}"
    cached = cache.get(cache_key)
    if cached:
        return cached
    
    # Compute stats
    stats = compute_stats(service_id, date_range)
    
    # Cache for 5 minutes
    cache.set(cache_key, stats, 300)
    return stats
```

---

## 📞 SUPPORT & CONTACT

**Engineering Team:** dev@intellihq.net  
**Documentation:** https://docs.intellihq.net  
**Status Page:** https://status.intellihq.net

---

**Document Version:** 2.0  
**Last Updated:** April 9, 2026  
**Maintained By:** Engineering Team
