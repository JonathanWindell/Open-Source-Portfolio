# Pydantic Core: Validation Logic Regression Guard

[![Pydantic](https://img.shields.io/badge/Pydantic-E92063?logo=Pydantic&logoColor=white)](https://github.com/pydantic/pydantic)
[![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Pytest](https://img.shields.io/badge/Pytest-fff?logo=pytest&logoColor=000)](https://docs.pytest.org/)

## Overview
This contribution focused on identifying and isolating a complex validation anomaly within the **Pydantic V2 Core**. The issue involved redundant execution of `model_validator(mode="after")` within nested models, which led to performance degradation and logic-breaking `AssertionErrors`.

---

## The Technical Challenge

### The Problem
When validating complex, nested schemas, Pydantic's internal schema generation logic was triggering "after" validators multiple times for the same instance during parent validation.

* **Impact:** Performance overhead in high-throughput applications.
* **Risk:** Data corruption or state-change side effects if validators were not idempotent.
* **Symptom:** Unexpected `AssertionErrors` during standard model instantiation.

### Root Cause Analysis (RCA)
Through deep-tracing of Pydantic’s internals, the issue was localized to how the core engine navigated the schema tree for nested models. The validator was being incorrectly re-registered in the execution stack of the parent model.

---

## Contribution Details

| Category | Description |
| :--- | :--- |
| **Focus** | Regression Testing & Bug Reproduction |
| **Action** | Traced execution flow of `model_validator` in nested contexts. |
| **Deliverable** | Implementation of a comprehensive `pytest` suite utilizing `xfail` (Expected Failure) logic. |
| **Status** | **Merged** into Pydantic Main – serving as a permanent regression guard. |

### Implementation Detail
The test suite was designed to mirror real-world complex schemas, ensuring that:
1.  Nested models are validated exactly once.
2.  Validator execution order is preserved.
3.  State remains consistent across parent/child boundaries.

---

## Project Links
* **Official Issue:** *[pydantic/pydantic#8452](https://github.com/pydantic/pydantic/issues/8452)*
* **Pull Request:** *[pydantic/pydantic#12420](https://github.com/pydantic/pydantic/pull/12420)*

---

## Key Takeaways
* Deepened expertise in **Pydantic Internals** and Type Validation theory.
* Demonstrated proficiency in **Regression Testing** for large-scale open-source projects.
* Collaborated with core maintainers to ensure long-term codebase stability.