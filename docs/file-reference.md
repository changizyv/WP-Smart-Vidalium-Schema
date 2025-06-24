# File Reference for Vidalium Smart Schema

This document provides a concise reference for all files in the **Vidalium Smart Schema** WordPress plugin, designed to enhance **Generative Engine Optimization (GEO)** through automated structured data (JSON-LD) generation. The plugin leverages an intelligent content analysis algorithm to suggest schema.org types with minimal server load, optimized for shared hosting. The table below lists each file, its path, purpose, and key dependencies, grouped by role. For detailed implementation, refer to [Architecture](architecture.md).

## File Reference Table

| File Name                     | Path                                           | Purpose                                                                 | Dependencies                     |
|-------------------------------|------------------------------------------------|-------------------------------------------------------------------------|-------------------------------|
| **vidalium-smart-schema.php** | `vidalium-smart-schema.php`                    | Main plugin file; initializes hooks, renders JSON-LD, checks sync       | SchemaController.php, SchemaModel.php, AdminUI.php |
| **AIAnalyzer.php**            | `includes/ai/AIAnalyzer.php`                   | Implements content analysis algorithm for schema suggestions            | DOMHelper.php, SchemaModel.php |
| **DOMHelper.php**             | `includes/helpers/DOMHelper.php`               | Provides DOM utility functions (e.g., XPath generation)                 | AIAnalyzer.php                |
| **SchemaController.php**      | `includes/controllers/SchemaController.php`    | Coordinates schema processing and conflict detection                   | AIAnalyzer.php, SchemaModel.php, AdminUI.php |
| **SchemaModel.php**           | `includes/models/SchemaModel.php`              | Manages database operations for schemas, settings, and logs            | SchemaAPI.php, SchemaController.php, AIAnalyzer.php |
| **AdminUI.php**               | `includes/ui/AdminUI.php`                      | Renders admin panel UI (settings, schema selector, sync reports)       | settings.php, schema-selector.php, admin.css, admin.js |
| **SchemaAPI.php**             | `includes/rest/SchemaAPI.php`                  | Provides REST API endpoints for schema and settings management         | SchemaModel.php, SchemaController.php |
| **ai-analyzer.js**            | `assets/js/ai-analyzer.js`                     | Handles client-side DOM analysis for real-time schema suggestions       | SchemaAPI.php, schema-selector.php, frontend.css |
| **admin.js**                  | `assets/js/admin.js`                           | Manages dynamic admin panel interactions (settings, reports)            | SchemaAPI.php, settings.php, sync-report.php, admin.css |
| **admin.css**                 | `assets/css/admin.css`                         | Styles admin panel UI (forms, tables, buttons)                         | AdminUI.php, settings.php, sync-report.php |
| **admin-rtl.css**             | `assets/css/admin-rtl.css`                     | RTL styles for admin panel (e.g., Persian support)                     | admin.css, AdminUI.php        |
| **frontend.css**              | `assets/css/frontend.css`                      | Styles frontend schema selection preview                               | schema-selector.php, ai-analyzer.js |
| **frontend-rtl.css**          | `assets/css/frontend-rtl.css`                  | RTL styles for frontend preview                                        | frontend.css, schema-selector.php |
| **settings.php**              | `assets/templates/settings.php`                | Template for admin settings page                                       | AdminUI.php, admin.css, admin.js |
| **schema-selector.php**       | `assets/templates/schema-selector.php`         | Template for schema selection UI                                       | AdminUI.php, ai-analyzer.js, frontend.css |
| **sync-report.php**           | `assets/templates/sync-report.php`             | Template for conflict report UI                                        | AdminUI.php, admin.js, admin.css |
| **LLMAnalyzer.php**           | `includes/llm/LLMAnalyzer.php`                 | Placeholder for future LLM-based schema analysis                       | None (future implementation)  |
| **LLMConnector.php**          | `includes/llm/LLMConnector.php`                | Placeholder for future LLM API integration                             | None (future implementation)  |
| **utils.php**                 | `includes/utils.php`                           | General utility functions (e.g., logging, sanitization)                | Multiple                      |
| **vidalium-smart-schema-fa_IR.po** | `languages/vidalium-smart-schema-fa_IR.po` | Persian translation file                                               | None                          |
| **CHANGELOG.md**              | `CHANGELOG.md`                                 | Tracks plugin version changes                                          | None                          |
| **composer.json**             | `composer.json`                                | Defines development dependencies (PHPUnit, PHP_CodeSniffer)            | None                          |
| **LICENSE.txt**               | `LICENSE.txt`                                  | GPL-2.0-or-later license file                                          | None                          |
| **readme.txt**                | `readme.txt`                                   | WordPress plugin repository metadata                                   | None                          |
| **bootstrap.php**             | `tests/bootstrap.php`                          | Sets up testing environment for PHPUnit                                | None                          |
| **test-aianalyzer.php**       | `tests/test-aianalyzer.php`                    | Unit tests for AIAnalyzer.php                                          | AIAnalyzer.php                |
| **test-schemacontroller.php** | `tests/test-schemacontroller.php`              | Unit tests for SchemaController.php                                    | SchemaController.php          |
| **test-schemamodel.php**      | `tests/test-schemamodel.php`                   | Unit tests for SchemaModel.php                                         | SchemaModel.php               |
| **test-schemaapi.php**        | `tests/test-schemaapi.php`                     | Unit tests for SchemaAPI.php                                           | SchemaAPI.php                 |

## Notes
- **GEO Role**: Files like `AIAnalyzer.php`, `SchemaController.php`, and `SchemaModel.php` drive GEO by enabling accurate schema generation for AI-driven search engines.
- **Content Analysis**: The algorithm in `AIAnalyzer.php` and `ai-analyzer.js` is optimized for performance, analyzing DOM and Elementor widgets without Python dependencies.
- **RTL Support**: `admin-rtl.css` and `frontend-rtl.css` ensure compatibility with Persian and other RTL languages.
- **Extensibility**: `LLMAnalyzer.php` and `LLMConnector.php` prepare the plugin for future LLM integration (see [Future LLM Integration](future-llm-integration.md)).
- **Testing**: Unit tests in the `tests/` directory ensure reliability, aligned with standards in `composer.json`.

For detailed implementation, see [Architecture](architecture.md). For schema generation details, refer to [Schema Usage](schema-usage.md).