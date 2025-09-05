# TopicTailor System Design

## 1. Overview
TopicTailor is a personalized AI news delivery platform that allows users to tailor their news experience by selecting topics, content types, expertise level, and delivery frequency. The system is designed for cost-effectiveness, modularity, and flexibility, with the ability to leverage both open-source and commercial AI models for content processing.

---

## 2. Architecture

### 2.1 High-Level Components
- **Frontend**: React-based web app for user interaction
- **Backend**: FastAPI (Python) REST API
- **Database**: PostgreSQL
- **Agents**: Modular Python services for content ingestion, classification, summarization, personalization, and delivery
- **Scheduler**: Celery with Redis
- **Email Service**: SMTP or third-party (e.g., SendGrid)

---

## 3. Agents and Their Tasks

| Agent Name                | Task Description                                                                                 | Model Type           |
|--------------------------|------------------------------------------------------------------------------------------------|----------------------|
| ContentFetcherAgent       | Fetches news articles from various sources (RSS, APIs, web scraping)                            | -                    |
| ContentClassifierAgent    | Classifies articles by topic and content type                                                   | Open-source (ML/NLP) |
| SummarizationAgent        | Summarizes articles at different expertise levels (beginner, intermediate, advanced)            | Open-source & Commercial |
| PersonalizationAgent      | Matches content to user preferences and filters accordingly                                    | Open-source (rules/ML)|
| DeliveryAgent             | Formats and sends personalized digests via email at scheduled times                            | -                    |
| SchedulerAgent            | Triggers periodic tasks (fetch, summarize, deliver)                                             | -                    |

---

## 4. Backend API Endpoints

| Endpoint                        | Method | Description                                      | Auth Required |
|----------------------------------|--------|--------------------------------------------------|---------------|
| /register                        | POST   | Register new user                                | No            |
| /login                           | POST   | User login, returns JWT                          | No            |
| /profile                         | GET    | Get user profile and preferences                 | Yes           |
| /profile                         | PUT    | Update user profile/preferences                  | Yes           |
| /topics                          | GET    | List available topics                            | No            |
| /content_types                   | GET    | List available content types                     | No            |
| /digest/preview                  | GET    | Preview personalized digest                      | Yes           |
| /digest/schedule                 | POST   | Set or update digest delivery schedule           | Yes           |
| /digest/history                  | GET    | View past digests                                | Yes           |
| /logout                          | POST   | Logout user (invalidate token)                   | Yes           |

---

## 5. Data Model (Simplified)

- **User**: id, email, password_hash, created_at
- **Preference**: user_id, topics, content_types, expertise_levels, frequency, delivery_time
- **Article**: id, title, url, content, topics, content_type, summary, fetched_at
- **DigestLog**: user_id, articles, sent_at

---

## 6. Content Sources (Suggested)
- RSS feeds from major AI/tech news sites (e.g., arXiv, VentureBeat AI, The Batch, MIT Tech Review, Synced)
- News APIs (e.g., NewsAPI.org, GNews, ContextualWeb News)
- (Optional) Web scraping for niche sources

---

## 7. Model Selection Logic
- Use open-source models (e.g., Llama, Mixtral, T5) for basic summarization and classification
- Use commercial APIs (e.g., OpenAI GPT-4/5) for complex/long-form summarization or when high quality is critical
- Model selection is configurable per agent/task

---

## 8. Cost-Effectiveness
- Prefer open-source and batch processing for most tasks
- Use commercial APIs only for premium/critical tasks
- Deploy on low-cost cloud or on-prem infrastructure

---

## 9. Security & Privacy
- Store passwords hashed (bcrypt/argon2)
- JWT for authentication
- Minimal data collection, no unnecessary tracking

---

## 10. Future Extensions
- Add web/app notifications
- Add admin/analytics dashboard
- Support for more content sources and languages

---

## 11. Open Questions (see questions_for_owner.md)
