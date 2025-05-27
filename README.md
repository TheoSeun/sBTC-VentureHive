---

# sBTC-VentureHive - Platform Smart Contract

## Overview

sBTC-VentureHive is a decentralized crowdfunding platform implemented as a smart contract on the Stacks blockchain. It enables entrepreneurs (founders) to create ventures with specific funding goals and defined sub-goals. Backers can fund these ventures by sending STX tokens, and depending on the success or failure of the venture, backers can either allow the funds to be used or withdraw their contributions.

## Features

* **Create Ventures:** Founders can create ventures with a funding goal, end date, and up to 5 funding sub-goals.
* **Back Ventures:** Users can back ventures by contributing STX tokens during the open funding period.
* **Goal Management:** Founders can mark individual goals as completed, and the venture can only be finalized when all goals are completed.
* **Finalize Ventures:** Founders can finalize a venture once all goals are met and funding is successful.
* **Withdrawal:** Backers can withdraw their funds if the venture fails to meet the funding goal by the end date.
* **Close Failed Ventures:** Allows the founder or platform admin to close ventures that did not reach the funding goal, enabling backers to withdraw their funds.
* **Withdrawal Eligibility:** Only ventures that failed funding and are closed allow withdrawals.

## Data Structures

* **Venture:** Contains metadata such as founder, name, summary, funding goal, amount collected, end date, status flags (`is-open`, `is-finalized`), and up to 5 goals with individual funding requirements and completion status.
* **Backers:** Tracks backer contributions and withdrawal status per venture.

## Constants

* **Platform Admin:** The deploying address (tx-sender at deployment) with special privileges.
* **Error Codes:** Standardized error responses for unauthorized actions, invalid inputs, funding status, etc.

## Contract Functions

### Public Functions

* **create-venture(name, summary, funding-goal, end-date, goals):**
  Allows founders to create a new venture with funding goals and sub-goals.

* **back-venture(venture-id, stx-transferred):**
  Enables backers to contribute STX tokens to an open venture.

* **request-withdrawal(venture-id):**
  Allows backers to withdraw their contributions if the venture funding fails.

* **close-failed-venture(venture-id):**
  Closes a failed venture and enables withdrawals.

* **complete-goal(venture-id, goal-index):**
  Marks a specific goal as completed by the founder.

* **finalize-venture(venture-id):**
  Finalizes a venture once all goals are completed and funding is successful.

### Read-Only Functions

* **get-goal-funds(goal):** Returns the required funds for a goal.
* **all-goals-completed?(goals):** Checks if all goals in a venture are completed.
* **is-goal-completed(goal):** Returns the completion status of a goal.
* **is-withdrawal-eligible(venture-id):** Checks if a venture is eligible for backers to withdraw funds.

### Private Functions

* **get-goal-by-index(venture-goals, goal-index):** Retrieves a goal by its index.
* **update-goal-list(goals, goal-index, updated-goal):** Updates a specific goal in the goal list.

## Error Handling

The contract uses predefined error constants for common failure cases such as:

* Unauthorized actions
* Insufficient funds
* Invalid venture or goal indices
* Attempting operations on closed or finalized ventures
* Withdrawal eligibility failures

## Usage Flow

1. **Founders** create ventures specifying the total funding goal, deadlines, and detailed goals.
2. **Backers** contribute STX tokens to ventures while funding is open.
3. If the venture **reaches its funding goal** and all goals are completed, the founder finalizes the venture.
4. If the venture **fails to reach its funding goal by the deadline**, it can be closed, allowing backers to withdraw their funds.
5. Founders can mark goals complete individually as progress is made.

## Deployment & Administration

* The **platform admin** is set at deployment and may have privileges (e.g., receiving withdrawn funds).
* Ventures track founders and backers independently.
* The contract manages state to prevent double withdrawals or unauthorized actions.

---
