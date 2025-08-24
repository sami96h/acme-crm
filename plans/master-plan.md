# Acme CRM - Comprehensive Development Plan

# 1. EXECUTIVE SUMMARY

**Acme CRM** is a comprehensive mid-sized customer relationship management platform designed to streamline sales operations through integrated contact management, deal tracking, pipeline visualization, and advanced reporting capabilities. The system will leverage a modern, scalable technology stack including Next.js 14.1 for the frontend, TypeScript 5.3 for type safety, Node.js 20.11 with Express 4.18 for the backend API, PostgreSQL 16.1 with Prisma 5.9 ORM for data management, all containerized with Docker 24.0 and deployed on AWS cloud infrastructure.

The platform targets sales teams, managers, and executives who require real-time visibility into customer relationships, sales performance, and revenue forecasting, delivering measurable improvements in sales efficiency and data-driven decision making.

# 2. MVP CORE GOALS

## Primary Objectives
- **Contact Management Excellence**: Implement comprehensive contact database supporting 10,000+ contacts with advanced search, filtering, and categorization capabilities, achieving <200ms query response times
- **Deal Pipeline Visualization**: Create intuitive drag-and-drop pipeline interface supporting 5+ customizable sales stages with real-time updates and progression tracking
- **Revenue Reporting Dashboard**: Deliver automated reporting system generating 15+ key sales metrics with data refresh intervals <5 minutes and export capabilities
- **User Authentication & Authorization**: Implement secure multi-role access control supporting Admin, Manager, and Sales Rep roles with JWT-based authentication and session management
- **Data Import/Export Functionality**: Enable bulk data operations supporting CSV/Excel formats for contacts and deals with validation and error handling
- **Mobile-Responsive Design**: Achieve 100% mobile compatibility across iOS/Android devices with PWA capabilities and offline data synchronization
- **Performance Standards**: Maintain application load times <3 seconds, API response times <500ms, and 99.5% uptime with comprehensive monitoring

## Non-goals
- Advanced marketing automation workflows
- Third-party CRM integrations (Salesforce, HubSpot)
- AI/ML-powered lead scoring
- Custom field builder for entities
- Multi-language localization
- Advanced financial forecasting models
- Video calling integration
- Social media monitoring

# 3. COMPREHENSIVE SYSTEM ARCHITECTURE

## High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        AWS CLOUD INFRASTRUCTURE                 │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐    ┌──────────────────┐   ┌──────────────┐ │
│  │   CloudFront    │    │   Application    │   │   RDS        │ │
│  │   (CDN/Cache)   │    │   Load Balancer  │   │ PostgreSQL   │ │
│  │                 │    │      (ALB)       │   │   Primary    │ │
│  └─────────────────┘    └──────────────────┘   └──────────────┘ │
│           │                       │                     │       │
│  ┌─────────────────┐    ┌──────────────────┐   ┌──────────────┐ │
│  │     S3 Bucket   │    │   ECS Cluster    │   │   RDS        │ │
│  │  (Static Assets)│    │   (Containers)   │   │ PostgreSQL   │ │
│  │                 │    │                  │   │  Read Replica │ │
│  └─────────────────┘    └──────────────────┘   └──────────────┘ │
│                                 │                               │
│         ┌───────────────────────┼───────────────────────┐       │
│         │                       │                       │       │
│  ┌─────────────┐    ┌─────────────────┐    ┌─────────────────┐   │
│  │   Next.js   │    │   Express API   │    │   Redis Cache  │   │
│  │  Frontend   │    │   Backend       │    │   (ElastiCache) │   │
│  │ (Container) │    │  (Container)    │    │                 │   │
│  └─────────────┘    └─────────────────┘    └─────────────────┘   │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │           MONITORING & LOGGING LAYER                        │ │
│  │  CloudWatch │ X-Ray │ ELK Stack │ Prometheus │ Grafana     │ │
│  └─────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

## Component Details

### Frontend Layer (Next.js 14.1 + TypeScript 5.3)
- **Core Responsibilities**: User interface rendering, client-side routing, state management, API integration
- **Technology Stack**: Next.js App Router, React 18.2, TypeScript, Tailwind CSS 3.4, Zustand state management
- **Integration Points**: REST API communication, WebSocket real-time updates, S3 file uploads
- **Performance Optimizations**: Server-side rendering, static generation, image optimization, code splitting

### API Gateway & Backend (Node.js 20.11 + Express 4.18)
- **Core Responsibilities**: Business logic processing, database operations, authentication, API endpoints
- **Technology Stack**: Express.js framework, Prisma ORM 5.9, JWT authentication, bcrypt encryption
- **Integration Points**: PostgreSQL database, Redis caching, AWS S3, email services (SES)
- **Scalability Features**: Horizontal scaling, connection pooling, rate limiting, request validation

### Database Layer (PostgreSQL 16.1 + Prisma 5.9)
- **Core Responsibilities**: Data persistence, transaction management, query optimization, relationship management
- **Technology Stack**: PostgreSQL with read replicas, Prisma ORM, connection pooling
- **Integration Points**: API backend, Redis caching, backup services, monitoring tools
- **Optimization Strategies**: Indexing, query optimization, partitioning, materialized views

### Infrastructure Layer (Docker 24.0 + AWS)
- **Core Responsibilities**: Container orchestration, service discovery, auto-scaling, load balancing
- **Technology Stack**: Docker containers, ECS Fargate, ALB, CloudFront, RDS, ElastiCache
- **Integration Points**: CI/CD pipelines, monitoring systems, logging aggregation, security services
- **Management Features**: Blue-green deployments, health checks, auto-scaling policies, disaster recovery

# 4. ROLES & PERMISSIONS MATRIX

| Feature | Admin | Sales Manager | Sales Rep | View Only |
|---------|-------|---------------|-----------|-----------|
| **Contact Management** |
| Create Contacts | ✅ | ✅ | ✅ | ❌ |
| Edit Own Contacts | ✅ | ✅ | ✅ | ❌ |
| Edit All Contacts | ✅ | ✅ | ❌ | ❌ |
| Delete Contacts | ✅ | ✅ | ❌ | ❌ |
| View All Contacts | ✅ | ✅ | ✅ | ✅ |
| Export Contacts | ✅ | ✅ | ✅ | ❌ |
| **Deal Management** |
| Create Deals | ✅ | ✅ | ✅ | ❌ |
| Edit Own Deals | ✅ | ✅ | ✅ | ❌ |
| Edit All Deals | ✅ | ✅ | ❌ | ❌ |
| Delete Deals | ✅ | ✅ | ❌ | ❌ |
| View Team Deals | ✅ | ✅ | ❌ | ✅ |
| Change Deal Stage | ✅ | ✅ | ✅ | ❌ |
| **Pipeline Management** |
| View Own Pipeline | ✅ | ✅ | ✅ | ✅ |
| View Team Pipeline | ✅ | ✅ | ❌ | ✅ |
| View All Pipelines | ✅ | ✅ | ❌ | ❌ |
| Configure Pipeline | ✅ | ✅ | ❌ | ❌ |
| **Reporting & Analytics** |
| View Own Reports | ✅ | ✅ | ✅ | ✅ |
| View Team Reports | ✅ | ✅ | ❌ | ✅ |
| View Company Reports | ✅ | ✅ | ❌ | ❌ |
| Export Reports | ✅ | ✅ | ✅ | ❌ |
| **System Administration** |
| User Management | ✅ | ❌ | ❌ | ❌ |
| Role Assignment | ✅ | ❌ | ❌ | ❌ |
| System Settings | ✅ | ❌ | ❌ | ❌ |
| Data Import/Export | ✅ | ✅ | ❌ | ❌ |
| Audit Logs | ✅ | ❌ | ❌ | ❌ |

## RBAC Enforcement Rules

```typescript
// Role-based access control middleware
export interface UserRole {
  id: string;
  name: 'ADMIN' | 'SALES_MANAGER' | 'SALES_REP' | 'VIEW_ONLY';
  permissions: Permission[];
}

export interface Permission {
  resource: 'CONTACT' | 'DEAL' | 'PIPELINE' | 'REPORT' | 'USER' | 'SYSTEM';
  action: 'CREATE' | 'READ' | 'UPDATE' | 'DELETE';
  scope: 'OWN' | 'TEAM' | 'ALL';
}

// Middleware implementation
export const checkPermission = (requiredPermission: Permission) => {
  return (req: AuthenticatedRequest, res: Response, next: NextFunction) => {
    const userRole = req.user.role;
    const hasPermission = userRole.permissions.some(permission => 
      permission.resource === requiredPermission.resource &&
      permission.action === requiredPermission.action &&
      (permission.scope === requiredPermission.scope || permission.scope === 'ALL')
    );
    
    if (!hasPermission) {
      return res.status(403).json({ error: 'Insufficient permissions' });
    }
    next();
  };
};
```

# 5. COMPREHENSIVE DATA MODEL

## Core Entity Schemas

### User Schema
```json
{
  "User": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "Primary key"
    },
    "email": {
      "type": "string",
      "format": "email",
      "unique": true,
      "maxLength": 255
    },
    "firstName": {
      "type": "string",
      "maxLength": 50,
      "required": true
    },
    "lastName": {
      "type": "string",
      "maxLength": 50,
      "required": true
    },
    "passwordHash": {
      "type": "string",
      "minLength": 60
    },
    "role": {
      "type": "string",
      "enum": ["ADMIN", "SALES_MANAGER", "SALES_REP", "VIEW_ONLY"]
    },
    "isActive": {
      "type": "boolean",
      "default": true
    },
    "lastLoginAt": {
      "type": "string",
      "format": "date-time"
    },
    "createdAt": {
      "type": "string",
      "format": "date-time"
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time"
    }
  }
}
```

### Contact Schema
```json
{
  "Contact": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "Primary key"
    },
    "firstName": {
      "type": "string",
      "maxLength": 50,
      "required": true
    },
    "lastName": {
      "type": "string",
      "maxLength": 50,
      "required": true
    },
    "email": {
      "type": "string",
      "format": "email",
      "maxLength": 255
    },
    "phone": {
      "type": "string",
      "maxLength": 20
    },
    "company": {
      "type": "string",
      "maxLength": 100
    },
    "jobTitle": {
      "type": "string",
      "maxLength": 100
    },
    "address": {
      "type": "object",
      "properties": {
        "street": {"type": "string", "maxLength": 255},
        "city": {"type": "string", "maxLength": 100},
        "state": {"type": "string", "maxLength": 50},
        "zipCode": {"type": "string", "maxLength": 10},
        "country": {"type": "string", "maxLength": 50}
      }
    },
    "tags": {
      "type": "array",
      "items": {"type": "string"},
      "maxItems": 10
    },
    "notes": {
      "type": "string",
      "maxLength": 2000
    },
    "source": {
      "type": "string",
      "enum": ["WEBSITE", "REFERRAL", "COLD_CALL", "EMAIL", "EVENT", "SOCIAL", "OTHER"]
    },
    "assignedToId": {
      "type": "string",
      "format": "uuid"
    },
    "createdById": {
      "type": "string",
      "format": "uuid",
      "required": true
    },
    "createdAt": {
      "type": "string",
      "format": "date-time"
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time"
    }
  }
}
```

### Deal Schema
```json
{
  "Deal": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "Primary key"
    },
    "title": {
      "type": "string",
      "maxLength": 200,
      "required": true
    },
    "description": {
      "type": "string",
      "maxLength": 2000
    },
    "value": {
      "type": "number",
      "minimum": 0,
      "multipleOf": 0.01
    },
    "currency": {
      "type": "string",
      "enum": ["USD", "EUR", "GBP", "CAD"],
      "default": "USD"
    },
    "stage": {
      "type": "string",
      "enum": ["LEAD", "QUALIFIED", "PROPOSAL", "NEGOTIATION", "CLOSED_WON", "CLOSED_LOST"]
    },
    "probability": {
      "type": "integer",
      "minimum": 0,
      "maximum": 100
    },
    "expectedCloseDate": {
      "type": "string",
      "format": "date"
    },
    "actualCloseDate": {
      "type": "string",
      "format": "date"
    },
    "contactId": {
      "type": "string",
      "format": "uuid",