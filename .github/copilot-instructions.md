## Domain Area Validation

The `{area}` component must be validated before execution.

---

### Domain Source

- Allowed areas are defined in:
  `scripts/domain.txt`

- The file must contain:
  - one value per line
  - lowercase values

Example:

energy  
powertrain  
driving  

---

### Validation Rules

1. If `scripts/domain.txt` exists:
   - The area MUST match one of the values in the file
2. If `{area}` is NOT found:
   - Call `suggest_area` skill
   - If a HIGH-confidence match exists:
     - Automatically replace `{area}` with the suggested value
     - Log correction internally
   - If a low-confidence or multiple matches exist:
     - Ask for confirmation:
       "Did you mean `<suggested_area>`?"
     - STOP processing
   - If no match exists:
     - Respond with error
     - Include valid values
     - STOP processing
3. If `scripts/domain.txt` does NOT exist:
   - Skip strict validation

---

## Branch Input Flexibility

Users may provide input in multiple formats:

- domain-energy-101
- energy-101
- energy 101
- energy101

All inputs MUST be normalized to:

domain-{area}-{number}

---

## Input Validation

- Input MUST contain exactly one `{area}` and one `{number}`
- If no valid pattern is detected:
  - The system MUST respond with an error
  - Provide examples of valid inputs
  - STOP processing immediately

---

### Number Validation

- The `{number}` must be numeric only
- If `{number}` is not numeric:
  - The system MUST respond with an error
  - STOP processing immediately
- If `{number}` is missing:
  - The system MUST respond with an error
  - The response MUST indicate that a number is required
  - STOP processing immediately
- If multiple numeric values are detected:
  - The system MUST respond with an error
  - Ask the user to provide only one number
  - STOP processing immediately

---

### Example (Invalid Input)

- "modeldiff energy" → missing number
- "modeldiff hello world" → no valid pattern

---

### Validation Order

- Validation MUST occur AFTER normalization
- The system MUST validate the normalized format:
  domain-{area}-{number}
- Validation MUST occur BEFORE Jenkins execution

---

## Jenkins Execution

The system must trigger the Jenkins job after all conditions are satisfied.

---

### Execution Conditions

Trigger ONLY IF:

1. intent = ModelDiff_PR
2. PR URL is fully constructed and valid
3. domain validation has PASSED
4. confirmation is obtained (if required)
5. no validation errors have occurred
6. a valid `{area}` and `{number}` have been successfully extracted

---

### Email Resolution Rules

The system must determine the email parameter as follows:

1. If the user input contains an email:
   - Use the provided email

2. If no email is provided in the user input:
   - Look for a default email in:
     `scripts/email.txt`

3. If `scripts/email.txt` exists:
   - Use the email from the file

4. If no email is found in both input and file:
   - Execute without email

---

### Email Priority

1. User-provided email (highest priority)
2. Email from `scripts/email.txt`
3. No email (fallback)

---

### Execution Action

Trigger Jenkins job with:

Job Name:
- ModelDiff_PR

Parameters:
- PR = <full URL>
- email = <resolved email if available>

---

### Notes

- email parameter is optional
- DO NOT include email if not available
- MUST use exact parameter names

``