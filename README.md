# Implement Client Script & UI Policy (Incident)

## Project Overview

This project demonstrates the implementation of UI Policies and Client Scripts on the ServiceNow Incident table to enforce conditional field behavior, automate field values, validate Incident submissions, and control list-based field editing.

The implementation is designed to improve data integrity and consistency during Incident creation and updating.

## Problem Statement

Incident records often require consistent and accurate data entry for effective triage, routing, and resolution. Relying only on user awareness and manual checks can result in incomplete, inconsistent, or incorrect information.

This project addresses this by implementing conditional field behavior and validation directly at the user interface level using ServiceNow UI Policies and Client Scripts.

## Objective

The objective of this project is to demonstrate how ServiceNow client-side controls can be used to enforce data integrity on Incident records.

The implementation demonstrates how UI Policies and Client Scripts can:

- Make fields mandatory based on conditions
- Make fields read-only based on conditions
- Automatically populate field values
- Prevent invalid Incident submission
- Control State changes through list editing
- Allow valid updates through the Incident form
- Maintain consistent Incident data

## Platform

- ServiceNow
- Incident Management
- UI Policies
- UI Policy Actions
- Client Scripts
- Form Validation

---

# Project Features

The project implements the following features:

1. High Impact UI Policy
2. Assignment Group mandatory enforcement
3. Urgency read-only control
4. Automatic Urgency update
5. Assigned To validation during submission
6. State list-edit restriction
7. Reverse UI Policy behavior
8. Form-based State update
9. Functional testing of all configured behaviors

---

# Task 1: Create UI Policy on Incident

## UI Policy

**Name:** High Impact Control

### Configuration

- Table: Incident
- Active: True
- Condition: Impact is 1 - High
- Reverse if false: True

### UI Policy Action

The UI Policy includes an action that makes the **Assignment Group** field mandatory when the Incident Impact is High.

### Expected Behavior

When:

**Impact = 1 - High**

the Assignment Group field becomes mandatory.

When the condition is no longer true, the UI Policy behavior is reversed.

---

# Task 2: Create UI Policy Action – Urgency

## UI Policy Action

The **High Impact Control** UI Policy contains an action for the Urgency field.

### Configuration

- Field: Urgency
- Read-only: True
- Visible: Unchanged

### Expected Behavior

When:

**Impact = 1 - High**

the Urgency field becomes read-only.

When Impact is changed to another value, the read-only behavior is reversed.

---

# Task 3: Create onChange Client Script

## Client Script

**Name:** Auto set urgency for high impact

### Configuration

- Table: Incident
- Type: onChange
- Field: Impact
- Active: True

### Function

When the Impact field is changed to High, the script automatically sets Urgency to High.

### Expected Behavior

**Impact = High**

→ **Urgency = High**

The script also displays an informational message confirming that Urgency was set to High.

### Script

```javascript
function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    if (newValue == '1') {
        g_form.setValue('urgency', '1');
        g_form.addInfoMessage(
            'Urgency set to High for high impact incident.'
        );
    }
}
