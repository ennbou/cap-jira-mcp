# SmartCook AI - Copilot Instructions

## Project Overview
SmartCook AI is a Flutter mobile app (iOS/Android) that uses AI to identify ingredients from photos and generate recipes. The app integrates with OpenRouter's AI API for vision and text generation.

## Tech Stack & Dependencies
- **Flutter**: 3.7.2+ / **Dart**: 3.7.2+
- **State Management**: Provider (`ChangeNotifier` pattern)
- **Navigation**: GoRouter (declarative routing)
- **Data Models**: Freezed + JSON Serializable (immutable models)
- **AI Integration**: OpenRouter API (via `http` client)
- **Image Handling**: `image_picker` + `cached_network_image`
- **Local Storage**: SQLite (`sqflite`)
- **Environment**: `flutter_dotenv` for API keys

## Architecture Pattern

### Feature-Based Structure
```
lib/
├── core/                   # Shared infrastructure
│   ├── constants/          # ApiConstants, RouteConstants, AppConstants
│   ├── models/             # Freezed models (Ingredient, Recipe)
│   ├── services/           # Singleton services (AIService, ImagePickerService)
│   ├── theme/              # AppTheme, colors, text styles
│   └── widgets/            # Common reusable widgets
├── features/               # Feature modules (home, ingredients, recipes, favorites)
│   └── [feature]/
│       ├── providers/      # ChangeNotifier providers (state management)
│       ├── screens/        # Full-screen widgets
│       └── widgets/        # Feature-specific widgets
├── shared/                 # Cross-cutting concerns
│   ├── router/             # GoRouter configuration
│   └── utils/              # AppLogger, exceptions, helpers
└── main.dart               # MultiProvider setup + app entry
```

### Provider Dependency Pattern
Services are injected into Providers using `ChangeNotifierProxyProvider`:
```dart
// In main.dart
ChangeNotifierProxyProvider2<AIService, ImagePickerService, IngredientsProvider>(
  create: (context) => IngredientsProvider(
    aiService: context.read<AIService>(),
    imagePickerService: context.read<ImagePickerService>(),
  ),
  // ...
)
```

**Key Rule**: Never call `context.watch()` or `context.read()` in Provider constructors. Always pass dependencies explicitly.

## Critical Development Workflows

### 1. Code Generation (Required After Model Changes)
When you modify any `@freezed` or `@JsonSerializable` models:
```bash
# From app/ directory
dart run build_runner build --delete-conflicting-outputs

# Or watch mode (auto-regenerates on changes)
dart run build_runner watch --delete-conflicting-outputs
```

**Files affected**: `*.freezed.dart`, `*.g.dart` (git-ignored, auto-generated)

### 2. Environment Setup (First Time)
```bash
cd app
cp .env.example .env
# Add your OPENROUTER_API_KEY to .env
flutter pub get
dart run build_runner build --delete-conflicting-outputs
```

### 3. Running & Testing
```bash
flutter run                    # Launch app (device auto-selected)
flutter test                   # Run unit/widget tests
flutter analyze                # Static analysis
flutter pub outdated           # Check dependency updates
```

## AI Service Integration

### OpenRouter API Flow
1. **Image Analysis**: `AIService.analyzeImage(File)` → sends base64 image to vision model
2. **Recipe Generation**: `AIService.generateRecipes(List<Ingredient>)` → generates recipes from ingredient list
3. **API Client**: Uses custom `http.Client` wrapper with retry logic and error handling

**Model Selection**: 
- Vision tasks: `google/gemini-2.0-flash-001` (ApiConstants.visionModel)
- Text tasks: `google/gemini-2.0-flash-001` (ApiConstants.defaultModel)

**Response Parsing**: AI responses are JSON-parsed into Freezed models. Errors throw custom exceptions (`ImageException`, `RecipeException`).

### Error Handling Pattern
```dart
try {
  final result = await _aiService.analyzeImage(file);
  _setSuccess(result);
} catch (e, stackTrace) {
  _handleError('Failed to analyze image', e, stackTrace);
  AppLogger.error('Operation failed', error: e, stackTrace: stackTrace);
}
```

Always use `AppLogger` for consistent logging (`debug`, `info`, `warning`, `error`).

## State Management Best Practices

### Provider Status Pattern
Each feature provider follows this pattern:
```dart
enum IngredientsStatus { initial, loading, success, error }

class IngredientsProvider extends ChangeNotifier {
  IngredientsStatus _status = IngredientsStatus.initial;
  String? _errorMessage;
  
  bool get isLoading => _status == IngredientsStatus.loading;
  
  void _setLoading() {
    _status = IngredientsStatus.loading;
    _errorMessage = null;
    notifyListeners();
  }
}
```

**Immutability**: Use `List.unmodifiable()` for getter collections to prevent external mutations.

### Provider Lifecycle
- **Create**: Instantiate in `MultiProvider` in `main.dart`
- **Dispose**: Implement `dispose()` for cleanup (streams, controllers, timers)
- **Access**: Use `context.watch<Provider>()` in `build()` methods, `context.read<Provider>()` for callbacks

## Navigation & Routing

### GoRouter Structure
Routes are declarative in `app_router.dart`:
```dart
GoRoute(
  path: RouteConstants.ingredientListPath,  // '/ingredients'
  name: RouteConstants.ingredientList,      // 'ingredientList'
  builder: (context, state) => const IngredientListScreen(),
)
```

**Navigation**: Use `context.goNamed(RouteConstants.routeName)` or `context.push(path)`.

**Deep Linking**: Routes are automatically linkable via URL paths.

## Testing Strategy

### Current Test Structure
- **Widget Tests**: Basic smoke tests (e.g., `widget_test.dart`)
- **Feature Tests**: Located in `test/features/[feature]/`
- **Mocking**: Use `mocktail` for service/provider mocks

### Running Tests
```bash
flutter test                           # All tests
flutter test test/features/recipes/    # Specific feature
flutter test --coverage                # Generate coverage report
```

## Code Quality Rules

### Widget Guidelines
- **const Constructors**: Use `const` wherever possible (performance optimization)
- **Widget Extraction**: Split complex `build()` methods into separate `StatelessWidget` classes
- **Max Indentation**: Keep nesting ≤ 3-4 levels (extract methods/widgets if deeper)

### Naming Conventions
- **Files**: `snake_case.dart`
- **Classes**: `PascalCase`
- **Variables/Methods**: `camelCase`
- **Private Members**: Prefix with `_`
- **Constants**: `camelCase` (not SCREAMING_SNAKE_CASE per Dart guidelines)

### Avoid Common Pitfalls
- **DON'T** call async operations in `build()` methods → Use `FutureBuilder` or Provider
- **DON'T** use `setState()` in Providers → Use `notifyListeners()`
- **DON'T** mix UI logic with business logic → Keep Providers pure
- **DON'T** hardcode strings/numbers → Use constants or localization
- **DON'T** commit `.env` files → Use `.env.example` as template

## Key Files to Reference

### Entry Points
- [main.dart](app/lib/main.dart) - Provider setup and app initialization
- [app_router.dart](app/lib/shared/router/app_router.dart) - Route definitions

### Core Services
- [ai_service.dart](app/lib/core/services/ai_service.dart) - OpenRouter API client
- [app_logger.dart](app/lib/shared/utils/app_logger.dart) - Logging utility

### Example Patterns
- [ingredients_provider.dart](app/lib/features/ingredients/providers/ingredients_provider.dart) - Status enum pattern
- [ingredient.dart](app/lib/core/models/ingredient.dart) - Freezed model example

## Project Management
- **Jira**: Backlog managed via Atlassian (see `mcp.json` for integration)
- **Git Workflow**: GitFlow (feature branches, PRs to `develop`)
- **Commit Format**: `[TICKET-ID] type(scope): message` (e.g., `[KAN-123] feat(recipes): add filter`)

## Known Constraints
- **iOS Minimum**: iOS 12.0+
- **Android Minimum**: Android 5.0+ (API 21)
- **API Key**: Requires valid `OPENROUTER_API_KEY` in `.env` for AI features
- **Platform-Specific**: Camera permissions must be declared in `Info.plist` (iOS) and `AndroidManifest.xml` (Android)
