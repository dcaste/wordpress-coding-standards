# WordPress Development Guidelines for AI Agents

This document provides structured guidance for AI coding assistants working with WordPress and WooCommerce projects.

## Project Context

This is a WordPress/WooCommerce development project following modern coding standards with:
- **PHP Version**: 8.3+
- **JavaScript**: Vanilla ES6+ (no jQuery unless approved)
- **CSS Architecture**: BEM-like naming with OOCSS principles
- **Standards**: WordPress Coding Standards

## Core Development Principles

### Code Quality Requirements

1. **Readability First**
   - Write minimal, focused code
   - Keep functions single-purpose
   - Avoid deep nesting (max 3 levels)
   - Lines under 80 characters when possible
   - Use 2-space indentation

2. **Naming Conventions**
   - PHP: `snake_case` for variables/functions
   - JavaScript: `camelCase` for variables/functions
   - Classes: `PascalCase`
   - CSS: BEM notation (`.block__element--modifier`)

3. **Documentation**
   - Comment non-obvious logic
   - Explain business rules and edge cases
   - Use `TODO` for improvement areas
   - English only for all comments

4. **Security**
   - Sanitize ALL input
   - Escape ALL output
   - Use prepared statements for DB queries
   - Validate permissions
   - Implement nonces for forms

## PHP Guidelines (8.3)

### Modern PHP Features
```php
// Use constructor property promotion
class UserProfile {
    public function __construct(
        private string $name,
        private int $age,
        private ?string $email = null
    ) {}
}

// Use match expressions over switch
$result = match($status) {
    'draft' => 'Not published',
    'published' => 'Live',
    'archived' => 'Old content',
    default => 'Unknown'
};

// Use typed properties and return types
function get_user_data(int $user_id): ?array {
    // Implementation
}
```

### WordPress-Specific
```php
// Always sanitize input
$user_input = sanitize_text_field($_POST['field_name']);

// Always escape output
echo esc_html($user_data);
echo esc_url($link);
echo esc_attr($attribute);

// Use WordPress functions
wp_enqueue_script();
wp_localize_script();
wp_nonce_field();

// Check permissions
if (!current_user_can('manage_options')) {
    wp_die('Unauthorized');
}
```

### Module Header Template
```php
/**
 * Module Name: [Module Name]
 * Created: [Date]
 * Author: [Name]
 * 
 * Description:
 * [Summary of functionality]
 * 
 * Functions:
 * - function_name_1() - Description
 * - function_name_2() - Description
 * 
 * Modification History:
 * [Date] - [Author] - [Description of changes]
 */
```

## JavaScript Guidelines

### Modern JavaScript (ES6+)
```javascript
// Use const/let, never var
const API_URL = '/wp-json/custom/v1';
let userData = null;

// Use arrow functions
const getUserData = async (userId) => {
  try {
    const response = await fetch(`${API_URL}/users/${userId}`);
    return await response.json();
  } catch (error) {
    console.error('Error fetching user:', error);
    return null;
  }
};

// Use template literals
const message = `Welcome ${userName}, you have ${count} notifications.`;

// Destructuring
const { name, email, role } = userData;
```

### JavaScript Hooks
```html
<!-- Separate CSS and JS classes -->
<button class="btn btn--primary js-submit-form">
  Submit
</button>
```

## CSS Architecture

### BEM Naming Convention
```css
/* Block - standalone component */
.card { }

/* Element - part of block */
.card__header { }
.card__body { }
.card__footer { }

/* Modifier - variation of block or element */
.card--featured { }
.card--large { }
.card__header--sticky { }
```

### File Structure
```css
/*------------------------------------*\
  #SECTION-TITLE
\*------------------------------------*/

.selector {
  property: value;
}

/* 1 line between related rulesets */
/* 2 lines between loosely related rulesets */
/* 5 lines between sections */
```

### Selector Best Practices
```css
/* ✅ Good - specific, reusable */
.site-nav { }
.btn { }
.product-card { }

/* ❌ Bad - location dependent */
header ul { }
.sidebar a { }

/* ❌ Bad - overly specific */
input.btn { }
#content .widget ul li { }
```

### OOCSS Pattern
```css
/* Structure (reusable) */
.btn {
  display: inline-block;
  padding: 1em 2em;
  text-align: center;
}

/* Skin (variation) */
.btn--primary {
  background-color: blue;
  color: white;
}

.btn--secondary {
  background-color: gray;
  color: white;
}
```

## WordPress/WooCommerce Specifics

### Performance Optimization
```php
// Use transients for expensive queries
$data = get_transient('my_cached_data');
if (false === $data) {
    $data = expensive_database_query();
    set_transient('my_cached_data', $data, HOUR_IN_SECONDS);
}

// Minimize queries
remove_action('wp_head', 'wp_generator');
remove_action('wp_head', 'wlwmanifest_link');
```

### WooCommerce Hooks
```php
// Use proper hooks
add_action('woocommerce_before_shop_loop', 'custom_function');
add_filter('woocommerce_product_get_price', 'modify_price', 10, 2);

// Check if WooCommerce is active
if (class_exists('WooCommerce')) {
    // WooCommerce-specific code
}
```

## Error Handling

### PHP
```php
try {
    // Code that may throw exception
    $result = risky_operation();
} catch (Exception $e) {
    error_log('Error: ' . $e->getMessage());
    // Handle gracefully
} finally {
    // Cleanup if needed
}
```

### JavaScript
```javascript
try {
  const result = await fetchData();
  processResult(result);
} catch (error) {
  console.error('Operation failed:', error);
  showErrorMessage('An error occurred. Please try again.');
} finally {
  hideLoader();
}
```

## Code Review Checklist

Before submitting code, verify:

- [ ] All input is sanitized
- [ ] All output is escaped
- [ ] Type declarations are used (PHP)
- [ ] Naming conventions followed
- [ ] Comments explain "why", not "what"
- [ ] No hardcoded values (use constants)
- [ ] Functions are single-purpose
- [ ] No deep nesting (refactor if >3 levels)
- [ ] Error handling implemented
- [ ] Performance considered (caching, queries)
- [ ] Security best practices followed
- [ ] WordPress coding standards met
- [ ] BEM naming for CSS
- [ ] No jQuery (unless approved)
- [ ] Backward compatibility maintained

## Common Patterns

### Custom Post Type
```php
function register_custom_post_type(): void {
    register_post_type('project', [
        'labels' => [
            'name' => __('Projects'),
            'singular_name' => __('Project')
        ],
        'public' => true,
        'has_archive' => true,
        'supports' => ['title', 'editor', 'thumbnail'],
        'rewrite' => ['slug' => 'projects'],
    ]);
}
add_action('init', 'register_custom_post_type');
```

### AJAX Handler
```php
// PHP handler
function handle_ajax_request(): void {
    check_ajax_referer('my_nonce', 'security');
    
    $data = sanitize_text_field($_POST['data']);
    
    // Process data
    $result = process_data($data);
    
    wp_send_json_success($result);
}
add_action('wp_ajax_my_action', 'handle_ajax_request');
add_action('wp_ajax_nopriv_my_action', 'handle_ajax_request');
```

```javascript
// JavaScript caller
const sendAjaxRequest = async (data) => {
  const formData = new FormData();
  formData.append('action', 'my_action');
  formData.append('security', myAjax.nonce);
  formData.append('data', data);
  
  try {
    const response = await fetch(myAjax.ajaxurl, {
      method: 'POST',
      body: formData
    });
    return await response.json();
  } catch (error) {
    console.error('AJAX error:', error);
    throw error;
  }
};
```

## Testing Guidelines

### Manual Testing Checklist
- Test with different user roles
- Check responsive behavior
- Verify form submissions
- Test error scenarios
- Check performance (query count)
- Validate accessibility
- Cross-browser testing

### Code Quality
- Use PHP_CodeSniffer with WordPress ruleset
- ESLint for JavaScript
- Run security scans
- Check for deprecated functions

## Version Control

### Commit Message Format
```
type: Brief description (50 chars max)

Detailed explanation if needed:
- What changed
- Why it changed
- Any breaking changes

References: #issue-number
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code restructuring
- `style`: Formatting changes
- `docs`: Documentation
- `perf`: Performance improvements
- `test`: Testing
- `chore`: Maintenance

## Resources

- [WordPress Coding Standards](https://developer.wordpress.org/coding-standards/)
- [PHP The Right Way](https://phptherightway.com/)
- [WooCommerce Development](https://woocommerce.com/document/create-a-plugin/)
- [CSS Guidelines by Harry Roberts](https://cssguidelin.es/)

---

**Note**: This document is meant to be used by AI coding assistants to maintain consistency with the project's established standards. Always prioritize security, performance, and maintainability.