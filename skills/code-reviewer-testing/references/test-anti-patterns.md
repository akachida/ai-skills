# Test Anti-Patterns Reference

**NOTE:** The examples below are for demonstration purposes only. They show what NOT to do and how to fix it in JavaScript/Go. Actual patterns MUST account for the project's programming language, framework, and testing practices.

---

## Anti-Pattern 1: Testing Mock Behavior

```javascript
// ❌ BAD: Test only verifies mock was called, not actual behavior
test("should process order", () => {
  const mockDB = jest.fn();
  processOrder(order, mockDB);
  expect(mockDB).toHaveBeenCalled(); // Only tests mock!
});

// ✅ GOOD: Test verifies actual business outcome
test("should process order", () => {
  const result = processOrder(validOrder);
  expect(result.status).toBe("processed");
  expect(result.total).toBe(100);
});
```

## Anti-Pattern 2: No Assertion / Weak Assertion

```javascript
// ❌ BAD: No meaningful assertion
test("should work", async () => {
  await processData(data); // No assertion!
});

// ❌ BAD: Weak assertion
test("should return result", () => {
  const result = calculate(5);
  expect(result).toBeDefined(); // Doesn't verify correctness!
});

// ✅ GOOD: Specific assertion
test("should calculate discount", () => {
  const result = calculateDiscount(100, 0.1);
  expect(result).toBe(90);
});
```

## Anti-Pattern 3: Test Order Dependency

```javascript
// ❌ BAD: Tests depend on shared state
let sharedUser;
test("should create user", () => {
  sharedUser = createUser();
});
test("should update user", () => {
  updateUser(sharedUser); // Fails if run alone!
});

// ✅ GOOD: Each test is independent
test("should update user", () => {
  const user = createUser(); // Own setup
  const updated = updateUser(user);
  expect(updated.name).toBe("new name");
});
```

## Anti-Pattern 4: Testing Implementation Details

```javascript
// ❌ BAD: Tests internal state/method calls
test("should use cache", () => {
  service.getData();
  expect(service._cache.size).toBe(1); // Implementation detail!
});

// ✅ GOOD: Tests observable behavior
test("should return cached data faster", () => {
  service.getData(); // Prime cache
  const start = Date.now();
  service.getData();
  expect(Date.now() - start).toBeLessThan(10);
});
```

## Anti-Pattern 5: Flaky Tests (Time-Dependent)

```javascript
// ❌ BAD: Depends on timing
test("should expire", async () => {
  await sleep(1000);
  expect(token.isExpired()).toBe(true);
});

// ✅ GOOD: Control time explicitly
test("should expire", () => {
  jest.useFakeTimers();
  jest.advanceTimersByTime(TOKEN_EXPIRY + 1);
  expect(token.isExpired()).toBe(true);
});
```

## Anti-Pattern 6: God Test (Too Much in One Test)

```javascript
// ❌ BAD: Tests too many things
test('should handle everything', () => {
  // 50 lines testing 10 different behaviors
});

// ✅ GOOD: One behavior per test
test('should reject invalid email', () => { ... });
test('should accept valid email', () => { ... });
test('should hash password', () => { ... });
```

## Anti-Pattern 7: Silenced Errors in Test Code

```go
// ❌ BAD: Error silently ignored - test may pass when helper fails
func TestSomething(t *testing.T) {
    data, _ := json.Marshal(input) // Silent failure!
    result := process(string(data))
    assert.NotNil(t, result)
}

// ✅ GOOD: Error propagated
func TestSomething(t *testing.T) {
    data, err := json.Marshal(input)
    require.NoError(t, err)
    result := process(string(data))
    assert.NotNil(t, result)
}
```

```javascript
// ❌ BAD: Empty catch hides failures
test("should process", async () => {
  await setupData().catch(() => {}); // Silent!
  const result = await process();
  expect(result).toBeDefined();
});

// ✅ GOOD: Errors surface
test("should process", async () => {
  await setupData(); // Fails test if setup fails
  const result = await process();
  expect(result).toBeDefined();
});
```

## Anti-Pattern 8: Misleading Test Names

```javascript
// ❌ BAD: "Success" prefix on failure test
test('Success - should return error for invalid input', () => {
  expect(() => process(null)).toThrow();
});

// ✅ GOOD: Name matches behavior
test('should throw error for null input', () => {
  expect(() => process(null)).toThrow();
});

// ❌ BAD: Vague names
test('test1', () => { ... });
test('should work', () => { ... });

// ✅ GOOD: Describes expected behavior
test('should calculate 10% discount on orders over $100', () => { ... });
```

**Test Name Checklist:**

- Does the name describe WHAT is being tested?
- Does the name describe the EXPECTED outcome?
- Would another developer understand the purpose from the name alone?

## Anti-Pattern 9: Testing Language Behavior

```go
// ❌ BAD: Testing Go's nil map behavior, not application logic
func TestNilMapLookup(t *testing.T) {
    var m map[string]int
    _, ok := m["key"]
    assert.False(t, ok)  // This is Go language behavior!
}

// ❌ BAD: Testing Go's append behavior
func TestAppendToNil(t *testing.T) {
    var slice []int
    slice = append(slice, 1)
    assert.Len(t, slice, 1)
}

// ✅ GOOD: Test application behavior
func TestCacheGetMissReturnsDefault(t *testing.T) {
    cache := NewCache()
    val := cache.Get("missing")
    assert.Equal(t, defaultValue, val)
}
```

**Detection Questions:**

- Would this test pass in ANY application using this language?
- Does this test verify YOUR code or the LANGUAGE runtime?
