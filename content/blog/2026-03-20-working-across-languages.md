+++
title = "Working Across Frontend, Backend & Data with JSON"
date = 2026-03-20T10:45:00Z
slug = "json-contracts-fullstack"
description = "A minimal, practical guide to using JSON as the contract across frontend, backend, data, and AI systems."

[taxonomies]
tags = ["json", "api", "fullstack", "backend", "frontend", "data", "ai"]

[extra]
featured = true
author = "Zubin J"
+++

## JSON = Contract

All layers (FE, BE, Data, AI) communicate via **JSON**. 

If the JSON shape is stable → teams move independently.

This is the only thing teams must agree on. Everything else can change independently.

### Minimal Architecture

Frontend ⇄ REST API ⇄ Backend ⇄ Data / ML

Only shared dependency: **JSON schema**

Think of the API as a boundary layer — it isolates systems while exposing structured data.

### Example Contract

Define this early. This is what every team builds against.

```json
{
  "user_id": "123",
  "score": 0.87,
  "label": "high_potential"
}
```

## Backend: Define Endpoint (Python / FastAPI)

The backend exposes data through a predictable JSON response via REST endpoints.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/score/{user_id}")
def get_score(user_id: str):
    return {
        "user_id": user_id,
        "score": 0.87,
        "label": "high_potential"
    }
```

Deploy → this becomes a **service**.

## Frontend: Consume It

Frontend simply calls the endpoint and parses JSON — no knowledge of backend logic required.

```javascript
fetch("https://api.yourapp.com/score/123")
  .then(res => res.json())
  .then(data => {
    console.log(data.label);
  });
```

Frontend only depends on **JSON format**.

## Swap Logic: Data / ML Integration

You can replace static logic with real models without breaking consumers.

```python
def get_score(user_id: str):
    score = model.predict(user_id)
    return {
        "user_id": user_id,
        "score": score,
        "label": "high_potential" if score > 0.8 else "low"
    }
```

No frontend changes required.

### AI Services Fit the Same Pattern

AI outputs should also conform to structured JSON for consistency and reuse.

```json
{
  "summary": "...",
  "sentiment": "positive",
  "confidence": 0.92
}
```

Expose via API → reusable across apps.

### Rules (Non-Negotiable)

These rules ensure systems remain decoupled and scalable:

- Define JSON schema first
- Version breaking changes
- Never couple UI to backend logic
- Keep responses predictable

### TL;DR

JSON is not just a format — it's your system contract.

Control the JSON → control the system.

Everything else is replaceable.