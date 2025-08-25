
##  README – **STX-VestedTrust: Smart Contract for Heir Vesting & Milestone Rewards**

### Overview

**VestedTrust** is a Clarity smart contract designed to manage STX fund allocations to heirs with milestone-based reward releases. It enables secure heir registration, milestone configuration, guardian oversight, and vesting validation over time. The contract supports bonus rewards and includes built-in safety mechanisms like age checks, deadlines, and guardian approvals.

---

### ✨ Key Features

* **Heir Management**: Register heirs with STX allocations and optional guardian assignment.
* **Milestone System**: Add custom milestones tied to age, deadlines, and guardian approval requirements.
* **Vesting Enforcement**: Enforces a vesting period before any withdrawal.
* **Bonus & Rewards**: Adds education bonuses and milestone multipliers to promote achievement.
* **Authorization & Access Control**: Functions restricted to contract owner or guardians when appropriate.
* **Safety Validations**: Prevents common errors with thorough validation for inputs and status.

---

### 🔐 Authorization Model

* **Contract Owner**: Full administrative access (can add heirs, milestones, bonuses).
* **Heir**: Can be assigned a guardian and receive funds upon meeting conditions.
* **Guardian**: Optional entity with approval power for specific milestone completions.

---

### 📊 Data Structures

#### **Heir Info**

Stored in the `heirs` map:

* `birth-height`: Block height representing birth
* `total-allocation`: Allocated STX
* `claimed-amount`: Total claimed so far
* `status`: "active" | "paused" | "completed"
* `guardian`: Optional guardian principal
* `vesting-start`: Block height when vesting begins
* `education-bonus`: STX bonus amount
* `last-activity`: Last interaction block height

#### **Milestones**

Stored in the `milestones` map:

* `description`: Description of the milestone
* `reward-amount`: STX reward
* `age-requirement`: Required age (in years)
* `deadline`: Optional block height deadline
* `bonus-multiplier`: Multiplier (e.g., u150 = 1.5x)
* `requires-guardian`: Boolean

#### **Guardian Approvals**

Stored in `guardian-approvals`:

* Key: `{ heir, milestone-id }`
* Value: `{ approved, timestamp }`

---

### 🔧 Constants

| Constant                   | Value    | Description          |
| -------------------------- | -------- | -------------------- |
| `MINIMUM-AGE-REQUIREMENT`  | u16      | Minimum eligible age |
| `MAXIMUM-AGE-REQUIREMENT`  | u100     | Max allowed age      |
| `MINIMUM_ALLOCATION`       | u1000000 | 1 STX                |
| `BLOCKS_PER_DAY`           | u144     | For age calculation  |
| `MAXIMUM-BONUS_MULTIPLIER` | u500     | 5x max               |

---

### 🚧 Key Public Functions

* `add-heir`: Register a new heir.
* `update-guardian`: Change guardian for a heir.
* `add-education-bonus`: Add education reward to heir.
* `add-milestone`: Define a new milestone with criteria.

---

### 🧪 Key Read-Only Functions

* `get-heir-info`: Fetch heir data.
* `get-milestone`: Get milestone by ID.
* `calculate-age`: Get age in years based on `birth-height`.
* `get-vesting-status`: Checks if vesting period is satisfied.

---

### ⚠️ Error Codes

| Error                     | Meaning                            |
| ------------------------- | ---------------------------------- |
| `ERR-NOT-AUTHORIZED`      | Not permitted to call the function |
| `ERR-ALREADY-INITIALIZED` | Duplicate entry                    |
| `ERR-INVALID-AGE`         | Age outside bounds                 |
| `ERR-INVALID-AMOUNT`      | Zero or invalid STX amount         |
| `ERR-ZERO-ALLOCATION`     | Below minimum allocation           |
| `ERR-SELF-GUARDIAN`       | Cannot assign self as guardian     |
| `ERR-TRANSFER-FAILED`     | STX transfer failed                |
| *(...and more)*           | See code for full list             |

---
