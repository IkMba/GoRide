# Ride-Sharing Platform: System Design

## High-Level Architecture
1. **Microservices Architecture**
   - User Service
   - Captain (Driver) Service
   - Ride Service
   - Payment Service
   - Notification Service
   - Geolocation Service

####  Payment service, Notifications service and Geolocation Service in progress

## Architecture Diagram
┌─────────────────────────────────────────────────────────────────┐
│                     API GATEWAY / LOAD BALANCER                 │
└───────────────────┬─────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────────────────────────┐
│                     RIDE SERVICE COMPONENTS                     │
│  ┌───────────────┐   ┌───────────────┐   ┌───────────────┐      │
│  │ Ride Request  │   │  Ride Matching│   │ Ride Tracking │      │
│  │ Management    │   │   Algorithm   │   │   Service     │      │
│  └───────┬───────┘   └───────┬───────┘   └───────┬───────┘      │
│          │                   │                   │              │
└──────────┼───────────────────┼───────────────────┼──────────────┘
           │                   │                   │
           ▼                   ▼                   ▼
┌─────────────────────────────────────────────────────────────────┐
│                      EXTERNAL INTEGRATIONS                      │
│  ┌───────────────┐   ┌───────────────┐   ┌───────────────┐      │
│  │ User Service  │   │ Captain Service│   │Payment Service│      │
│  └───────────────┘   └───────────────┘   └───────────────┘      │
└─────────────────────────────────────────────────────────────────┘
                    │           │           │
                    ▼           ▼           ▼
┌─────────────────────────────────────────────────────────────────┐
│                       DATA PERSISTENCE                          │
│  ┌───────────────┐   ┌───────────────┐   ┌───────────────┐      │
│  │  Ride Database│   │Location Cache │   │ Audit Logging │      │
│  │  (Mongo Db )  │   │   (Redis)     │   │(Elastic search)      │  
│  └───────────────┘   └───────────────┘   └───────────────┘      │
└─────────────────────────────────────────────────────────────────┘

## Database Relationship Diagram
┌───────────────┐        ┌───────────────┐
│     USERS     │        │    CAPTAINS   │
├───────────────┤        ├───────────────┤
│ PK: id        │        │ PK: id        │
│ first_name    │        │ first_name    │
│ last_name     │        │ last_name     │
│ email         │        │ email         │
│ phone_number  │        │ phone_number  │
│ account_status│        │ driver_license│
└───┬───────────┘        └───┬───────────┘
    │                        │
    │                        │
    │                        ▼
┌───▼───────────────┐    ┌───────────────┐
│     VEHICLES      │    │   RIDE TYPE   │
├───────────────────┤    ├───────────────┤
│ PK: id            │    │ standard      │
│ FK: captain_id    │    │ premium       │
│ make              │    │ shared        │
│ model             │    │ accessibility │
│ license_plate     │    └───────────────┘
│ vehicle_type      │
└───┬───────────────┘
    │
    ▼
┌───────────────────────────────┐
│            RIDES              │
├───────────────────────────────┤
│ PK: id                        │◄────┐
│ FK: user_id                   │     │
│ FK: captain_id                │     │
│ FK: vehicle_id                │     │
│ status                        │     │
│ ride_type                     │     │
│ pickup_location               │     │
│ dropoff_location              │     │
│ estimated_distance            │     │
│ total_fare                    │     │
│ requested_at                  │     │
│ completed_at                  │     │
└───────────────────────────────┘     │
    │                                 │
    ▼                                 │
┌───────────────┐        ┌────────────┴────────┐
│   PAYMENTS    │        │      RATINGS        │
├───────────────┤        ├────────────────────┤
│ PK: id        │        │ PK: id             │
│ FK: ride_id   │        │ FK: ride_id        │
│ FK: user_id   │        │ rating             │
│ payment_method│        │ review_text        │
│ total_amount  │        └────────────────────┘
└───────────────┘

## Key Design Principles
Microservices architecture
Event-driven design
Scalable and distributed
High availability
Real-time processing
Fault tolerance

## Technology Stack
Backend: Node.js
API Framework: Express.js
Database: Redis, MongoDB, Elastic Search
Message Queue: Apache Kafka/RabbitMq
Caching: Redis
Monitoring: Prometheus, Grafana

## Security Measures
- JWT-based authentication
- Role-based access control
- End-to-end encryption
- Regular security audits
- Rate limiting
- DDoS protection

## Potential Future Enhancements
- Containerization
- Adding payments, Notifications and Geolocation services
- Machine Learning for dynamic pricing
- Advanced fraud detection
- Accessibility features

  NB: please note that this project is currently in progress.
