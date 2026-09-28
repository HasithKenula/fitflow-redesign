# ai-service – FastAPI AI microservice

Internal-only service (not exposed to the internet) called by the core API and workers.

Endpoints (planned):
- `POST /v1/plans/generate` – personalised weekly workout plan
- `POST /v1/nutrition/recognise` – meal photo to food items + estimated macros
- `GET  /v1/recommendations/{userId}` – exercise / content recommendations

Stack: FastAPI, Pydantic, scikit-learn / PyTorch, ONNX Runtime for inference, pgvector for embeddings.
Only pseudonymised user IDs and the minimum required features are sent to this service.
