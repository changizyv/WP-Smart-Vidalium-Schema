# Contributing to WP-Smart-Vidalium-Schema

Thank you for considering contributing to **WP-Smart-Vidalium-Schema**, a WordPress plugin designed for **Generative Engine Optimization (GEO)**. This plugin automates JSON-LD schema generation using an intelligent content analysis algorithm, optimized for shared hosting. Your contributions can help enhance its features, performance, and compatibility for the WordPress and SEO communities. We welcome all forms of support, from code to feedback!

## Ways to Contribute
You can contribute to the project in several ways:
- **Code**: Add new features, fix bugs, or optimize the content analysis algorithm.
- **Documentation**: Improve or translate documentation (e.g., `README.md`, `docs/` files).
- **Bug Reports**: Report issues with schema generation, UI, or performance.
- **Feature Requests**: Suggest new schema types or GEO-related enhancements.
- **Testing**: Test the plugin on different WordPress setups or hosting environments.
- **Translations**: Add or improve translations (e.g., expand `vidalium-smart-schema-fa_IR.po` for Persian).

## Setting Up the Development Environment
To contribute code or test the plugin, follow these steps:
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/changizyv/WP-Smart-Vidalium-Schema.git
   cd WP-Smart-Vidalium-Schema
   ```
2. **Install Dependencies**:
   - Ensure you have [Composer](https://getcomposer.org/) installed.
   - Run `composer install` to set up development tools (PHPUnit, PHP_CodeSniffer) as defined in `composer.json`.
3. **Set Up WordPress**:
   - Install a local WordPress instance (e.g., via [LocalWP](https://localwp.com/)).
   - Copy the plugin folder to `/wp-content/plugins/`.
   - Activate the plugin in the WordPress admin panel.
4. **Database Setup**:
   - The plugin creates four tables (`wp_ai_schema_blocks`, `wp_ai_schema_sync_log`, `wp_ai_schema_settings`, `wp_ai_schema_types`) on activation. See [Database](docs/database.md) for details.
5. **Testing Environment**:
   - Run unit tests with `composer run test` (see `tests/` directory).
   - Use PHP_CodeSniffer to check coding standards: `composer run lint`.

## Coding Guidelines
To ensure consistency and quality, please follow these standards:
- **PHP**: Adhere to [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/). Use `composer run lint` to validate.
- **JavaScript/CSS**: Follow [WordPress JavaScript Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/) and ensure RTL compatibility (e.g., for `admin-rtl.css`).
- **Documentation**: Use Markdown for docs, following the structure in `docs/` (e.g., `schema-usage.md`).
- **Commit Messages**: Write clear, concise messages (e.g., `Fix: Handle invalid HTML in AIAnalyzer.php`).
- **GEO Focus**: Ensure contributions enhance schema accuracy or performance for AI-driven search engines.
- **Sanitization**: Use WordPress functions (e.g., `sanitize_text_field`, `wp_kses`) for security.

## Submitting Contributions
1. **Create an Issue**:
   - Check existing issues in the [GitHub Issues](https://github.com/changizyv/WP-Smart-Vidalium-Schema/issues) tab.
   - Open a new issue for bugs or feature requests, describing the problem or idea clearly.
2. **Fork and Branch**:
   - Fork the repository and create a branch (e.g., `feature/add-new-schema` or `fix/bug-xpath`).
   - Keep changes focused and atomic.
3. **Submit a Pull Request (PR)**:
   - Push your changes to your fork and create a PR against the `main` branch.
   - Reference the related issue (e.g., `Fixes #123`).
   - Describe your changes in the PR description, including GEO impact if applicable.
4. **Review Process**:
   - PRs are reviewed for code quality, functionality, and alignment with GEO goals.
   - Tests must pass (`composer run test`) before merging.

## Contact
For questions or ideas, reach out via:
- **GitHub Issues**: [github.com/changizyv/WP-Smart-Vidalium-Schema/issues](https://github.com/changizyv/WP-Smart-Vidalium-Schema/issues)
- **Website**: [hashem.changizy.ir](https://hashem.changizy.ir)
- **LinkedIn**: [linkedin.com/in/changizy](https://www.linkedin.com/in/changizy/)

## Code of Conduct
We aim to foster an inclusive and respectful community. Please be kind and constructive in all interactions.

## Thank You!
Your contributions help make **WP-Smart-Vidalium-Schema** better for WordPress users and the GEO community. We appreciate your time and effort!