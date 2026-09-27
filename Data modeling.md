# Entities, Objects and Relationships

We have, 
- Events
- Endpoints
- Delivery
- Relay strategy
- Notification/Delivery service

ERD :

┌────────────────────┐
│       users        │
├────────────────────┤
│ PK id              │
│ email              │
│ created_at         │
│ updated_at         │
└─────────┬──────────┘
          │
          │ 1:N
          ├──────────────────────┐
          │                      │
          ▼                      ▼
┌──────────────────┐    ┌────────────────────┐
│    api_keys      │    │     endpoints      │
├──────────────────┤    ├────────────────────┤
│ PK id            │    │ PK id              │
│ FK user_id       │    │ FK user_id         │
│ key_hash         │    │ url                │
│ name             │    │ status             │
│ created_at       │    │ created_at         │
│ revoked_at       │    │ updated_at         │
└──────────────────┘    └─────────┬──────────┘
                                  │
                                  │ 1:N
                                  ▼
                        ┌────────────────────┐
                        │   subscriptions    │
                        ├────────────────────┤
                        │ PK id              │
                        │ FK endpoint_id     │
                        │ event_type         │
                        │ created_at         │
                        └────────────────────┘


┌────────────────────┐
│       events       │
├────────────────────┤
│ PK id              │
│ FK user_id         │
│ event_type         │
│ payload            │
│ idempotency_key    │
│ created_at         │
└─────────┬──────────┘
          │
          │ 1:N
          ▼
┌────────────────────┐
│     deliveries     │
├────────────────────┤
│ PK id              │
│ FK event_id        │
│ FK endpoint_id     │
│ status             │
│ attempt_count      │
│ next_retry_at      │
│ created_at         │
│ updated_at         │
└─────────┬──────────┘
          │
          │ 1:N
          ▼
┌──────────────────────┐
│   delivery_attempts  │
├──────────────────────┤
│ PK id                │
│ FK delivery_id       │
│ attempt_number       │
│ status_code          │
│ response_body        │
│ error                │
│ duration_ms          │
│ created_at           │
└──────────────────────┘

### Question: Should events have user_id directly, or should ownership be inferred through another relationship?