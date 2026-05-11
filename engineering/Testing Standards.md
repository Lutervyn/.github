# Testing Standards

Effective Date: January 1, 2024
Last Updated: May 12, 2024

## 1. Overview

This document defines testing standards for all Lutervyn projects. Quality testing ensures reliability, prevents regressions, and maintains code quality.

## 2. Testing Pyramid

```
       △ E2E Tests
      △△ Integration
     △△△ Unit Tests
```

**Distribution:**
- Unit Tests: 70%
- Integration Tests: 20%
- E2E Tests: 10%

## 3. Unit Testing

### 3.1 Coverage Requirements

- Minimum 80% code coverage
- 100% coverage for critical paths
- Critical business logic fully covered
- Public APIs all tested
- Error cases tested

### 3.2 Test Structure

```python
class TestCalculateDiscount:
    def setup_method(self):
        self.calculator = DiscountCalculator()
    
    def test_no_discount_for_quantity_under_5(self):
        result = self.calculator.calculate(price=100, quantity=4)
        assert result == 400
    
    def test_5_percent_discount_for_quantity_5_to_9(self):
        result = self.calculator.calculate(price=100, quantity=5)
        assert result == 475  # 5% discount
    
    def teardown_method(self):
        self.calculator = None
```

### 3.3 Assertions

```python
# Good - specific assertions
assert user.name == "John"
assert user.email == "john@example.com"
assert len(users) == 5

# Bad - too broad
assert user
assert users
```

## 4. Integration Testing

### 4.1 Scope

- Multiple units working together
- Database interactions
- API endpoints
- External service interactions
- Message queue interactions

### 4.2 Test Fixtures

```python
@pytest.fixture
def db_session():
    # Setup
    session = create_session()
    yield session
    # Teardown
    session.rollback()
    session.close()

def test_create_user_in_database(db_session):
    user = create_user(db_session, "john", "john@example.com")
    assert user.id is not None
    assert db_session.query(User).count() == 1
```

### 4.3 Mocking External Services

```python
@patch('requests.get')
def test_get_user_from_external_api(mock_get):
    mock_get.return_value.json.return_value = {"id": 1, "name": "John"}
    
    user = fetch_user_from_api(1)
    
    assert user.name == "John"
    mock_get.assert_called_once()
```

## 5. End-to-End Testing

### 5.1 Test Cases

- Critical user workflows
- Primary features
- Cross-browser compatibility
- Mobile responsiveness
- Performance requirements

### 5.2 Tools

- Selenium for web apps
- Cypress for modern web apps
- Appium for mobile
- Postman for APIs

## 6. Test Data Management

### 6.1 Test Data

- Use factories for consistent data
- Isolate test data
- Clean up after tests
- Avoid interdependencies
- Use realistic data

### 6.2 Database Seeding

```python
class UserFactory:
    @staticmethod
    def create(name="John", email="john@example.com", **kwargs):
        user = User(name=name, email=email)
        user.save()
        return user

def test_user_creation():
    user = UserFactory.create(name="Jane")
    assert user.name == "Jane"
```

## 7. Performance Testing

### 7.1 Load Testing

- Simulate user load
- Identify bottlenecks
- Capacity planning
- Tools: JMeter, Locust, Apache Bench

### 7.2 Benchmarking

```python
import timeit

def benchmark_function():
    result = timeit.timeit(
        'calculate_total(items)',
        globals=globals(),
        number=10000
    )
    assert result < 5  # Should complete in < 5 seconds
```

## 8. Security Testing

### 8.1 Static Analysis

- SAST tools (SonarQube, Bandit)
- Dependency scanning
- Code review
- Regular updates

### 8.2 Dynamic Analysis

- Penetration testing
- Vulnerability scanning
- OWASP testing
- Security headers check

### 8.3 Common Vulnerabilities

Test for:
- SQL injection
- Cross-site scripting (XSS)
- Cross-site request forgery (CSRF)
- Authentication bypasses
- Authorization flaws
- Input validation

## 9. Continuous Integration Testing

### 9.1 Pre-Commit

```bash
#!/bin/bash
# Run linting
flake8 .
black --check .

# Run tests
pytest -v

# Check coverage
pytest --cov=src --cov-fail-under=80
```

### 9.2 CI Pipeline

1. Code commit
2. Lint and format check
3. Unit tests
4. Coverage report
5. Integration tests
6. Security scanning
7. Build artifact
8. Deploy to staging
9. E2E tests

## 10. Test Documentation

### 10.1 Test Case Format

```python
def test_create_user_with_valid_email_succeeds():
    """
    Test creating a user with valid email.
    
    Given: Valid user data
    When: Creating user
    Then: User is created with correct attributes
    """
    # Setup
    user_data = {"name": "John", "email": "john@example.com"}
    
    # Execute
    user = create_user(user_data)
    
    # Assert
    assert user.email == "john@example.com"
```

## 11. Regression Testing

- Automated test suite
- All previous bugs have tests
- CI/CD integration
- Regular test reviews
- Update when fixing bugs

## 12. Test Maintenance

### 12.1 Flaky Tests

- Identify flaky tests
- Fix root causes
- Use proper waits
- Isolate dependencies
- Mark known issues

### 12.2 Test Obsolescence

- Review tests regularly
- Remove obsolete tests
- Update outdated tests
- Refactor repetitive tests

## 13. Coverage Reports

- Generate coverage reports
- Track trends
- Identify gaps
- Set improvement goals
- Celebrate milestones

## 14. Testing Checklist

- [ ] Unit tests written
- [ ] Integration tests included
- [ ] Coverage >80%
- [ ] Edge cases tested
- [ ] Error cases tested
- [ ] Security tests run
- [ ] Performance acceptable
- [ ] All tests passing
- [ ] Documentation complete
- [ ] CI/CD passed

## 15. Tools and Frameworks

**Python:**
- pytest
- unittest
- Coverage.py
- Mock

**JavaScript:**
- Jest
- Mocha
- Chai
- Sinon

**Java:**
- JUnit
- Mockito
- TestNG
- Selenium

## 16. Resources

- Test documentation: https://lutervyn.pages.dev/engineering/testing
- Example test suite: https://github.com/Lutervyn/test-examples
- Testing guide: https://lutervyn.pages.dev/engineering/testing-guide
