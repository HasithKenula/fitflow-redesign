# frontend – Flutter app

Single Flutter codebase targeting iOS, Android and Web (PWA).

- State management: Riverpod
- Networking: Dio (REST) + socket_io_client (real-time)
- Auth: Amplify Flutter (Amazon Cognito)
- Health data: platform channels to HealthKit (Swift) and Health Connect (Kotlin)
- Local storage: flutter_secure_storage (tokens), Drift/SQLite (offline workout log)

Create the project here with `flutter create --org com.fitflow --platforms=ios,android,web .`
