# WP Plugin Conflict Mapper

**Author**: Jonathan
**Version**: 1.0.0
**License**: GNU Affero General Public License v3.0
**Requires at least**: WordPress 5.8
**Requires PHP**: 7.4+

> Advanced plugin overlap and conflict diagnostics with ranked plugin recommendations for WordPress

## Overview

WP Plugin Conflict Mapper is a comprehensive WordPress plugin that helps you identify and resolve conflicts between installed plugins. It analyzes your WordPress installation to detect:

- **Hook Conflicts**: Multiple plugins using the same WordPress hooks
- **Function Name Conflicts**: Duplicate function definitions across plugins
- **Global Variable Conflicts**: Shared global variable usage
- **Database Table Conflicts**: Plugins creating or using the same database tables
- **Functional Overlaps**: Multiple plugins providing similar functionality
- **Security Vulnerabilities**: Potential security issues in plugin code
- **Performance Impact**: Plugin size, complexity, and resource usage

## Features

### 🔍 Conflict Detection
- Automatically scans all active plugins for conflicts
- Identifies hook, function, global variable, and database table conflicts
- Categorizes conflicts by severity (low, medium, high, critical)
- Provides actionable recommendations for resolution

### 📊 Plugin Rankings
- Ranks plugins based on compatibility and performance
- Scores range from 0-100 with detailed breakdowns
- Identifies problematic plugins that may need replacement
- Provides insights into plugin complexity and resource usage

### 🔐 Security Scanning
- Scans for dangerous function usage (eval, exec, system, etc.)
- Detects potential SQL injection vulnerabilities
- Identifies XSS (Cross-Site Scripting) risks
- Checks for insecure file operations

### ⚡ Performance Analysis
- Measures plugin file size and complexity
- Counts database tables created by plugins
- Analyzes asset loading (CSS/JS files)
- Calculates hook usage intensity

### 📈 Detailed Reports
- Save scan results for historical comparison
- Export reports in JSON or CSV format
- View detailed conflict information
- Track improvements over time

### 🎯 Functional Overlap Detection
- Identifies plugins with similar purposes
- Recommends consolidation opportunities
- Suggests popular alternatives for each category
- Helps reduce plugin bloat

### 🔧 Multiple Interfaces
- **WordPress Admin Dashboard**: User-friendly web interface
- **REST API**: Programmatic access for automation
- **WP-CLI**: Command-line interface for developers

## Installation

### From Source

1. Download or clone this repository
2. Upload the entire `wp-plugin-conflict-mapper` folder to `/wp-content/plugins/`
3. Activate the plugin through the 'Plugins' menu in WordPress
4. Navigate to **Conflict Mapper** in the WordPress admin menu

### Requirements

- WordPress 5.8 or higher
- PHP 7.4 or higher
- MySQL 5.6 or higher

## Usage

### Running a Scan

#### Via Admin Dashboard

1. Go to **Conflict Mapper → Dashboard**
2. Click **Run New Scan**
3. Wait for the scan to complete
4. Review the results showing conflicts, overlaps, and recommendations

#### Via WP-CLI

```bash
# Run a basic scan
wp conflict-mapper scan

# Run a scan and save results
wp conflict-mapper scan --save

# Export scan as JSON
wp conflict-mapper scan --format=json

# List all plugins with scores
wp conflict-mapper list-plugins

# Get detailed report for a specific plugin
wp conflict-mapper report akismet
```

#### Via REST API

```bash
# Run a scan
curl -X POST https://yoursite.com/wp-json/wpcm/v1/scan \
  -H "Authorization: Bearer YOUR_TOKEN"

# Get scan results
curl https://yoursite.com/wp-json/wpcm/v1/scan/1 \
  -H "Authorization: Bearer YOUR_TOKEN"

# Get all plugins
curl https://yoursite.com/wp-json/wpcm/v1/plugins \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Understanding Scan Results

#### Conflict Types

**Hook Conflicts**
- Multiple plugins hooking into the same WordPress action or filter
- Can cause unexpected behavior or performance issues
- Severity depends on the hook and number of plugins using it

**Function Conflicts**
- Two or more plugins defining functions with the same name
- Will cause fatal PHP errors if both plugins are active
- Always high severity

**Global Variable Conflicts**
- Plugins using the same global variable names
- Can lead to data corruption or unexpected behavior
- Medium severity in most cases

**Database Table Conflicts**
- Multiple plugins creating or accessing the same database tables
- High risk of data corruption
- Always high severity

#### Overlap Categories

The plugin categorizes overlaps into common functionality areas:
- SEO
- Caching
- Security
- Backup
- Forms
- E-commerce
- Social Media
- Analytics
- Media Management
- Email/Newsletter
- Page Builders
- Spam Protection

### Plugin Rankings

Plugins are scored 0-100 based on:

- **Conflicts** (max -40 points): Penalties for involvement in conflicts
- **Overlaps** (max -30 points): Penalties for functional redundancy
- **Complexity** (max -20 points): Code complexity and size
- **Size** (max -10 points): File size impact
- **Maintenance**: Version information availability

**Score Interpretation:**
- 80+: Excellent - Well-behaved, minimal issues
- 60-79: Good - Minor issues to monitor
- 40-59: Fair - Several issues, review recommended
- <40: Poor - Significant problems, consider alternatives

### Security Analysis

The security scanner checks for:

1. **Dangerous Functions**: eval(), exec(), system(), shell_exec(), etc.
2. **SQL Injection Risks**: Direct database queries without preparation
3. **XSS Vulnerabilities**: Unescaped user input
4. **File Operation Risks**: Insecure file handling

**Risk Levels:**
- **Safe**: No issues detected
- **Low**: Minor concerns, generally acceptable
- **Medium**: Some issues present, monitoring recommended
- **High**: Significant vulnerabilities found
- **Critical**: Severe security problems, immediate action required

### Settings

Configure the plugin behavior:

**Automatic Scanning**
- Enable/disable scheduled scans
- Set frequency: daily, weekly, or monthly

**Data Retention**
- Specify how long to keep scan results
- Default: 30 days

**Alert Threshold**
- Choose minimum severity for notifications
- Options: low, medium, high, critical

**Email Reports**
- Enable/disable email notifications
- Configure notification email address

### Export & Reporting

Export scan results in multiple formats:

**JSON Export**
- Complete scan data with all details
- Ideal for programmatic processing
- Includes metadata and timestamps

**CSV Export**
- Conflict summary in spreadsheet format
- Easy to share with non-technical stakeholders
- Includes conflict type, severity, and affected plugins

## Best Practices

### Conflict Resolution

1. **Identify High-Severity Conflicts First**
   - Focus on function and table conflicts
   - These can cause immediate site breakage

2. **Review Functional Overlaps**
   - Keep only one plugin per function
   - Example: Use only one SEO plugin, one caching plugin

3. **Monitor Hook Conflicts**
   - Some hook conflicts are acceptable
   - Watch for conflicts on critical hooks (init, wp_head, etc.)

4. **Test Changes**
   - Always test in a staging environment first
   - Deactivate one plugin at a time
   - Verify functionality after each change

### Performance Optimization

1. **Reduce Plugin Count**
   - Each plugin adds overhead
   - Consolidate functionality where possible

2. **Choose Lightweight Alternatives**
   - Use the rankings to identify heavy plugins
   - Look for alternatives with better scores

3. **Regular Scans**
   - Run scans after plugin updates
   - Schedule periodic scans (weekly recommended)

4. **Clean Up Unused Plugins**
   - Delete inactive plugins
   - Remove plugins that are no longer needed

## Developer Documentation

### Extending the Plugin

#### Hooks & Filters

```php
// Modify scan results before saving
add_filter('wpcm_scan_results', function($results) {
    // Your custom logic
    return $results;
});

// Add custom conflict detector
add_action('wpcm_detect_conflicts', function($plugins) {
    // Your custom detection logic
});

// Modify plugin ranking
add_filter('wpcm_plugin_score', function($score, $plugin_file) {
    // Adjust score based on custom criteria
    return $score;
}, 10, 2);
```

#### REST API Endpoints

**Base URL**: `/wp-json/wpcm/v1/`

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/plugins` | GET | Get all installed plugins |
| `/scan` | POST | Run a new conflict scan |
| `/scan/{id}` | GET | Get specific scan results |
| `/scans` | GET | Get recent scans |
| `/stats` | GET | Get scanning statistics |

**Authentication**: All endpoints require `manage_options` capability.

#### Database Schema

**wpcm_scans**
```sql
CREATE TABLE wp_wpcm_scans (
    id bigint(20) UNSIGNED NOT NULL AUTO_INCREMENT,
    scan_date datetime NOT NULL,
    plugin_count int(11) NOT NULL,
    conflict_count int(11) NOT NULL,
    overlap_count int(11) NOT NULL,
    scan_data longtext,
    scan_type varchar(50) NOT NULL,
    PRIMARY KEY (id)
);
```

**wpcm_conflicts**
```sql
CREATE TABLE wp_wpcm_conflicts (
    id bigint(20) UNSIGNED NOT NULL AUTO_INCREMENT,
    scan_id bigint(20) UNSIGNED NOT NULL,
    conflict_type varchar(50) NOT NULL,
    severity varchar(20) NOT NULL,
    affected_plugins text,
    conflict_data longtext,
    created_at datetime NOT NULL,
    PRIMARY KEY (id)
);
```

### Code Examples

#### Custom Scan Handler

```php
add_action('wpcm_loaded', function() {
    $scanner = wpcm()->scanner;
    $detector = wpcm()->detector;

    $plugins = $scanner->get_active_plugins();
    $conflicts = $detector->detect_conflicts($plugins);

    // Process results
    foreach ($conflicts as $type => $conflict_list) {
        // Handle each conflict type
    }
});
```

#### Cache Integration

```php
$cache = new WPCM_Cache();

// Store data
$cache->set('my_key', $data, 3600);

// Retrieve data
$data = $cache->get('my_key');

// Remember pattern (get or generate)
$data = $cache->remember('my_key', function() {
    return expensive_operation();
}, 3600);
```

## Troubleshooting

### Common Issues

**Scan Takes Too Long**
- Large number of plugins (50+) can slow scans
- Increase PHP max_execution_time
- Run scans via WP-CLI for better performance

**Memory Errors**
- Increase PHP memory_limit (recommended: 256M+)
- Reduce number of active plugins
- Use command-line scanning for large installations

**Database Errors**
- Ensure database user has CREATE TABLE permissions
- Check database server connection
- Verify table prefix matches WordPress config

**Permission Errors**
- Plugin requires `manage_options` capability
- Only administrators can run scans
- Check user roles and capabilities

## Performance Considerations

- Scans can be resource-intensive on large sites
- Use caching to improve subsequent scan performance
- Schedule scans during low-traffic periods
- Consider WP-CLI for large installations
- Limit scan history retention for better database performance

## Security

- All AJAX requests use nonce verification
- Capability checks on all admin operations
- SQL queries use prepared statements
- User input is properly sanitized and escaped
- REST API endpoints require authentication

## Support

For issues, questions, or contributions:

- **GitHub Issues**: [Report bugs](https://github.com/Hyperpolymath/wp-plugin-conflict-mapper/issues)
- **Documentation**: See CLAUDE.md for development guidelines
- **License**: GNU AGPL v3.0 - See LICENSE file

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

## License

This project is licensed under the GNU Affero General Public License v3.0. See the [LICENSE](LICENSE) file for details.

### What This Means

- You may use, modify, and distribute this software
- Any modifications must also be released under AGPL v3.0
- If you run a modified version on a network server, you must provide source code access
- Maintain copyright and license notices
- No warranty is provided

## Credits

Developed by Jonathan as part of WordPress performance and security optimization research.

## Roadmap

Future enhancements being considered:

- [ ] Visual dependency graphs
- [ ] Integration with plugin vulnerability databases
- [ ] Automated conflict resolution suggestions
- [ ] Multi-site network support
- [ ] Plugin comparison tool
- [ ] Performance benchmarking
- [ ] Email digest reports
- [ ] Integration with monitoring tools

## Contributing

Contributions are welcome! Please follow WordPress coding standards and include appropriate tests.

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

**Made with ❤️ for the WordPress community**
