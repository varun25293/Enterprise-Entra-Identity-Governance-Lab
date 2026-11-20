# Enterprise Microsoft Entra ID Governance Lab (Module 11)

## Overview
This repository documents an end-to-end **Microsoft Entra ID Governance** deployment built in the `ApexModernWork.onmicrosoft.com` enterprise tenant. The architecture eliminates standing privilege and privilege creep by combining **Entitlement Management (Access Packages)**, self-service request/approval workflows via the **My Access** portal (`myaccess.microsoft.com`), and automated **Access Reviews** with zero-trust fail-safe revocation.

---

## Architecture & Implemented Controls

### 1. Entitlement Management Catalog & Access Package (`01-Entitlement-Management-Catalog`)
- **Resource Catalog:** Created `Apex Finance & Security Onboarding Catalog` to govern sensitive group memberships and audit resources.
- **Access Package:** Configured `Apex Privileged Finance & Audit Access` (`Object ID: 3d9f1bb5-0592-4cfd-bb0e-7544f5bac2b8`) bundling Security/M365 group `Member` permissions.
- **Zero-Trust Policy Guardrails:**
  - Restricted self-service eligibility to internal directory members (`All members (excluding guests)`).
  - Enforced mandatory requestor business justification and single-stage approval by `Varun Mahajan` (`admin@ApexModernWork.onmicrosoft.com`).
  - Configured automatic **30-day assignment expiration** (`12/21/2026`) and recurring quarterly access reviews.
- **Evidence:** [`01-access-package-overview-config.png`](./01-Entitlement-Management-Catalog/01-access-package-overview-config.png)

### 2. Self-Service Request & Approval Workflow (`02-Access-Package-Request-Approval`)
- **End-User Self-Service (`myaccess.microsoft.com`):** Verified standard user (`pilotuser1@ApexModernWork.onmicrosoft.com`) self-service request submission with business justification.
- **Admin Governance Approval:** Reviewed and approved the request via the My Access Approvals queue with documented approver justification.
- **Automated Provisioning:** Verified dynamic group membership provisioning and `Delivered` assignment state with automatic 30-day expiration tracking in Entra ID.
- **Evidence:** [`02-access-package-approved-delivered.png`](./02-Access-Package-Request-Approval/02-access-package-approved-delivered.png)

### 3. Automated Access Reviews & Fail-Safe Revocation (`03-Automated-Access-Reviews`)
- **Access Review Policy:** Deployed `Apex Quarterly Privileged Group Access Review` targeting privileged group memberships (`Everyone` scope).
- **Automated Remediation & Decision Helpers:**
  - Enabled **Auto-apply results to resource** so denied or unreviewed memberships are automatically stripped upon review completion.
  - Enforced zero-trust fail-safe rule: **If reviewers don't respond -> Remove access**.
  - Enabled **30-day inactive sign-in decision helpers** and mandatory reviewer justification.
- **Evidence:** [`03-automated-access-review-config.png`](./03-Automated-Access-Reviews/03-automated-access-review-config.png)

---

## Key Security & SC-300 Takeaways
- **Preventing Privilege Creep:** Pairing 30-day Access Package expiration with `Auto-apply = Enabled` and `No response = Remove access` ensures temporary project or audit permissions never persist indefinitely.
- **Self-Service Offloading:** Routing access requests through `https://myaccess.microsoft.com` removes manual L1 helpdesk group-management tickets while preserving a complete audit trail.