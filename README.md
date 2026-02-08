# 📰 News Intelligence Platform

**Enterprise news intelligence SaaS — Django only**

> Transform information overload into actionable insights with automated summarization, AI-driven personalization, and real-time trend detection.

---

## 🔎 Project snapshot

A compact, production-ready Django project that focuses on news ingestion, source management, and per-company story tracking. Built with pure Django (no separate microservices) and designed to be extended with AI summarization, analytics, and alerting.

## ✨ Key highlights

* Enterprise-focused news intelligence platform
* AI-ready architecture for summarization, personalization & trends
* Automated news ingestion via Django management command
* User & company-aware story tracking
* Multi-source monitoring with configurable sources
* Django templates + focused static CSS for UI
* Mature migration history (production-style schema evolution)
* Straightforward integration points for Celery, Redis & LLM APIs

---

## 🚀 What's included (actual structure)

### Project root: `news_monitoring_system/`

```
news_monitoring_system/
├── __init__.py
├── asgi.py
├── settings.py
├── urls.py
└── wsgi.py
```

### Core app: `news/`

```
news/
├── __init__.py
├── admin.py
├── apps.py
├── decorator.py
├── forms.py
├── models.py
├── tests.py
├── urls.py
├── views.py
├── migrations/
│   ├── 0001_initial.py
│   ├── 0002_company_users.py
│   ├── ...
│   └── 0012_alter_company_name_and_more.py
├── management/commands/
│   └── update_stories.py
├── templates/news/
│   ├── base.html
│   ├── login.html
│   ├── signup.html
│   ├── user_sources.html
│   ├── user_stories.html
│   ├── add_source.html
│   ├── edit_source.html
│   └── delete_story.html
└── static/news/
    ├── style.css
    ├── login_style.css
    ├── signup_style.css
    └── ...
```

> Note: Your project already contains a mature migrations history and a management command (`update_stories`) — ideal hooks for scheduled ingestion and AI pipelines.

---

## 🧭 Quickstart — run locally

1. **Create & activate virtualenv**

```bash
python -m venv venv
# mac/linux
source venv/bin/activate
# windows (powershell)
venv\Scripts\Activate.ps1
```

2. **Install dependencies**

```bash
pip install -r requirements.txt
```

3. **Environment variables**

Create a `.env` from `.env.example` with at least:

```
DEBUG=True
SECRET_KEY=replace-me
DATABASE_URL=sqlite:///db.sqlite3   # or postgres://user:pass@host:port/dbname
REDIS_URL=redis://localhost:6379/0   # optional
AI_API_KEY=your-llm-key              # optional (for future AI features)
```

4. **Apply migrations**

```bash
python manage.py migrate
```

5. **Create superuser (admin)**

```bash
python manage.py createsuperuser
```

6. **Run dev server**

```bash
python manage.py runserver
```

7. **Run the automated updater**

```bash
python manage.py update_stories
```

This management command is the place where your ingestion, deduplication, and (future) summarization logic should live or be orchestrated.

---

## 📁 Important files & extension points

* `news/models.py` — core domain models: `Story`, `Source`, `Company`, `RegisteredUser`.
* `news/management/commands/update_stories.py` — news ingestion, deduplication, and scheduled tasks.
* `news/templates/news/*` & `news/static/news/*` — the UI surface (auth, source CRUD, story CRUD, dashboards).
* `news/views.py` & `news/forms.py` — request handling and form validation.
* `news/decorator.py` — custom decorators (authorization / company scoping).

**Where to plug AI & analytics**

* Inside `update_stories.py` after fetching raw content: call a `summarize_article()` helper.
* Add `summaries` and `sentiment` fields to `models.py` (if not present) and store results.
* Create a `services/` module (e.g., `news/services/ai.py`) to wrap LLM calls and prompt engineering.

---

## ✅ Production checklist (recommended)

* Use **PostgreSQL** in production (update `DATABASES` in `settings.py` or use `DATABASE_URL`).
* Serve static files with `collectstatic`, and configure **Nginx** + **Gunicorn**.
* Use **HTTPS** (Let's Encrypt) and a proper secrets management system.
* Add **logging** (structured logs) and monitoring (Sentry / Prometheus).
* Replace management-run ingestion with **Celery + Redis** for background jobs and periodic tasks.
* Add automated tests for `update_stories`, model behavior, and view permissions.

---

## 🌱 Suggested roadmap (next features)

1. **AI Summarization** — run LLM inference in `update_stories` and persist concise summaries.
2. **Entity Extraction & Tagging** — store companies, people, locations to enable filtering.
3. **Personalization** — preference model per user and ranked story feeds.
4. **Trend Detection** — batch process term frequencies & identify spikes per company/industry.
5. **Notifications** — email/in-app alerts for spike events or matched keywords.
6. **Dockerize** — add `Dockerfile` and `docker-compose.yml` for repeatable deployments.

---

## 🧩 Tips for contributors

* Follow PEP8 and use `black` for formatting.
* Add tests under `news/tests.py` or `news/tests/` per feature.
* Write migrations when changing models and include a short note in commit messages.
* Keep template logic minimal — prefer view/context processing.
