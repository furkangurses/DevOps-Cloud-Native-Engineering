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


---


# Behave BDD: Test Environment Lifecycle & Fixture Management

[![Testing Framework](https://img.shields.io/badge/Framework-Behave-blue.svg)](https://behave.readthedocs.io/)
[![Automation](https://img.shields.io/badge/Tool-Selenium-green.svg)](https://www.selenium.dev/)
[![DevOps](https://img.shields.io/badge/Environment-CI%2FCD%20Ready-orange.svg)]()

This repository implements a professional-grade Behavior Driven Development (BDD) environment setup using Python's **Behave** framework. It focuses on lifecycle management via `environment.py`, ensuring a clean, scalable, and configurable infrastructure for automated UI testing.

---

## 🏗️ Architecture Overview

In a production-level BDD suite, managing the state of the system under test (SUT) is critical. This project utilizes **Environment Hooks (Fixtures)** to automate setup and teardown processes at various levels of granularity.

### The Behave Hook Hierarchy

| Hook Level | Function | Production Use Case |
| :--- | :--- | :--- |
| **All** | `before_all` / `after_all` | Initializing WebDriver, DB Connections, Global Configs. |
| **Feature** | `before_feature` / `after_feature` | Preparing feature-specific data or state. |
| **Scenario** | `before_scenario` / `after_scenario` | Resetting application state to ensure test isolation. |
| **Step** | `before_step` / `after_step` | Debugging or real-time logging (rarely used). |
| **Tag** | `before_tag` / `after_tag` | Targeted setup for @slow, @wip, or @smoke tests. |

---

## ⚙️ Configuration & Environment Variables

Following **12-Factor App** principles, our testing environment is driven by environment variables. This allows the same suite to run against `localhost`, `staging`, or `production` without code changes.

| Variable | Default | Description |
| :--- | :--- | :--- |
| `BASE_URL` | `http://localhost:8080` | The target URL for the System Under Test (SUT). |
| `WAIT_SECONDS` | `60` | Implicit wait time for Selenium WebDriver. |

---

## 🚀 Implementation: `environment.py`

The `environment.py` file acts as the controller for the testing lifecycle. Below is the production-ready implementation utilizing **Headless Chrome** for CI/CD compatibility.

```python
import os
from selenium import webdriver

def before_all(context):
    """
    Global setup executed once before any features are run.
    Sets up the WebDriver and global context variables.
    """
    # 1. Load Configuration from Environment
    context.base_url = os.getenv('BASE_URL', 'http://localhost:8080')
    context.wait_seconds = int(os.getenv('WAIT_SECONDS', '60'))

    # 2. WebDriver Orchestration (Headless Chrome for CI/CD)
    options = webdriver.ChromeOptions()
    options.add_argument("--headless")
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")
    
    context.driver = webdriver.Chrome(options=options)
    
    # 3. Configure Driver Performance
    context.driver.implicitly_wait(context.wait_seconds)
    context.driver.maximize_window()

    print(f"[INFO] Environment Initialized. Testing against: {context.base_url}")

def after_all(context):
    """
    Global teardown executed once after all features are processed.
    Ensures no orphaned processes remain in memory.
    """
    if hasattr(context, 'driver'):
        context.driver.quit()
        print("[INFO] WebDriver session closed successfully.")

def before_feature(context, feature):
    """Execution logic before each feature file."""
    pass

def before_scenario(context, scenario):
    """Execution logic before each individual scenario."""
    pass

```

----------

## 🛠️ Usage Instructions

### 1. Environment Setup

Ensure your local environment has the necessary drivers installed (e.g., ChromeDriver).

### 2. Running Tests

You can trigger the tests using the standard Behave CLI. To override default configurations, pass environment variables inline:

Bash

```
# Default execution
behave

# Targeting a specific environment with custom timeout
BASE_URL=[https://staging.myapp.com](https://staging.myapp.com) WAIT_SECONDS=30 behave

```

----------

## 💎 Key Engineering Principles Applied

-   **Isolation:** Each test scenario can have its own environment lifecycle to prevent state leakage.
    
-   **Portability:** Use of `os.getenv` ensures the suite is platform-agnostic and CI/CD ready.
    
-   **Resource Management:** The `after_all` hook guarantees that WebDriver instances are properly disposed of, preventing memory leaks in automated runners (Jenkins/GitHub Actions).
    
-   **Context Sharing:** The `context` object is leveraged as a thread-safe container to share the `driver` and `config` across all step definitions.
    

> **Note:** The `context` object is the backbone of Behave. Any attribute attached to it (e.g., `context.driver`) becomes globally accessible to your step implementation files.


---



# 🚀 BDD Test Data Provisioning with Behave

> **Overview:** This repository contains the reference implementation for seeding test data within our Behavior-Driven Development (BDD) pipelines. We utilize Python's `behave` framework combined with REST API interactions to ensure a clean, deterministic state before running end-to-end integration tests.


---

## 🏗 Architecture & Approach

In automated testing for cloud and microservices architectures, tests often require a strict initial dataset. Instead of hardcoding this in scripts, we declare the desired state dynamically inside the `.feature` file. 

> *"By defining infrastructure and service states in the `Background` section, we make our test requirements readable for both business stakeholders and platform engineers."*

When Behave reads the Gherkin tables, it automatically injects them into the `context.table` object as a list of dictionaries. We then use the `requests` library to interface with our Mock API to purge old configurations and inject the defined state.

---

## 📝 Feature File Specifications

In your `features/cloud_provisioning.feature` file, define the initial state of your cloud instances. Notice how we use a data table directly under the `Background` keyword.

```gherkin
Feature: Cloud Instance Management API
  As a DevOps engineer
  I want to interact with the Cloud Provisioning API
  So that I can automate infrastructure scaling

  Background:
    Given the following cloud instances
      | hostname  | environment | active | instance_type | region    |
      | web-01    | production  | True   | t3.medium     | us-east-1 |
      | db-master | staging     | False  | r5.large      | eu-west-1 |
      | cache-01  | development | True   | t3.micro      | us-east-1 |

  Scenario: Verify active production nodes
    # ... your test steps here ...

```

----------

## ⚙️ Implementation Guide

All step definitions should reside in the `features/steps/` directory. For this workflow, we will create `infrastructure_steps.py`.

### 1. Step Definition Setup

Map the step string explicitly to match the Gherkin statement using the `@given` decorator.

Python

```
from behave import given

@given('the following cloud instances')
def step_impl(context):
    """Refreshes the mock cloud database with declared instances."""
    pass

```

### 2. State Teardown (Cleanup)

Before injecting new records, we must guarantee a clean state. We issue a `GET` request to fetch active instances, loop through them, and send a `DELETE` request for each.

Python

```
    # List active instances and terminate them
    response = requests.get(f"{context.base_url}/instances")
    assert response.status_code == 200, "Failed to retrieve initial instances"
    
    for instance in response.json():
        del_response = requests.delete(f"{context.base_url}/instances/{instance['id']}")
        assert del_response.status_code == 204, f"Failed to delete {instance['id']}"

```

### 3. State Provisioning (Loading)

Iterate over `context.table`, map the strings to native Python types (especially booleans), and push the payload via a `POST` request.

Python

```
    # Provision new nodes from the Gherkin table
    for row in context.table:
        payload = {
            "hostname": row['hostname'],
            "environment": row['environment'],
            "active": row['active'] in ['True', 'true', '1'], # Typecast to Boolean
            "instance_type": row['instance_type'],
            "region": row['region']
        }
        post_response = requests.post(f"{context.base_url}/instances", json=payload)
        assert post_response.status_code == 201, "Failed to provision instance"

```

----------

## 💻 Complete Implementation

Here is the final, production-ready code for `features/steps/infrastructure_steps.py`:

Python

```
# pylint: disable=function-redefined, missing-function-docstring
# flake8: noqa
"""
Infrastructure Steps
Handles state initialization for Cloud Provisioning tests.
"""
import requests
from behave import given

@given('the following cloud instances')
def step_impl(context):
    """
    Purge existing instances and provision new ones based on the feature table.
    """
    
    # --- PHASE 1: TEARDOWN ---
    get_response = requests.get(f"{context.base_url}/instances")
    assert get_response.status_code == 200, "API Unreachable"
    
    for instance in get_response.json():
        del_response = requests.delete(f"{context.base_url}/instances/{instance['id']}")
        assert del_response.status_code == 204, f"Delete failed for {instance['id']}"

    # --- PHASE 2: PROVISIONING ---
    for row in context.table:
        payload = {
            "hostname": row['hostname'],
            "environment": row['environment'],
            "active": row['active'] in ['True', 'true', '1'],
            "instance_type": row['instance_type'],
            "region": row['region']
        }
        
        post_response = requests.post(f"{context.base_url}/instances", json=payload)
        assert post_response.status_code == 201, f"Failed to create {row['hostname']}"

```

----------

## 🚀 Execution

To execute the test suite and trigger the data loading process, simply run the following command in your terminal from the root of the project repository:

Bash

```
behave

```

_If configured correctly, you should see Behave parsing the `Background` step, hitting the API, and turning the terminal output green._

----------

## ☑️ Pre-flight Checklist

Ensure the following items are verified before merging your testing logic to the main branch:

-   [x] Defined `BASE_URL` in `environment.py` (`context.base_url`).
    
-   [ ] Created `cloud_provisioning.feature` with valid Gherkin syntax.
    
-   [ ] Added `infrastructure_steps.py` to the `features/steps/` directory.
    
-   [ ] Handled boolean logic correctly for the `active` attribute.
    
-   [ ] Tested endpoint reachability locally before executing `behave`.

---



# 🚀 BDD Automation: Generating Step Definitions with Behave

![Python Version](https://img.shields.io/badge/python-3.9+-blue.svg)
![Framework](https://img.shields.io/badge/framework-Behave-green.svg)
![DevOps](https://img.shields.io/badge/pipeline-CI%2FCD%20Ready-orange.svg)

## 📋 Overview
In a professional **DevOps and QA Automation** ecosystem, speed and consistency are key. This project demonstrates how to leverage **Behave** (Python's Cucumber-based BDD framework) to automatically generate boilerplate step definitions. This "contract-first" approach ensures that our test suites perfectly align with our infrastructure requirements and business logic.

> **Engineer's Note:** Instead of manually writing boilerplate, we use the `behave` CLI to detect undefined steps and scaffold our Python implementation files.

---

## 🛠 Workflow Architecture

The BDD lifecycle follows a pattern similar to TDD (Test-Driven Development), specifically the **Red/Yellow/Green** cycle:

| Color | Status | Meaning |
| :--- | :--- | :--- |
| 🟨 **Yellow** | **Undefined** | The Gherkin step exists, but no Python function matches it. |
| 🟦 **Blue** | **Skipped** | Steps following a failed or undefined step are bypassed to save resources. |
| 🟥 **Red** | **Failed** | The step is implemented but the logic or assertion failed. |
| 🟩 **Green** | **Passed** | The step is implemented and the requirements are met. |

---

## 🚀 Lab Exercise: Cloud Resource Management
In this professional context, we are testing a **Cloud Inventory Service** rather than a generic search.

### 1. The Feature Specification
`features/cloud_inventory.feature`
```gherkin
Feature: Cloud Resource Inventory Management
    As a Cloud Engineer
    I want to query my infrastructure resources
    So that I can verify the state of my environment

    Scenario: Retrieve Active Instance Details
        Given I am authenticated to the "Cloud Management Console"
        When I filter resources by category "compute"
        And I click the "Sync Inventory" button
        Then I should see the message "Sync Successful"
        And I should see "web-server-01" in the active list
        But I should not see "legacy-db-backup" in the results

```

### 2. Generating Step Stubs

To identify what needs to be coded, run the following command in your terminal:

Bash

```
behave

```

**Standard Output Analysis:** Behave will scan the `./features` directory. Finding no matches in `./features/steps`, it will output **Yellow** text for the undefined steps and provide **Code Snippets** at the bottom.

----------

### 3. Implementation (The "Engineer" Approach)

Paste the generated snippets into your `web_steps.py` file. We wrap these in a professional structure using `NotImplementedError` to ensure no "silent passes" occur during development.

`features/steps/web_steps.py`

Python

```
from behave import given, when, then
from selenium.webdriver.common.by import By

# --- Infrastructure Setup Steps ---

@given(u'I am authenticated to the "{page_name}"')
def step_impl(context, page_name):
    """ Authenticates the session against the target environment """
    raise NotImplementedError(u'STEP: Given I am authenticated to the Cloud Portal')

# --- Action/Event Steps ---

@when(u'I filter resources by category "{category}"')
def step_impl(context, category):
    """ Applies API or UI filters based on resource tags """
    raise NotImplementedError(u'STEP: When I set the Category to {}'.format(category))

@when(u'I click the "{button_name}" button')
def step_impl(context, button_name):
    """ Triggers a DOM event or API post request """
    raise NotImplementedError(u'STEP: When I click the {} button'.format(button_name))

# --- Assertion/Validation Steps ---

@then(u'I should see the message "{message}"')
def step_impl(context, message):
    """ Validates flash messages or toast notifications """
    raise NotImplementedError(u'STEP: Then I should see status: {}'.format(message))

@then(u'I should see "{resource_id}" in the active list')
def step_impl(context, resource_id):
    """ Verifies presence of specific resource in the resulting dataset """
    raise NotImplementedError(u'STEP: Then I should see {} in results'.format(resource_id))

@then(u'I should not see "{resource_id}" in the results')
def step_impl(context, resource_id):
    """ Ensures decommissioned or filtered resources are hidden """
    raise NotImplementedError(u'STEP: Then I should not see {}'.format(resource_id))

```

---


# ☁️ BDD Automation: Implementing Step Definitions for Cloud Infrastructure

![Python Version](https://img.shields.io/badge/python-3.9+-blue.svg)
![Framework](https://img.shields.io/badge/framework-Behave-green.svg)
![Selenium](https://img.shields.io/badge/Selenium-4.19+-orange.svg)
![CI/CD](https://img.shields.io/badge/build-passing-brightgreen.svg)

## 📋 Overview
In this module, we transition from generated **Behavior-Driven Development (BDD)** stubs to actual functional test code. We will implement the Python steps required to validate our Cloud Infrastructure Management Console using **Behave** and **Selenium WebDriver**.

This guide covers the core **Red/Green/Refactor** workflow, mapping Gherkin feature steps to executable Python actions such as DOM interaction, event triggering, and state validation.

> **⚠️ CRITICAL ENGINEERING UPDATE: Selenium 4.x Syntax**
> We have upgraded our pipeline to Selenium 4.19.0. The legacy locator method `find_element_by_id(element_id)` is **deprecated and removed**. 
> All DOM queries must now strictly use the `By` class: `context.driver.find_element(By.ID, element_id)`.

---

## 🔄 The BDD Implementation Lifecycle

Before diving into the code, understand the terminal outputs when running `behave`:

| State | Console Color | Meaning in CI/CD Pipeline | Next Action |
| :--- | :---: | :--- | :--- |
| **Missing** | 🟨 Yellow | Step definition does not exist. | Generate stubs and add to `steps.py`. |
| **Skipped** | 🟦 Blue | Skipped due to a prior failure in the scenario. | Fix the preceding broken step. |
| **Failing** | 🟥 Red | Step exists but raised an exception (e.g., `NotImplementedError`). | **Implement the step logic.** |
| **Passing** | 🟩 Green | Step successfully executed. | Move to the next red step. |

---

## 🛠️ Step-by-Step Implementation Guide

Our feature scenario validates that an engineer can successfully filter active compute instances while ensuring decommissioned nodes are hidden. 

### Prerequisites Setup
Ensure your `features/steps/web_steps.py` file includes the necessary imports:
```python
from behave import given, when, then
from selenium.webdriver.common.by import By

```

### Step 1: Navigating to the Target Environment

**Gherkin:** `Given I am on the "Cloud Console Dashboard"`

We need to resolve the base URL from our environment context and command the WebDriver to issue an HTTP GET request.

Python

```
@given('I am on the "Cloud Console Dashboard"')
def step_impl(context):
    """ Instructs the web driver to load the root URL of the service """
    context.response = context.driver.get(context.base_url)

```

### Step 2: Inputting Query Data

**Gherkin:** `When I set the "Resource Type" to "ec2-instance"`

To interact with input fields, we locate the element, clear any existing cache/data, and inject our string. _(Assuming the DOM element ID is `resource_type`)_.

Python

```
@when('I set the "Resource Type" to "{resource}"')
def step_impl(context, resource):
    """ Locates the filter input, clears it, and inputs the target resource type """
    element = context.driver.find_element(By.ID, 'resource_type')
    element.clear()
    element.send_keys(resource)

```

### Step 3: Triggering Pipeline Actions

**Gherkin:** `And I click the "Filter" button`

Buttons and links require the `.click()` method to simulate user interaction. _(Assuming standard UI ID convention: `lowercase_name-btn`)_.

Python

```
@when('I click the "Filter" button')
def step_impl(context):
    """ Simulates an event click on the filter execution button """
    element = context.driver.find_element(By.ID, 'filter-btn')
    element.click()

```

### Step 4: Validating System State (Assertions)

**Gherkin:** `Then I should see the message "Query Successful"`

We must validate that the system responds correctly via the UI's status alert box (ID: `status_alert`).

Python

```
@then('I should see the message "Query Successful"')
def step_impl(context):
    """ Asserts that the flash message element contains the success string """
    element = context.driver.find_element(By.ID, 'status_alert')
    assert "Query Successful" in element.text

```

### Step 5: Validating Data Presence and Absence

**Gherkin:** `And I should see "prod-web-01" in the active list`

**Gherkin:** `But I should not see "deprecated-db-node" in the active list`

Finally, we parse the returned infrastructure table (ID: `infrastructure_table`) to verify both expected inclusions and expected exclusions.

Python

```
@then('I should see "prod-web-01" in the active list')
def step_impl(context):
    """ Confirms the target node exists in the resulting DOM table """
    element = context.driver.find_element(By.ID, 'infrastructure_table')
    assert "prod-web-01" in element.text

@then('I should not see "deprecated-db-node" in the active list')
def step_impl(context):
    """ Ensures decommissioned nodes are successfully filtered out """
    element = context.driver.find_element(By.ID, 'infrastructure_table')
    assert "deprecated-db-node" not in element.text
```
