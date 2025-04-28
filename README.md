
# LexStack -  Legal Contract Management System

A smart contract-based system for managing **legal agreements** on the **Stacks blockchain**, written in **Clarity**.  
It supports creation, versioning, signature collection, access control, and event logging for contracts.

---

## ✨ Features

- **Create Legal Contracts** with initial versions
- **Add Signatures** to active contracts
- **Version Management**: Create new versions of contracts
- **Access Control**: Grant users read/write/admin permissions
- **Audit Events**: Detailed event logs for every major action
- **Error Handling**: Standardized error codes for robust interaction

---

## 📚 Contract Structure

### Constants

| Constant | Value | Description |
| :------- | :---- | :---------- |
| `ERR_NOT_AUTHORIZED` | `err u100` | Unauthorized action |
| `ERR_INVALID_INPUT` | `err u101` | Invalid input provided |
| `ERR_CONTRACT_NOT_FOUND` | `err u102` | Contract ID does not exist |
| `ERR_MAX_SIGNATURES_REACHED` | `err u103` | Max signatures reached |
| `ERR_INVALID_STATE` | `err u104` | Invalid operation due to contract state |
| `ERR_VERSION_NOT_FOUND` | `err u105` | No version exists for the contract |
| `ERR_EVENT_FAILED` | `err u106` | Failed to log event |
| `MAX_SIGNATURES` | `u8` | Maximum allowed signatures |
| `ACCESS_LEVEL_READ` | `u0` | Read-only permission |
| `ACCESS_LEVEL_WRITE` | `u1` | Write/update permission |
| `ACCESS_LEVEL_ADMIN` | `u2` | Admin permission |

---

## 🗄️ Data Structures

### Maps
- **`legal-contracts`**: Stores contract metadata (title, description, status, created-by, etc.)
- **`contract-versions`**: Tracks different versions of a contract.
- **`contract-signatures`**: Stores user signatures per contract and version.
- **`contract-access`**: Defines access levels (read/write/admin) for users.
- **`contract-events`**: Logs important events (creation, signature added, access granted, etc.)

### Variables
- **`contract-nonce`**: Auto-incremented contract ID counter.
- **`event-nonce`**: Auto-incremented event ID counter.

---

## 🛠️ Public Functions

| Function | Description |
| :------- | :---------- |
| `create-contract(title, description, required-signatures, initial-content-hash)` | Create a new contract with metadata and an initial version |
| `add-signature(contract-id, signature-hash)` | Add a user's signature to an active contract |
| `create-version(contract-id, content-hash, metadata)` | Create a new version of an existing contract |
| `grant-access(contract-id, user, access-level)` | Grant read/write/admin access to a user |
| `get-contract-details(contract-id)` | Fetch details of a given contract |

---

## 🧩 Private Helper Functions

- **`is-contract-admin`**: Checks if caller has admin rights on a contract.
- **`increment-nonce`**: Increments a given nonce.
- **`validate-contract-exists`**: Verifies if a contract exists.
- **`validate-description`**, **`validate-content-hash`**, **`validate-signature-hash`**, **`validate-metadata`**, **`validate-user-principal`**: Input validation helpers.
- **`log-contract-event`**: Creates an audit event in the system.
- **`get-latest-version`**: Fetches the latest version of a contract.
- **`update-contract-timestamp`**: Updates the `updated-at` field when a contract is modified.

---

## ⚙️ How It Works

1. **Contract Creation**  
   - Deploys a new contract with a title, description, required number of signatures, and an initial content hash.
   - Creator gets admin access automatically.

2. **Signing Contracts**  
   - Users can add a cryptographic signature to a contract if it is active and not already signed.

3. **Versioning**  
   - Admins can create new versions of the contract content with updated metadata and content hashes.

4. **Access Control**  
   - Admins can grant other users `read`, `write`, or `admin` access.

5. **Event Logging**  
   - Every action (creation, signature, access granting, versioning) is logged for auditability.

---

## 🚨 Error Handling

All public functions use `asserts!` and `unwrap!` to ensure:
- Proper user permissions
- Input correctness
- Contract and version existence
- Safe event logging

Errors are returned with meaningful and consistent codes.

---

## 🔒 Access Control Overview

| Access Level | Capabilities |
|:------------ |:------------ |
| `Read` | View contract details |
| `Write` | Modify contract (if logic extended) |
| `Admin` | Grant access, create new versions, full control |

---

## 🧪 Example Usage

```lisp
;; Create a contract
(create-contract "Service Agreement" "Legal terms for service" u3 0xabc123...)

;; Sign a contract
(add-signature u0 0xdeadbeef...)

;; Create a new version
(create-version u0 0xbeefcafe... "Updated terms with GDPR compliance")

;; Grant admin access to another user
(grant-access u0 'SPXYZ... u2)
```

---
