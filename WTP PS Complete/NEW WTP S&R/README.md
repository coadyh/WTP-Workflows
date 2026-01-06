# 📐 NEW WTP S&R (Step & Repeat)

## 🎯 Workflow Overview

This workflow handles the primary stepping logic for WTP labels, including specific checks for sandwich distortions and existing die-line configurations.

## 🚦 Decision Logic (Routers)

| Router Node                  | Logic / Criteria                                       | Result                                                        |
| :--------------------------- | :----------------------------------------------------- | :------------------------------------------------------------ |
| **Sandwich Distortion**      | Checks if the "Sandwich Distortion" option is enabled. | Routes to specialized distortion scaling if TRUE.             |
| **Checking Die1 / Die2**     | Verifies die-line presence and naming conventions.     | Ensures the stepping engine has a valid path to follow.       |
| **File contains HPI-White?** | Scans ink list for High-Opacity White.                 | Triggers specific white ink under-print logic for HP presses. |

## 🛠️ Key Technical Notes

> **⚠️ Patch Note:** As of 12/31/20, a "temp solution" sticky note is active regarding a **weird combo die issue**. This should be reviewed before making permanent changes to the 'checking die' router logic.

## 🖼️ Workflow Map

![WTP S&R Map](./Assets/workflow-screenshot.png)

[⬅️ Back to Master Menu](../../README.md)
