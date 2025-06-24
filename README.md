# Vidalium Smart Schema

**Vidalium Smart Schema** is a powerful WordPress plugin designed to enhance **Generative Engine Optimization (GEO)** by automatically generating and managing structured data (JSON-LD) for WordPress pages, posts, and custom post types. Built with performance in mind, the plugin leverages an innovative content analysis algorithm to suggest and apply schema.org types (e.g., Article, Product, FAQPage) with minimal server load, making it ideal for shared hosting environments. This project showcases advanced SEO techniques tailored for AI-driven search engines, ensuring content is discoverable and optimized for modern search experiences.

Developed by **[Hashem Changizy](https://hashem.changizy.ir)**, a GEO specialist passionate about semantic optimization and AI-driven solutions, this plugin reflects years of expertise in crafting lightweight, efficient tools for web developers and site owners. Connect with Hashem on [GitHub](https://github.com/changizyv) or [LinkedIn](https://www.linkedin.com/in/changizy/) to explore more of his work in GEO and SEO.

## Why This Matters for GEO
In the era of generative search engines (e.g., ChatGPT, Perplexity), structured data is critical for making content easily interpretable by AI models. Vidalium Smart Schema automates the creation of JSON-LD schemas, improving content discoverability and enhancing visibility in AI-driven search results. Its **intelligent content analysis algorithm** identifies patterns in HTML, URLs, and Elementor widgets without requiring resource-heavy dependencies like Python, delivering high accuracy with minimal server impact.

## Key Features
- **Intelligent Content Analysis**: A custom algorithm analyzes DOM structures, keywords, URLs, and Elementor widgets to suggest relevant schemas (e.g., FAQPage, Product) with scores based on text length, keywords, HTML structure, and more.
- **Lightweight Performance**: Optimized for shared hosting, requiring no external dependencies and processing up to 100 DOM nodes efficiently.
- **Schema Support**: Supports 24 schema.org types, including Article, Product, FAQPage, and LocalBusiness, with a limit of 5 schemas per page.
- **User-Friendly Interface**: Admin panel for settings, page preview for schema selection, and conflict reports for easy management.
- **RTL Support**: Initial support for right-to-left languages like Persian via CSS.
- **Modular Design**: Built for extensibility, with future plans for integration with large language models (LLMs).

## Table of Contents
- [Architecture](docs/architecture.md): Overview of the plugin’s MVC architecture and file structure.
- [Schema Usage](docs/schema-usage.md): Detailed guide on supported schemas and the content analysis algorithm.
- [Database](docs/database.md): Structure and purpose of database tables.
- [Future LLM Integration](docs/future-llm-integration.md): Plans for enhancing schema generation with language models.
- [File Reference](docs/file-reference.md): Quick reference for all plugin files and their roles.

## Advantages
- **GEO Optimization**: Enhances content visibility for AI-driven search engines through structured data.
- **High Performance**: Lightweight design ensures compatibility with resource-constrained hosting.
- **Ease of Use**: Intuitive UI for both manual and automatic schema management.
- **Extensibility**: Modular architecture supports future enhancements, such as LLM integration.
- **RTL Compatibility**: Initial support for Persian and other RTL languages.

## Limitations
- **JS-Heavy Pages**: Analysis may be incomplete for pages with dynamic JavaScript rendering.
- **RTL Support**: Currently limited to CSS-based adjustments; full support requires further testing.
- **Schema Limit**: Up to 5 schemas per page.
- **Change History**: Not tracked in the initial version.

## Installation
1. Download the plugin from the WordPress repository or clone this repository.
2. Upload the `vidalium-smart-schema` folder to `/wp-content/plugins/`.
3. Activate the plugin via the WordPress admin panel.
4. Configure settings in the admin panel under "Vidalium Smart Schema."

## Requirements
- **WordPress**: 5.0 or higher
- **PHP**: Compatible with shared hosting
- **License**: [GPL-2.0-or-later](LICENSE.txt)

## Contributing
Contributions are welcome! Please submit issues or pull requests on the [GitHub repository](https://github.com/changizyv/vidalium-smart-schema). For development, refer to `composer.json` for testing and coding standards.

## About the Developer
**Hashem Changizy** is a GEO and SEO specialist focused on building tools that bridge traditional web technologies with AI-driven search solutions. Visit [hashem.changizy.ir](https://hashem.changizy.ir) to learn more about his projects, or connect on [LinkedIn](https://www.linkedin.com/in/changizy/) and [GitHub](https://github.com/changizyv).

## License
This project is licensed under the [GPL-2.0-or-later](LICENSE.txt).