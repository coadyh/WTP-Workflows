# 📐 NEW WTP S&R (Step & Repeat)

## 🎯 Workflow Overview

This workflow serves as the primary automation engine for WTP Pressure Sensitive (PS) labels. It includes automated cleanup, die-line verification, and press-specific marking logic.

---

## 🧹 Initial Cleanup: "Side Mission" Branch

To ensure a clean production environment, the workflow launches a parallel, non-blocking utility branch at the `Start` node.

- **Task**: `Select Old Stepped File` -> `Delete File`.
- **Logic**: Uses a SmartName based on the current label number to search the output folder for an existing PDF.
- **Purpose**: Deletes any pre-existing stepped file to prevent "overwriting errors" or duplicate file naming at the end of the run.
- **Behavior**: This path is independent; the main workflow continues regardless of whether an old file is found.

---

## 🚦 Die Verification & Manual Gates

The workflow utilizes a tiered checking system to identify jobs that are too complex for standard automation.

### 1. Die Number Validation (SmartName API)

The logic pulls die data from an earlier API call converted into SmartNames: `[API WTP Die #1]` and `[API WTP Die #2]`.

| Node Name         | Logic / Condition   | Result                   |
| :---------------- | :------------------ | :----------------------- |
| **checking die1** | Is equal to `30942` | Proceeds to check Die 2. |
| **checking die2** | Is equal to `2034`  | **Trigger Manual Stop.** |

### 🛑 Manual Stop: Unique Die Combo

If the **30942 / 2034** combination is detected, the workflow triggers a `Wait for Interaction` (TODO) node.

- **Message**: _"This job will need to be stepped using WTP PS Complete MANUAL workflow."_
- **Reasoning**: This specific die pair requires manual layout handling that the automated engine cannot currently process.

---

## 🛠️ File Optimization & Layer Naming

- **Node**: `Optimize PDF` (Orange Gear).
- **Logic**: Uses wildcards to ensure technical separations are renamed exactly to `dieline` and `techinfo`.
- **Reliability**: The task is configured not to fail the workflow if a match isn't found, allowing the file to proceed with its original naming if necessary.

---

## 🎨 Ink & Marking Logic

After file optimization, the workflow handles registration marks based on substrate and ink requirements.

- **background color**: Scans for high-density backgrounds that may interfere with sensor tracking.
- **eyemark wht**: Automatically applied if the `file contains HPI-White?` router is positive.
  - _Purpose_: Ensures HP press sensors can maintain registration on clear or metallic materials.

---

## 🔗 Sub-Workflow Jump Points

The following complex logic blocks are handled in nested workflows:

- **[Special Finish LILO](./Sub-Workflows/LILO.md)**: (Documentation Pending)
- **[WTP Domino S&R](./Sub-Workflows/Domino.md)**: (Documentation Pending)
