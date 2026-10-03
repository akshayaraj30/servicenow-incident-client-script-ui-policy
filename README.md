# 🚀 Naann Mudhalvan

# PROJECT: IMPLEMENT CLIENT SCRIPT & UI POLICY

### *Powered by ServiceNow Incident Management*

![ServiceNow](https://img.shields.io/badge/Platform-ServiceNow-107C41?style=for-the-badge&logo=servicenow&logoColor=white)
![Client Script](https://img.shields.io/badge/Configuration-Client%20Script-FF6F00?style=for-the-badge)
![UI Policy](https://img.shields.io/badge/Configuration-UI%20Policy-0078D4?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

---

## 📌 Project Metadata

| **Attribute** | **Details** |
|---|---|
| **Project Title** | Implement Client Script & UI Policy (Incident) |
| **Course / Module** | ServiceNow System Administrator |
| **Platform** | ServiceNow |
| **Application** | Incident Management |
| **Table** | Incident |
| **Team Lead** | 👩‍💻 **Akshaya B** |
| **Team Member** | Madhusri S T |

---

## 🎯 1. Project Overview & Objective

### The Challenge

Incident management requires proper validation and control of incident information. Without appropriate client-side controls, users may enter incomplete information, modify important fields incorrectly, or make unwanted changes directly from list views.

### The Solution

This project implements **ServiceNow UI Policies and Client Scripts** to dynamically control Incident form behavior.

The solution provides:

- Conditional mandatory fields
- Read-only field control
- Automatic field updates
- Save-time validation
- List-edit restrictions
- Improved data integrity and user experience

---

## ✨ 2. Key Features & Functionality

- 🔐 **Conditional Mandatory Fields**  
  Assignment Group becomes mandatory when Impact is set to High.

- 🔒 **Read-Only Field Control**  
  Urgency becomes read-only for High Impact Incidents.

- ⚡ **Automatic Field Update**  
  Urgency is automatically set to High when Impact is changed to High.

- ✅ **Save-Time Validation**  
  High Impact Incidents cannot be saved when Assigned To is empty.

- 🚫 **List Edit Restriction**  
  State changes through direct list editing are blocked.

- 📝 **Form-Based State Update**  
  Users can update Incident State through the Incident form.

- 🧪 **Functional Testing**  
  All implemented configurations were tested using multiple validation scenarios.

---

## 🛠️ 3. Technology Stack

- 🟢 **Platform:** `ServiceNow`
- 🔵 **Application:** `Incident Management`
- 🟠 **Table:** `Incident`
- 🟣 **Configuration:** `UI Policy`
- 🟡 **Validation & Automation:** `Client Scripts`
- 🔹 **Client Script Types:** `onChange`, `onSubmit`, `onCellEdit`

---

# ⚙️ 4. Implementation

## 🔹 Task 1 – High Impact Control UI Policy

A UI Policy named **High Impact Control** was created on the Incident table.

### Configuration

| Property | Value |
|---|---|
| Name | High Impact Control |
| Table | Incident |
| Active | True |
| Condition | Impact is 1 - High |
| Reverse if False | True |

### Functionality

When:

`Impact = High`

the **Assignment Group** field becomes mandatory.

When the Impact condition is no longer true, the policy is reversed.

---

## 🔹 Task 2 – Urgency UI Policy Action

A UI Policy Action was created under **High Impact Control**.

### Configuration

| Property | Value |
|---|---|
| Field | Urgency |
| Read-only | True |
| Visible | Unchanged |

### Functionality

When Impact is High:

`Urgency → Read Only`

When Impact changes to another value:

`Urgency → Editable`

---

## 🔹 Task 3 – Auto Set Urgency Using onChange Client Script

An **onChange Client Script** was created for the Impact field.

### Configuration

| Property | Value |
|---|---|
| Name | Auto set urgency for high impact |
| Table | Incident |
| Type | onChange |
| Field | Impact |
| Active | True |

### Script

```javascript
function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    if (newValue == '1') {
        g_form.setValue('urgency', '1');
        g_form.addInfoMessage(
            'Urgency set to High for High impact incident.'
        );
    }
}
