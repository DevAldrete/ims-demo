---
id: IMS-24
title: "Implement Webhook event dispatcher for stock change events"
status: backlog
type: story
epic: EPIC-06
points: 5
priority: 3
labels: [webhooks, events]
order: 50
created: 2026-11-04
updated: 2026-11-04
---

## Context

Notify third-party subscribers in real time whenever stock levels cross safety thresholds or shipment milestones occur.

## Acceptance criteria

- [ ] Entity `WebhookSubscription` with TargetUrl, SecretKey, EventFilter (e.g. `stock.updated`, `po.received`, `order.shipped`)
- [ ] Asynchronous task queue handler dispatching HTTP POST payloads with HMAC SHA-256 signatures
- [ ] Exponential backoff retry mechanism (1s, 8s, 60s, dead-letter) on delivery failures
- [ ] Delivery history log with HTTP response status codes
