# Topic 01 Study Note — CTFL 1.1–1.2 Terminology

> Scope: ISTQB CTFL v4.0.1 §1.1–§1.2.
>
> This note teaches the terminology before assessment. It is not a replacement for the official syllabus or ISTQB Glossary.

## 1. Testing

**Meaning for this topic:** a set of activities used to evaluate test objects and discover defects or other quality problems.

Typical objectives include:
- evaluating work products such as requirements, user stories, designs, code, or the running system;
- verifying whether specified requirements are fulfilled;
- validating whether the test object is fit for intended use and satisfies user/stakeholder needs;
- finding defects and failures;
- reducing the level of product-quality risk;
- providing information to stakeholders so they can make informed decisions;
- building confidence in the level of quality.

### Important boundary
Testing can contribute evidence about quality. It does **not** prove that the product is defect-free.

---

## 2. Debugging

**Meaning for this topic:** activities used to find, analyze, and remove the cause of a failure or defect.

A useful separation:

- **Testing:** exposes or evaluates a problem.
- **Debugging:** investigates why it happened and fixes the cause.
- **Retesting / confirmation testing:** testing again after a fix to determine whether the problem was resolved.

### Example
A tester observes that saving a valid form returns HTTP 500.

- Reproducing the 500 and recording evidence → **testing**.
- Developer traces the exception to a bad null check → **debugging**.
- Developer changes the code → part of fixing/debugging work.
- Tester repeats the original scenario after the change → **testing again**.

---

## 3. Quality Assurance (QA) vs Testing

### Testing
Product-oriented and largely corrective: evaluates work products and the product to reveal quality problems and provide evidence.

### Quality Assurance
Process-oriented and preventive: focuses on defining, implementing, evaluating, and improving the processes used to develop and test the product.

### Example
- Finding that a requirement is ambiguous → **testing/static testing activity**.
- Changing the team's requirement-review process so the same class of ambiguity is prevented systematically → **QA/process improvement**.

### Important boundary
Testing is not a synonym for QA. Testing is one major quality-control activity; QA is broader and process-oriented.

---

## 4. Error

An **error** is a human mistake: an incorrect action, judgement, assumption, or decision.

Examples:
- an analyst misunderstands a business rule;
- a developer chooses the wrong comparison operator;
- a tester writes the wrong expected result because they misunderstood the requirement.

An error may produce a defect, but not every error necessarily reaches a work product.

---

## 5. Defect

A **defect** is an imperfection or fault in a work product.

A defect can exist in:
- requirements;
- design;
- source code;
- test cases/scripts;
- configuration/build files;
- other work products.

Examples:
- requirement states the wrong limit;
- code uses `>` where `>=` is required;
- test script contains an incorrect expected result.

### Important boundary
A defect can exist without producing a visible failure in every execution. It may require specific conditions to be activated.

---

## 6. Failure

A **failure** is an observed event in which the system or component does not perform a required function within specified limits.

In practical QA language: the running system behaves differently from what it should do.

Examples:
- valid data is rejected;
- an expected result is missing;
- the system crashes;
- an operation returns an incorrect result.

### Important boundary
A failure is an **observed behavior**, not the defect itself.

---

## 7. Root Cause

A **root cause** is the fundamental reason why a problem occurred.

It is not automatically:
- the visible failure;
- the first defect found;
- the nearest technical symptom;
- whatever seems plausible.

Root cause normally requires analysis.

### Example chain

A requirement is vague  
→ analyst makes an incorrect assumption (**error**)  
→ specification contains an incorrect rule (**defect**)  
→ developer implements that rule  
→ user receives an incorrect result (**failure**)

A deeper process problem such as “critical rules are never reviewed with domain owners” may be the **root cause** if analysis supports that conclusion.

### QA discipline
If evidence is insufficient, write:

> Root cause: not established yet.

Do not convert a hypothesis into a fact.

---

## 8. Error → Defect → Failure → Root Cause

The relationship is not a guaranteed one-to-one chain.

A common pattern is:

**root cause → human error → defect → failure**

But:
- one error can create several defects;
- one defect can cause several failures;
- a defect may never be triggered;
- failures can also arise from environmental conditions;
- root cause may exist at process, organizational, technical, or communication level.

---

## 9. Verification and Validation

These concepts support the test objectives in this topic.

### Verification
Are the specified requirements correctly fulfilled?

Short version:
> Are we building the product right?

### Validation
Does the resulting product satisfy intended use and stakeholder/user needs?

Short version:
> Are we building the right product?

The short versions are memory aids, not complete definitions.

---

## 10. Quick classification rule

When reading a scenario, ask in this order:

1. **What did a person do or decide incorrectly?** → possible **error**
2. **What is wrong in a work product?** → possible **defect**
3. **What wrong behavior was actually observed while executing/using the system?** → **failure**
4. **Why did this class of problem arise in the first place?** → possible **root cause**
5. **Are we observing/evaluating the product or work product?** → **testing**
6. **Are we locating and removing the cause of a problem?** → **debugging**
7. **Are we improving the process so similar problems are prevented?** → **QA**

---

## 11. Minimal mastery check before assessment

Before attempting certification tasks, you should be able to explain without notes:

- at least five typical test objectives;
- testing vs debugging;
- testing vs QA;
- verification vs validation;
- error vs defect vs failure vs root cause;
- why a defect may exist without a failure;
- why a root cause must not be guessed.

If any of these are unclear, stay in **Learning** rather than moving to **Practicing**.
