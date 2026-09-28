# Data Flows for Critical Features

## Personalised workout plan

1. User completes goals/fitness questionnaire in the Flutter app; health metrics (steps, heart rate, sleep) are read from HealthKit / Health Connect with the user's consent.
2. App calls POST /workouts/plans on the NestJS API with the Cognito JWT; API Gateway validates the token.
3. NestJS checks Redis for a cached, still-valid plan. If none, it loads the profile, history and metrics from PostgreSQL.
4. NestJS sends a pseudonymised feature payload (no name/email) to the FastAPI AI service.
5. The AI service generates the plan (rules + ML model), returns it as JSON, and stores the embedding in pgvector for future recommendations.
6. NestJS saves the plan in PostgreSQL, caches it in Redis, and returns it to the app. When the user logs a workout, the result is fed back so the next plan adapts (progressive overload).

## Social sharing & live challenges

1. User finishes a workout and taps Share; the app requests a pre-signed S3 URL and uploads the photo directly to S3.
2. App calls POST /social/posts with the caption, S3 key and visibility (friends / public / private). Health metrics are shared only if the user explicitly ticks them.
3. NestJS stores the post in PostgreSQL and publishes a post.created event to Redis pub/sub and SQS.
4. The WebSocket gateway pushes the new post and leaderboard updates (Redis sorted sets) to online friends in real time.
5. A background worker consumes the SQS event and sends push notifications via FCM/APNs to offline friends; media is served through CloudFront.

## Nutrition tracking

1. User logs a meal by barcode, text search or photo.
2. Barcode/text: NestJS queries the nutrition database API (results cached in Redis for 24 h).
3. Photo: image is uploaded to S3; NestJS asks the AI service to recognise the food items and estimate portions and macros.
4. The user confirms or edits the result in the app (human-in-the-loop keeps data accurate).
5. NestJS stores the entry in PostgreSQL and updates the daily totals; the dashboard refreshes instantly over WebSocket.
6. A nightly worker aggregates weekly trends, and the AI service adjusts calorie/macro targets based on the workout plan.
