# Intelli Competitive Analysis & Feature Development Plan

**Date:** April 9, 2026  
**Prepared for:** Intelli Product Development Team  
**Focus:** Upstream Systems, MoEngage, CleverTap Comparison & Indian Market Analysis

---

## Executive Summary

This document provides a comprehensive competitive analysis comparing Intelli's current capabilities against leading mobile marketing automation platforms (Upstream Systems, MoEngage, CleverTap) and outlines a strategic feature development roadmap to enhance Intelli's market position.

**Key Findings:**
- **Intelli's Strengths:** Strong VAS analytics, fraud detection, telco-specific MSISDN capture, campaign tracking, multi-tenant architecture
- **Critical Gaps:** No visual campaign builder, limited automation workflows, no AI-powered features, basic channel orchestration
- **Market Opportunity:** Indian market has mature solutions (MoEngage, CleverTap) but lacks telco-VAS specific platforms like Intelli

---

## 1. COMPETITIVE LANDSCAPE ANALYSIS

### 1.1 Upstream Systems (Global - Mobile Marketing Leader)

**Business Model:** SaaS Platform + Managed Services  
**Scale:** 1.2B consumer reach, 3.1B monthly interactions, 370+ advertisers  
**Target Market:** Mobile operators, E-commerce, Digital service providers  

#### Core Offerings

| Category | Capabilities |
|----------|-------------|
| **Campaign Studio** | • Drag-and-drop multi-channel journey builder<br>• No-code campaign creation<br>• Content studio for creative assets<br>• Template library<br>• Real-time campaign preview |
| **Marketing Automation** | • Event-driven campaigns<br>• Smart triggers based on user behavior<br>• Cart/browser abandonment recovery<br>• Retargeting purchase paths<br>• Contextual messaging |
| **Channel Orchestration** | • SMS, RCS, Push, Web, App unified<br>• Works across devices/platforms<br>• Offline/out-of-data user support<br>• Multi-channel from single dashboard |
| **Audience Management** | • Advanced segmentation beyond basic datasets<br>• ID resolution (cross-channel user identification)<br>• MSISDN lookup & real-time actions<br>• Customer data platform capabilities |
| **Insights & Analytics** | • Campaign performance metrics (engagement, churn, ROI)<br>• User behavior tracking<br>• Performance-per-creative analysis<br>• Real-time optimization |
| **Ad Fraud Prevention (Secure-D)** | • Predictive ad blocking<br>• Behavioral pattern detection<br>• Real-time fraud blocking<br>• Device infection notifications |

#### Results Claimed
- 25x ROI for e-commerce clients (Jogê - 2 months)
- 600+ campaigns managed in <1 year (Vivo)
- 3.7M new paying subscribers (DTAC)
- 10.5% revenue saved via cart abandonment recovery

---

### 1.2 MoEngage (India-based - Cross-Channel Engagement)

**Business Model:** SaaS Platform  
**Scale:** 1,350+ global brands, 1.3B end consumers/month, 1T data points analyzed monthly  
**Target Market:** E-commerce, Financial Services, Media/Entertainment, Food & Beverage  
**Founded:** India (Global presence)  

#### Core Offerings

| Category | Capabilities |
|----------|-------------|
| **Merlin AI** | • **Predictive Segments:** Predict purchase, churn, affinity<br>• **Decisioning Agents:** 1:1 experience at scale with AI<br>• **Copywriter:** AI-powered email/message copy generation<br>• **Path Optimizer:** AI finds best conversion path<br>• **Segment Assist:** Natural language audience discovery<br>• **Flow Assist:** Create journey flows with prompts<br>• **Designer:** AI-generated on-brand visuals |
| **Cross-Channel Marketing** | • App notifications, Email, SMS, WhatsApp, Push<br>• 12+ channel support<br>• Real-time segmentation & engagement<br>• Preferred channel/time delivery<br>• Multi-touch attribution |
| **Analytics & Insights** | • 360° customer view<br>• AI-driven insights<br>• Advanced reporting dashboards<br>• User behavior analysis<br>• RFM segmentation |
| **Marketing Automation** | • Journey flows (visual builder)<br>• Event-triggered campaigns<br>• Behavioral automation<br>• Cart abandonment flows<br>• Lifecycle campaigns |
| **Personalization** | • Web & app personalization<br>• Dynamic content<br>• Product recommendations<br>• Personalized messaging at scale |
| **Data Management** | • Customer data platform (CDP)<br>• Unified data dashboard<br>• ETL/data cleansing<br>• Real-time data activation |

#### Results Claimed
- 300% growth in weekly database acquisition (AZADEA)
- 6x increase in retention rate (Speedi)
- 240% uplift in bookings (Gathern)
- 212% uplift in CRM-driven revenue (Xcite)
- 32.3% revenue uplift (V Perfumes)

---

### 1.3 CleverTap (India-based - Customer Lifecycle Management)

**Business Model:** All-in-One Engagement Platform  
**Scale:** 2000+ brands globally  
**Target Market:** E-commerce, Subscriptions, Financial Services, Gaming  
**Founded:** India (Now global)  

#### Core Offerings

| Category | Capabilities |
|----------|-------------|
| **CleverAI™** | • **AI Agents:** Autonomous campaign optimization<br>• **Predictions Agent:** Churn prediction, LTV forecasting<br>• Best time to send<br>• AI-driven segmentation<br>• Prescriptive intelligence |
| **Campaign Orchestration** | • Visual journey builder<br>• Multi-channel orchestration<br>• Event-triggered campaigns<br>• A/B testing & experimentation<br>• Dynamic content |
| **Channels** | • Push Notifications, Email, WhatsApp, SMS<br>• In-App Messages, Web Messaging<br>• RCS, Reminders<br>• Scribe (AI content generation)<br>• IntelliNODE (intelligent routing)<br>• RenderMax (personalized rendering) |
| **Customer Data & Analytics** | • Unified customer profiles<br>• Real-time analytics<br>• Behavioral analytics<br>• Funnel analysis<br>• Cohort analysis<br>• Product analytics |
| **Personalization** | • Real-time personalization<br>• Dynamic recommendations<br>• Behavioral targeting<br>• Geo-targeting<br>• Product experiences |
| **Experimentation** | • A/B/n testing<br>• Multivariate testing<br>• Feature flags<br>• Progressive rollouts |

#### Results Claimed
- 5x boost in retention (Tata CLiQ Luxury)
- 60% increase in CTRs using "Best Time" (ZEE5)
- 45% reduction in support tickets (Edenred)
- 40% uplift in push notification CTRs (Mobile Premier League)

---

## 2. INTELLI CURRENT CAPABILITIES ASSESSMENT

### 2.1 What Intelli Has (Strengths)

| Area | Current Capabilities | Maturity Level |
|------|---------------------|----------------|
| **Campaign Tracking** | ✅ Click-level attribution<br>✅ Geo-enrichment (IP → location)<br>✅ Duplicate detection<br>✅ Provider-level routing<br>✅ CampaignTracker model<br>✅ CampaignURL management | **Strong** |
| **Analytics & Reporting** | ✅ Service-level KPIs (revenue, subs, churn)<br>✅ Channel breakdown<br>✅ Time-series trends<br>✅ Marketing insights (top/bottom performers)<br>✅ Conversion rate analysis<br>✅ Provider drill-down<br>✅ Real-time dashboards | **Strong** |
| **Fraud Detection** | ✅ ENVINA integration<br>✅ SECURED platform integration<br>✅ Early fraud blocking<br>✅ Activation notification processing<br>✅ Duplicate tracking | **Strong** |
| **MSISDN Capture** | ✅ Multi-telco support (MTN, GLO, etc.)<br>✅ WiFi user handling<br>✅ State-based security (HMAC-SHA256)<br>✅ Redis-backed polling<br>⚠️ Telco-specific (Nigeria focus) | **Moderate** (Telco-specific) |
| **Multi-Tenancy** | ✅ Institution-based isolation<br>✅ Staff role-based access<br>✅ Organization switching<br>✅ API key management | **Strong** |
| **Subscription Management** | ✅ Lifecycle tracking<br>✅ Billing records<br>✅ Datasync integration<br>✅ Churn monitoring | **Strong** |
| **Customer Intelligence** | ✅ MSISDN-level analytics<br>✅ Customer segmentation<br>✅ Subscription history<br>✅ Trend analysis | **Moderate** |
| **Database Architecture** | ✅ Dual-database strategy<br>✅ Primary DB (config) + Datasync DB (events)<br>✅ PostgreSQL<br>✅ Write separation | **Strong** |

### 2.2 What Intelli Lacks (Critical Gaps)

| Area | Missing Capabilities | Impact | Priority |
|------|---------------------|--------|----------|
| **Visual Campaign Builder** | ❌ No drag-and-drop journey builder<br>❌ No visual workflow editor<br>❌ No template library<br>❌ No campaign preview | **HIGH** - Manual campaign setup, no self-service | **P0** |
| **Marketing Automation** | ❌ No event-driven automation<br>❌ No cart abandonment workflows<br>❌ No lifecycle campaigns<br>❌ No trigger-based messaging<br>❌ No retargeting flows | **HIGH** - Limited automation, manual intervention needed | **P0** |
| **AI/ML Capabilities** | ❌ No predictive analytics<br>❌ No churn prediction<br>❌ No AI-powered segmentation<br>❌ No smart send-time optimization<br>❌ No AI copywriting<br>❌ No path optimization | **HIGH** - Competitive disadvantage | **P1** |
| **Channel Orchestration** | ❌ Only tracking (no sending)<br>❌ No email integration<br>❌ No WhatsApp integration<br>❌ No push notification sending<br>❌ No in-app messaging<br>❌ No RCS support | **CRITICAL** - Intelli is analytics-only, not engagement | **P0** |
| **A/B Testing** | ❌ No experimentation framework<br>❌ No multivariate testing<br>❌ No winner auto-selection | **MEDIUM** - Can't optimize campaigns scientifically | **P2** |
| **Advanced Segmentation** | ⚠️ Basic filtering only<br>❌ No predictive segments<br>❌ No lookalike audiences<br>❌ No RFM automation<br>❌ No behavioral cohorts | **MEDIUM** - Limited targeting precision | **P1** |
| **Customer Data Platform** | ❌ No unified customer profile<br>❌ No cross-service identity resolution<br>❌ No data cleansing<br>❌ No external data enrichment | **MEDIUM** - Siloed customer data | **P2** |
| **Content Management** | ❌ No creative studio<br>❌ No asset library<br>❌ No dynamic content<br>❌ No personalized templates | **MEDIUM** - Marketers can't manage creatives | **P2** |
| **Real-Time Personalization** | ❌ No web personalization<br>❌ No dynamic recommendations<br>❌ No real-time offers | **LOW** - Nice-to-have for VAS | **P3** |

---

## 3. INDIAN MARKET COMPETITIVE LANDSCAPE

### 3.1 Major Players

| Platform | Origin | Strengths | Target Segments |
|----------|--------|-----------|-----------------|
| **MoEngage** | India (Bengaluru) | • Mature AI/ML capabilities<br>• Strong in MENA, SEA, India<br>• 1,350+ brands<br>• Series F funded ($180M) | E-commerce, Financial Services, Media |
| **CleverTap** | India (Mumbai) | • All-in-one platform<br>• 2000+ brands<br>• Strong mobile-first approach<br>• Global presence | E-commerce, Subscriptions, Gaming |
| **WebEngage** | India (Mumbai) | • Cross-channel CDP<br>• Journey designer<br>• Revenue attribution<br>• Strong in India | E-commerce, SaaS, EdTech |
| **Netcore Cloud** | India (Mumbai) | • Email focus with expansion<br>• AI-powered Raman platform<br>• Older player (2000s) | E-commerce, BFSI, Travel |

### 3.2 Market Gaps & Intelli's Opportunity

**What Indian Platforms Do Well:**
- Mature visual campaign builders
- Multi-channel orchestration
- AI-powered segmentation & predictions
- Cross-industry adaptability
- Self-service SaaS model

**What They Lack (Intelli's Advantage):**
- ❌ **Telco VAS Specialization** - None are VAS-specific
- ❌ **MSISDN Capture Integration** - No telco SIM-based capture
- ❌ **Fraud Detection for VAS** - Generic fraud prevention, not VAS-specific
- ❌ **Nigerian Market Expertise** - Limited Africa focus
- ❌ **Subscription Lifecycle for VAS** - E-commerce focused, not recurring VAS
- ❌ **Marketing Partner Payouts** - No CPA/CPL payout automation

**Strategic Positioning:**
Intelli can position as **"The VAS Marketing Platform"** - combining:
1. Intelli's VAS domain expertise (fraud, MSISDN, telco partnerships)
2. Competitor's campaign automation & AI capabilities
3. Africa/emerging market focus

---

## 4. FEATURE GAP ANALYSIS

### 4.1 Gap Matrix

| Feature Category | Upstream | MoEngage | CleverTap | Intelli | Gap Score |
|-----------------|----------|----------|-----------|---------|-----------|
| Visual Campaign Builder | ✅ Strong | ✅ Strong | ✅ Strong | ❌ None | **10/10** |
| Marketing Automation | ✅ Strong | ✅ Strong | ✅ Strong | ❌ None | **10/10** |
| AI/ML Features | ⚠️ Moderate | ✅ Advanced | ✅ Advanced | ❌ None | **9/10** |
| Channel Sending (Email, SMS, Push) | ✅ Strong | ✅ Strong | ✅ Strong | ❌ None | **10/10** |
| A/B Testing | ✅ Yes | ✅ Yes | ✅ Advanced | ❌ None | **8/10** |
| Advanced Segmentation | ✅ Strong | ✅ AI-powered | ✅ AI-powered | ⚠️ Basic | **7/10** |
| Customer Data Platform | ✅ Yes | ✅ Strong | ✅ Strong | ❌ None | **7/10** |
| Analytics & Reporting | ✅ Strong | ✅ Strong | ✅ Strong | ✅ Strong | **2/10** |
| Fraud Detection | ✅ Strong | ⚠️ Basic | ⚠️ Basic | ✅ VAS-specific | **-2/10** (Advantage) |
| MSISDN Capture | ⚠️ Generic | ❌ None | ❌ None | ✅ Telco-integrated | **-5/10** (Advantage) |
| VAS Subscription Mgmt | ⚠️ Generic | ⚠️ Generic | ⚠️ Generic | ✅ VAS-specific | **-3/10** (Advantage) |
| Marketer Payout Automation | ⚠️ Basic | ❌ None | ❌ None | ✅ Yes | **-2/10** (Advantage) |

**Gap Score Legend:**
- **10/10** = Critical gap, urgent priority
- **7-9/10** = Major gap, high priority
- **4-6/10** = Moderate gap
- **1-3/10** = Minor gap
- **Negative** = Intelli advantage

---

## 5. FEATURE DEVELOPMENT ROADMAP

### Phase 1: Foundation (Q2-Q3 2026) - **Campaign Automation MVP**
**Goal:** Enable self-service campaign creation with basic automation

#### 5.1.1 Visual Campaign Builder (8 weeks)

**User Story:** *"As a marketer, I want to create multi-step campaigns visually without developer help"*

**Components:**
1. **Journey Canvas (React Flow)**
   - Drag-and-drop node-based editor
   - Nodes: Trigger, Wait, Condition, Action, End
   - Visual connectors with logic (if/else paths)
   - Auto-save & version history
   
2. **Trigger Library**
   - User subscribed to service
   - User clicked campaign link
   - User churned (no activity for X days)
   - Subscription renewal approaching
   - Custom API event
   
3. **Action Nodes**
   - Send SMS (via Twilio/Africa's Talking integration)
   - Send Email (via SendGrid integration)
   - Wait (delay for X hours/days)
   - Update customer attribute
   - End journey
   
4. **Condition Nodes**
   - Filter by: Telco, location, service, subscription status
   - Time-based: Is business hours, is weekend, is specific date
   - Engagement: Clicked link, opened email, converted
   
5. **Template Library**
   - Pre-built: Welcome series, Win-back campaign, Renewal reminder
   - Industry templates: VAS subscription onboarding, Churn prevention
   
**Technical Implementation:**
```typescript
// New Models
interface Campaign {
  id: number;
  institution_id: number;
  service_id: number;
  name: string;
  status: 'draft' | 'active' | 'paused' | 'completed';
  trigger: CampaignTrigger;
  nodes: CampaignNode[];
  edges: CampaignEdge[];
  created_by: number;
  created_at: Date;
  updated_at: Date;
}

interface CampaignNode {
  id: string;
  type: 'trigger' | 'action' | 'condition' | 'wait' | 'end';
  config: NodeConfig;
  position: { x: number; y: number };
}

interface CampaignTrigger {
  type: 'subscription' | 'click' | 'churn' | 'api_event' | 'scheduled';
  config: TriggerConfig;
}
```

**Database Schema:**
```sql
CREATE TABLE campaign_campaigns (
    id BIGSERIAL PRIMARY KEY,
    institution_id INTEGER REFERENCES organization_institution(id),
    service_id INTEGER REFERENCES service_service(id),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    status VARCHAR(20) DEFAULT 'draft',
    trigger_type VARCHAR(50),
    trigger_config JSONB,
    nodes JSONB,
    edges JSONB,
    created_by INTEGER REFERENCES ums_user(id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE campaign_executions (
    id BIGSERIAL PRIMARY KEY,
    campaign_id BIGINT REFERENCES campaign_campaigns(id),
    user_msisdn VARCHAR(20),
    current_node VARCHAR(100),
    state JSONB,
    status VARCHAR(20),
    started_at TIMESTAMP,
    completed_at TIMESTAMP
);

CREATE TABLE campaign_node_logs (
    id BIGSERIAL PRIMARY KEY,
    execution_id BIGINT REFERENCES campaign_executions(id),
    node_id VARCHAR(100),
    node_type VARCHAR(50),
    action_result JSONB,
    executed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**API Endpoints:**
```python
# Backend (Django)
POST   /api/campaigns/                    # Create campaign
GET    /api/campaigns/                    # List campaigns
GET    /api/campaigns/{id}/               # Get campaign details
PUT    /api/campaigns/{id}/               # Update campaign
DELETE /api/campaigns/{id}/               # Delete campaign
POST   /api/campaigns/{id}/activate/      # Activate campaign
POST   /api/campaigns/{id}/pause/         # Pause campaign
GET    /api/campaigns/{id}/analytics/     # Campaign performance
GET    /api/campaigns/templates/          # Get templates
```

**Celery Tasks:**
```python
@shared_task
def execute_campaign_node(execution_id, node_id):
    """Execute a single campaign node"""
    execution = CampaignExecution.objects.get(id=execution_id)
    node = get_node_from_campaign(execution.campaign, node_id)
    
    if node.type == 'action':
        if node.config.action == 'send_sms':
            send_sms_via_provider(
                to=execution.user_msisdn,
                message=render_template(node.config.message, execution.state)
            )
        elif node.config.action == 'send_email':
            # Email sending logic
            pass
    elif node.type == 'wait':
        # Schedule next node execution
        execute_campaign_node.apply_async(
            args=[execution_id, node.next_node],
            countdown=node.config.wait_seconds
        )
    elif node.type == 'condition':
        # Evaluate condition and route
        next_node = evaluate_condition(node, execution)
        execute_campaign_node.delay(execution_id, next_node)
    
    # Log execution
    CampaignNodeLog.objects.create(
        execution_id=execution_id,
        node_id=node_id,
        node_type=node.type,
        action_result={...}
    )
```

---

#### 5.1.2 SMS & Email Channel Integration (4 weeks)

**Integrations:**

1. **SMS Providers**
   - Africa's Talking (primary - Africa focus)
   - Twilio (backup/global)
   - Termii (Nigeria-specific)
   
2. **Email Providers**
   - SendGrid (primary)
   - Amazon SES (backup)
   - Already have ZeptoMail (enhance)

**Features:**
- Template management with variables `{{customer_name}}`
- Delivery tracking & webhooks
- Bounce/complaint handling
- Rate limiting per provider
- Cost tracking per message

**Database Schema:**
```sql
CREATE TABLE campaign_message_logs (
    id BIGSERIAL PRIMARY KEY,
    campaign_id BIGINT REFERENCES campaign_campaigns(id),
    execution_id BIGINT REFERENCES campaign_executions(id),
    channel VARCHAR(20),  -- 'sms', 'email', 'whatsapp'
    recipient VARCHAR(255),
    message_id VARCHAR(255),  -- Provider message ID
    provider VARCHAR(50),
    status VARCHAR(50),  -- 'queued', 'sent', 'delivered', 'failed', 'bounced'
    cost_ngn DECIMAL(10, 4),
    sent_at TIMESTAMP,
    delivered_at TIMESTAMP,
    error_message TEXT,
    metadata JSONB
);

CREATE INDEX idx_message_logs_campaign_id ON campaign_message_logs(campaign_id);
CREATE INDEX idx_message_logs_status ON campaign_message_logs(status);
```

---

#### 5.1.3 Basic Triggered Campaigns (6 weeks)

**Pre-built Triggers:**

1. **Welcome Campaign**
   - Trigger: New subscription created
   - Flow: SMS confirmation → Wait 1 day → Email with tips → Wait 3 days → SMS usage reminder
   
2. **Churn Prevention**
   - Trigger: No activity for 7 days
   - Flow: SMS re-engagement → Wait 2 days → Email with offer → Wait 3 days → SMS final attempt
   
3. **Renewal Reminder**
   - Trigger: Subscription expires in 3 days
   - Flow: SMS reminder → Wait 1 day → Email with renewal link → Wait 1 day → SMS final reminder

**Implementation:**
```python
# Trigger detection via Celery Beat
@periodic_task(run_every=crontab(minute='*/15'))
def detect_churn_triggers():
    """Find users inactive for 7 days and trigger churn campaigns"""
    inactive_users = Subscription.objects.filter(
        last_activity__lt=timezone.now() - timedelta(days=7),
        status='active'
    ).exclude(
        msisdn__in=CampaignExecution.objects.filter(
            campaign__trigger_type='churn',
            created_at__gte=timezone.now() - timedelta(days=7)
        ).values('user_msisdn')
    )
    
    churn_campaign = Campaign.objects.get(
        trigger_type='churn',
        status='active'
    )
    
    for subscription in inactive_users:
        CampaignExecution.objects.create(
            campaign=churn_campaign,
            user_msisdn=subscription.msisdn,
            state={'subscription_id': subscription.id},
            status='active'
        )
        execute_campaign_node.delay(execution.id, churn_campaign.entry_node)
```

---

### Phase 2: Intelligence (Q4 2026) - **AI-Powered Optimization**
**Goal:** Add predictive analytics and smart automation

#### 5.2.1 Churn Prediction Model (8 weeks)

**Features:**
- ML model trained on historical churn data
- Churn risk score (0-100) per subscriber
- Auto-segment: "High Risk", "Medium Risk", "Low Risk"
- Trigger campaigns automatically for high-risk users

**Data Features:**
```python
# Feature engineering
features = [
    'days_since_subscription',
    'total_sessions',
    'sessions_last_7_days',
    'sessions_last_30_days',
    'avg_session_duration',
    'last_session_days_ago',
    'billing_amount',
    'failed_payments_count',
    'service_type',
    'telco',
    'geo_location',
    'acquisition_channel',
    'campaign_interactions_count',
    'email_open_rate',
    'sms_click_rate'
]

target = 'churned_within_30_days'  # Binary classification
```

**Model Stack:**
- **Algorithm:** XGBoost or LightGBM (handles imbalanced data well)
- **Training:** Monthly retraining on past 12 months data
- **Inference:** Daily batch scoring for all active subscribers
- **Storage:** New `CustomerRiskScore` model

**Database Schema:**
```sql
CREATE TABLE ml_customer_risk_scores (
    id BIGSERIAL PRIMARY KEY,
    msisdn VARCHAR(20) NOT NULL,
    service_id INTEGER REFERENCES service_service(id),
    churn_probability DECIMAL(5, 4),  -- 0.0000 to 1.0000
    risk_segment VARCHAR(20),  -- 'high', 'medium', 'low'
    feature_importance JSONB,
    scored_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    model_version VARCHAR(50)
);

CREATE INDEX idx_risk_scores_msisdn ON ml_customer_risk_scores(msisdn);
CREATE INDEX idx_risk_scores_segment ON ml_customer_risk_scores(risk_segment);
```

**API Integration:**
```python
GET /api/ml/churn-predictions/?service_id=1&risk_segment=high
GET /api/ml/churn-predictions/{msisdn}/
POST /api/ml/retrain-model/  # Admin only
```

---

#### 5.2.2 Send-Time Optimization (6 weeks)

**Features:**
- Analyze historical engagement by time-of-day & day-of-week
- Per-user optimal send time
- Auto-delay campaign messages to optimal times

**Implementation:**
```python
# Analyze engagement patterns
def calculate_optimal_send_time(msisdn):
    engagement_data = CampaignMessageLog.objects.filter(
        recipient=msisdn,
        status='delivered'
    ).annotate(
        hour=ExtractHour('sent_at'),
        day_of_week=ExtractWeekDay('sent_at'),
        engaged=(
            Q(clicked=True) | Q(converted=True)
        )
    ).values('hour', 'day_of_week').annotate(
        total=Count('id'),
        engaged_count=Count('id', filter=Q(engaged=True))
    ).annotate(
        engagement_rate=F('engaged_count') * 100.0 / F('total')
    ).order_by('-engagement_rate')
    
    optimal = engagement_data.first()
    return {
        'hour': optimal['hour'],
        'day_of_week': optimal['day_of_week'],
        'engagement_rate': optimal['engagement_rate']
    }
```

---

#### 5.2.3 AI-Powered Segmentation (8 weeks)

**Features:**
1. **Lookalike Modeling**
   - Upload high-value customer list
   - Find similar users algorithmically
   
2. **RFM Automation**
   - Auto-segment by Recency, Frequency, Monetary
   - "Champions", "Loyal Customers", "At Risk", "Lost"
   
3. **Behavioral Cohorts**
   - High engagers, Low engagers, Weekend users, etc.
   
4. **Natural Language Segments** (Future)
   - "Show me users who subscribed in last 30 days but haven't used the service"

---

### Phase 3: Omnichannel (Q1 2027) - **Multi-Channel Orchestration**
**Goal:** Expand beyond SMS/Email to become true omnichannel platform

#### 5.3.1 WhatsApp Business API Integration (8 weeks)

**Provider Options:**
- Twilio WhatsApp API
- 360Dialog
- MessageBird

**Features:**
- Template message approval flow
- Rich media (images, documents, buttons)
- Interactive messages (quick replies, buttons)
- Two-way conversations
- WhatsApp Business Account management

---

#### 5.3.2 Push Notification System (6 weeks)

**Stack:**
- Firebase Cloud Messaging (FCM) for Android
- Apple Push Notification Service (APNS) for iOS
- OneSignal as aggregator (optional)

**Requirements:**
- Intelli mobile app (future roadmap)
- Partner app SDK integration

---

#### 5.3.3 RCS Business Messaging (4 weeks)

**Why RCS:**
- Rich content (images, carousels, buttons)
- Read receipts
- Suggested actions
- Growing adoption in Africa (MTN, Vodacom support)

**Provider:**
- Africa's Talking RCS (Nigeria, Kenya)
- Twilio Conversations API

---

### Phase 4: Advanced Features (Q2-Q3 2027)

#### 5.4.1 A/B Testing Framework
- Test subject lines, message content, send times
- Auto-winner selection
- Statistical significance calculation

#### 5.4.2 Customer Data Platform (CDP)
- Unified customer profiles across services
- Cross-service identity resolution
- External data source integration (CRM, billing systems)
- Data cleansing & deduplication

#### 5.4.3 Content Studio
- Creative asset library (images, videos, templates)
- WYSIWYG email/sms template editor
- Dynamic content blocks
- Brand guideline enforcement

#### 5.4.4 Advanced Analytics
- Attribution modeling (multi-touch)
- Customer journey analytics
- Funnel visualization
- Predictive LTV

---

## 6. TECHNICAL ARCHITECTURE UPDATES

### 6.1 New Service: intelli-engage (Campaign Execution Engine)

**Purpose:** Separate campaign execution from core service to handle high-volume message sending

**Tech Stack:**
- **Framework:** FastAPI (async for performance)
- **Queue:** Redis + Celery (message scheduling)
- **Database:** PostgreSQL (campaign definitions) + Redis (execution state)
- **Channels:** Twilio, SendGrid, Africa's Talking, WhatsApp APIs

**Architecture:**
```
┌────────────────────────────────────────────────────────┐
│                  INTELLI ECOSYSTEM V3                   │
└────────────────────────────────────────────────────────┘

┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│  intelli-    │         │  intelli-    │         │  intelli-    │
│  assets      │────────▶│  service     │◀────────│  promo       │
│              │         │              │         │              │
│  Dashboard   │         │  Core API    │         │  Tracking    │
└──────────────┘         └──────────────┘         └──────────────┘
        │                        │                        │
        │                        ▼                        │
        │               ┌──────────────────┐             │
        └──────────────▶│  intelli-engage  │◀────────────┘
                        │                  │
                        │  Campaign Engine │
                        │  - Execution     │
                        │  - Channels      │
                        │  - ML Models     │
                        └──────────────────┘
                                 │
                  ┌──────────────┼──────────────┐
                  ▼              ▼              ▼
            ┌──────────┐  ┌──────────┐  ┌──────────┐
            │ Twilio/  │  │ SendGrid │  │ Africa's │
            │ WhatsApp │  │  Email   │  │ Talking  │
            └──────────┘  └──────────┘  └──────────┘
```

---

### 6.2 Database Updates

**New Databases:**
1. **Campaign DB** (Primary) - Campaign definitions, templates
2. **Execution DB** (High-volume) - Campaign executions, message logs
3. **ML Models DB** - Feature store, predictions, model metadata

**Partitioning Strategy:**
```sql
-- Partition message logs by month for performance
CREATE TABLE campaign_message_logs (
    ...
) PARTITION BY RANGE (sent_at);

CREATE TABLE campaign_message_logs_2026_04 PARTITION OF campaign_message_logs
FOR VALUES FROM ('2026-04-01') TO ('2026-05-01');
```

---

### 6.3 API Rate Limiting

**Provider Limits:**
- Twilio SMS: 200 msg/sec (default), upgradeable
- SendGrid Email: 100 msg/sec (free tier), 10k+/sec (paid)
- Africa's Talking: Varies by country

**Implementation:**
```python
# Redis-based rate limiter
from django_ratelimit.decorators import ratelimit

@ratelimit(key='user', rate='1000/hour', method='POST')
def send_campaign_message(request):
    ...
```

---

## 7. GO-TO-MARKET STRATEGY

### 7.1 Pricing Model

**Tier 1: Analytics (Current Intelli)**
- $199/month base
- Service analytics, fraud detection, MSISDN capture
- Campaign click tracking
- Up to 3 services

**Tier 2: Engage (New)**
- $499/month base + usage
- Everything in Analytics
- Visual campaign builder
- SMS: $0.02/msg, Email: $0.0001/msg
- Triggered campaigns (5 templates)
- Up to 10 services

**Tier 3: Enterprise (New)**
- $1,499/month base + usage
- Everything in Engage
- AI-powered churn prediction
- Send-time optimization
- WhatsApp Business API
- Unlimited services
- Dedicated support

---

### 7.2 Target Customers

**Phase 1 (2026):** Nigerian VAS providers (existing customers)
**Phase 2 (2027):** African telcos (MTN, Airtel, Glo, 9mobile)
**Phase 3 (2027+):** E-commerce (Jumia, Konga), Fintech (Paystack, Flutterwave), EdTech

---

### 7.3 Competitive Positioning

**vs. MoEngage/CleverTap:**
- ✅ 10x cheaper for African market
- ✅ VAS-specific features (MSISDN capture, telco fraud)
- ✅ Nigerian market expertise
- ✅ Local payment methods
- ⚠️ Smaller channel support (initially)

**vs. Upstream Systems:**
- ✅ Self-service SaaS (no managed service needed)
- ✅ Transparent pricing
- ✅ Faster deployment
- ⚠️ Smaller scale (for now)

---

## 8. RESOURCE REQUIREMENTS

### 8.1 Team Expansion

| Role | Quantity | Timing | Justification |
|------|----------|--------|---------------|
| **Backend Engineer (Python)** | 2 | Q2 2026 | Campaign engine, ML pipeline |
| **Frontend Engineer (React)** | 1 | Q2 2026 | Campaign builder UI |
| **ML Engineer** | 1 | Q3 2026 | Churn prediction, send-time optimization |
| **DevOps Engineer** | 1 | Q3 2026 | Scaling message delivery, monitoring |
| **Product Manager** | 1 | Q2 2026 | Campaign product ownership |
| **UX Designer** | 1 (part-time) | Q2 2026 | Campaign builder UX |

**Total Investment:** ~$500K/year (Africa salary rates)

---

### 8.2 Infrastructure Costs

| Component | Monthly Cost | Annual Cost |
|-----------|--------------|-------------|
| **SendGrid** (100K emails/month) | $20 | $240 |
| **Africa's Talking** (100K SMS/month) | $2,000 | $24,000 |
| **Twilio** (backup, 20K SMS/month) | $400 | $4,800 |
| **AWS RDS** (PostgreSQL upgrade) | $300 | $3,600 |
| **AWS Elasticache** (Redis) | $100 | $1,200 |
| **Celery Workers** (5x instances) | $250 | $3,000 |
| **ML Training** (GPU instances) | $200 | $2,400 |
| **WhatsApp Business API** (Q1 2027) | $500 | $6,000 |

**Total Infrastructure:** ~$3,770/month = **$45,240/year**

---

### 8.3 ROI Projection

**Assumptions:**
- 20 customers upgrade to Engage tier (year 1)
- 5 enterprise customers (year 2)
- Average SMS volume: 50K/month per customer

**Revenue:**
- Year 1: 20 × $499 × 12 = $119,760 + usage fees (~$40K) = **$160K**
- Year 2: 30 × $499 × 12 + 5 × $1,499 × 12 + usage (~$100K) = **$369K**

**Break-even:** 18 months

---

## 9. SUCCESS METRICS

### 9.1 Product Metrics (6 months post-launch)

| Metric | Target | Measurement |
|--------|--------|-------------|
| **Campaigns Created** | 500+ | Track campaign_campaigns table |
| **Active Campaigns** | 100+ | Count status='active' |
| **Messages Sent/Month** | 1M+ | Sum campaign_message_logs |
| **Campaign Conversion Rate** | >5% | Conversions / Messages Sent |
| **Automated Campaign %** | >60% | Triggered / Total Campaigns |
| **User Adoption Rate** | >70% | Users creating campaigns / Total users |

---

### 9.2 Business Metrics (12 months)

| Metric | Target |
|--------|--------|
| **Customers on Engage Tier** | 25+ |
| **MRR Growth** | +$12K |
| **Churn Reduction** | >30% (for customers using churn campaigns) |
| **Customer NPS** | >40 |

---

## 10. RISKS & MITIGATION

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| **SMS Cost Overruns** | High | Medium | Implement strict rate limits, cost alerts, quota management |
| **Provider API Downtime** | High | Low | Multi-provider failover, queue retry logic |
| **GDPR/Data Privacy** | High | Low | Consent management, data retention policies, encryption |
| **Feature Complexity** | Medium | High | MVP first, phased rollout, user testing |
| **Resource Constraints** | High | Medium | Hire incrementally, outsource ML initially |
| **Competition** | Medium | High | Focus on VAS niche, leverage existing customers |

---

## 11. NEXT STEPS (IMMEDIATE ACTIONS)

### Week 1-2: Discovery & Validation
- [ ] Customer interviews (10 existing customers) - validate demand
- [ ] Feature prioritization workshop with team
- [ ] Technical feasibility assessment
- [ ] Budget approval

### Week 3-4: Planning
- [ ] Detailed technical design doc (campaign engine)
- [ ] UI/UX wireframes for campaign builder
- [ ] Choose SMS/email providers (RFP)
- [ ] Hire product manager (start recruitment)

### Week 5-8: Foundation
- [ ] Setup intelli-engage service (skeleton)
- [ ] Integrate Africa's Talking SMS API
- [ ] Integrate SendGrid Email API
- [ ] Database schema implementation

### Week 9-16: Campaign Builder MVP
- [ ] Build React Flow campaign canvas
- [ ] Implement trigger detection (Celery tasks)
- [ ] Build execution engine
- [ ] Testing & QA

### Week 17-20: Beta Launch
- [ ] Invite 5 pilot customers
- [ ] Monitor performance & costs
- [ ] Gather feedback
- [ ] Iterate

---

## CONCLUSION

Intelli has a **strong foundation** in VAS analytics and fraud detection but **critical gaps** in campaign automation, AI/ML, and channel orchestration compared to competitors like Upstream Systems, MoEngage, and CleverTap.

**Strategic Recommendation:**
1. **Invest aggressively** in Phase 1 (Campaign Automation MVP) to close the gap
2. **Leverage existing strengths** (VAS expertise, African market, fraud prevention)
3. **Differentiate** as "The VAS Marketing Platform" vs. generic engagement platforms
4. **Price competitively** for African market (10x cheaper than MoEngage/CleverTap)
5. **Expand gradually** to omnichannel in 2027 after validating core automation

**Expected Outcome:**
- 3x revenue growth within 18 months
- Market leadership position in African VAS marketing automation
- Foundation for expansion to e-commerce, fintech, and other verticals

---

**Prepared by:** Intelli Product Team  
**Review Date:** April 9, 2026  
**Next Review:** July 1, 2026
