# Testing Methodology

## Philosophy

Tests prove correctness. Code without tests is unverified code. For production software, verification is not optional.

**The test pyramid:**

```
           /\
          /  \         E2E / Integration
         /____\        (Full system validation)
        /      \        
       /________\      Integration Tests
      /          \     (Service boundaries, APIs)
     /____________\    
    /              \   Unit Tests
   /________________\  (Functions, algorithms)
```

---

## Unit Tests

### When Required

Unit tests are mandatory for:
- Pure functions (math, algorithms, data transformations)
- State machines
- Protocol parsers and serializers
- Configuration parsing
- Business logic

### Frameworks

<!-- TODO: Add your testing frameworks -->

| Repository | Language | Framework |
|------------|----------|-----------|
| [repo_1] | [Language] | [Framework] |
| [repo_2] | [Language] | [Framework] |
| [repo_docs] | [Language] | [Framework] |

### Coverage Targets

| Code Type | Target |
|-----------|--------|
| Critical paths (safety, control) | 100% |
| General business logic | 80%+ |
| Utility scripts | 70%+ |

### Unit Test Structure

```python
# Good: Descriptive name, Arrange-Act-Assert pattern
def test_steering_bounds_positive():
    """Steering target must be bounded to ±45 degrees."""
    # Arrange
    raw_target = 50.0  # degrees
    max_steering = 45.0
    
    # Act
    bounded = bound_steering(raw_target, max_steering)
    
    # Assert
    assert bounded == 45.0


def test_steering_bounds_negative():
    """Negative steering targets must also be bounded."""
    # Arrange
    raw_target = -60.0
    max_steering = 45.0
    
    # Act
    bounded = bound_steering(raw_target, max_steering)
    
    # Assert
    assert bounded == -45.0


def test_crc8_valid():
    """CRC8 should match reference implementation."""
    # Arrange
    data = bytes([0xAA, 0x05, 0x22, 0x00, 0x00, 0xE8, 0x03])
    
    # Act
    crc = crc8_calc(data[1:])  # Exclude SOF
    
    # Assert
    assert crc == data[-1]  # CRC is last byte
```

### Test File Header

Every test file must include:

```python
"""
Test suite for <component name>

Purpose: <What this test suite verifies>
Prerequisites: <What must be set up before running>
Run: <How to execute these tests>
"""
```

---

## Integration Tests

### API/Service Tests

<!-- TODO: Customize for your API framework -->

#### Endpoint Tests

```python
def test_api_health_check():
    """API health endpoint should return 200."""
    # Call health endpoint
    response = client.get("/health")
    
    # Verify response
    assert response.status_code == 200
    assert response.json()["status"] == "healthy"
```

#### Database Integration Tests

```python
def test_database_connection():
    """Database should be reachable."""
    # Attempt connection
    conn = get_db_connection()
    
    # Verify connection
    assert conn is not None
    assert conn.is_connected()
```

### Message Queue Tests

```python
def test_message_flow():
    """Messages should flow from producer to consumer."""
    # Publish test message
    message = {"id": 1, "data": "test"}
    publish_message("test_queue", message)
    
    # Wait for consumption
    consumed = wait_for_message("test_queue", timeout=5)
    
    # Verify message received
    assert consumed == message
```

### Integration Test File Header

```python
"""
Integration test suite for <system>

Purpose: <End-to-end scenarios this tests>
Prerequisites: <Services, databases, fixtures>
Run: <How to execute>
Environment: <Required test environment>
"""
```

---

## End-to-End Testing

### Setup Requirements

<!-- TODO: Add your E2E test requirements -->

| Component | Requirement |
|-----------|-------------|
| [Component A] | [Requirement] |
| [Component B] | [Requirement] |
| [Test Data] | [Setup instructions] |

### Test Scenarios

#### Full Pipeline Test

```
[Input] → [Processing] → [Output]
```

1. Provide test input
2. Verify processing occurs
3. Verify expected output
4. Verify side effects (database, logs, etc.)

#### Error Handling Tests

```python
def test_invalid_input_handling():
    """Invalid input should return appropriate error."""
    # Send invalid input
    response = api.post("/process", data={"invalid": "data"})
    
    # Verify error response
    assert response.status_code == 400
    assert "error" in response.json()
```

#### Performance Tests

```python
def test_response_time():
    """API should respond within acceptable time."""
    import time
    
    start = time.time()
    response = api.get("/data")
    elapsed = time.time() - start
    
    assert elapsed < 0.5  # 500ms threshold
```

---

## Test Data Management

### Location and Format

<!-- TODO: Add your test data locations -->

| Repository | Location | Format |
|------------|----------|--------|
| [repo_1] | `test_data/` | [Formats] |
| [repo_2] | `test/` | [Formats] |

### Data Sources

- **Synthetic data:** Generated for specific test cases
- **Sample data:** Real anonymized data
- **Fixtures:** Standard test objects

### File Size Limits

| Type | Limit | Action |
|------|-------|--------|
| Individual files | 10 MB | Use git-lfs |
| Total test data | 100 MB | Prune old data |
| Build artifacts | - | Never commit |

### Provenance

Document test data origin:

```markdown
# test_data/README.md

## test_dataset_v1.json

- **Date created:** 2024-03-15
- **Source:** [How generated or obtained]
- **Contents:** [Description]
- **Used for:** [Test purposes]
```

---

## Regression Testing

### Pre-Commit Checklist

Before any commit:

1. [ ] Run unit tests
2. [ ] Run integration tests
3. [ ] Check for new compiler warnings
4. [ ] Verify no performance regression
5. [ ] Manual review of test coverage

### CI/CD Pipeline

<!-- TODO: Add your CI/CD pipeline -->

| Repository | Trigger | Checks |
|------------|---------|--------|
| [repo_1] | [Trigger] | [Checks] |
| [repo_2] | [Trigger] | [Checks] |

### Performance Benchmarks

Track key metrics over time:

<!-- TODO: Add your performance benchmarks -->

| Metric | Baseline | Threshold |
|--------|----------|-----------|
| [Metric 1] | [Value] | < [Threshold] |
| [Metric 2] | [Value] | < [Threshold] |

---

## Test Documentation

### Coverage Reports

Generate and review coverage:

<!-- TODO: Add coverage commands for your stack -->

```bash
# Example: Python
pytest --cov=src --cov-report=html

# Example: JavaScript
npm run test -- --coverage
```

### Test Summaries

Include in commit messages:

```
test: Add unit tests for [module] validation

- Test valid input handling
- Test invalid input rejection
- Test edge cases
- Test timeout handling

Coverage: [module] from 45% to 89%
```

---

## Testing Best Practices

1. **Test behavior, not implementation** - Tests should verify what code does, not how
2. **One concept per test** - Each test should verify one thing
3. **Independent tests** - Tests should not depend on each other
4. **Fast tests** - Unit tests should run in milliseconds
5. **Deterministic tests** - Same input should always produce same output
6. **Readable tests** - Tests document expected behavior

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| <!-- TODO: Date --> | Initial creation | <!-- TODO: Author --> |
