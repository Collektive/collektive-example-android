# Collektive Android Example Application

This is an Android application demonstrating the Collektive framework for distributed collective algorithms. The app uses MQTT for device communication and displays nearby devices using a collective neighbor discovery algorithm.

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Environment Setup
- **CRITICAL**: Internet connectivity is REQUIRED for building this project. Without internet access, Gradle cannot download Android plugins and dependencies.
- Java 17+ is required and should be available at `/usr/bin/java`
- Android SDK is pre-installed at `/usr/local/lib/android/sdk`
- Set required environment variables:
  ```bash
  export ANDROID_HOME=/usr/local/lib/android/sdk
  export ANDROID_SDK_ROOT=/usr/local/lib/android/sdk
  export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools
  ```

### Building the Application
- **NEVER CANCEL**: Build commands may take 5-15 minutes on first run due to dependency downloads. Always set timeout to 30+ minutes.
- Initial build with dependency download:
  ```bash
  cd /home/runner/work/collektive-example-android/collektive-example-android
  export ANDROID_HOME=/usr/local/lib/android/sdk
  export ANDROID_SDK_ROOT=/usr/local/lib/android/sdk
  export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools
  ./gradlew build
  ```
  - First build takes 10-15 minutes. NEVER CANCEL. Set timeout to 30+ minutes.
  - Subsequent builds take 2-5 minutes.

- Create debug APK:
  ```bash
  ./gradlew assembleDebug
  ```
  - Takes 3-8 minutes. NEVER CANCEL. Set timeout to 20+ minutes.

- Create release APK:
  ```bash
  ./gradlew assembleRelease
  ```
  - Takes 3-8 minutes. NEVER CANCEL. Set timeout to 20+ minutes.

### Testing
- Run unit tests:
  ```bash
  ./gradlew test
  ```
  - Takes 2-5 minutes. NEVER CANCEL. Set timeout to 15+ minutes.

- Run instrumented tests (requires Android emulator or device):
  ```bash
  ./gradlew connectedAndroidTest
  ```
  - Takes 5-15 minutes depending on emulator setup. NEVER CANCEL. Set timeout to 30+ minutes.
  - Note: Emulator setup may not be available in all environments.

### Code Quality and Linting
- Run detekt linting:
  ```bash
  ./gradlew detekt
  ```
  - Takes 1-3 minutes. Set timeout to 10+ minutes.

- Run Android lint:
  ```bash
  ./gradlew lint
  ```
  - Takes 2-5 minutes. Set timeout to 15+ minutes.

- **ALWAYS run linting before committing** or the CI (.github/workflows/build-and-deploy.yml) will fail.

### Validation Scenarios
- Run `./gradlew build assemble`

### Known Limitations
- **Internet connectivity required**: Without internet access, `./gradlew build` fails with "Plugin not found" errors because Gradle cannot download the Android Gradle Plugin and dependencies.
- **MQTT connectivity**: The app connects to `broker.hivemq.com` - ensure network allows MQTT connections on port 1883/8883.
- **Device testing**: Full collective algorithm testing requires multiple devices running the application.

## Common Tasks and Reference Information

### Project Structure
```
collektive-example-android/
├── app/                              # Main Android application module
│   ├── build.gradle.kts             # App-level build configuration
│   └── src/
│       ├── main/java/it/unibo/collektive/
│       │   ├── MainActivity.kt       # Main application entry point
│       │   ├── viewmodels/
│       │   │   └── NearbyDevicesViewModel.kt  # Core collective algorithm logic
│       │   └── network/mqtt/
│       │       └── MqttMailbox.kt    # MQTT communication implementation
│       ├── test/                     # Unit tests
│       └── androidTest/              # Instrumented tests
├── build.gradle.kts                  # Root build configuration
├── settings.gradle.kts               # Gradle settings and modules
├── gradle/libs.versions.toml         # Version catalog for dependencies
└── .github/workflows/                # CI/CD configuration
```

### Key Files to Check After Changes
- Always check `MainActivity.kt` after modifying UI components
- Always check `NearbyDevicesViewModel.kt` after changing collective algorithm logic
- Always check `MqttMailbox.kt` after modifying network communication
- Always run `./gradlew detekt` after modifying any Kotlin files

### Dependencies and Versions
- Android Gradle Plugin: 8.13.0
- Kotlin: 2.2.10
- Gradle: 9.0.0 (wrapper)
- Collektive Framework: 26.1.2
- Minimum SDK: 30, Target SDK: 35
- Jetpack Compose for UI
- HiveMQ MQTT client for communication

### Gradle Tasks Reference
```bash
# View all available tasks
./gradlew tasks

# Build tasks
./gradlew build              # Complete build (10-15 min first time)
./gradlew assembleDebug      # Debug APK only (3-8 min)
./gradlew assembleRelease    # Release APK only (3-8 min)

# Testing tasks  
./gradlew test              # Unit tests (2-5 min)
./gradlew connectedAndroidTest  # Instrumented tests (5-15 min)

# Code quality tasks
./gradlew detekt            # Kotlin static analysis (1-3 min)
./gradlew lint              # Android linting (2-5 min)

# Cleanup
./gradlew clean             # Clean build outputs (1-2 min)
```

### APK Output Locations
- Debug APK: `app/build/outputs/apk/debug/app-debug.apk`
- Release APK: `app/build/outputs/apk/release/app-release.apk`

### Common Error Solutions
- **"Plugin not found" errors**: Ensure internet connectivity for dependency downloads
- **"Android SDK not found"**: Verify ANDROID_HOME environment variable is set correctly
- **Build failures**: Run `./gradlew clean` then retry the build command
- **Detekt failures**: Check `app/build/reports/detekt/detekt.html` for specific issues

### CI/CD Integration
- The project uses `.github/workflows/build-and-deploy.yml` for automated builds
- The CI uses `DanySK/build-check-deploy-gradle-action@4.0.5` for standardized Android builds
- All code quality checks must pass for CI success
