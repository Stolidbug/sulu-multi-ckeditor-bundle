# Contributing to CFC Editor Bundle

We welcome contributions to the CFC Editor Bundle! This document outlines the development process and guidelines.

## Development Setup

### Requirements

- PHP 8.1+
- Sulu CMS 2.5+
- Symfony 5.4+ || 6.x || 7.x
- Node.js 14+ (for admin interface development)
- Composer

### Installation

1. Clone the repository:
```bash
git clone https://github.com/cfc-corporate/sulu-editor-bundle.git
cd sulu-editor-bundle
```

2. Install PHP dependencies:
```bash
composer install
```

3. Install JavaScript dependencies:
```bash
npm install
```

4. Run tests:
```bash
vendor/bin/phpunit
```

## Development Workflow

### Coding Standards

- **PSR-12**: Follow PHP coding standard
- **Strict Types**: Use `declare(strict_types=1);` in all PHP files
- **Type Hints**: Use proper type hints for all method parameters and return types
- **PHPDoc**: Document all public methods and complex logic
- **Tests**: Maintain 100% test coverage

### Git Workflow

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Commit changes**: Use semantic commit messages
4. **Push to branch**: `git push origin feature/amazing-feature`
5. **Open a Pull Request**

### Commit Message Format

Use conventional commits format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

Examples:
```
feat(content-type): add paragraph tag stripping functionality
fix(admin): resolve CKEditor configuration parameter passing
docs(readme): update installation instructions
test(content-type): add comprehensive PHPCR integration tests
```

## Testing

### Running Tests

```bash
# Run all tests
vendor/bin/phpunit

# Run tests with coverage
vendor/bin/phpunit --coverage-html coverage

# Run specific test class
vendor/bin/phpunit tests/Content/Type/MultiTextEditorTypeTest.php
```

### Writing Tests

- **Unit Tests**: Test individual classes and methods
- **Integration Tests**: Test component interactions
- **Feature Tests**: Test complete user workflows
- **Follow AAA Pattern**: Arrange, Act, Assert

### Test Requirements

- All new code must have tests
- Maintain 100% code coverage
- Tests should be deterministic and fast
- Use meaningful test names that describe behavior

## Code Quality

### Static Analysis

```bash
# Run PHPStan
vendor/bin/phpstan analyze

# Run with specific level
vendor/bin/phpstan analyze --level=8
```

### Code Formatting

```bash
# Check code style
vendor/bin/php-cs-fixer fix --dry-run

# Fix code style
vendor/bin/php-cs-fixer fix
```

## Documentation

### Documentation Requirements

- **README**: Keep installation and usage instructions updated
- **API Documentation**: Document all public methods
- **Examples**: Provide working examples for new features
- **Changelog**: Update CHANGELOG.md for all changes

### Writing Documentation

- Use clear, concise language
- Include code examples
- Provide troubleshooting sections
- Keep examples up-to-date with API changes

## Frontend Development

### React Components

- Use functional components with hooks when possible
- Follow React best practices
- Include TypeScript definitions
- Test component behavior

### Building Assets

```bash
# Development build
npm run dev

# Production build
npm run build

# Watch mode for development
npm run watch
```

## Bundle Architecture

### Content Types

- Extend appropriate Sulu base classes
- Implement proper PHPCR integration
- Support all required CRUD operations
- Include parameter validation

### Services

- Use dependency injection
- Follow single responsibility principle
- Include proper error handling
- Document service configurations

### Admin Interface

- Follow Sulu admin conventions
- Provide consistent user experience
- Include proper form validation
- Support internationalization

## Release Process

### Version Numbering

We follow [Semantic Versioning](https://semver.org/):
- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes (backward compatible)

### Release Checklist

1. Update CHANGELOG.md
2. Update version in composer.json
3. Run full test suite
4. Update documentation
5. Create release notes
6. Tag release: `git tag v1.0.0`
7. Push to repository

## Getting Help

### Communication Channels

- **GitHub Issues**: Bug reports and feature requests
- **GitHub Discussions**: Questions and community support
- **Email**: dev@cfc-corporate.fr for private matters

### Reporting Issues

When reporting issues, please include:
- Sulu CMS version
- PHP version
- Bundle version
- Steps to reproduce
- Expected vs actual behavior
- Relevant error messages or logs

### Feature Requests

For feature requests, please:
- Describe the use case
- Explain why it would be valuable
- Provide implementation suggestions if possible
- Consider backward compatibility

## Code of Conduct

### Our Standards

- Be respectful and inclusive
- Welcome newcomers and help them learn
- Focus on constructive feedback
- Respect different viewpoints and experiences

### Enforcement

Instances of unacceptable behavior may be reported to dev@cfc-corporate.fr. All complaints will be reviewed and investigated promptly and fairly.

## License

By contributing to CFC Editor Bundle, you agree that your contributions will be licensed under the MIT License.

## Recognition

Contributors will be recognized in:
- CHANGELOG.md for significant contributions
- README.md contributors section
- Release notes for major contributions

Thank you for contributing to the CFC Editor Bundle!