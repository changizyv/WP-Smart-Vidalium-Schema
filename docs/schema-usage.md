# Schema Usage in Vidalium Smart Schema

This document details the schema generation process in **Vidalium Smart Schema**, a WordPress plugin designed to enhance **Generative Engine Optimization (GEO)** through automated structured data (JSON-LD) creation. The plugin’s core strength lies in its **intelligent content analysis algorithm**, which identifies content patterns and suggests relevant schema.org types (e.g., Article, Product, FAQPage) with high accuracy and minimal server load. Optimized for shared hosting, the algorithm avoids heavy dependencies like Python, making it efficient and accessible. This document covers supported schemas, the content analysis algorithm, and their role in GEO.

## Supported Schemas
The plugin supports **24 schema.org types**, carefully selected for their relevance to modern SEO and GEO. These schemas are stored in the `wp_ai_schema_types` database table, which includes detection patterns and JSON-LD templates. Users can select up to **5 schemas per page** via the page preview interface. Below is the list of supported schemas:

| Schema Type       | Description                                      | Common Use Case                     |
|-------------------|--------------------------------------------------|-------------------------------------|
| Article           | General articles and blog posts                  | Blog posts, news                    |
| BlogPosting       | Specific blog posts with detailed metadata        | Blog content                           |
| Product           | Products with pricing and review details        | E-commerce (e.g., WooCommerce)      |
| FAQPage           | Frequently asked questions                      | FAQ sections, support pages      |
| Speakable            | Content optimized for voice assistants          | Articles, FAQs                      |
| Event             | Events with date, time, and location            | Event pages, schedules      |
| Recipe            | Cooking recipes with ingredients and steps       | Food blogs                         |
| Course            | Educational or training courses              | Online learning platforms          |
| Organization      | Company or website information                   | Homepage, About pages              |
| Person            | Author or individual profiles                   | Author bios, team pages            |
| Review            | Reviews of products or services                  | Product pages, testimonials            |
| AboutPage         | About us pages                                  | Company About pages                |
| ContactPage       | Contact information pages                       | Contact forms, support pages       |
| ProfilePage       | User or author profile pages                    | Social profiles, bios              |
| CollectionPage    | Galleries of products or blog posts              | Category pages, portfolios         |
| Service           | Service descriptions                            | Service-based businesses           |
| WebPage           | Generic web pages                             | Landing pages                      |
| NewsArticle       | News-specific articles                          | News websites                      |
| Blog              | Overall blog structure                          | Blog homepage                      |
| ItemList          | Lists of items (e.g., products, services)       | Galleries, lists                   |
| HowTo             | Step-by-step guides                             | Tutorials, guides                  |
| LocalBusiness     | Local business details                          | Small business websites            |
| Offer             | Discounts or special offers                     | E-commerce promotions              |
| AggregateRating   | Overall rating for products or services         | Review summaries                   |

### Implementation Notes
- Schemas are stored in the `wp_ai_schema_types` table with fields for `schema_type`, `description`, `detection_patterns` (JSON), and `json_template`.
- Users can manually select schemas in the page preview (`schema-selector.php`) or rely on automatic suggestions.
- The limit of 5 schemas per page ensures performance and compliance with search engine guidelines.

## Intelligent Content Analysis Algorithm
The plugin’s **content analysis algorithm** is designed to identify content patterns and suggest appropriate schemas with high accuracy while maintaining lightweight performance. Unlike traditional solutions that rely on resource-intensive tools like Python, this algorithm processes DOM structures, URLs, and Elementor widgets directly in PHP, ensuring compatibility with shared hosting environments.

### Objectives
- Identify content structures (e.g., FAQs, products, articles) using HTML patterns, keywords, URL structures, and Elementor widgets.
- Suggest relevant schema.org types with confidence scores.
- Minimize server load by limiting node processing and avoiding external dependencies.
- Enhance GEO by producing structured data that AI-driven search engines can easily interpret.

### Algorithm Steps
1. **DOM Scanning**:
   - Uses `DOMDocument` and `DOMXPath` to extract key HTML nodes (`section`, `article`, `div`, `ul`, `h1-h6`, `p`, `img`, `time`).
   - Ignores irrelevant tags (`script`, `style`, `iframe`) to reduce noise.
   - Limits processing to 100 nodes to optimize performance.
   - File: `AIAnalyzer.php`, `DOMHelper.php`

2. **URL Structure Analysis**:
   - Examines URL patterns to infer page type (e.g., `/product/`, `/blog/`, `/faq/`).
   - Common patterns:
     - Products: `/product/`, `/shop/`, `/item/`
     - Articles: `/blog/`, `/article/`, `/news/`
     - FAQs: `/faq/`, `/help/`, `/questions/`
     - About: `/about/`, `/about-us/`
     - Contact: `/contact/`, `/contact-us/`
   - Weight: 15% of the total score.
   - File: `AIAnalyzer.php`

3. **Feature Extraction**:
   - **Text Length**: Measures character count of node content (longer texts score higher).
   - **Keywords**: Detects terms like "question," "price," "author" to match schema types.
   - **HTML Structure**: Identifies patterns like `<h3>` + `<p>` for FAQs or `<img>` + `<p>` for Products.
   - **Attributes**: Checks for specific classes (e.g., `elementor-accordion`, `wp-block-post`).
   - **Elementor Widgets**: Recognizes widgets like accordions (`elementor-accordion`, `elementor-tab`) for potential FAQPage schemas.
   - File: `AIAnalyzer.php`

4. **Scoring**:
   - Each node is scored from 0 to 100 based on weighted criteria:
     | Criterion          | Weight | Description                                      |
     |--------------------|--------|--------------------------------------------------|
     | Text Length        | 20%    | Longer content scores higher                    |
     | Keywords           | 30%    | Presence of schema-relevant terms (e.g., "question") |
     | HTML Structure     | 25%    | Matches patterns like FAQ or Product layouts    |
     | Specific Attributes| 20%    | Gutenberg/Elementor classes or semantic tags    |
     | URL Structure      | 15%    | Matches patterns like `/product/`, `/faq/`      |

   - Formula:  
     `Score = (Text_Score × 0.2) + (Keyword_Score × 0.3) + (Structure_Score × 0.25) + (Attribute_Score × 0.2) + (URL_Score × 0.15)`
   - File: `AIAnalyzer.php`

5. **Classification and Suggestion**:
   - **Score ≥ 80%**: Primary suggestion, displayed prominently in the UI.
   - **Score 50–80%**: Secondary suggestion with a warning for user review.
   - **Score < 50%**: Ignored to avoid irrelevant suggestions.
   - Suggestions include `schema_type`, `xpath`, `score`, and `json_template`.
   - File: `AIAnalyzer.php`, `ai-analyzer.js`

6. **Manual Selection**:
   - Users can choose from the 24 supported schemas in the page preview interface (`schema-selector.php`).
   - Maximum of 5 schemas per page, enforced by `ai-analyzer.js`.
   - File: `schema-selector.php`, `ai-analyzer.js`

### Elementor Widget Detection
- **Accordion Widgets**: Detects `elementor-accordion` or `elementor-tab` classes, commonly used for FAQs.
- **Keyword Boost**: Presence of terms like "question," "answer," or "FAQ" increases the FAQPage score.
- **Weight**: Contributes 60% to the HTML structure score for FAQ detection.
- File: `AIAnalyzer.php`

### Error Handling
- **Invalid HTML**: Suppressed using `@` in `DOMDocument::loadHTML` and logged in `wp_ai_schema_sync_log` or `error_log`.
- **Unanalyzable Nodes**: Filtered out if score < 50.
- **Server Load**: Limited to 100 nodes to prevent performance issues.
- **Logging**: Errors are recorded in `wp_ai_schema_sync_log` or PHP error logs.
- File: `AIAnalyzer.php`, `utils.php`

### Key Files
- **AIAnalyzer.php**: Core logic for DOM analysis, scoring, and schema suggestion.
- **DOMHelper.php**: Helper functions for XPath extraction and node validation.
- **ai-analyzer.js**: Client-side DOM analysis in page preview for real-time suggestions.
- **wp_ai_schema_types**: Database table storing schema types, detection patterns, and JSON-LD templates.

## Role in Generative Engine Optimization (GEO)
The content analysis algorithm and schema generation process are tailored for GEO, ensuring content is optimized for AI-driven search engines:
- **Semantic Clarity**: JSON-LD schemas provide structured metadata, making content easily interpretable by AI models.
- **Contextual Relevance**: The algorithm’s keyword and structure analysis ensures schemas align with page intent (e.g., FAQPage for question-based content).
- **Performance Efficiency**: Lightweight design allows GEO optimization on resource-constrained servers, democratizing access to advanced SEO tools.
- **Future-Proofing**: Support for 24 schemas and extensible design prepare sites for evolving AI search algorithms.

### Example Impact
- **FAQPage Schema**: Improves visibility in AI-generated answers for question-based queries (e.g., "What is X?").
- **Product Schema**: Enhances product discoverability in e-commerce searches with rich snippets for price and reviews.
- **Article Schema**: Boosts article ranking in AI-driven news or blog summaries.

## Implementation Notes
- **Performance**: The algorithm processes up to 100 DOM nodes to balance accuracy and speed.
- **Accuracy**: Keyword and Elementor widget detection ensure high relevance, with manual overrides for user control.
- **Extensibility**: Detection patterns in `wp_ai_schema_types` can be updated to support new schemas.
- **UI Integration**: Suggestions are displayed in the page preview (`schema-selector.php`) with highlighted nodes for user interaction.
- **GEO Focus**: Schemas are selected to maximize compatibility with AI-driven search engines, prioritizing types like FAQPage, Product, and Article.

For details on the plugin’s architecture, see [Architecture](architecture.md). For database structure, refer to [Database](database.md).
