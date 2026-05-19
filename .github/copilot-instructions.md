## Jenkins Execution

The system must trigger the Jenkins job after all conditions are satisfied.

---

### Execution Conditions

Trigger ONLY IF:

1. intent = ModelDiff_PR
2. PR URL is fully constructed and valid
3. confirmation is obtained (if required)

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
- email = <if available>

---

### Execution Format (Conceptual)

trigger_build(
  job="ModelDiff_PR",
  parameters={
    "PR": "<PR_URL>",
    "email": "<EMAIL_IF_EXISTS>"
  }
)

---

### Notes

- email parameter is optional
- DO NOT include email if not available
- MUST use exact parameter names

---

### Example

Input:
"modeldiff domain-energy-102 user@test.com"

Execution:

trigger_build(
  job="ModelDiff_PR",
  parameters={
    "PR": "https://cc.net/swh/ddad-domains-energy/pull/102",
    "email": "user@test.com"
  }
)