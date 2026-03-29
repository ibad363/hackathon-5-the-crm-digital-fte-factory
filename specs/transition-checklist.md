# Transition Checklist: Prototype to Production

This document outlines the steps required to move the Customer Success FTE from its Python prototype (Incubation Phase 1) to a full, production-ready system (Specialization Phase 2).

## 1. Project Restructuring
- [ ] Create the `production/` directory structure.
- [ ] Set up `production/agent/` for the core OpenAI agent code.
- [ ] Set up `production/channels/` for external integrations (Gmail, Twilio).
- [ ] Set up `production/workers/` for Kafka consumers.
- [ ] Set up `production/api/` for FastAPI endpoints.
- [ ] Set up `production/database/` for PostgreSQL schemas and models.

## 2. Infrastructure & Data Layer
- [ ] Initialize PostgreSQL.
- [ ] Write `schema.sql` (Tables: `Customers`, `Tickets`, `Messages`, `AgentState`).
- [ ] Initialize Apache Kafka (Topics: `incoming_messages`, `outgoing_messages`, `escalations`).
- [ ] Create robust database connection pools (e.g., SQLAlchemy/asyncpg).

## 3. The OpenAI Custom Agent
- [ ] Install `openai` and `pydantic`.
- [ ] Translate the 5 prototype skills into strict `@function_tool` decorated functions.
- [ ] Write the system prompt instructing the agent on brand voice, E.A.R. methodology, and escalation adherence.
- [ ] Implement robust error handling (e.g., fallback responses when an API or tool call fails).

## 4. Channel Integrations
- [ ] Build `channels/gmail_handler.py`: Authenticate and read/send emails via Gmail API.
- [ ] Build `channels/whatsapp_handler.py`: Set up a Twilio webhook to receive and reply to WhatsApp messages.
- [ ] Build `channels/web_form_handler.py`: Create a FastAPI endpoint to ingest Next.js form submissions.

## 5. Message Routing (Kafka)
- [ ] Build the Kafka Consumer worker to ingest messages from the `incoming_messages` topic.
- [ ] Connect the worker to the OpenAI agent for processing.
- [ ] Build the mechanism for the agent to publish to `outgoing_messages` or `escalations`.

## 6. Testing & Validation
- [ ] Write unit tests for all `@function_tool` tools using `pytest`.
- [ ] Write integration tests for the Kafka consumers and FastAPI endpoints.
- [ ] Create `production/tests/test_transition.py` to ensure all prototype test cases (e.g., sentiment rules, escalation keywords) still pass in the production codebase.

## 7. Deployment Readiness
- [ ] Create a production `Dockerfile` for the FastAPI/Agent service.
- [ ] Create a `Dockerfile` for the Kafka workers.
- [ ] Write Kubernetes manifests (`k8s/deployment.yaml`, `k8s/service.yaml`, `k8s/configmap.yaml`, `k8s/secrets.yaml`).
- [ ] Ensure `.env` files are securely managed (not committed, per `.gitignore`).