# Comprehensive Unified Development Plan: Acme CRM

# 1. EXECUTIVE SUMMARY

Acme CRM is a mid-sized customer relationship management platform designed to streamline contact management, deal tracking, and pipeline reporting for growing businesses. The system leverages a modern technology stack including Next.js 14.0+ with TypeScript for the frontend, Node.js 20+ with Express for the backend API, PostgreSQL 15+ as the database with Prisma ORM, containerized with Docker, and deployed on AWS infrastructure. This solution provides businesses with a comprehensive, scalable CRM platform that enhances customer relationship management, improves sales pipeline visibility, and delivers actionable business insights through advanced reporting capabilities.

**Primary Stakeholders:** Sales teams, marketing professionals, business executives, IT administrators
**End Users:** Sales representatives, account managers, marketing coordinators, sales managers

# 2. MVP CORE GOALS

## Primary Objectives

• **Contact Management Excellence:** Implement comprehensive contact management supporting 10,000+ contacts with real-time search, advanced filtering, and relationship mapping capabilities
• **Pipeline Visualization:** Develop interactive deal pipeline with drag-and-drop functionality supporting 5+ customizable stages and automated stage progression
• **Deal Tracking Precision:** Enable complete deal lifecycle management with value tracking, probability scoring, and automated follow-up reminders
• **Reporting Dashboard:** Create real-time analytics dashboard with 10+ pre-built reports including conversion rates, pipeline health, and performance metrics
• **Multi-user Collaboration:** Support 50+ concurrent users with role-based access control and real-time collaboration features
• **Data Import/Export:** Provide robust CSV/Excel import/export functionality supporting 10,000+ record batches
• **Performance Optimization:** Achieve sub-2-second page load times and handle 1,000+ concurrent API requests

## Non-goals (MVP Scope Exclusions)

• Advanced AI/ML predictive analytics
• Mobile native applications (web-responsive only)
• Third-party integrations (Salesforce, HubSpot, etc.)
• Advanced marketing automation workflows
• Voice/video calling integration
• Advanced document management
• Multi-language support
• Custom field types beyond basic text/number/date/select

# 3. COMPREHENSIVE SYSTEM ARCHITECTURE

## High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           AWS CLOUD INFRASTRUCTURE                         │
├─────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │   Route 53   │  │ CloudFront   │  │     WAF      │  │  Certificate │   │
│  │    (DNS)     │  │    (CDN)     │  │ (Security)   │  │   Manager    │   │
│  └──────────────┘  └──────────────┘  └──────────────┘  └──────────────┘   │
├─────────────────────────────────────────────────────────────────────────────┤
│                           APPLICATION LAYER                                │
├─────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────┐  ┌──────────────────────────────────┐ │
│  │        ALB (Load Balancer)       │  │         API Gateway              │ │
│  └──────────────────────────────────┘  └──────────────────────────────────┘ │
├─────────────────────────────────────────────────────────────────────────────┤
│                          COMPUTE LAYER                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐              │
│  │   ECS Fargate   │ │   ECS Fargate   │ │   ECS Fargate   │              │
│  │ (Next.js Frontend) │ (Express API)  │ (Background Jobs) │              │
│  │  - Next.js 14   │ │ - Node.js 20   │ │ - Node.js 20    │              │
│  │  - TypeScript   │ │ - Express 4.18 │ │ - Bull Queue    │              │
│  │  - Tailwind CSS │ │ - Prisma ORM   │ │ - Redis         │              │
│  └─────────────────┘ └─────────────────┘ └─────────────────┘              │
├─────────────────────────────────────────────────────────────────────────────┤
│                           DATA LAYER                                       │
├─────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────┐  ┌──────────────────────────────────┐ │
│  │         RDS PostgreSQL           │  │        ElastiCache Redis         │ │
│  │      - Primary Instance          │  │     - Session Storage            │ │
│  │      - Read Replicas (2)         │  │     - Cache Layer                │ │
│  │      - Automated Backups         │  │     - Rate Limiting              │ │
│  └──────────────────────────────────┘  └──────────────────────────────────┘ │
├─────────────────────────────────────────────────────────────────────────────┤
│                          STORAGE LAYER                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────┐  ┌──────────────────────────────────┐ │
│  │           S3 Buckets             │  │        CloudWatch Logs          │ │
│  │    - File Uploads                │  │     - Application Logs           │ │
│  │    - Static Assets               │  │     - Metrics & Monitoring       │ │
│  │    - Database Backups            │  │     - Error Tracking             │ │
│  └──────────────────────────────────┘  └──────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Component Details

### Frontend Layer (Next.js 14.0.3)
- **Technologies:** Next.js 14.0.3, TypeScript 5.2+, Tailwind CSS 3.3+, React 18.2+
- **Core Responsibilities:** 
  - Server-side rendering for optimal SEO and performance
  - Client-side routing and state management with Zustand
  - Real-time UI updates via WebSocket connections
  - Progressive Web App capabilities
- **Integration Points:** Express API via Axios HTTP client, WebSocket connection for real-time features

### API Layer (Express 4.18.2 + Node.js 20.9.0)
- **Technologies:** Node.js 20.9.0 LTS, Express 4.18.2, TypeScript 5.2+, Prisma 5.6+
- **Core Responsibilities:**
  - RESTful API endpoints with OpenAPI 3.0 documentation
  - JWT-based authentication and authorization
  - Rate limiting and request validation with Joi
  - Background job processing with Bull Queue
- **Integration Points:** PostgreSQL via Prisma ORM, Redis for caching and sessions

### Database Layer (PostgreSQL 15.4)
- **Technologies:** AWS RDS PostgreSQL 15.4, Prisma ORM 5.6+
- **Core Responsibilities:**
  - ACID-compliant transactional data storage
  - Advanced indexing strategies for query optimization
  - Connection pooling with PgBouncer
  - Automated backups and point-in-time recovery

### Infrastructure Layer (AWS + Docker)
- **Technologies:** Docker 24.0+, AWS ECS Fargate, AWS ALB, AWS RDS
- **Core Responsibilities:**
  - Container orchestration and auto-scaling
  - Load balancing and health monitoring
  - Secure network configuration with VPC
  - Monitoring and logging with CloudWatch

# 4. ROLES & PERMISSIONS MATRIX

| Feature/Action | Super Admin | Admin | Sales Manager | Sales Rep | Marketing User | Viewer |
|----------------|-------------|--------|---------------|-----------|----------------|---------|
| **User Management** |
| Create/Delete Users | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Edit User Roles | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| View All Users | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| **Contact Management** |
| Create Contacts | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Edit Own Contacts | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| Edit All Contacts | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Delete Contacts | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| View All Contacts | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |
| Import/Export Contacts | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ |
| **Deal Management** |
| Create Deals | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Edit Own Deals | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| Edit Team Deals | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Delete Deals | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| View Pipeline | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| **Reporting** |
| View Own Reports | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| View Team Reports | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |
| View Company Reports | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Create Custom Reports | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| **System Configuration** |
| Manage Pipeline Stages | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Configure Integrations | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ |
| System Settings | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |

## RBAC Enforcement Rules

```typescript
// middleware/rbac.ts
export interface Permission {
  resource: string;
  action: string;
  condition?: (user: User, resource: any) => boolean;
}

export const rolePermissions: Record<UserRole, Permission[]> = {
  SUPER_ADMIN: [{ resource: '*', action: '*' }],
  ADMIN: [
    { resource: 'users', action: 'create|read|update|delete' },
    { resource: 'contacts', action: '*' },
    { resource: 'deals', action: '*' },
    { resource: 'reports', action: '*' }
  ],
  SALES_MANAGER: [
    { resource: 'contacts', action: '*' },
    { resource: 'deals', action: '*' },
    { 
      resource: 'users', 
      action: 'read',
      condition: (user, targetUser) => user.teamId === targetUser.teamId 
    }
  ],
  SALES_REP: [
    { 
      resource: 'contacts', 
      action: 'create|read|update',
      condition: (user, contact) => contact.ownerId === user.id 
    },
    { 
      resource: 'deals', 
      action: 'create|read|update',
      condition: (user, deal) => deal.ownerId === user.id 
    }
  ]
};
```

# 5. COMPREHENSIVE DATA MODEL

## Core Entity Schemas (Prisma)

```typescript
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
  previewFeatures = ["fullTextSearch", "postgresqlExtensions"]
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
  extensions = [pg_trgm]
}

model User {
  id            String    @id @default(cuid())
  email         String    @unique
  firstName     String
  lastName      String
  role          UserRole  @default(SALES_REP)
  teamId        String?
  isActive      Boolean   @default(true)
  lastLoginAt   DateTime?
  passwordHash  String
  avatarUrl     String?
  timezone      String    @default("UTC")
  
  // Relationships
  team          Team?     @relation(fields: [teamId], references: [id])
  ownedContacts Contact[] @relation("ContactOwner")
  ownedDeals    Deal[]    @relation("DealOwner")
  activities    Activity[]
  sessions      Session[]
  
  // Timestamps
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
  
  @@map("users")
  @@index([email])
  @@index([teamId])
  @@index([role])
}

model Team {
  id          String @id @default(cuid())
  name        String
  description String?
  managerId   String
  isActive    Boolean @default(true)
  
  // Relationships
  members     User[]
  
  // Timestamps
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  
  @@map("teams")
}

model Contact {
  id              String            @id @default(cuid())
  firstName       String
  lastName        String
  email           String?
  phone           String?
  company         String?
  jobTitle        String?
  website         String?
  linkedinUrl     String?
  address         Json?             // { street, city, state, country, zipCode }
  tags            String[]          @default([])
  customFields    Json?             // Flexible custom data
  notes           String?
  leadScore       Int?              @default(0)
  leadSource      LeadSource?
  status          ContactStatus     @default(ACTIVE)
  ownerId         String
  
  // Relationships
  owner           User              @relation("ContactOwner", fields: [ownerId], references: [id])
  deals           Deal[]
  activities      Activity[]
  
  // Timestamps
  createdAt       DateTime          @default(now())
  updatedAt       DateTime          @updatedAt
  lastContactedAt DateTime?
  
  @@map("contacts")
  @@index([email])
  @@index([company])
