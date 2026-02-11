# Plan: Flutter SmartCook AI Project Setup & Development

This plan outlines a systematic approach to set up the SmartCook AI Flutter project from scratch, implementing proper architecture, dependencies, services, and features while adhering to the clean code and project architecture guidelines. The setup will follow a feature-based structure with Provider state management, AI integration via OpenRouter, and comprehensive error handling.

## Steps

### 1. Project Setup
the application root folder is `./app`. Create folder structure (`core/`, `features/`, `shared/`), update `pubspec.yaml` with dependencies (provider, image_picker, go_router, freezed, cached_network_image), and run `flutter pub get`.

**Deliverables:**
- Folder structure established in `lib/`
- `pubspec.yaml` updated with all required dependencies
- Dependencies installed successfully

**Key Dependencies to Add:**
- `provider: ^6.0.0` — State management
- `image_picker: ^1.0.0` — Camera/gallery access
- `go_router: ^13.0.0` — Navigation
- `freezed_annotation: ^2.4.0` — Immutable models
- `json_annotation: ^4.8.0` — JSON serialization
- `json_serializable: ^6.7.0` — Code generation (dev)
- `build_runner: ^2.4.0` — Code generation (dev)
- `cached_network_image: ^3.3.0` — Image caching
- `flutter_dotenv: ^5.1.0` — Environment variables

### 2. Project Architecture
Establish core modules: constants, models (Ingredient, Recipe), theme, and reusable widgets; organize `features/` with home, ingredients, and recipes; add shared utilities, extensions, and mixins.

**Deliverables:**
- Complete folder structure matching `project_arch.instructions.md`
- Core modules with base implementations
- Theme configuration with colors, text styles
- Shared widgets (buttons, input fields, error states)

**Folders to Create:**
```
lib/
├── core/
│   ├── constants/
│   ├── models/
│   ├── services/
│   ├── theme/
│   └── widgets/
├── features/
│   ├── home/
│   │   ├── screens/
│   │   └── widgets/
│   ├── ingredients/
│   │   ├── providers/
│   │   ├── screens/
│   │   └── widgets/
│   └── recipes/
│       ├── providers/
│       ├── screens/
│       └── widgets/
├── shared/
│   ├── utils/
│   ├── extensions/
│   └── mixins/
└── main.dart
```

### 3. Dependencies Setup
Configure and test all packages: Provider, http client, image_picker, go_router navigation, json_serializable, freezed, and cached_network_image to ensure proper imports and initialization.

**Deliverables:**
- All packages properly imported and tested
- Build runner configured for code generation
- Provider setup in main.dart with MultiProvider
- Go router navigation configured with routes
- Image picker service initialized

**Configuration Tasks:**
- Setup `build_runner` for freezed code generation
- Configure `.env` file for API keys
- Test all service initializations
- Verify imports compile without errors

### 4. Services
Implement singleton services: `AIService` (OpenRouter API calls with error handling), `ImagePickerService` (camera/gallery access), and optional caching/logging services.

**Deliverables:**
- `AIService` — Handles all OpenRouter API calls with error handling
- `ImagePickerService` — Manages camera and gallery access
- Error handling with meaningful messages
- Logging service for debugging

**Service Implementations:**
- `core/services/ai_service.dart` — OpenRouter integration
- `core/services/image_picker_service.dart` — Image handling
- Error models for consistent error handling
- Request/response DTOs for API communication

### 5. Global Error Handling
Create error boundary widgets, custom error states in Providers, meaningful user feedback (SnackBars/Dialogs), and logging integration using standard loggers.

**Deliverables:**
- Custom error classes and exceptions
- Error boundary widget wrapper
- Error handling in all Providers
- User-friendly error messages
- Logging integration

**Implementation Details:**
- Create `AppException` and custom error types
- Implement error state in Providers (loading, success, error)
- Build `ErrorDialog` and `ErrorSnackBar` widgets
- Setup logger (using standard Dart logging)
- Add try-catch blocks in all async operations

### 6. Home Feature
Build home screen with navigation to ingredients and recipes, implement `HomeScreen` with go_router integration, add feature-specific widgets.

**Deliverables:**
- `HomeScreen` displaying navigation options
- Go router routes configured
- Feature-specific widgets for home screen
- Navigation to ingredients and recipes features

**Implementation:**
- `features/home/screens/home_screen.dart`
- Feature buttons for ingredients and recipes
- Go router integration for deep linking

### 7. Ingredients Feature
Develop `IngredientsProvider` for state management, camera/photo upload with `ImagePickerService`, manual ingredient entry, and `AIService` integration for photo recognition.

**Deliverables:**
- `IngredientsProvider` with state management
- Camera screen with photo capture
- Ingredient list screen
- AI-powered photo recognition
- Manual ingredient entry
- Error handling for invalid inputs

**Implementation:**
- `features/ingredients/providers/ingredients_provider.dart`
- `features/ingredients/screens/camera_screen.dart`
- `features/ingredients/screens/ingredient_list_screen.dart`
- Feature-specific widgets for UI components

### 8. Recipes Feature
Create `RecipesProvider` with `AIService` calls for recipe generation, implement recipe list/detail screens, and display generated recipes with cached images.

**Deliverables:**
- `RecipesProvider` with recipe generation logic
- Recipe list screen with cached images
- Recipe detail screen with full information
- Loading states and error handling
- Recipe data display with proper formatting

**Implementation:**
- `features/recipes/providers/recipes_provider.dart`
- `features/recipes/screens/recipe_list_screen.dart`
- `features/recipes/screens/recipe_detail_screen.dart`
- Feature-specific widgets for recipe display

## Further Considerations

### 1. Provider Setup Strategy
**Question:** Should main.dart use `MultiProvider` with all providers at root level, or lazy-load providers per feature?

**Recommendation:** Multi-provider at root for simplicity, split to feature-level if complexity increases.

**Details:**
- Start with centralized provider setup in main.dart
- Use `create` vs `lazy` initialization based on usage frequency
- Consider feature-level providers only if state isolation is critical
- Example: Global `AIService` provider, feature-specific data providers

### 2. API Key Management
**Question:** Store OpenRouter API key securely in `.env` file (with flutter_dotenv) or use build configurations?

**Recommendation:** Use environment variable approach to avoid committing sensitive keys.

**Details:**
- Create `.env` file (add to `.gitignore`)
- Use `flutter_dotenv` package for loading
- Access via `dotenv.env['OPENROUTER_API_KEY']`
- Never commit `.env` to version control
- Document required environment variables

### 3. Navigation Strategy
**Question:** Should go_router use named routes (string-based) or typed routes?

**Recommendation:** Use go_router with proper deep linking support for future web expansion.

**Details:**
- Define routes in a central `routes.dart` file
- Use named routes for clarity and maintainability
- Support deep linking for sharing and redirects
- Implement proper error handling for invalid routes

### 4. State Immutability
**Question:** Use `freezed` package for all models or only for complex states?

**Recommendation:** Apply consistently to both domain models and provider states for data integrity.

**Details:**
- Use `freezed` for all data models (Ingredient, Recipe)
- Use `freezed` for provider states (IngredientsState, RecipesState)
- Leverage copyWith for state updates
- Ensure immutability for predictable state management

## Timeline & Phases

**Phase 1 (Setup & Foundation):** Steps 1-3 — Project setup, architecture, and dependencies
**Phase 2 (Core Services):** Step 4-5 — Services and error handling
**Phase 3 (Features):** Steps 6-8 — Home, ingredients, and recipes features

## Success Criteria

- ✅ Project builds without errors
- ✅ All dependencies installed and configured
- ✅ Proper folder structure matching architecture guidelines
- ✅ Services functional with proper error handling
- ✅ Provider state management working correctly
- ✅ Navigation between features smooth and type-safe
- ✅ Error handling consistent across the app
- ✅ Code follows clean code guidelines from instructions
