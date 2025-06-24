# Architecture of Vidalium Smart Schema

This document describes the architecture of **Vidalium Smart Schema**, a WordPress plugin designed to enhance **Generative Engine Optimization (GEO)** by automating structured data (JSON-LD) generation. The plugin follows a **Model-View-Controller (MVC)** architecture to ensure modularity, maintainability, and scalability. Its core strength lies in an intelligent content analysis algorithm that suggests schema.org types (e.g., Article, Product, FAQPage) with high accuracy and minimal server load, optimized for shared hosting without requiring heavy dependencies like Python. This document outlines the MVC structure, key files, and a reference table for all project files.

## MVC Architecture
The plugin is built on the **Model-View-Controller (MVC)** pattern to separate concerns and streamline development:
- **Model**: Manages data interactions with the WordPress database, handling schema storage, settings, and synchronization logs. Implemented primarily in `SchemaModel.php`.
- **View**: Renders the user interface, including the admin panel, schema selection preview, and conflict reports. Key files include `AdminUI.php`, `settings.php`, `schema-selector.php`, and `sync-report.php`.
- **Controller**: Coordinates logic between the model and view, managing schema processing, content analysis, and conflict detection. Implemented in `SchemaController.php`.

This structure ensures extensibility, supports future integration with large language models (LLMs), and maintains compatibility with WordPress standards and right-to-left (RTL) languages like Persian.

## Key Components
Below are the primary files and their roles in the plugin’s architecture, with a focus on the intelligent content analysis algorithm and GEO optimization.

### 1. vidalium-smart-schema.php
- **Path**: `vidalium-smart-schema.php`
- **Purpose**: Main plugin file, serving as the entry point for initializing hooks, rendering JSON-LD, and managing core functionality.
- **Key Functions**:
  - `init_plugin()`: Registers hooks for admin and frontend functionality.
  - `render_schema()`: Injects JSON-LD into page head or footer based on settings.
  - `check_schema_sync($post_id, $post, $update)`: Triggers schema synchronization on post updates.
- **Dependencies**: `SchemaController.php`, `SchemaModel.php`, `AdminUI.php`
- **Database Dependencies**: `wp_ai_schema_blocks`, `wp_ai_schema_sync_log`
- **GEO Role**: Ensures schemas are correctly rendered for AI-driven search engines, enhancing content discoverability.
- **Implementation Notes**:
  - Uses WordPress hooks (`wp_head`, `wp_footer`, `save_post`) for integration.
  - Conditionally loads RTL styles (`admin-rtl.css`, `frontend-rtl.css`) using `is_rtl()`.

### 2. AIAnalyzer.php
- **Path**: `includes/ai/AIAnalyzer.php`
- **Purpose**: Implements the intelligent content analysis algorithm to suggest schemas by analyzing DOM, URLs, keywords, and Elementor widgets.
- **Key Functions**:
  - `analyze_dom($html, $url)`: Scans HTML and generates schema suggestions.
  - `calculate_node_score($node, $patterns, $url)`: Scores nodes based on text length, keywords, HTML structure, and URL patterns.
  - `suggest_schema($node, $score, $patterns)`: Maps scores to schema types.
- **Dependencies**: `DOMHelper.php`, `SchemaModel.php`
- **Database Dependencies**: `wp_ai_schema_types` for detection patterns.
- **GEO Role**: Drives GEO by generating accurate schemas (e.g., FAQPage, Product) that AI search engines can interpret, optimized for performance without Python.
- **Implementation Notes**:
  - Limits node processing to 100 for performance.
  - Suppresses HTML errors with `@` in `DOMDocument::loadHTML`.
  - Uses JSON-encoded patterns from `wp_ai_schema_types` for extensibility.

### 3. SchemaController.php
- **Path**: `includes/controllers/SchemaController.php`
- **Purpose**: Coordinates schema processing, conflict detection, and integration between model and view.
- **Key Functions**:
  - `process_schema($post_id)`: Generates and saves schemas for a page.
  - `check_conflicts()`: Detects conflicts with other plugins (e.g., Yoast SEO).
- **Dependencies**: `AIAnalyzer.php`, `SchemaModel.php`, `AdminUI.php`
- **Database Dependencies**: `wp_ai_schema_blocks`, `wp_ai_schema_sync_log`
- **GEO Role**: Ensures schema consistency and relevance, critical for maintaining GEO accuracy.
- **Implementation Notes**:
  - Triggered by `save_post` hook for real-time synchronization.
  - Logs conflicts in `wp_ai_schema_sync_log`.

### 4. SchemaModel.php
- **Path**: `includes/models/SchemaModel.php`
- **Purpose**: Manages database operations for schemas, settings, and logs.
- **Key Functions**:
  - `save_block($post_id, $data)`: Saves schema data for a page.
  - `get_blocks_by_post($post_id)`: Retrieves schemas for a page.
  - `save_setting($key, $value, $post_type)`: Stores plugin settings.
- **Dependencies**: `SchemaAPI.php`, `SchemaController.php`, `AIAnalyzer.php`
- **Database Dependencies**: `wp_ai_schema_blocks`, `wp_ai_schema_settings`, `wp_ai_schema_sync_log`, `wp_ai_schema_types`
- **GEO Role**: Stores and retrieves structured data, ensuring schemas are accessible to AI-driven search engines.
- **Implementation Notes**:
  - Uses WordPress sanitization functions for security.
  - Supports serialized data for complex settings.

### 5. AdminUI.php
- **Path**: `includes/ui/AdminUI.php`
- **Purpose**: Renders the admin panel interface, including settings, schema selection, and conflict reports.
- **Key Functions**:
  - `render_settings_page()`: Displays the settings form.
  - `render_schema_selector($post_id)`: Shows the schema selection UI.
  - `render_sync_report()`: Displays conflict reports.
- **Dependencies**: `settings.php`, `schema-selector.php`, `sync-report.php`, `admin.css`, `admin.js`
- **GEO Role**: Provides user control over GEO settings, ensuring schemas align with content intent.
- **Implementation Notes**:
  - Uses WordPress admin APIs for secure rendering.
  - Supports RTL via `admin-rtl.css`.

### 6. DOMHelper.php
- **Path**: `includes/helpers/DOMHelper.php`
- **Purpose**: Provides utility functions for DOM processing, such as generating XPath and validating nodes.
- **Key Functions**:
  - `get_xpath($node)`: Generates unique XPath for a DOM node.
  - `validate_node($node)`: Ensures nodes are valid for analysis.
- **Dependencies**: `AIAnalyzer.php`
- **GEO Role**: Supports accurate schema mapping by linking JSON-LD to specific content elements.
- **Implementation Notes**:
  - Ensures stable XPath generation for consistent schema application.

### 7. ai-analyzer.js
- **Path**: `assets/js/ai-analyzer.js`
- **Purpose**: Handles client-side DOM analysis in the page preview for real-time schema suggestions and user interactions.
- **Key Functions**:
  - `analyzeDOM(url)`: Scans DOM and generates schema suggestions.
  - `highlightNode(xpath)`: Highlights suggested nodes in the UI.
  - `selectSchema(schemaType, xpath)`: Saves user-selected schemas via AJAX.
- **Dependencies**: `SchemaAPI.php`, `schema-selector.php`, `frontend.css`
- **Database Dependencies**: Indirectly interacts with `wp_ai_schema_blocks` via API.
- **GEO Role**: Enables interactive schema selection, enhancing GEO usability.
- **Implementation Notes**:
  - Limits to 5 schemas per page.
  - Uses `wp_localize_script` for API URLs and nonces.

### 8. SchemaAPI.php
- **Path**: `includes/rest/SchemaAPI.php`
- **Purpose**: Provides REST API endpoints for managing schemas, settings, and synchronization logs.
- **Key Endpoints**:
  - `/vidalium-schema/v1/settings`: GET/POST for plugin settings.
  - `/vidalium-schema/v1/sync-issues`: GET for conflict reports.
  - `/vidalium-schema/v1/schemas`: GET/POST for schema data.
- **Dependencies**: `SchemaModel.php`, `SchemaController.php`
- **Database Dependencies**: All four tables (`wp_ai_schema_blocks`, `wp_ai_schema_sync_log`, `wp_ai_schema_settings`, `wp_ai_schema_types`).
- **GEO Role**: Enables programmatic access to schema data, supporting integration with external GEO tools.
- **Implementation Notes**:
  - Secured with WordPress nonces and permission checks.
  - Uses REST API standards for extensibility.

## File Reference
The following table lists all project files, grouped by role, with brief descriptions. For detailed implementation, refer to the key components above or specific files as needed.

| File Name                | Path                              | Purpose                                                                 | Dependencies                     |
|--------------------------|-----------------------------------|-------------------------------------------------------------------------|----------------------------------|
| admin.css                | assets/css/admin.css              | Styles for admin panel UI (forms, tables, buttons)                      | AdminUI.php, settings.php        |
| admin-rtl.css            | assets/css/admin-rtl.css          | RTL styles for admin panel (Persian support)                            | admin.css, AdminUI.php           |
| frontend.css             | assets/css/frontend.css           | Styles for frontend preview (schema selector)                           | schema-selector.php, ai-analyzer.js |
| frontend-rtl.css         | assets/css/frontend-rtl.css       | RTL styles for frontend preview                                         | frontend.css, schema-selector.php |
| admin.js                 | assets/js/admin.js                | Dynamic interactions for admin panel (settings, reports)                | SchemaAPI.php, settings.php      |
| settings.php             | assets/templates/settings.php     | Template for admin settings page                                       | AdminUI.php, admin.css           |
| schema-selector.php      | assets/templates/schema-selector.php | Template for schema selection UI                                      | AdminUI.php, ai-analyzer.js      |
| sync-report.php          | assets/templates/sync-report.php  | Template for conflict report UI                                        | AdminUI.php, admin.js            |
| LLMAnalyzer.php          | includes/llm/LLMAnalyzer.php      | Placeholder for future LLM-based schema analysis                        | None (future implementation)      |
| LLMConnector.php         | includes/llm/LLMConnector.php     | Placeholder for future LLM API integration                              | None (future implementation)      |
| utils.php                | includes/utils.php                | General utility functions (e.g., logging, sanitization)                | Multiple                         |
| vidalium-smart-schema-fa_IR.po | languages/vidalium-smart-schema-fa_IR.po | Persian translation file                                 | None                             |
| CHANGELOG.md             | CHANGELOG.md                      | Tracks plugin version changes                                          | None                             |
| composer.json            | composer.json                     | Defines development dependencies (PHPUnit, PHP_CodeSniffer)             | None                             |
| readme.txt               | readme.txt                        | WordPress plugin repository metadata                                   | None                             |
| bootstrap.php            | tests/bootstrap.php               | Sets up testing environment for PHPUnit                                | None                             |
| test-aianalyzer.php      | tests/test-aianalyzer.php         | Unit tests for AIAnalyzer.php                                          | AIAnalyzer.php                   |
| test-schemacontroller.php| tests/test-schemacontroller.php   | Unit tests for SchemaController.php                                    | SchemaController.php             |
| test-schemamodel.php     | tests/test-schemamodel.php        | Unit tests for SchemaModel.php                                         | SchemaModel.php                  |
| test-schemaapi.php       | tests/test-schemaapi.php          | Unit tests for SchemaAPI.php                                           | SchemaAPI.php                    |

## Implementation Notes
- **Content Analysis Algorithm**: The algorithm in `AIAnalyzer.php` is optimized for GEO, analyzing DOM, URLs, and Elementor widgets to suggest schemas with minimal server load, avoiding Python dependencies.
- **Performance**: Limits DOM node processing to 100 and uses efficient database queries (via indexes in `wp_ai_schema_blocks` and `wp_ai_schema_types`).
- **RTL Support**: Conditional loading of `admin-rtl.css` and `frontend-rtl.css` ensures compatibility with Persian and other RTL languages.
- **Security**: Uses WordPress sanitization functions and REST API permission checks to prevent vulnerabilities.
- **Extensibility**: Modular design supports future LLM integration (see `LLMAnalyzer.php`, `LLMConnector.php`).
- **Testing**: Unit tests in the `tests/` directory ensure reliability, using PHPUnit and PHP_CodeSniffer per `composer.json`.

## Role in GEO
The MVC architecture and content analysis algorithm are tailored for GEO:
- **Semantic Optimization**: Generates accurate JSON-LD schemas for AI-driven search engines.
- **Performance Efficiency**: Lightweight design ensures GEO accessibility on shared hosting.
- **User Control**: Interactive UI and API endpoints allow precise schema management, enhancing content discoverability.

For details on schema generation, see [Schema Usage](schema-usage.md). For database structure, refer to [Database](database.md).