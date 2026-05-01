# Test and Behavior Driven Development (TDD & BDD)

This repository contains the projects, laboratory exercises, and strategic implementation notes from the **"Test and Behavior Driven Development"** course by IBM, part of the **DevOps and Software Engineering Professional Certificate**.

---

## 🎯 Course Overview
In modern DevOps practices, testing is not a phase that happens after development—it is the engine that drives it. This course focuses on shifting quality to the left by utilizing **Test-Driven Development (TDD)** to ensure code correctness and **Behavior-Driven Development (BDD)** to align technical implementation with business requirements.



## 🛠 Engineering Core Competencies

### 1. Test-Driven Development (TDD)
- **Red-Green-Refactor Cycle:** Writing failing tests before code to ensure 100% test coverage and intent-based programming.
- **Unit Testing with PyUnit (unittest):** Mastering the standard Python testing framework to isolate and validate small units of code.
- **Mocking & Patching:** Using `unittest.mock` to simulate external dependencies (APIs, Databases), ensuring fast and isolated test execution.
- **Advanced Assertions:** Implementing robust validation logic to catch edge cases early in the SDLC.

### 2. Behavior-Driven Development (BDD)
- **Gherkin Syntax:** Writing executable specifications using `Given`, `When`, and `Then` steps.
- **Behave Framework:** Bridging the gap between stakeholders and engineers by automating human-readable requirements.
- **Integration Testing:** Validating that multiple microservices work together as expected according to business user stories.

### 3. Agile & DevOps Integration
- **Code Coverage:** Utilizing tools to measure the effectiveness of the test suite.
- **CI/CD Alignment:** Integrating automated test suites into deployment pipelines to prevent regressions in production.


---



# Advanced Test Automation: Python Test Runners and Code Coverage

This documentation outlines the transition from standard Python testing to a professional-grade automation suite using **Nose**, **Pinocchio**, and **Coverage**. In a DevOps-centric environment, maintaining high code quality requires expressive test reports and rigorous metric tracking.



## 🎯 Implementation Objectives
*   Replace standard `unittest` output with human-readable specifications.
*   Implement a **Red-Green-Refactor** workflow using color-coded feedback.
*   Integrate **Code Coverage** analysis to identify untested execution paths.
*   Standardize testing parameters via **Configuration as Code** (`setup.cfg`).

---

## 🛠️ Toolchain Overview

| Tool | Role | Industry Purpose |
| :--- | :--- | :--- |
| **Nose** | Advanced Test Runner | Extensible framework for automated test discovery and execution. |
| **Pinocchio** | Output Formatter | Transforms cryptic function names into descriptive behavioral specs. |
| **Coverage** | Static/Dynamic Analysis | Measures the effectiveness of the test suite against the codebase. |

---

## 🚀 Environment Setup

Initialize your virtual environment and install the professional testing stack:
```bash
# Standard installation for Python 3.x environments
pip install nose pinocchio coverage

```

----------

## 🧪 Evolution of Test Execution

### 1. Standard Unittest (The Baseline)

The default Python test runner is functional but lacks verbosity for complex CI/CD logs.

Bash

```
python3 -m unittest -v

```

> **Issue:** Verbose output often duplicates docstrings and function names, cluttering the terminal during large-scale deployments.

### 2. Behavioral Specifications with Nose

By using **Pinocchio**, we pivot to a "Specification" style. This is critical for aligning developer implementation with business logic requirements.

Bash

```
# Execute tests with formatted specifications and color coding
nosetests --with-spec --spec-color

```

### 3. Integrated Code Coverage Analysis

A "Passed" test is only meaningful if it covers the critical logic. We utilize the `--with-coverage` flag to ensure 100% path coverage.

Bash

```
# Analyze code coverage and identify missing lines (-m)
nosetests --with-coverage
coverage report -m

```

----------

## ⚙️ Configuration as Code: `setup.cfg`

To ensure consistency across the engineering team and the CI/CD pipeline (Jenkins, GitHub Actions, etc.), we move all CLI flags into a persistent configuration file. This prevents "Human Error" during manual test execution.

Create a `setup.cfg` file in the root of your repository:

Ini, TOML

```
[nosetests]
# Increase verbosity for detailed logs
verbosity=2
# Enable Pinocchio specifications
with-spec=1
# Enable terminal color coding (Red/Green)
spec-color=1
# Enable Coverage analysis
with-coverage=1
# Reset coverage data before each run to avoid cumulative errors
cover-erase=1
# Define the target package for coverage analysis
cover-package=core_logic

[coverage:report]
# Automatically show which line numbers are missing test cases
show_missing = True

```

**Execution becomes a single, standardized command:**

Bash

```
nosetests

```

----------

## 📊 Analyzing Coverage Reports

In a production-ready microservice, we aim for **100% Coverage**. If the report shows missing lines, it indicates a logic branch (e.g., an `if/else` for error handling) that has never been executed.

**Example Report Analysis:**

-   **60% Coverage:** High Risk. Critical paths (like input validation or exception handling) are ignored.
    
-   **80% Coverage:** Moderate. Usually missing edge cases (e.g., passing non-integer values to a math utility).
    
-   **100% Coverage:** Professional Standard. Every line of the `core_logic.py` is validated.


---


# Software Testing Mastery: Test Fixtures, Assertions & Design Patterns

[![Testing Framework: PyUnit](https://img.shields.io/badge/Framework-PyUnit%20%2F%20Unittest-blue?style=for-the-badge&logo=python)](https://docs.python.org/3/library/unittest.html)
[![DevOps: CI/CD Ready](https://img.shields.io/badge/DevOps-CI%2FCD%20Ready-orange?style=for-the-badge&logo=github-actions)](https://github.com/features/actions)

## 📌 Executive Summary
This document serves as a professional technical guide for implementing robust unit tests in Python. It covers the transition from basic scripting to **Production-Grade Quality Assurance**, focusing on data structures (Stacks), mathematical stability, and the "Happy vs. Sad Path" testing paradigm.

---

## 🛠️ Part 1: Testing Data Structures (LIFO Stack)
A **Stack** is a fundamental data structure following the **LIFO (Last-In, First-Out)** principle. To ensure a reliable implementation in a microservices or backend environment, we must validate its core state transitions.

### Core Stack Operations
| Operation | Description | Logic |
| :--- | :--- | :--- |
| `push(item)` | Adds an element to the top. | $S_{n+1} = S_n + \{x\}$ |
| `pop()` | Removes and returns the top element. | $x = S_{top}; S_{n-1}$ |
| `peek()` | Returns the top element without removal. | $x = S_{top}$ |

### Implementation & Test Lifecycle
In professional DevOps workflows, we use **Test Fixtures** to maintain an idempotent testing environment.


```python
import unittest
from core.structures import Stack

class TestStackImplementation(unittest.TestCase):
    
    def setUp(self):
        """Fixture: Initialize a fresh stack before every test."""
        self.stack = Stack()

    def tearDown(self):
        """Fixture: Destruct the object to prevent memory leaks/side effects."""
        self.stack = None

    def test_push_integrity(self):
        """Validates that pushed items are correctly placed at the pointer."""
        test_value = 9
        self.stack.push(test_value)
        self.assertEqual(self.stack.peek(), test_value, "Top of stack should match pushed value.")

    def test_pop_functional_flow(self):
        """Validates LIFO removal and empty state transition."""
        self.stack.push(9)
        popped_value = self.stack.pop()
        
        self.assertEqual(popped_value, 9)
        self.assertTrue(self.stack.is_empty(), "Stack must be empty after popping the final element.")

```

----------

## 🧪 Part 2: Advanced Assertions & Error Handling

Assertions are the "Truth Detectors" of your code. In professional engineering, we don't just check if code _works_; we check how it _fails_.

### PyUnit Assertion Toolkit

> **Note:** Prefer specialized assertions over generic `assert()` for better failure reporting in CI/CD logs.

-   `assertEqual(a, b)`: Standard equality check.
    
-   `assertAlmostEqual(a, b)`: **Critical for Floating Point Math.** Checks up to 7 decimal places to handle precision errors.
    
-   `assertRaises(Exception)`: Validates that your error handlers are actually firing.
    
-   `assertIsInstance(obj, type)`: Validates Type Safety.
    

----------

## 🛣️ Part 3: Testing Paradigms (Happy vs. Sad Path)

A senior engineer designs tests for two distinct realities:

### 1. The Happy Path (The "Ideal" User)

Validates that the business logic produces the correct output under valid conditions.

-   **Example:** Calculating the area of a triangle with positive floats.
    
-   **Formula:**
    
    $$Area = \frac{base \times height}{2}$$
    

### 2. The Sad Path (The "Chaos" Factor)

Validates that the system handles invalid input gracefully without crashing (Robustness).

-   **Type Errors:** Passing strings or booleans instead of numbers.
    
-   **Value Errors:** Passing negative dimensions for physical objects.
    

----------

## 💻 Professional Implementation: `TriangleService`

Below is a production-ready example of the `Triangle` area calculation service and its corresponding test suite.

Python

```
# service.py
def calculate_triangle_area(base: float, height: float) -> float:
    if not (isinstance(base, (int, float)) and isinstance(height, (int, float))):
        raise TypeError("Inputs must be numeric types.")
    if base < 0 or height < 0:
        raise ValueError("Dimensions cannot be negative.")
    
    return 0.5 * base * height

# test_service.py
class TestTriangleService(unittest.TestCase):
    
    # --- HAPPY PATH ---
    def test_area_calculation_accuracy(self):
        # Testing floating point precision
        self.assertAlmostEqual(calculate_triangle_area(3.4556, 8.3567), 14.43870626)
        # Testing zero-bound
        self.assertEqual(calculate_triangle_area(0, 5), 0.0)

    # --- SAD PATH ---
    def test_negative_dimensions_error(self):
        """Ensures ValueError is raised for illogical dimensions."""
        with self.assertRaises(ValueError):
            calculate_triangle_area(-2, 5)

    def test_invalid_type_injection(self):
        """Ensures TypeError is raised for non-numeric injections (DevSecOps focus)."""
        with self.assertRaises(TypeError):
            calculate_triangle_area("2", 5)
        with self.assertRaises(TypeError):
            calculate_triangle_area(True, 5)
```

---


# Lab: Engineering Robust Test Assertions in Python

This repository documents the implementation of advanced test assertions for a **Stack Data Structure**. In a DevOps environment, writing assertions is not just about checking for "success"—it is about ensuring state integrity, validating boundary conditions, and preventing regressions in the CI/CD pipeline.

---

## 🎯 Learning Objectives

*   **Logic Validation:** Moving beyond simple "pass/fail" to robust state verification.
*   **Test Atomicity:** Understanding why tests run in random order to ensure zero dependency between suites.
*   **The --stop Workflow:** Utilizing `nosetests --stop` to implement incremental fixes (Red-Green-Refactor).
*   **Fixture Management:** Using `setUp()` and `tearDown()` to ensure a clean state for every atomic test.

---

## 🏗️ The System Under Test (SUT): The Stack

We are validating a **LIFO (Last-In, First-Out)** Stack implementation. To truly test a stack, we must verify its behavior when transitioning between states (Empty ↔ Non-Empty).



### Stack API Specification:
| Method | Description |
| :--- | :--- |
| `push(data)` | Adds an item to the **top**. |
| `pop()` | Removes and returns the **top** item. |
| `peek()` | Returns the **top** item without removing it. |
| `is_empty()` | Returns `True` if the stack contains zero items. |

---

## 🛠️ Implementation Strategy

### 1. Initial State & Transition (`is_empty`)
A professional test doesn't just check if a new stack is empty. It checks if it **becomes** non-empty after an operation.
```python
def test_is_empty(self):
    # Initial state verification
    self.assertTrue(self.stack.is_empty())
    # State transition
    self.stack.push(5)
    self.assertFalse(self.stack.is_empty())

```

### 2. Validating Positional Logic (`peek`)

If you only push **one** item, you cannot distinguish between the "top" and the "bottom" of the stack. We push multiple items to verify the LIFO pointer is correct.

Python

```
def test_peek(self):
    self.stack.push(3) # Bottom
    self.stack.push(5) # Top
    # Assert peek targets the correct index
    self.assertEqual(self.stack.peek(), 5)

```

### 3. Destruction & Integrity (`pop`)

Testing `pop()` requires verifying three things: it returns the correct item, it removes the item, and it updates the new top.

Python

```
def test_pop(self):
    self.stack.push(3)
    self.stack.push(5)
    # 1. Check returned value
    self.assertEqual(self.stack.pop(), 5)
    # 2. Check state mutation (new top is now 3)
    self.assertEqual(self.stack.peek(), 3)
    # 3. Check exhaustion
    self.stack.pop()
    self.assertTrue(self.stack.is_empty())

```

----------

## ⚙️ Engineering Workflow

Follow these steps to execute the test suite:

1.  **Environment Sync:**
    
    Bash
    
    ```
    python3.8 -m pip install -r requirements.txt
    
    ```
    
2.  **Continuous Integration Simulation:** Run the tests to identify unimplemented features (`NotImplementedError`):
    
    Bash
    
    ```
    nosetests --stop
    
    ```
    
3.  **Red-Green-Refactor:**
    
    -   **Red:** Observe the failure.
        
    -   **Green:** Implement the assertion logic in `test_stack.py`.
        
    -   **Refactor:** Ensure the assertions cover corner cases (e.g., pushing multiple items).
  

---

# Lab: State Management with Test Fixtures

In software engineering, testing isolated logic is only half the battle. When dealing with databases or external states, test suites must maintain strict isolation. If Test A leaves residual data in a database, Test B might fail unpredictably. 

This lab demonstrates how to use **Test Fixtures** to orchestrate the environment lifecycle—ensuring a clean, predictable state for every single atomic test.

---

## 🎯 Engineering Objectives

*   **Lifecycle Hooks:** Master `PyUnit` fixtures (`setUpClass`, `setUp`, `tearDown`, `tearDownClass`).
*   **Database Orchestration:** Initialize and destroy SQLAlchemy database connections programmatically.
*   **Data Ingestion:** Load static JSON fixtures into memory to provide realistic test data.
*   **State Isolation:** Prevent test-pollution by truncating tables before every execution.

---

## 🧠 The Anatomy of PyUnit Fixtures

Understanding *when* a fixture runs is critical to pipeline performance. You don't want to rebuild a database 1,000 times if it only needs to be built once per module.

| Fixture | Scope | DevOps Use Case |
| :--- | :--- | :--- |
| `setUpModule()` / `tearDownModule()` | **Module** (Once per file) | Starting a Mock Server / Docker Container. |
| `setUpClass()` / `tearDownClass()` | **Class** (Once per test suite) | Creating DB schema, loading large JSON files into memory. |
| `setUp()` / `tearDown()` | **Instance** (Before/After *every* test) | Truncating tables, resetting in-memory caches. |

---

## 🛠️ Implementation Steps

### Step 1: Global Setup & Teardown (The Class Level)
We need to construct the SQLAlchemy tables and close the connection when all tests finish. Since this is an expensive operation, we do it *once* per class.

We also load our mock data (`account_data.json`) into memory here.
```python
import json
from models import db, Account
from unittest import TestCase

# Global variable to hold our mock data
ACCOUNT_DATA = []

class TestAccountModel(TestCase):

    @classmethod
    def setUpClass(cls):
        """ Runs ONCE before the test suite starts. """
        # 1. Initialize the database schema
        db.create_all()  
        
        # 2. Load JSON fixture data into memory
        global ACCOUNT_DATA
        with open('tests/fixtures/account_data.json') as json_data:
            ACCOUNT_DATA = json.load(json_data)

    @classmethod
    def tearDownClass(cls):
        """ Runs ONCE after the test suite finishes. """
        # 3. Gracefully close the DB connection
        db.session.close()

```

### Step 2: Test Pollution & The Instance Level (`setUp`)

If we create an account in `test_A`, and then count accounts in `test_B`, `test_B` will fail because `test_A` polluted the database.

To achieve **Test Isolation**, we must clear the database _before every single test_.

Python

```
    def setUp(self):
        """ Runs BEFORE every individual test method. """
        # Truncate the table to ensure a clean slate
        db.session.query(Account).delete()
        db.session.commit()

    def tearDown(self):
        """ Runs AFTER every individual test method. """
        # Clear the SQLAlchemy session cache
        db.session.remove()

```

### Step 3: Writing the Test Cases

Now that our environment is strictly controlled, we can write our tests knowing the database is empty when the test starts.

**Test 1: Single Record Creation** Notice how we use `data` to unpack the dictionary into keyword arguments.

Python

```
    def test_create_an_account(self):
        """ Test creating a single Account """
        # Arrange: Grab the first mock account
        data = ACCOUNT_DATA[0] 
        account = Account(**data)
        
        # Act: Save to DB
        account.create()
        
        # Assert: Verify exactly 1 account exists
        self.assertEqual(len(Account.all()), 1)

```

**Test 2: Bulk Record Creation** Because of our `setUp` method, we know the database is empty again when this test starts.

Python

```
    def test_create_all_accounts(self):
        """ Test creating multiple Accounts sequentially """
        # Arrange & Act
        for data in ACCOUNT_DATA:
            account = Account(**data)
            account.create()
            
        # Assert: Verify the DB count matches our JSON array length
        self.assertEqual(len(Account.all()), len(ACCOUNT_DATA))

```

----------

## ⚙️ Execution

Run the suite using Nose. If the setup is correct, the tests should run in isolation and pass cleanly.

Bash

```
nosetests --with-spec --spec-color
```


---


# 🛡️ Infrastructure Testing: Code Coverage Lab

> **System Architecture Context:** High test coverage is critical for Infrastructure as Code (IaC) and cloud-native microservices. In this lab, we use `nosetests` and `coverage` to analyze our `Account` model (representing cloud infrastructure accounts/tenants), identify untested execution paths, and implement test cases to reach a 100% coverage threshold.

---

## 📋 Lab Objectives Checklist
- [x] Run baseline test suite and generate a coverage report.
- [ ] Implement coverage for string representation (`__repr__`).
- [ ] Implement coverage for state serialization (`to_dict` & `from_dict`).
- [ ] Implement coverage for database state transitions (`update`).
- [ ] Handle sad paths and exception handling (Missing ID validations).
- [ ] Implement coverage for infrastructure teardown (`delete`).
- [ ] Achieve **100% Code Coverage**.

---

## 🏗️ 0. Open the Code Editor & Baseline Diagnostics

In your Cloud IDE or local terminal, navigate to the testing directory containing the source code for this environment:

```bash
cd labs/04_test_coverage

```

Before committing any new test cases, establish a baseline. Run the `nosetests` suite to see the current pipeline state:

Bash

```
nosetests --with-coverage --cover-package=models

```


_Current coverage sits at 72%. Let's secure our pipeline by addressing the missing lines in `models/account.py`._

----------

## 🛠️ Step 1: Missing Line 26 (Representation)

Our CI/CD logs heavily rely on the `__repr__` method to print out the active cloud account during debugging. Line 26 is currently untested.

**Target Code (`models/account.py`):**

Python

```
def __repr__(self):
    return '<Account %r>' % self.name

```

**Implementation:**

Add a test case to validate the string formatting. We will simulate a production environment account.

Python

```
    def test_repr(self):
        """Test the string representation of a cloud account for logging"""
        account = Account()
        account.name = "aws-prod-cluster-01"
        self.assertEqual(str(account), "<Account 'aws-prod-cluster-01'>")

```

----------

## 🛠️ Step 2: Missing Line 30 (Serialization)

When pushing state to our REST APIs or generating Terraform variables, we serialize the Python objects into dictionaries via `to_dict()`.

**Target Code (`models/account.py`):**

Python

```
    def to_dict(self) -> dict:
        """Serializes the class as a dictionary for API payloads"""
        return {c.name: getattr(self, c.name) for c in self.__table__.columns}

```

**Implementation:**

Write a test to assert that all attributes serialize correctly.

Python

```
    def test_to_dict(self):
        """ Test account serialization to dictionary """
        data = ACCOUNT_DATA[self.rand] # Retrieve randomized test fixture
        account = Account(**data)
        result = account.to_dict()
        
        self.assertEqual(account.name, result["name"])
        self.assertEqual(account.email, result["email"])
        self.assertEqual(account.phone_number, result["phone_number"])
        self.assertEqual(account.disabled, result["disabled"])
        self.assertEqual(account.date_joined, result["date_joined"])

```

----------

## 🛠️ Step 3: Missing Lines 34-35 (Deserialization)

We frequently receive JSON payloads via webhooks. The `from_dict()` method binds this incoming JSON back into our `Account` object.

**Target Code (`models/account.py`):**

Python

```
    def from_dict(self, data: dict) -> None:
        """Sets attributes from a webhook dictionary payload"""
        for key, value in data.items():
            setattr(self, key, value)

```

**Implementation:**

Simulate an incoming dictionary and assert the object adopts the properties.

Python

```
    def test_from_dict(self):
        """ Test account deserialization from webhook payload """
        data = ACCOUNT_DATA[self.rand] 
        account = Account()
        account.from_dict(data)
        
        self.assertEqual(account.name, data["name"])
        self.assertEqual(account.email, data["email"])
        self.assertEqual(account.phone_number, data["phone_number"])
        self.assertEqual(account.disabled, data["disabled"])

```

----------

## 🛠️ Step 4: Missing Lines 45-48 (Happy Path: Update)

Lines 45-48 govern state updates in the database layer. This simulates modifying a resource, much like applying a new configuration tag.

**Target Code (`models/account.py`):**

Python

```
    def update(self):
        """Updates an account state in the persistent datastore"""
        logger.info("Saving state for %s", self.name)
        if not self.id:
            raise DataValidationError("Update called with empty ID field")
        db.session.commit()

```

**Implementation:**

Create an account, modify its name, trigger an update, and explicitly verify the new state from the database.

Python

```
    def test_update_an_account(self):
        """ Test Account state update using validated data """
        data = ACCOUNT_DATA[self.rand] 
        account = Account(**data)
        account.create()
        self.assertIsNotNone(account.id)
        
        # Simulate an infrastructure rename
        account.name = "k8s-failover-node"
        account.update()
        
        # Fetch directly from DB to confirm transaction
        found = Account.find(account.id)
        self.assertEqual(found.name, "k8s-failover-node")

```

----------

## 🛠️ Step 5: Missing Line 47 (Sad Path: Validation Error)

> **⚠️ DevOps Pro-Tip:** Testing "Sad Paths" is arguably more important than testing happy paths. Unhandled exceptions during a pipeline run can lead to blocked deployments or corrupted state files.

If we attempt to run an `update()` on an unprovisioned account (ID is `None`), it should raise a `DataValidationError`.

**Implementation:**

Use `assertRaises` to ensure the safeguard works.

Python

```
    def test_invalid_id_on_update(self):
        """ Test system safeguard: Invalid ID update attempt """
        data = ACCOUNT_DATA[self.rand] 
        account = Account(**data)
        account.id = None
        
        self.assertRaises(DataValidationError, account.update)

```

----------

## 🛠️ Step 6: Missing Lines 52-54 (Resource Teardown)

Finally, we must test the destruction phase (`delete`). An unverified delete method can cause zombie resources and ballooning cloud costs.

**Target Code (`models/account.py`):**

Python

```
def delete(self):
    """Removes an account from the data store (Teardown)"""
    logger.info("Deleting resource %s", self.name)
    db.session.delete(self)
    db.session.commit()

```

**Implementation:**

Verify the cluster size decreases after the `.delete()` operation.

Python

```
    def test_delete_an_account(self):
        """ Test Account resource teardown """
        data = ACCOUNT_DATA[self.rand] 
        account = Account(**data)
        account.create()
        
        # Ensure it exists
        self.assertEqual(len(Account.all()), 1)
        
        # Trigger teardown
        account.delete()
        
        # Verify empty state
        self.assertEqual(len(Account.all()), 0)

```

----------

## 🎯 Step 7: Verifying 100% Test Coverage

With all edge cases and execution paths documented and tested, run the final coverage check:

Bash

```
nosetests --with-coverage --cover-package=models

```

---


# Enterprise Account Provisioning: Factory & Fake Data Integration

> **Engineering Directive:** Static JSON test fixtures are inadequate for simulating high-traffic, enterprise-scale environments. This document outlines the engineering procedure for migrating our Cloud IAM test suite to dynamic data generation using `factory_boy` and `faker`. This approach ensures scalable, randomized test coverage required for robust infrastructure testing.

---

## Architecture Overview

In our current CI/CD pipeline, tests rely on hardcoded `account_data.json` fixtures. To support large-scale enterprise load testing, we are deprecating static fixtures in favor of dynamic factories. 

### Core Dependencies
| Dependency | Purpose | Target Version |
| :--- | :--- | :--- |
| `nose` | Test execution and discovery | `>= 1.3.7` |
| `factory_boy` | Object factory for Python | `>= 3.2.1` |
| `Faker` | Generator for pseudo-real test data | `>= 13.0.0` |

---

## Pre-Flight Checks

Before applying structural changes to the test suite, ensure the baseline environment is stable. Execute the test runner to verify 100% pass rate on existing static tests.

```bash
# Execute within the virtual environment
$ nosetests --with-coverage --cover-package=models

```

_Expected Result: All tests pass (green). Any failure here indicates a corrupted environment state._

----------

## Implementation: Account Factory

We require a factory class capable of generating realistic User/IAM profiles on the fly. We utilize `Faker` providers for standard strings and `Fuzzy` attributes for Booleans and dates.

**File:** `tests/factories.py`

Python

```
import factory
from datetime import date
from factory.fuzzy import FuzzyChoice, FuzzyDate
from models.account import Account

class AccountFactory(factory.Factory):
    """ 
    Generates dynamic Account payloads for enterprise environments.
    Simulates high-volume user generation.
    """

    class Meta:
        model = Account

    # Sequential ID generation (e.g., 1, 2, 3...)
    id = factory.Sequence(lambda n: n)
    
    # Faker attributes mapping to realistic data
    name = factory.Faker("name")
    email = factory.Faker("email")
    phone_number = factory.Faker("phone_number")
    
    # Randomized state for feature flags and timestamps
    disabled = FuzzyChoice(choices=[True, False])
    date_joined = FuzzyDate(date(2008, 1, 1))

```

----------

## Test Suite Migration Lifecycle

The following test cases require refactoring. We will inject `AccountFactory` to replace the static `ACCOUNT_DATA` dictionary.

**File:** `tests/test_account.py`

First, import the factory into your test suite:

Python

```
from factories import AccountFactory

```

### Refactoring Checklist

-   [x] **`test_create_all_accounts`**: Simulate bulk provisioning (e.g., 10 accounts).
    
-   [x] **`test_create_an_account`**: Standard instance creation.
    
-   [x] **`test_to_dict`**: Verify serialization format.
    
-   [x] **`test_from_dict`**: Verify deserialization payload mapping.
    
-   [x] **`test_update_an_account`**: Verify mutable state updates (e.g., changing names).
    
-   [x] **`test_invalid_id_on_update`**: Edge-case validation (Null IDs).
    
-   [x] **`test_delete_an_account`**: Verify teardown procedures.
    

### Key Refactoring Examples

**1. Bulk Provisioning Test**

Python

```
    def test_create_all_accounts(self):
        """ Verify system stability during batch account creation """
        for _ in range(10):
            account = AccountFactory()
            account.create()
        
        self.assertEqual(len(Account.all()), 10)

```

**2. Dictionary Serialization Test**

Python

```
    def test_to_dict(self):
        """ Ensure Factory payload correctly maps to system dictionary format """
        account = AccountFactory()
        result = account.to_dict()
        
        self.assertEqual(account.name, result["name"])
        self.assertEqual(account.email, result["email"])
        self.assertEqual(account.phone_number, result["phone_number"])
        self.assertEqual(account.disabled, result["disabled"])
        self.assertEqual(account.date_joined, result["date_joined"])

```

**3. State Mutation Test**

Python

```
    def test_update_an_account(self):
        """ Verify system accepts valid data mutations on provisioned instances """
        account = AccountFactory()
        account.create()
        self.assertIsNotNone(account.id)
        
        account.name = "Enterprise_Admin_01"
        account.update()
        
        found = Account.find(account.id)
        self.assertEqual(found.name, account.name)

```

----------

## Legacy Cleanup

Once all tests dynamically generate data via the factory, the legacy static components must be purged from the codebase to reduce technical debt.

1.  **Purge Fixture Loading:** Remove the global file read in `setUpClass()`:
    
    Python
    
    ```
    # REMOVE THESE LINES:
    # global ACCOUNT_DATA
    # with open('tests/fixtures/account_data.json') as json_data:
    #     ACCOUNT_DATA = json.load(json_data)
    
    ```
    
2.  **Purge Unused Variables:** Remove `self.rand = randrange(0, len(ACCOUNT_DATA))` from `setUp()`.
    
3.  **Purge Unused Imports:** Remove `import json` and `from random import randrange`.
    
4.  **Delete File:** Safely delete `tests/fixtures/account_data.json`.
    

----------

## Coverage Metrics

Run the final pipeline validation to ensure structural integrity post-migration.

Bash

```
$ nosetests --with-coverage
```

---


# External API Integration: Mocking Strategy & SOP

> **Engineering Directive:** In our CI/CD pipelines, unit tests executing against external 3rd-party services (e.g., IMDb APIs) introduce unacceptable latency, test flakiness, and risk exhausting enterprise API rate limits. This document outlines the standard operating procedure (SOP) for isolating external HTTP dependencies using Python's `unittest.mock` library. **Never make real network calls in the unit test suite.**

---

## 🏗️ Architecture & Strategy

Our testing strategy relies on injecting mock objects into the system under test, overriding external dependencies with predictable, controlled fixture data. 

### Core Concepts
*   **Target Namespace:** Always patch the dependency *where it is imported*, not where it is defined. (e.g., patch `models.imdb.requests.get`, not `requests.get`).
*   **Mocking Responses:** When patching `requests.get`, the mock must replicate the behavior of `requests.Response`. This includes mimicking attributes like `status_code` and methods like `json()`.
*   **Test Fixtures:** Static JSON responses recorded from actual API calls are stored in `IMDB_DATA` to simulate authentic data structures.

---

## 🧪 Testing Scenarios Matrix

| Scenario ID | Endpoint / Action | Mock Target | Simulated HTTP Status | Expected Outcome | Status |
| :--- | :--- | :--- | :---: | :--- | :---: |
| `TC-01` | Title Search | `IMDb.search_titles` | N/A (Bypassed) | Returns valid mock dataset. | [x] |
| `TC-02` | Empty Search | `requests.get` | `404` | Returns empty dictionary `{}`. | [x] |
| `TC-03` | Invalid API Key | `requests.get` | `200` | Returns API error message. | [x] |
| `TC-04` | Movie Ratings | `requests.get` | `200` | Extracts rating metrics. | [x] |

---

## 🛠️ Implementation Guidelines

Below are the approved implementation patterns for our IMDb integration module (`tests/test_imdb.py`).

### 1. High-Level Method Patching
When you only need to bypass a class method entirely, patch the method directly.

```python
from unittest.mock import patch

@patch('test_imdb.IMDb.search_titles')
def test_search_by_title(self, imdb_mock):
    """ TC-01: Bypassing the network call to inject fixture data """
    
    # Inject static fixture data
    imdb_mock.return_value = IMDB_DATA["GOOD_SEARCH"]
    
    # Execute Test
    imdb = IMDb("k_12345678")
    results = imdb.search_titles("Bambi")
    
    # Assertions
    self.assertIsNotNone(results)
    self.assertIsNone(results["errorMessage"])
    self.assertIsNotNone(results["results"])
    self.assertEqual(results["results"][0]["id"], "tt1375666")

```

### 2. Simulating HTTP 404 (Not Found)

To test how our models handle HTTP errors, we mock `requests.get` and configure the mock to return a `404` status code.

Python

```
from unittest.mock import patch, Mock

@patch('models.imdb.requests.get')
def test_search_with_no_results(self, imdb_mock):
    """ TC-02: Forcing a 404 response to validate fallback logic """
    
    # Configure mock to act as an HTTP response
    imdb_mock.return_value = Mock(status_code=404)
    
    imdb = IMDb("k_12345678")
    results = imdb.search_titles("Bambi")
    
    self.assertEqual(results, {})

```

### 3. Simulating Authentication Failures

Some APIs return a `200 OK` status but include an error payload (e.g., Invalid API key). We must spec our mock as a `Response` object and mock the `json()` method.

Python

```
from unittest.mock import patch, Mock
from requests import Response

@patch('models.imdb.requests.get')
def test_search_by_title_failed(self, imdb_mock):
    """ TC-03: Simulating a 200 OK with an embedded API validation error """
    
    imdb_mock.return_value = Mock(
        spec=Response,
        status_code=200,
        json=Mock(return_value=IMDB_DATA["INVALID_API"])
    )
    
    imdb = IMDb("bad-key")
    results = imdb.search_titles("Bambi")
    
    self.assertIsNotNone(results)
    self.assertEqual(results["errorMessage"], "Invalid API Key")

```

### 4. Simulating Payload Extraction (Happy Path)

Testing a successful data retrieval pipeline using a deep mock structure.

Python

```
@patch('models.imdb.requests.get')
def test_movie_ratings(self, imdb_mock):
    """ TC-04: Validating extraction of nested JSON nodes on 200 OK """
    
    imdb_mock.return_value = Mock(
        spec=Response,
        status_code=200,
        json=Mock(return_value=IMDB_DATA["GOOD_RATING"])
    )
    
    imdb = IMDb("k_12345678")
    results = imdb.movie_ratings("tt1375666")
    
    self.assertIsNotNone(results)
    self.assertEqual(results["title"], "Bambi")
    self.assertEqual(results["filmAffinity"], 3)
    self.assertEqual(results["rottenTomatoes"], 5)

```

----------

## 🚀 CI/CD Execution & Coverage

Before merging code to the `main` branch, ensure the test runner executes with no network latency and achieves the minimum coverage threshold (75%+).

**Execution Command:**

Bash

```
$ nosetests --with-coverage --cover-package=models

```

**Expected Pipeline Output:**

Plaintext

```
Tests Cases for IMDb Database
- Test movie Ratings
- Test searching by title
- Test searching by title failed
- Test searching with no results

Name                 Stmts   Miss  Cover   Missing
--------------------------------------------------
models/__init__.py       1      0   100%
models/imdb.py          24      6    75%   27-31, 39
--------------------------------------------------
TOTAL                   25      6    76%
----------------------------------------------------------------------
Ran 4 tests in 0.109s

OK

```

> **Notice:** Test execution time is `~0.109s`. By implementing local memory mocks, we save approximately 1.5 seconds per test cycle compared to live network requests, massively accelerating our build pipelines.

---


# High-Availability RESTful Counter Microservice (TDD Implementation)

> **Engineering Brief:** This repository demonstrates the implementation of a distributed counter management service using **Test-Driven Development (TDD)**. By adopting the **Red-Green-Refactor** workflow, we ensure 100% test coverage and alignment with RESTful architectural constraints before a single line of production logic is finalized.

---

## 🏗 System Architecture & Specification

The service provides a stateless interface to manage named counters, simulating a metric collection system often found in cloud-native monitoring agents.

### REST API Design Documentation

| Action | HTTP Method | Endpoint | Success Code | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Create** | `POST` | `/counters/{name}` | `201 Created` | Initializes a new counter at zero. |
| **Read** | `GET` | `/counters/{name}` | `200 OK` | Retrieves current counter state. |
| **Update** | `PUT` | `/counters/{name}` | `200 OK` | Increments the counter value by 1. |
| **Delete** | `DELETE` | `/counters/{name}` | `204 No Content` | Terminates the counter resource. |

### Technical Stack
- **Framework:** Flask (Python)
- **Testing:** `unittest`, `nosetests`
- **Quality Assurance:** `coverage.py`
- **Style:** PEP 8 Compliance

---

## 🧪 TDD Workflow: The Red-Green-Refactor Cycle

We follow the industry-standard TDD cycle to minimize technical debt and prevent regression during microservice evolution.



### Phase 1: Resource Provisioning (POST)
**Goal:** Implement resource creation with collision detection.
- **Red:** Define `test_create_a_counter` and `test_duplicate_persistance`. Expect `404 Not Found` or `500 Error`.
- **Green:** Implement the `/counters/<name>` route with `methods=["POST"]`.
- **Logic:** Validate if the identifier exists in the global `COUNTERS` registry; return `409 Conflict` if applicable.

### Phase 2: State Mutation (PUT)
**Goal:** Atomically increment the counter.
- **Red:** Write `test_update_a_counter`. Assert that after a `PUT` request, the new value is $Baseline + 1$.
- **Green:** Implement `update_counter(name)`.
- **Refactor:** Move `app.test_client()` instantiation into a `setUp()` fixture to maintain **DRY (Don't Repeat Yourself)** principles.

### Phase 3: Resource Retrieval (GET)
**Goal:** Expose current telemetry data.
- **Red:** Assert that a `GET` request to a newly provisioned counter returns `{ "name": 0 }`.
- **Green:** Implement `read_counter(name)` to return the current dictionary value.

### Phase 4: Resource Termination (DELETE)
**Goal:** Cleanup and idempotent deletion.
- **Red:** Assert that a `DELETE` request returns `204 No Content`.
- **Green:** Use the `del` keyword on the internal registry and return an empty string with the correct status code.

---

## 🛠 Project Structure
```text
.
├── counter.py          # Production Logic (Flask Application)
├── status.py           # HTTP Status Code Constants
├── test_counter.py     # TDD Test Suite (unittest.TestCase)
└── setup.cfg           # Coverage and Nose configurations

```

----------

## 🚀 Execution Guide

### 1. Environment Setup

Ensure your virtual environment is active and dependencies are installed:

Bash

```
pip install flask nose coverage

```

### 2. Running the Test Suite

To execute the tests and view the descriptive docstrings in the console:

Bash

```
# Run tests with verbose output
$ nosetests -v

# Run tests with coverage reporting
$ nosetests --with-coverage --cover-package=counter

```

### 3. Production Readiness Check

A successful TDD implementation should yield the following coverage report:

Plaintext

```
Name            Stmts   Miss  Cover   Missing
---------------------------------------------
counter.py         42      0   100%
---------------------------------------------
TOTAL              42      0   100%

```

----------

## 🛡 Security & Boundary Conditions (Next Steps)

While the "Happy Path" is fully tested, robust microservices require handling **Sad Paths**:

-   [ ] **Request Validation:** Return `400 Bad Request` for non-alphanumeric counter names.
    
-   [ ] **Missing Resources:** Ensure `GET`, `PUT`, and `DELETE` return `404 Not Found` if the resource does not exist.
    
-   [ ] **Concurrency:** Implement thread-safe increments for high-concurrency environments.
    

> **Engineering Note:** "The test case defines the behavior of the code. Code is merely the implementation detail of the test's intent."

