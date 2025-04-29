# 📋 Software Testing Types

A categorized overview of functional and non-functional software testing types.

---

## ✅ Functional Testing Types

| **Type**               | **Purpose**                                      | **Banking Domain Example**                                                                 |
|------------------------|--------------------------------------------------|---------------------------------------------------------------------------------------------|
| **Unit Testing**        | Tests individual functions/modules.              | Test the interest calculation logic in a savings account module.                            |
| **Integration Testing** | Tests interaction between modules.               | Ensure the loan module interacts correctly with the credit score service.                   |
| **System Testing**      | Tests the complete system end-to-end.            | Test full loan application process, from data input to approval.                            |
| **UAT (Acceptance)**    | Validates software meets business needs.         | Bank employee checks whether customer onboarding meets actual operational flow.             |
| **Smoke Testing**       | Quick test to check if key features work.        | Test login, view balance, and transfer money after a deployment.                            |
| **Sanity Testing**      | Validates specific new functionality or bug fix. | Verify a fix for "failed transaction retry" works as expected.                              |
| **Regression Testing**  | Ensures existing features still work.            | After adding a new payment option, verify that existing features like balance check work.   |
| **API Testing**         | Verifies API endpoints work as expected.         | Test `/api/transferfunds` works with valid and invalid input data.                          |

---

## ✅ Non-Functional Testing Types

| **Type**              | **Purpose**                                                                 | **Banking Domain Example**                                                                 |
|-----------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| **Performance Testing** | Checks responsiveness, stability under load.                               | Measure response time of account statement generation during peak usage.                    |
| **Load Testing**        | Test system behavior under expected load.                                  | Simulate 500 users logging in and transferring funds simultaneously.                        |
| **Stress Testing**      | Test limits of the system by pushing beyond normal load.                   | Simulate 10,000 login attempts to check how the banking server handles extreme load.        |
| **Soak Testing (Stability)** | Run system for extended periods under load to detect memory leaks.         | Run online banking for 24 hours continuously with ongoing transactions to detect slowdowns. |
| **Volume Testing**      | Check how the system handles large volumes of data.                        | Upload 1 million transactions and check system response.                                    |
| **Endurance Testing**   | Similar to soak, but focuses more on prolonged usage rather than just memory. | Run the mobile banking app for a week with repeated usage to check resource exhaustion.     |
| **Scalability Testing** | Evaluate how system scales up with increased users or data.                | Add 10,000 new users and observe whether the fund transfer module performance degrades.     |
| **Security Testing**    | Detect vulnerabilities and protect against attacks.                        | Check if user credentials are encrypted and validate session timeout for online banking.    |
| **Compatibility Testing** | Ensure app works across devices, browsers, OS versions.                   | Test banking app on iOS 15, Android 14, Chrome, and Safari.                                 |
| **Usability Testing**   | Ensures app is user-friendly and intuitive.                                | Observe how easily customers can schedule auto-pay for credit cards.                        |
| **Accessibility Testing** | Ensures app is usable by people with disabilities.                        | Check if a visually impaired user can navigate the online portal using screen readers.      |
| **Recovery Testing**    | Ensures system can recover from crashes, failures.                         | Simulate server crash and verify the last transaction is not lost or duplicated.            |
| **Installation Testing** | Verify installation and upgrade processes.                               | Check whether the mobile banking app installs correctly and retains user data post-upgrade. |

---
