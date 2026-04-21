# Testing Interview Questions — Top 50

> **Difficulty Split:** Basic (Q1–Q20) · Intermediate (Q21–Q40) · Advanced (Q41–Q50)

---

## Basic (Q1–Q20)

### Q1: What is software testing?

**Answer:** Software testing is the process of evaluating a software application to find defects, verify that it meets requirements, and ensure it works as expected before delivery to users.

**💡 Tip:** Testing = quality gate. The goal isn't to prove software works — it's to find problems before users do.

---

### Q2: What are the main types of testing?

**Answer:**
- **Unit Testing** — tests individual functions/components in isolation
- **Integration Testing** — tests how multiple units work together
- **End-to-End (E2E) Testing** — tests complete user workflows
- **Functional Testing** — tests what the system does (behavior)
- **Non-functional Testing** — tests performance, security, usability

**💡 Tip:** Think of testing as a pyramid: many unit tests (fast, cheap) → some integration tests → few E2E tests (slow, expensive).

---

### Q3: What is unit testing?

**Answer:** Testing individual units (functions, methods, classes) in isolation from their dependencies. Each test focuses on a small, specific piece of functionality and should be fast, deterministic, and independent.

**💡 Tip:** Unit test = testing one brick. Fast, isolated, no database, no network, no filesystem. Mock all external dependencies.

---

### Q4: What is integration testing?

**Answer:** Testing how multiple modules or services work together. It verifies that combined components function correctly, often involving databases, APIs, or external services.

**💡 Tip:** Integration test = testing how bricks form a wall. Multiple units interact. May involve real databases or APIs.

---

### Q5: What is end-to-end (E2E) testing?

**Answer:** Testing complete user workflows from start to finish, simulating real user behavior through the entire application stack (UI → API → Database). Tools: Cypress, Playwright, Selenium.

**💡 Tip:** E2E = testing the whole house. Slowest but most realistic. Automate critical user journeys (login, checkout, signup).

---

### Q6: What is the testing pyramid?

**Answer:** A framework for test distribution:
- **Bottom (70%)** — Unit tests: fast, cheap, many
- **Middle (20%)** — Integration tests: moderate speed, some
- **Top (10%)** — E2E tests: slow, expensive, few

**💡 Tip:** Pyramid shape = lots of cheap/fast unit tests at the bottom, few expensive E2E tests at the top. Inverted pyramid = fragile and slow.

---

### Q7: What is a test fixture?

**Answer:** A fixed state or set of conditions used as a baseline for running tests. It includes setup (creating test data, initializing objects) and teardown (cleaning up) to ensure tests run in a consistent environment.

**💡 Tip:** Fixture = the stage setup before each scene. `beforeEach` / `afterEach` are common fixture hooks in test frameworks.

---

### Q8: What is the difference between manual testing and automated testing?

**Answer:** Manual testing involves a human executing test cases by hand. Automated testing uses scripts/tools to run tests programmatically. Automation is faster for regression but manual testing is better for UX and exploratory testing.

**💡 Tip:** Automate repetitive, regression tests. Keep manual testing for exploratory, usability, and ad-hoc testing.

---

### Q9: What is regression testing?

**Answer:** Testing existing functionality to ensure that new changes (bug fixes, features, refactors) haven't broken anything that was previously working.

**💡 Tip:** Regression = "did I break what was working?" Every new change should trigger the regression suite. Automation is essential here.

---

### Q10: What is a test case?

**Answer:** A set of conditions and steps to verify a specific feature or behavior. It includes: preconditions, input data, execution steps, and expected results.

**💡 Tip:** Test case = "given X, when Y, then Z." The GWT (Given-When-Then) pattern makes tests readable and structured.

---

### Q11: What is a mock in testing?

**Answer:** A mock is a fake object that simulates the behavior of a real dependency (database, API, service) in a controlled way. You define what it should return, so tests are deterministic and isolated.

**💡 Tip:** Mock = a stunt double. It looks and acts like the real thing but follows your script. Use mocks for external dependencies (DB, APIs, email).

---

### Q12: What is a stub?

**Answer:** A stub is a simplified implementation that returns predefined responses. Unlike mocks, stubs don't verify interactions — they just provide canned answers to calls.

**💡 Tip:** Stub = answering machine (always the same response). Mock = recording device (verifies what was called). Stubs respond, mocks verify.

---

### Q13: What is test-driven development (TDD)?

**Answer:** A development approach where you write tests **before** writing the actual code. The cycle: Red (write failing test) → Green (write minimal code to pass) → Refactor (improve code while keeping tests green).

**💡 Tip:** TDD = Red → Green → Refactor. Write the test first, watch it fail, make it pass, then clean up. Tests drive the design.

---

### Q14: What is code coverage?

**Answer:** A metric measuring the percentage of code exercised by tests. Common types: line coverage, branch coverage, function coverage, statement coverage.

**💡 Tip:** 80% coverage is a common target. 100% doesn't mean bug-free. Coverage tells you what's tested, not if tests are good.

---

### Q15: What is the difference between white-box and black-box testing?

**Answer:** White-box testing examines the internal logic and structure of the code (developer perspective). Black-box testing evaluates functionality without knowing internal implementation (user perspective).

**💡 Tip:** White-box = testing with X-ray vision (see code). Black-box = testing blindfolded (only inputs and outputs matter).

---

### Q16: What are smoke tests?

**Answer:** A minimal set of tests that verify the most critical functionality works. They run first to determine if the build is stable enough for more thorough testing. If smoke tests fail, stop and fix immediately.

**💡 Tip:** Smoke test = "is it on fire?" Quick check before deep testing. Name comes from hardware testing — if it smokes, turn it off.

---

### Q17: What is a test suite?

**Answer:** A collection of test cases grouped together to test a specific area of the application. Suites can be organized by feature, module, or test type (unit, integration, E2E).

**💡 Tip:** Suite = a folder of related tests. Running "auth suite" tests all authentication-related functionality together.

---

### Q18: What is the Arrange-Act-Assert (AAA) pattern?

**Answer:** A standard structure for writing tests:
1. **Arrange** — set up test data and conditions
2. **Act** — execute the function/method being tested
3. **Assert** — verify the result matches expectations

**💡 Tip:** AAA = Given-When-Then in code form. Every test should follow this pattern for clarity.

---

### Q19: What is a test runner?

**Answer:** A tool/framework that executes tests and reports results. Common test runners: Jest (JS/TS), pytest (Python), JUnit (Java), Vitest (Vite projects), Mocha (JS).

**💡 Tip:** Test runner = the referee. It runs your tests, collects results, and shows pass/fail/skip counts.

---

### Q20: What is snapshot testing?

**Answer:** A testing technique where you capture the output of a component/function and save it as a "snapshot." Future runs compare output against the saved snapshot — any change is flagged as a test failure.

**💡 Tip:** Snapshot = polaroid of your output. If anything changes, the test alerts you. Useful for UI components and serialized data. Don't overuse — review snapshot changes carefully.

---

## Intermediate (Q21–Q40)

### Q21: What is the difference between Jest and Mocha?

**Answer:**
- **Jest** — all-in-one (test runner, assertions, mocking, coverage). Zero config for most projects. Built-in snapshot testing.
- **Mocha** — flexible test runner only. You choose assertion library (Chai), mocking (Sinon), and coverage tools separately.

**💡 Tip:** Jest = batteries included. Mocha = mix and match. Jest for most projects; Mocha when you need custom tool combinations.

---

### Q22: What is the difference between `describe`, `it`, and `test` in Jest?

**Answer:**
- `describe` — groups related tests into a suite
- `it` / `test` — defines an individual test case (`it` and `test` are aliases)
- Nested `describe` blocks create test hierarchies

**💡 Tip:** `describe` = folder, `it/test` = file. Use `describe` to group by feature, `it` for individual behaviors.

---

### Q23: What is the difference between `beforeEach`, `afterEach`, `beforeAll`, and `afterAll`?

**Answer:**
- `beforeEach` — runs before **each** test (reset state)
- `afterEach` — runs after **each** test (cleanup)
- `beforeAll` — runs **once** before all tests (expensive setup like DB connection)
- `afterAll` — runs **once** after all tests (close connections)

**💡 Tip:** "Each" = per test. "All" = per suite. Use "Each" for test isolation, "All" for expensive one-time setup.

---

### Q24: What is a spy in testing?

**Answer:** A spy wraps an existing function and records information about its calls (arguments, return values, call count). Unlike mocks, spies don't replace the original — they observe it.

**💡 Tip:** Spy = a detective watching. It records calls but lets the real function run. Mock = replaces the function entirely.

---

### Q25: What is mutation testing?

**Answer:** A testing technique where small changes (mutations) are made to the code (e.g., changing `>` to `<`, `+` to `-`). If tests catch the mutation, the test is good. If not, the test coverage is weak despite high line coverage.

**💡 Tip:** Mutation testing = stress-testing your tests. It proves tests actually check logic, not just execute lines. Tools: Stryker, PITest.

---

### Q26: What is property-based testing?

**Answer:** Instead of testing specific examples, you define **properties** (invariants) that should always hold true, and the framework generates hundreds of random inputs to try to falsify them. Tools: Fast-check (JS), Hypothesis (Python).

**💡 Tip:** Example-based = "test with 5." Property-based = "test with ANY number, and it should always satisfy X." Catches edge cases you'd never think of.

---

### Q27: What is the difference between functional and non-functional testing?

**Answer:**
- **Functional** — tests WHAT the system does (features, behavior, outputs)
- **Non-functional** — tests HOW the system performs (speed, security, usability, scalability, reliability)

**💡 Tip:** Functional = "does it work?" Non-functional = "how well does it work?" Performance, load, stress, security = non-functional.

---

### Q28: What is load testing?

**Answer:** Testing how the system performs under expected load (normal and peak conditions). It measures response times, throughput, and resource utilization to ensure the system handles the expected number of users.

**💡 Tip:** Load testing = rush hour simulation. Tools: k6, JMeter, Artillery, Locust. Know your expected user count and test against it.

---

### Q29: What is stress testing?

**Answer:** Testing the system beyond normal load to find its breaking point. The goal is to identify the maximum capacity and observe how the system fails and recovers.

**💡 Tip:** Stress = push until it breaks, then see how it recovers. Load = test normal capacity. Stress = test extreme capacity.

---

### Q30: What is a test double?

**Answer:** A generic term for any object that stands in for a real object during testing. Types: mock, stub, spy, fake, dummy.

- **Dummy** — passed but never used (filler)
- **Stub** — returns canned responses
- **Spy** — records calls, delegates to real
- **Mock** — verifies interactions
- **Fake** — working implementation (e.g., in-memory database)

**💡 Tip:** Test double = stunt double for code. Each type serves a different purpose in isolation.

---

### Q31: What is contract testing?

**Answer:** Testing the contract (interface/API agreement) between services. Each side verifies it adheres to the agreed format/schema. Tools: Pact. Essential in microservices to catch breaking changes.

**💡 Tip:** Contract test = "does your API still speak the same language?" Verifies request/response shape without running both services together.

---

### Q32: What is the difference between code coverage and branch coverage?

**Answer:**
- **Code (line) coverage** — percentage of code lines executed by tests
- **Branch coverage** — percentage of decision branches (if/else, switch) taken by tests

**💡 Tip:** 100% line coverage ≠ 100% branch coverage. An if-else where only `if` is tested = 100% line coverage but 50% branch coverage.

---

### Q33: What is flaky testing?

**Answer:** A test that sometimes passes and sometimes fails without any code changes. Causes: timing issues, shared state, external dependencies, test order dependencies, race conditions.

**💡 Tip:** Flaky = unreliable. The enemy of CI/CD. Fix immediately or quarantine. Common causes: `setTimeout`, shared database, random data.

---

### Q34: How do you test async code?

**Answer:**
- **Callbacks** — use `done` parameter in Jest
- **Promises** — return the promise from the test
- **Async/await** — use `async` test function with `await`
- Always test both success and error paths

**💡 Tip:** Always `return` or `await` promises in tests. Forgetting this = test passes immediately without waiting for the async result.

---

### Q35: What is the difference between `toEqual` and `toBe` in Jest?

**Answer:**
- `toBe` — strict equality (`===`), checks reference identity
- `toEqual` — deep equality, recursively checks all properties of objects/arrays

**💡 Tip:** `toBe` = same object in memory. `toEqual` = same content. Use `toBe` for primitives, `toEqual` for objects/arrays.

---

### Q36: What is behavior-driven development (BDD)?

**Answer:** A development approach that extends TDD by writing tests in natural language describing expected behavior. Uses Given-When-Then format. Frameworks: Cucumber, Jest with `describe`/`it`.

**💡 Tip:** BDD = TDD in plain English. "Given a user is logged in, When they click logout, Then they see the login page." Tests become documentation.

---

### Q37: How do you test database interactions?

**Answer:**
1. Use an in-memory database (SQLite for testing)
2. Use a test database that's reset between tests
3. Use transactions that roll back after each test
4. Mock the database layer for unit tests
5. Use real DB for integration tests

**💡 Tip:** Unit tests = mock the DB. Integration tests = use real/test DB with cleanup. Never unit test with a real database.

---

### Q38: What is test isolation?

**Answer:** Each test should run independently — no dependencies on other tests, no shared mutable state. Tests should produce the same result regardless of execution order.

**💡 Tip:** Isolation = each test is a fresh start. `beforeEach` resets state. No test should depend on another test running first.

---

### Q39: What is a fake in testing?

**Answer:** A working implementation that takes shortcuts, making it suitable for testing but not production. Example: an in-memory repository that stores data in a Map instead of a real database.

**💡 Tip:** Fake = real behavior, shortcut implementation. In-memory DB = most common fake. Easier to maintain than mocks for complex dependencies.

---

### Q40: How do you handle testing of external API calls?

**Answer:**
1. **Mock** — replace the API call with a mock that returns predefined responses
2. **Stub** — use a stub server (Nock, MSW) to intercept HTTP requests
3. **Contract test** — verify the API contract without calling the real API
4. **Integration test** — call the real API in a dedicated test environment

**💡 Tip:** MSW (Mock Service Worker) = intercepts network requests in both browser and Node. Best tool for mocking APIs in tests.

---

## Advanced (Q41–Q50)

### Q41: What is shift-left testing?

**Answer:** Moving testing activities earlier in the development lifecycle — writing tests during (or before) development rather than after. Includes TDD, static analysis, and code reviews with test requirements.

**💡 Tip:** Shift-left = test early, test often. Catch bugs when they're cheapest to fix (during development, not production).

---

### Q42: What is chaos testing (chaos engineering)?

**Answer:** Deliberately introducing failures into a system (killing containers, adding latency, dropping network packets) to verify resilience and discover weaknesses before real failures occur. Tool: Chaos Monkey, Litmus.

**💡 Tip:** Chaos testing = "what happens if this breaks?" Break things on purpose in staging to build confidence in production resilience.

---

### Q43: What is the test impact analysis (TIA)?

**Answer:** A technique that analyzes code changes to determine which tests are relevant to run. Instead of running the entire test suite, TIA runs only tests affected by the change — saving time in large projects.

**💡 Tip:** TIA = smart test selection. "You changed auth.js → only run auth tests." Speeds up CI significantly for large codebases.

---

### Q44: What are acceptance tests?

**Answer:** Tests that verify the system meets business requirements and user expectations. They're typically defined by product owners or QA and represent real user scenarios. Often the final check before release.

**💡 Tip:** Acceptance tests = "does the business accept this?" Written from the user's perspective, not the developer's.

---

### Q45: How do you test event-driven or message-queue-based systems?

**Answer:**
1. Use embedded message brokers for integration tests
2. Mock the message producer/consumer for unit tests
3. Test message schema compatibility
4. Test idempotency (processing same message twice)
5. Test dead letter queue behavior
6. Test message ordering guarantees

**💡 Tip:** Focus on: message format, delivery guarantees (at-least-once vs exactly-once), idempotency, and failure handling (DLQ).

---

### Q46: What is visual regression testing?

**Answer:** Comparing screenshots of UI components/pages against baseline images to detect unintended visual changes. Tools: Percy, Chromatic, Playwright visual comparisons.

**💡 Tip:** Visual regression = "did the UI change?" Automated pixel comparison catches CSS changes, layout shifts, and responsive breakpoints that functional tests miss.

---

### Q47: What is the difference between CI testing and CD testing?

**Answer:**
- **CI testing** — runs on every commit/PR: unit tests, linting, type checks, fast integration tests. Goal: fast feedback (<10 min).
- **CD testing** — runs during deployment: E2E tests, smoke tests, performance tests, canary tests. Goal: validate the build in production-like conditions.

**💡 Tip:** CI tests = fast, frequent, catch regressions. CD tests = thorough, slower, catch deployment issues.

---

### Q48: How do you implement testing in a microservices architecture?

**Answer:**
1. **Unit tests** per service (mock external deps)
2. **Contract tests** between services (Pact)
3. **Integration tests** per service (with real DB)
4. **E2E tests** across services (few, critical paths)
5. **Chaos tests** for resilience
6. **API tests** for each service's public interface

**💡 Tip:** Test pyramid per service + contract tests between services. Don't E2E test everything — it's too slow and brittle in microservices.

---

### Q49: What is test observability?

**Answer:** Applying observability practices (metrics, traces, logs) to testing. Understanding why tests fail by capturing detailed context: stack traces, test metadata, flakiness rates, failure patterns, and historical trends.

**💡 Tip:** Test observability = debugging test failures at scale. Track flakiness rates, failure patterns, and test execution times across CI runs.

---

### Q50: How do you balance testing speed and thoroughness?

**Answer:**
1. Follow the test pyramid (many fast unit, few slow E2E)
2. Use parallel test execution
3. Implement test impact analysis for targeted runs
4. Separate fast CI suite from thorough nightly suite
5. Use mocking strategically (mock external, test internal)
6. Monitor and fix flaky tests immediately
7. Set coverage thresholds (e.g., 80%) — don't chase 100%

**💡 Tip:** Balance = fast feedback loop + confidence in quality. Optimize for developer experience: tests should be fast enough that developers actually run them.
