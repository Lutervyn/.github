# Coding Standards

Effective Date: January 1, 2024
Last Updated: May 12, 2024

## 1. Overview

This document defines the coding standards and best practices for all Lutervyn projects. These standards ensure code quality, maintainability, consistency, and security across all projects.

## 2. General Principles

### 2.1 Readability First

- Code is read more often than written
- Optimize for human understanding
- Clear intent matters more than cleverness
- Comments should explain why, not what
- Use descriptive names

### 2.2 Maintainability

- Code should be easy to modify
- Follow established patterns
- Avoid tight coupling
- Keep functions focused
- Document important decisions

### 2.3 Performance

- Write efficient code
- Avoid premature optimization
- Profile before optimizing
- Consider algorithmic complexity
- Balance readability and performance

### 2.4 Security

- Follow security best practices
- Validate all inputs
- Sanitize outputs
- Use secure libraries
- Never hardcode secrets
- Keep dependencies updated

## 3. Naming Conventions

### 3.1 Variables

```python
# Good
user_count = 0
total_revenue = 100.50
is_active = True
MAX_RETRIES = 3

# Bad
uc = 0
tr = 100.50
active = True
max_retries = 3  # Should be uppercase for constants
```

### 3.2 Functions

```python
# Good
def calculate_total_price(items):
    pass

def validate_email(email):
    pass

def get_user_by_id(user_id):
    pass

# Bad
def calc(i):
    pass

def validEmail(e):
    pass

def user(id):
    pass
```

### 3.3 Classes

```python
# Good
class UserManager:
    pass

class ValidationError(Exception):
    pass

# Bad
class usermanager:
    pass

class userManager:  # Inconsistent with convention
    pass
```

### 3.4 Constants

```python
# Good
MAX_USERS = 1000
DEFAULT_TIMEOUT = 30
APP_VERSION = "1.0.0"

# Bad
max_users = 1000
defaultTimeout = 30
appVersion = "1.0.0"
```

## 4. Function Design

### 4.1 Function Length

- Keep functions focused and small
- Aim for <30 lines per function
- Functions should do one thing well
- Consider readability and maintainability
- Break complex logic into smaller functions

### 4.2 Function Parameters

```python
# Good - few parameters
def create_user(name, email, role):
    pass

# Bad - too many parameters
def create_user(name, email, role, status, created_at, updated_at, active, verified):
    pass

# Better - use configuration object
class UserConfig:
    name: str
    email: str
    role: str
    status: str = "active"
    verified: bool = False

def create_user(config: UserConfig):
    pass
```

### 4.3 Return Values

- Functions should return consistent types
- Avoid returning None when not needed
- Use result objects for complex returns
- Raise exceptions for errors, don't return error codes
- Use tuple unpacking for multiple returns

## 5. Code Organization

### 5.1 File Structure

```
project/
├── src/
│   ├── __init__.py
│   ├── main.py
│   ├── models.py
│   ├── services/
│   │   ├── __init__.py
│   │   └── user_service.py
│   └── utils/
│       ├── __init__.py
│       └── validators.py
├── tests/
│   ├── __init__.py
│   ├── test_main.py
│   └── test_services/
├── docs/
├── requirements.txt
└── README.md
```

### 5.2 Imports

```python
# Standard library imports
import os
import sys
from pathlib import Path

# Third-party imports
import requests
import numpy as np

# Local imports
from .models import User
from .utils import validate_email
```

### 5.3 Module Organization

1. Module docstring
2. Imports
3. Constants
4. Classes
5. Functions
6. Main block

## 6. Error Handling

### 6.1 Exception Handling

```python
# Good - specific exceptions
try:
    user = get_user(user_id)
except UserNotFoundError:
    logger.warning(f"User {user_id} not found")
    return None
except DatabaseError as e:
    logger.error(f"Database error: {e}")
    raise

# Bad - catching all exceptions
try:
    user = get_user(user_id)
except:
    pass
```

### 6.2 Custom Exceptions

```python
class LutervynException(Exception):
    """Base exception for Lutervyn"""
    pass

class ValidationError(LutervynException):
    """Validation failed"""
    pass

class ResourceNotFoundError(LutervynException):
    """Resource not found"""
    pass
```

## 7. Comments and Documentation

### 7.1 Comments

```python
# Good - explain why
if user.created_at < thirty_days_ago:
    # Archive old inactive users to improve performance
    archive_user(user)

# Bad - state the obvious
if user.created_at < thirty_days_ago:  # if user created before 30 days
    archive_user(user)
```

### 7.2 Docstrings

```python
def calculate_discount(price, quantity):
    """
    Calculate volume discount on purchases.
    
    Args:
        price: Product price per unit
        quantity: Number of units purchased
        
    Returns:
        float: Discounted total price
        
    Raises:
        ValueError: If price or quantity is negative
        
    Examples:
        >>> calculate_discount(10.00, 5)
        47.50
    """
    if price < 0 or quantity < 0:
        raise ValueError("Price and quantity must be positive")
    
    total = price * quantity
    if quantity >= 10:
        return total * 0.9  # 10% discount
    elif quantity >= 5:
        return total * 0.95  # 5% discount
    return total
```

## 8. Type Hints

```python
# Good - use type hints
from typing import List, Optional, Dict

def get_users(active_only: bool = True) -> List[User]:
    pass

def find_user_by_email(email: str) -> Optional[User]:
    pass

def get_user_settings(user_id: int) -> Dict[str, any]:
    pass
```

## 9. Testing Standards

### 9.1 Test Coverage

- Minimum 80% code coverage
- 100% coverage for critical paths
- All public APIs tested
- Happy path and error cases
- Edge cases considered

### 9.2 Test Naming

```python
# Good - describes what is tested
test_calculate_discount_with_quantity_10_returns_90_percent_price

test_get_user_by_invalid_id_raises_error

test_create_user_with_valid_data_succeeds

# Less clear
test_discount

test_user
```

### 9.3 Test Structure

```python
class TestUserCreation:
    def setup_method(self):
        """Setup test fixtures"""
        self.user_data = {"name": "John", "email": "john@example.com"}
    
    def test_create_user_with_valid_data(self):
        """Test user creation with valid data"""
        # Arrange
        expected_name = "John"
        
        # Act
        user = create_user(self.user_data)
        
        # Assert
        assert user.name == expected_name
        assert user.email == "john@example.com"
```

## 10. Performance Considerations

### 10.1 Time Complexity

- Understand algorithm complexity
- Avoid O(n²) in loops when possible
- Consider caching for expensive operations
- Profile before optimizing

### 10.2 Memory Usage

- Avoid unnecessary copies
- Use generators for large data sets
- Clean up resources properly
- Be aware of memory leaks

## 11. Security Practices

### 11.1 Input Validation

```python
# Good
def create_user(email: str, age: int):
    if not validate_email(email):
        raise ValidationError("Invalid email")
    if age < 0 or age > 150:
        raise ValidationError("Invalid age")
    # ... continue

# Bad
def create_user(email, age):
    # Trusting user input
    # ... continue
```

### 11.2 Secrets Management

```python
# Good - use environment variables or secure vaults
db_password = os.getenv("DATABASE_PASSWORD")
api_key = get_secret("api_key")

# Bad - hardcoded secrets
db_password = "secretpassword123"
api_key = "sk_live_1234567890"
```

## 12. Language-Specific Standards

### 12.1 Python

- Follow PEP 8
- Use `black` for formatting
- Use `pylint` or `flake8` for linting
- Type hints recommended (PEP 484)
- Docstrings for all modules, classes, functions

### 12.2 JavaScript/TypeScript

- Follow ESLint configuration
- Use Prettier for formatting
- Use strict mode
- Prefer const over let over var
- Use async/await over callbacks

### 12.3 Java

- Follow Java conventions
- Use Maven or Gradle
- JavaDoc for public APIs
- Follow Google Java Style
- Use spotbugs for quality

## 13. Code Review Checklist

Before submitting code for review:

- [ ] Code follows naming conventions
- [ ] Code is documented
- [ ] Tests are included
- [ ] Code coverage is adequate
- [ ] No hardcoded secrets
- [ ] Error handling is appropriate
- [ ] Performance is acceptable
- [ ] Security best practices followed
- [ ] Linting passes
- [ ] Type hints used (where applicable)

## 14. Tools and Enforcement

### 14.1 Linters

- Python: `pylint`, `flake8`
- JavaScript: `eslint`
- Java: `checkstyle`

### 14.2 Formatters

- Python: `black`
- JavaScript: `prettier`
- Java: `google-java-format`

### 14.3 CI/CD Integration

- Linting in pre-commit hooks
- Automated checks in CI pipeline
- Build fails on violations
- Coverage reports required
- Security scanning integrated

## 15. Questions and Clarifications

For coding standards questions:

- Ask in #engineering-standards Slack channel
- Check documentation wiki
- Bring up in code review
- Request standards discussion
- Contact engineering-lead@lutervyn.com
