# 📐 NEW WTP S&R (Step & Repeat)

## 🎯 Workflow Overview

This workflow serves as the primary automation engine for WTP Pressure Sensitive (PS) labels. It includes automated cleanup, die-line verification, and press-specific marking logic.

---

# 📐 **NEW WTP S&R: Master Workflow Audit**

> **Internal Technical Archive** | _Belmark Prepress Automation_

---

## 🏁 **PHASE 1: INITIATION & SAFETY GATES**

_The entry point where we determine material type and check for "problem" dies._

1: **Base Workflow** / `Router` 🧭
a: Evaluates the name of the originating workflow to set material-specific flags. - 🔍 Checks if the SmartName `[wfp.Base Workflow Name]` contains the text **"Sandwich"**.

2: **sandwich distortion option** / `Modify Parameter` 🛠️
a: Adjusts internal variables to account for sandwich-style print scaling. - 📝 Modifies the **"Distort?"** parameter using the value provided by `<<wfp.Distort?>>`.

3: **checking die1** / `Router` 🛑
a: First gate in a multi-step check for specific physical die dimensions. - 🔢 Performs a numerical comparison on `[API WTP Die #1]` to check for value **30942**.

4: **checking die2** / `Router` 🛑
a: Second gate in the die validation logic to identify a problematic combination. - 🔢 Performs a numerical comparison on `[API WTP Die #2]` to check for value **2034**.

5: **stop** / `Wait for Action` ✋
a: Pauses the automation if the specific die combo from steps 3 and 4 is met. - 💬 Displays a custom message to the **Task Owner** instructing them to use the **MANUAL** workflow.

---

## 🎨 **PHASE 2: INK & LAYER PREPARATION**

_Standardizing separations and clearing out customer-specific technical layers._

6: **white ink check** / `Router` ⚪
a: Scans the file separations to identify if white ink is present. - 🧪 Routes based on **Separation Name** containing the strings **"HPI-White"** or **"WHT"**.

7: **background color check** / `Router` 🌈
a: Evaluates substrate type to determine if high-contrast marks are required for press sensors. - 🧬 Checks `[API WTP Background Color]` for matches containing **"Metallic"**, **"Clear"**, or **"white"**.

8: **eyemark wht** / `Modify Parameter` ✒️
a: Signals the marking engine to apply a white base to the registration marks. - ✅ Sets the internal workflow parameter **"eyemark wht"** to **Yes**.

9: **customer legend layer check** / `Router` 🏷️
a: Identifies if the file contains specific layers that require removal for CVS standards. - 📂 Scans for any **Layer Name** that contains the string **"Customer Legend"**.

10: **remove customer legend** / `Optimize PDF (Classic)` 🧹
a: Cleans up technical layers to prevent printing interference in the legend area. - ✂️ Isolates and removes the **"Customer Legend"** and **"Technical - Legend"** layers.

---

## 📏 **PHASE 3: SIZE VALIDATION & STEPPING**

_Final physical checks before the file is multiplied._

11: **max length check** / `Router` 📏
a: Performs a final physical dimension check against the press repeat limit. - 📏 Compares `[wfp.page size overall around]` to see if it is greater than **38.58**.

12: **over max around** / `Wait for Action` ⚠️
a: Triggers a notification when the calculated layout exceeds press capabilities. - 📧 Displays message: **"OVERALL WIDTH TOO LARGE FOR MARKS"**.

13: **remap dieline and techinfo** / `Optimize PDF` ⚙️
a: Standardizes separation names for downstream automation. - 🔄 Maps **"dieline"** ➡️ **"dieline"** | **"techinfo"** ➡️ **"techinfo"** | **"invisible ink"** ➡️ **"HPI-White"**.

14: **dynamic stepping** / `Step & Repeat` 📑
a: Primary S&R Engine. - 🚀 Handles the bulk of single-label production stepping and gap calculation.

---

## 🏁 **PHASE 4: EXPORT & SUCCESS**

_Moving the final file to the press-ready directory._

15: **copy to stepped folder** / `Copy File` 🚚
a: Final file delivery to the production server. - 📂 Renames and moves file to: `[Job_URL]/Stepped/[File_wE].pdf`.

16: **OK** / `Terminal` ✅
a: Workflow Complete. - ✨ Signals a successful run and ends the task.

- **[Special Finish LILO](./Sub-Workflows/LILO.md)**: (Documentation Pending)
- **[WTP Domino S&R](./Sub-Workflows/Domino.md)**: (Documentation Pending)

test

## 📝 Recent Change Log
