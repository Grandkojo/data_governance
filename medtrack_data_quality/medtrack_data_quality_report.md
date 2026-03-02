# Data Quality Assessment Report: MedTrack Ghana

**Prepared by:** Junior Developer
**Date:** March 2, 2026

---

## Task 1: Identify Quality Issues

| Dimension | Violation Description | Specific Example from Dataset |
| :--- | :--- | :--- |
| **Accuracy** | Data that is factually incorrect or logically impossible. | **P002 (Record 2)**: Phone number `244789012` is missing the leading '0' required for Ghana telcos, making it an inaccurate representation of the reachable number. |
| **Completeness** | Missing required information in mandatory fields. | **P004**: The `PatientName` field is completely empty, even though a `PatientID` and `PhoneNumber` exist. |
| **Consistency** | The same data or entity is represented in different formats. | **Doctor Names/Payment**: `Dr. Osei` vs `dr. osei` and `Paid` vs `paid`. This creates distinct categories for identical values. |
| **Timeliness** | Outdated information or records that conflict chronologically. | **P001**: Multiple records for the same patient on different dates (`2025-10-15` and `2025-10-20`) without clear versioning or "most recent" flag. |
| **Validity** | Data that does not follow the defined business or system rules/formats. | **P002 (Record 2)**: `AppointmentDate` is in `15/10/2025` (DD/MM/YYYY) format, while all other records use `YYYY-MM-DD`. |
| **Uniqueness** | Unnecessary duplication of the same event or entity. | **P002**: Two records exist for the same patient (`Ama Serwa`) for the same date (`2025-10-15`) with slightly different formatting, counting one appointment twice. |

---

## Task 2: Assess Business Impact

| Quality Issue | Operational Problem | Affected Business Function |
| :--- | :--- | :--- |
| **Invalid Phone (Accuracy/Validity)** | **SMS Failures**: Patients do not receive appointment reminders, leading to high "no-show" rates and wasted clinical time. | **Operations** |
| **Duplicate Records (Uniqueness)** | **Incorrect Reporting**: Reports show inflated patient counts (e.g., counting Ama Serwa twice), leading to flawed resource planning. | **Clinical / Management** |
| **Inconsistent Payment (Consistency)** | **Billing Failures**: Automated billing systems fail to recognize `paid` as identical to `Paid`, causing reconciliation errors and delayed revenue. | **Finance** |

---

## Task 3: Recommend Solutions

### 1. Technical Solution: Input Validation & Formatting (Fixes Validity/Accuracy)
*   **Fix**: Implement Regex-based validation for the `PhoneNumber` field (e.g., `^0[25][0-9]{8}$`) and a standardized Date Picker for the `AppointmentDate` field to force `YYYY-MM-DD` format.
*   **Responsibility**: Frontend Engineering Team.
*   **Verification**: Attempt to enter `244789012` or `15/10/2025` in the UI; the system should reject the input with a clear error message.

### 2. Technical Solution: Data Normalization Script (Fixes Consistency)
*   **Fix**: Run a backend normalization script to `UPPER()` or `Proper()` case all `DoctorName` and `PaymentStatus` fields. Implement dropdown menus (Enums) instead of free-text fields.
*   **Responsibility**: Backend/Database Developer.
*   **Verification**: Run a `SELECT DISTINCT PaymentStatus` query; the result should return exactly one row for 'Paid'.

### 3. Technical Solution: Database Constraints (Fixes Uniqueness)
*   **Fix**: Apply a **Composite Unique Constraint** on `(PatientID, AppointmentDate)`. This prevents the same patient from being booked twice at the exact same time/day.
*   **Responsibility**: Database Administrator (DBA).
*   **Verification**: Attempt to insert a duplicate record for `P002` on `2025-10-15`; the database should throw a `UniqueConstraintViolation` error.

---

## Task 4: Specialization Perspective

From a **Data Governance** perspective, the biggest risk of poor data consistency is **Clinical Misdiagnosis and Patient Safety**. When patient names or IDs are inconsistent (e.g., a "Kofi Annan" and "K. Annan" being treated as two different people), their medical history becomes fragmented. A doctor might prescribe a medication that conflicts with a previous treatment recorded under the "other" profile, or miss an allergy alert because it was logged inconsistently. Consistency is not just a database requirement; in health tech, it is a prerequisite for safe clinical decision-making.
