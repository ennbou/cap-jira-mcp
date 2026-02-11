---
applyTo: '**'
---

# SmartCook AI - Project Architecture

## Folder Structure
The project follows a simplified feature-based structure:

```
lib/
├── core/                   # Shared code across features
│   ├── constants/          # API keys, routes, static strings
│   ├── models/             # Shared Data Models (Ingredient, Recipe)
│   ├── services/           # Singleton Services (AIService, ImagePickerService)
│   ├── theme/              # AppTheme, Colors, TextStyles
│   └── widgets/            # Common reusable widgets (Buttons, InputFields)
├── features/               # Feature-specific code
│   ├── home/               # Entry point / Dashboard
│   │   ├── screens/        # Screens for the Home feature
│   │   │   └── home_screen.dart
│   │   └── widgets/        # Widgets specific to the Home feature
│   ├── ingredients/        # Ingredient Management (Photo & Manual)
│   │   ├── providers/      # State management logic for ingredients
│   │   │   └── ingredients_provider.dart
│   │   ├── screens/        # Screens for Ingredient Management
│   │   │   ├── camera_screen.dart
│   │   │   └── ingredient_list_screen.dart
│   │   └── widgets/        # Widgets specific to Ingredient Management
│   └── recipes/            # Recipe Display
│       ├── providers/      # State management logic for recipes
│       │   └── recipes_provider.dart
│       ├── screens/        # Screens for Recipe Display
│       │   ├── recipe_list_screen.dart
│       │   └── recipe_detail_screen.dart
│       └── widgets/        # Widgets specific to Recipe Display
├── shared/                 # Shared utilities and helpers
│   ├── utils/              # Utility functions
│   ├── extensions/         # Dart extensions
│   └── mixins/             # Common mixins
└── main.dart               # App entry point, Provider setup
```

## Data Flow
1.  **Input**: User takes a photo or types ingredients.
2.  **Logic**: `IngredientsProvider` calls `AIService`.
3.  **Service**: `AIService` sends data to the AI endpoint and returns `List<Ingredient>`.
4.  **State Update**: Provider updates its state; UI rebuilds to show the list.
5.  **Recipe Generation**: User confirms list -> `RecipesProvider` calls `AIService` -> returns `List<Recipe>`.

## Tech Stack
-   **Flutter**: UI Framework.
-   **Dart**: Programming Language.
-   **Provider**: State Management.
-   **http**: API communication.
-   **image_picker**: Camera/Gallery access.
-   **json_serializable**: JSON parsing.
-   **freezed**: Immutable data classes.
-   **json_annotation**: JSON serialization.
-   **openrouter_api**: AI interactions.
-   **go_router**: Navigation.
-   **cached_network_image**: Image caching.

## Development Rules
1.  **Keep it Simple**: Do not add layers (Repositories, DTOs) unless the logic gets complex.
2.  **UI/Logic Separation**: Widgets should only display data. Logic goes in Providers.
3.  **AI Integration**: All AI calls go through `OpenRouter_api`. Do not make API calls inside Widgets.
4.  **Error Handling**: Use try-catch blocks for async operations and provide meaningful error messages.
5.  **Performance Optimization**: Use const widgets where possible, lazy load lists, and profile the app regularly.
6.  **Code Quality**: Follow Dart naming conventions, avoid magic numbers, and document public APIs.

