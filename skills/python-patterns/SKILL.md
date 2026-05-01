---
name: python-patterns
description: "Guides the agent when writing, reviewing, or refactoring Python code. Triggers on Python syntax, type hints, error handling, async operations, data structures, package design, and tooling choices."
---

# Python Patterns

Idiomatic Python patterns for robust, readable, and maintainable code.

## When to Use

- Writing new Python code, scripts, or applications
- Reviewing or refactoring existing Python code
- Designing Python packages and modules
- Implementing Python-specific features (type hints, error handling, async, etc.)

## Quick Reference

| Pattern | Details |
|---------|---------|
| Core Principles | [reference.md#core-principles](reference.md#core-principles) |
| Type Hints | [reference.md#type-hints](reference.md#type-hints) |
| Error Handling | [reference.md#error-handling-patterns](reference.md#error-handling-patterns) |
| Context Managers | [reference.md#context-managers](reference.md#context-managers) |
| Comprehensions & Generators | [reference.md#comprehensions-and-generators](reference.md#comprehensions-and-generators) |
| Data Classes | [reference.md#data-classes-and-named-tuples](reference.md#data-classes-and-named-tuples) |
| Decorators | [reference.md#decorators](reference.md#decorators) |
| Concurrency | [reference.md#concurrency-patterns](reference.md#concurrency-patterns) |
| Package Organization | [reference.md#package-organization](reference.md#package-organization) |
| Memory & Performance | [reference.md#memory-and-performance](reference.md#memory-and-performance) |
| Tooling | [reference.md#python-tooling-integration](reference.md#python-tooling-integration) |
| Idioms & Anti-Patterns | [reference.md#quick-reference-python-idioms](reference.md#quick-reference-python-idioms) |

## Key Patterns

### Readability & Explicitness
Python prioritizes clear, obvious code over clever one-liners. Descriptive names and explicit configuration reduce bugs and speed up onboarding.

**Why:** Readable code is reviewed faster and breaks less often because intent is obvious.

```python
# Good: Clear and readable
def get_active_users(users: list[User]) -> list[User]:
    return [user for user in users if user.is_active]
```

### EAFP (Easier to Ask Forgiveness Than Permission)
Prefer `try/except` over pre-checking conditions. This keeps the happy path unindented and avoids race conditions.

**Why:** Exception handling isolates the exceptional case, making normal flow easier to read and often more performant.

```python
# Good: EAFP style
def get_value(dictionary: dict, key: str) -> Any:
    try:
        return dictionary[key]
    except KeyError:
        return default_value
```

## Success Criteria

- Code follows PEP 8 and uses type hints where appropriate.
- Error handling is specific and never silently swallows exceptions.
- Resources are managed with context managers (`with`).
- Dependencies use generic version placeholders in configuration examples.
