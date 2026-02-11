---
applyTo: '**/*.dart'
---

# Flutter Mobile App Development Best Practices

## Project Structure
- Use feature-based folder structure instead of layer-based
- Keep widgets in separate files when they become complex
- Group related files in appropriate directories (models, services, screens)
- Ensure logical grouping of files to maintain clarity and scalability
- Separate navigation logic from UI components for better maintainability

## State Management
- Use Provider state manager
- Keep business logic separate from UI widgets
- Use immutable state objects when possible
- Avoid using setState in complex screens; prefer BLoC, Cubit, or Provider for state management
- Ensure BuildContext is used safely across async gaps (check if mounted)

## Widget Development
- Prefer composition over inheritance
- Extract reusable widgets into separate files
- Use const constructors wherever possible for performance
- Keep build methods simple, readable, and side-effect free
- Split large build methods into smaller StatelessWidget classes instead of helper methods

## Performance
- Use const widgets to reduce rebuilds
- Implement lazy loading for lists with large datasets
- Optimize images and use appropriate formats
- Profile your app regularly using Flutter DevTools
- Avoid heavy computations in build methods; move them to Providers or Controllers
- Cache network images using libraries like cached_network_image

## Code Quality
- Follow Dart naming conventions (camelCase, PascalCase)
- Use meaningful variable and function names
- Keep functions small and focused on single responsibility
- Add proper documentation and comments
- Ensure methods are less than 40 lines and maintain a maximum indentation level of 3-4
- Avoid magic numbers and hardcoded strings; use constants or localization

## Error Handling
- Implement proper error boundaries
- Use try-catch blocks for async operations
- Provide meaningful error messages to users
- Log errors appropriately for debugging
- Avoid silent failures (e.g., empty catch blocks)
- Integrate crash reporting tools like Sentry or Firebase Crashlytics

## Dependencies
- Keep dependencies up to date
- Use specific version constraints
- Minimize external dependencies
- Audit packages for security and maintenance
- Avoid using deprecated or unmaintained packages