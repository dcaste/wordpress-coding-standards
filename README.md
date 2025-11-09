# WordPress Development Reference - Coding Standards and Guidelines

## Overview

This document provides comprehensive coding standards and best practices for WordPress and WooCommerce development, focusing on maintainable, scalable, and secure code implementation.

## General Coding Standards

### Purpose and Benefits

Coding standards ensure consistency, readability, and collaboration across the development team, while reducing errors and improving code quality. Key benefits include:

- **Consistency**: Uniform codebase for easier reading and maintenance
- **Error Prevention**: Early detection of common mistakes
- **Scalability**: Code that can grow without becoming unmanageable
- **Cross-Team Collaboration**: Shared standards facilitate teamwork
- **Efficient Maintenance**: Simplified debugging and refactoring


### Core Best Practices

#### 1. Code Readability

- Write minimal lines of code
- Use appropriate naming conventions (camelCase for JavaScript, snake_case for PHP)
- Segment code into logical paragraphs
- Use proper indentation (2 spaces)
- Keep functions focused on single tasks
- Apply DRY principle (Don't Repeat Yourself)
- Avoid deep nesting
- Keep lines under 80 characters when possible


#### 2. Naming Conventions

- **camelCase**: JavaScript variables and functions (`userName`, `reverseName()`)
- **PascalCase**: Class names (`Person`, `UserProfile`)
- **kebab-case**: CSS classes (when needed, though BEM is preferred)
- Use meaningful names that convey purpose
- Avoid single identifiers for multiple purposes


#### 3. Documentation and Comments

- Comment non-obvious code segments
- Explain business rules and domain-specific logic
- Document edge cases and exceptional scenarios
- Mark areas needing improvement with `TODO`
- Avoid redundant comments that repeat obvious code
- Use English for all code comments


#### 4. Exception Handling

- Use try-catch blocks for error-prone code
- Enable auto-recovery mechanisms
- Implement real-time log analysis
- Handle network/software slowness gracefully

```javascript
try {
    // Code that may throw exception
    const result = numerator / denominator;
} catch (error) {
    console.error("Error:", error.message);
} finally {
    // Cleanup tasks
}
```


#### 5. Security and Privacy

- Extract insights without compromising privacy
- Collect only necessary user data
- Implement proper data validation
- Follow security best practices


#### 6. Version Control

- Make daily backups after every modification
- Use consistent commit messages
- Follow team branching strategies
- Document modification history in module headers


## CSS Guidelines

### Syntax and Formatting

#### File Structure

- Split CSS into multiple files by component/section
- Include table of contents in main stylesheet
- Use 80-character line width
- Multi-line CSS (one declaration per line)
- 2-space indentation


#### Section Organization

```css
/*------------------------------------*\
  #SECTION-TITLE
\*------------------------------------*/

.selector {
    property: value;
}
```


#### Ruleset Anatomy

```css
.foo,
.foo--bar,
.baz {
    display: block;
    background-color: green;
    color: red;
}
```


#### Whitespace Usage

- 1 empty line between closely related rulesets
- 2 empty lines between loosely related rulesets
- 5 empty lines between new sections


### BEM-like Naming Convention

#### Structure

- **Block**: Root component (`.person`)
- **Element**: Component part (`.person__head`)
- **Modifier**: Variant or extension (`.person--tall`)


#### Syntax Rules

- Elements use double underscore: `__`
- Modifiers use double hyphen: `--`
- Hyphen-delimited strings for multi-word names


#### Examples

```css
/* Block */
.btn { }

/* Element */
.btn__icon { }

/* Modifier */
.btn--primary { }
.btn--large { }

/* Combined */
.profile { }
.profile__avatar { }
.profile__bio { }
.profile--featured { }
```


#### HTML Implementation

```html
<div class="[ box  box--highlight ]  [ profile  profile--featured ]">
    <img class="profile__avatar" />
    <p class="profile__bio">...</p>
</div>
```


### CSS Selectors

#### Selector Intent

- Select elements for specific, well-reasoned purposes
- Avoid overly greedy selectors
- Use explicit, unambiguous selectors

```css
/* Bad - poor selector intent */
header ul { }

/* Good - explicit selector */
.site-nav { }
```


#### Location Independence

- Style based on what components are, not where they are
- Avoid location-dependent selectors
- Prefer reusable classes

```css
/* Bad - location dependent */
.promo a { }

/* Good - location independent */
.btn { }
```


#### Specificity Management

- Avoid IDs in CSS
- Minimize nesting (especially in preprocessors)
- Use `!important` sparingly and only when justified
- Prefer multiple classes over qualified selectors

```css
/* Bad - qualified selector */
input.btn { }

/* Good - unqualified, reusable */
.btn { }
```


### JavaScript Hooks

- Never bind CSS and JavaScript to the same class
- Use `js-` prefix for JavaScript-specific classes
- Don't use `data-*` attributes as hooks (they're for storing data)

```html
<!-- Correct separation -->
<input type="submit" class="btn  js-btn" value="Follow" />
```


### Commenting

#### High-level Comments

```css
/**
 * The site's main page-head can have two different states:
 *
 * 1) Regular page-head with no backgrounds or extra treatments
 * 2) Masthead with fluid-height and large background image
 *
 * The regular page-head is simple, but the masthead version has
 * slightly intermingled dependency with its wrapper.
 */
```


#### Low-level Comments (Footnote style)

```css
.selector {
    display: block; /* [^1] */
    padding-top: 56.25%; /* [^2] */
}

/**
 * [^1] Ensures proper rendering context
 * [^2] Creates 16:9 aspect ratio using padding technique
 */
```


## Architectural Principles

### Object-Oriented CSS (OOCSS)

#### Separation of Structure and Skin

```css
/* Structure (object) */
.btn {
    display: inline-block;
    padding: 1em 2em;
    vertical-align: middle;
}

/* Skin (theme) */
.btn--positive {
    background-color: green;
    color: white;
}

.btn--negative {
    background-color: red;
    color: white;
}
```


### Single Responsibility Principle

- Each class should have one clear purpose
- Easier to maintain and understand
- Promotes reusability


### Open/Closed Principle

- Open for extension, closed for modification
- Extend via additional classes, not by editing originals
- Use modifier classes to add variations


### DRY (Don't Repeat Yourself)

- Abstract meaningful repetition
- Don't force abstractions for coincidental similarities
- Use mixins for thematically grouped declarations


### Composition Over Inheritance

- Favor multiple classes over complex inheritance chains
- Better paper trail in markup
- Greater flexibility and composition


## PHP Standards (PHP 8.3)

### Core Requirements

- Use PHP 8.3 features and syntax
- Follow WordPress Coding Standards
- Implement proper type declarations
- Use modern PHP features (match expressions, constructor property promotion, etc.)


### Security

- Sanitize all input data
- Escape all output
- Use prepared statements for database queries
- Validate user permissions
- Implement nonces for form submissions


## JavaScript Standards

### Modern JavaScript (ES6+)

- Use vanilla JavaScript only (no jQuery unless specifically approved)
- Implement modern syntax (arrow functions, const/let, template literals)
- Use async/await for asynchronous operations
- Follow modular patterns


### Code Organization

- Keep functions small and focused
- Use meaningful variable names
- Comment complex logic
- Implement error handling


## WordPress/WooCommerce Specific

### Performance Optimization

- Minimize database queries
- Use transients for caching
- Optimize asset loading
- Implement lazy loading where appropriate


### Code Reviews and Refactoring

- Engage QA during refactoring
- Isolate debugging from refactoring
- Document all changes
- Test thoroughly before deployment


### Module Headers

Each module should include:

- Module name
- Date of creation
- Creator name
- Modification history
- Summary of functionality
- Functions list
- Variables accessed


## Tools and Workflow

### Code Quality Management

- Use linting tools
- Implement automated testing
- Regular code reviews
- Continuous integration


### Best Practices Summary

1. **Consistency**: Maintain uniform coding style
2. **Documentation**: Comment thoroughly and meaningfully
3. **Modularity**: Break code into reusable components
4. **Performance**: Optimize for speed and efficiency
5. **Security**: Always validate, sanitize, and escape
6. **Maintainability**: Write code others can understand
7. **Testing**: Test all functionality before deployment
8. **Version Control**: Commit regularly with clear messages

***

*This reference document combines coding standards from BrowserStack best practices and CSS Guidelines by Harry Roberts, adapted for WordPress/WooCommerce development with PHP 8.3 and modern JavaScript practices.*

[^2]: CSS-Guidelines-2.2.5-High-level-advice-and-guidelines-for-writing-sane-manageable-scalable-CSS.pdf

