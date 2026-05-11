# Style Guide

Last Updated: May 12, 2024

## 1. Code Formatting

### 1.1 Indentation

- 2 spaces (JavaScript)
- 4 spaces (Python)
- Tabs: Converted to spaces
- No mixed indentation
- IDE auto-formatting
- EditorConfig configured

### 1.2 Line Length

- 80 characters (documentation)
- 100 characters (code)
- 120 characters (long expressions)
- Break logical sections
- Readability prioritized

## 2. Naming Conventions

### 2.1 Variables

- camelCase in JavaScript
- snake_case in Python
- Descriptive names (>2 chars)
- No single letters (except i, j)
- Boolean: is/has prefix
- Constants: UPPER_CASE

### 2.2 Functions

- Verb prefix (get, set, is)
- camelCase (JS), snake_case (Python)
- Descriptive action
- Avoid abbreviations
- Clear intent

## 3. Comments

### 3.1 Inline Comments

- Explain 'why' not 'what'
- Above the code
- Concise and clear
- Current with code
- TODO markers allowed

### 3.2 Block Comments

- For complex logic
- Multi-line explanations
- Algorithm descriptions
- Edge case handling
- Performance notes

### 3.3 Documentation

- JSDoc/docstrings
- Parameter description
- Return type
- Exceptions
- Usage examples

## 4. File Organization

### 4.1 Structure

- Header comment
- Imports at top
- Constants
- Types/interfaces
- Main logic
- Utilities
- Exports

### 4.2 File Size

- Maximum 500 lines
- Single responsibility
- Cohesive functions
- Clear purpose
- Easier testing

## 5. Imports

### 5.1 Organization

- Standard library first
- Third-party packages
- Internal modules
- One import per line
- Alphabetically sorted
- No circular dependencies

### 5.2 Practices

- No unused imports
- Explicit paths
- Relative imports for local
- Absolute for packages
- Organized sections

## 6. Spacing

### 6.1 Blank Lines

- Two lines between functions
- One line between methods
- Around logical sections
- Around constants
- No multiple blank lines

### 6.2 Operators

- Space around operators
- No space in parentheses
- Space after commas
- Operator precedence
- Parentheses when unclear

## 7. Language-Specific

### 7.1 JavaScript

- Use const by default
- let for reassigned
- Avoid var
- Template literals for strings
- Arrow functions
- async/await preferred

### 7.2 Python

- PEP 8 compliance
- Type hints recommended
- Docstrings for modules
- f-strings for formatting
- List comprehensions

## 8. Git Commits

### 8.1 Message Format

- Type(scope): subject
- feat(auth): add login
- fix(api): handle null
- docs(readme): update
- Empty line then body

### 8.2 Best Practices

- Present tense
- Imperative mood
- Capitalized subject
- 50 character limit
- Body: 72 characters
- Reference issues: #123

## 9. Contact

- Code Review: code-review@lutervyn.com
- Style Questions: style@lutervyn.com
