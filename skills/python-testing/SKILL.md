---
name: python-testing
description: "Use when writing Python tests or setting up test infrastructure. Examples: pytest, unittest, Python test, fixtures, mocking, parametrize, test coverage, conftest."
---

# Python Testing

## When to Use

- Writing new Python code (follow TDD: red, green, refactor)
- Designing test suites for Python projects
- Reviewing Python test coverage
- Setting up testing infrastructure
- Writing tests with mocks, fixtures, or parametrization

## Core TDD Workflow

Test-Driven Development prevents bugs by forcing you to define expected behavior before implementation. This ensures your code is testable by design and that every feature has verifiable requirements.

Follow the TDD cycle:

1. **RED**: Write a failing test for the desired behavior
2. **GREEN**: Write minimal code to make the test pass
3. **REFACTOR**: Improve code while keeping tests green

```python
# Step 1: RED — Write the failing test first
def test_calculate_discount():
    assert calculate_discount(100, 0.2) == 80

# Step 2: GREEN — Minimal implementation
def calculate_discount(price, discount):
    return price * (1 - discount)

# Step 3: REFACTOR — Improve if needed
```

## Quick Reference

| Pattern | Usage |
|---------|-------|
| `pytest.raises()` | Test expected exceptions |
| `@pytest.fixture()` | Create reusable test fixtures |
| `@pytest.mark.parametrize()` | Run tests with multiple inputs |
| `@pytest.mark.slow` | Mark slow tests |
| `pytest -m "not slow"` | Skip slow tests |
| `@patch()` | Mock functions and classes |
| `tmp_path` fixture | Automatic temp directory |
| `pytest --cov` | Generate coverage report |

## Success Criteria

- Every new feature starts with a failing test
- Tests are independent and do not share state
- External dependencies are mocked
- Critical paths have 100% coverage; overall target is 80%+

## Detailed Reference

For comprehensive coverage of fixtures, parametrization, markers, mocking, async testing, exceptions, side effects, test organization, configuration, and CLI usage, see [reference.md](reference.md).
