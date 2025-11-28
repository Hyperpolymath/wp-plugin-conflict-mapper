# Contributing to WP Plugin Conflict Mapper

Thank you for your interest in contributing to WP Plugin Conflict Mapper! This document provides guidelines and instructions for contributing.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Process](#development-process)
- [Coding Standards](#coding-standards)
- [Testing Requirements](#testing-requirements)
- [Security Considerations](#security-considerations)
- [Submission Guidelines](#submission-guidelines)
- [Community](#community)

## Code of Conduct

This project adheres to a Code of Conduct that all contributors are expected to follow. Please read [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) before contributing.

## Getting Started

### Prerequisites

- **PHP**: 7.4 or higher
- **WordPress**: 5.8 or higher
- **Git**: For version control
- **Composer** (optional): For dependency management
- **WP-CLI** (optional): For testing CLI commands

### Setting Up Development Environment

1. **Clone the repository**
   ```bash
   git clone https://github.com/Hyperpolymath/wp-plugin-conflict-mapper.git
   cd wp-plugin-conflict-mapper
   ```

2. **Install WordPress locally**
   - Use Local by Flywheel, XAMPP, or Docker
   - Install WordPress 5.8+

3. **Symlink plugin to WordPress**
   ```bash
   ln -s $(pwd) /path/to/wordpress/wp-content/plugins/wp-plugin-conflict-mapper
   ```

4. **Activate plugin**
   ```bash
   wp plugin activate wp-plugin-conflict-mapper
   ```

## Development Process

### Branching Strategy

We use a simplified Git Flow:

- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/*` - New features
- `bugfix/*` - Bug fixes
- `hotfix/*` - Critical production fixes

### Workflow

1. **Create an issue** describing your contribution
2. **Fork the repository**
3. **Create a feature branch** from `develop`:
   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/your-feature-name
   ```
4. **Make your changes** following coding standards
5. **Test thoroughly** (see Testing Requirements)
6. **Commit your changes** with clear messages
7. **Push to your fork**
8. **Create a Pull Request** to `develop` branch

### Commit Message Format

Use clear, descriptive commit messages:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Build process or auxiliary tool changes

**Example:**
```
feat(scanner): Add support for multisite networks

- Detect network-activated plugins
- Handle site-specific plugin configurations
- Add network admin menu

Fixes #42
```

## Coding Standards

### WordPress Coding Standards

Follow [WordPress PHP Coding Standards](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/):

```bash
# Install PHP_CodeSniffer
composer global require squizlabs/php_codesniffer
composer global require wp-coding-standards/wpcs

# Check your code
phpcs --standard=WordPress path/to/your/file.php

# Auto-fix issues
phpcbf --standard=WordPress path/to/your/file.php
```

### Key Standards

- **Naming**: Use `snake_case` for functions and variables (WordPress style)
- **Indentation**: Use tabs, not spaces
- **Line Length**: Max 100 characters
- **Documentation**: PHPDoc for all functions and classes
- **Security**: Sanitize input, escape output, use nonces
- **Internationalization**: Use `__()` and `esc_html_e()` for all strings

### Code Example

```php
<?php
/**
 * Get plugin conflicts
 *
 * Retrieves all conflicts for a specific plugin.
 *
 * @since 1.0.0
 *
 * @param string $plugin_file Plugin file path.
 * @return array Array of conflicts.
 */
function wpcm_get_plugin_conflicts( $plugin_file ) {
    global $wpdb;

    $conflicts = $wpdb->get_results(
        $wpdb->prepare(
            "SELECT * FROM {$wpdb->prefix}wpcm_conflicts WHERE affected_plugins LIKE %s",
            '%' . $wpdb->esc_like( $plugin_file ) . '%'
        )
    );

    return $conflicts;
}
```

## Testing Requirements

All contributions must include appropriate tests.

### Manual Testing

Before submitting, test:

1. **Basic functionality**: Core features work as expected
2. **Edge cases**: Handle empty data, large datasets, errors
3. **WordPress versions**: Test on WP 5.8, 6.0, latest
4. **PHP versions**: Test on PHP 7.4, 8.0, 8.1+
5. **Browser compatibility**: Chrome, Firefox, Safari, Edge
6. **Accessibility**: Keyboard navigation, screen readers

### Testing Checklist

- [ ] Plugin activates without errors
- [ ] All admin pages load correctly
- [ ] Scan completes without errors
- [ ] Export functions work (JSON, CSV)
- [ ] Settings save correctly
- [ ] AJAX operations work
- [ ] WP-CLI commands execute properly
- [ ] REST API endpoints respond correctly
- [ ] No PHP notices or warnings
- [ ] No JavaScript console errors

### Test Data

Use diverse test scenarios:

- **Small sites**: 0-5 plugins
- **Medium sites**: 10-25 plugins
- **Large sites**: 50+ plugins
- **Conflict scenarios**: Install conflicting plugins
- **Clean installs**: Fresh WordPress installation

## Security Considerations

Security is paramount. All contributions must:

### Required Security Practices

1. **Input Validation**
   ```php
   $user_input = sanitize_text_field( $_POST['field'] );
   $email = sanitize_email( $_POST['email'] );
   $number = absint( $_POST['number'] );
   ```

2. **Output Escaping**
   ```php
   echo esc_html( $text );
   echo esc_attr( $attribute );
   echo esc_url( $url );
   ```

3. **Nonce Verification**
   ```php
   check_ajax_referer( 'wpcm_ajax_nonce', 'nonce' );
   wp_nonce_field( 'wpcm_settings_nonce' );
   ```

4. **Capability Checks**
   ```php
   if ( ! current_user_can( 'manage_options' ) ) {
       wp_die( 'Unauthorized' );
   }
   ```

5. **Database Security**
   ```php
   $wpdb->prepare(
       "SELECT * FROM {$wpdb->prefix}table WHERE id = %d",
       $id
   );
   ```

### Security Checklist

- [ ] All user inputs sanitized
- [ ] All outputs escaped
- [ ] All AJAX requests use nonces
- [ ] All admin functions check capabilities
- [ ] All database queries use prepared statements
- [ ] No `eval()` or dynamic code execution
- [ ] No direct file system access from user input
- [ ] No external URL fetching without validation

## Submission Guidelines

### Pull Request Process

1. **Update documentation** if needed
2. **Add changelog entry** to CHANGELOG.md
3. **Ensure all tests pass**
4. **Update version numbers** if applicable
5. **Request review** from maintainers
6. **Address feedback** promptly
7. **Squash commits** if requested

### Pull Request Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that breaks existing functionality)
- [ ] Documentation update

## Testing
- [ ] Manual testing completed
- [ ] WordPress 5.8+ tested
- [ ] PHP 7.4+ tested
- [ ] No console errors
- [ ] No PHP warnings

## Security
- [ ] Input sanitization verified
- [ ] Output escaping verified
- [ ] Capability checks verified
- [ ] No new vulnerabilities introduced

## Checklist
- [ ] Code follows WordPress coding standards
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] No breaking changes (or documented)
```

### Review Process

1. **Automated checks**: CI/CD pipeline runs
2. **Code review**: Maintainer reviews code quality
3. **Security review**: Security checklist verified
4. **Testing**: Maintainer tests functionality
5. **Approval**: Minimum 1 maintainer approval required
6. **Merge**: Squash and merge to develop

## What to Contribute

### High Priority

- Bug fixes
- Security improvements
- Performance optimizations
- Documentation improvements
- Accessibility enhancements

### Welcome Contributions

- New conflict detection algorithms
- Additional plugin categories
- Internationalization/translations
- UI/UX improvements
- Test coverage
- Code refactoring

### Need Help?

Check issues tagged with:
- `good first issue` - Good for newcomers
- `help wanted` - Maintainers need assistance
- `documentation` - Documentation improvements

## Community

### Communication Channels

- **GitHub Issues**: Bug reports and feature requests
- **GitHub Discussions**: General questions and ideas
- **Pull Requests**: Code contributions

### Getting Help

- Check [README.md](README.md) for usage documentation
- Check [DEVELOPER.md](DEVELOPER.md) for technical documentation
- Search existing issues before creating new ones
- Provide reproduction steps for bugs

## Recognition

Contributors are recognized in:
- CHANGELOG.md (for significant contributions)
- GitHub contributors page
- Release notes

## License

By contributing, you agree that your contributions will be licensed under the GNU Affero General Public License v3.0.

---

**Thank you for contributing to WP Plugin Conflict Mapper!**

Questions? Open an issue or reach out to the maintainers.
