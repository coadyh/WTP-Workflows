# 🎥 **WTP HP20K Film Complete**

> **Internal Technical Archive** | _Belmark Prepress Automation_

---

## 🏗️ **WORKFLOW SUMMARY**

This branch handles wide-format film substrate logic, specifically optimized for the HP Indigo 20000 series press.

---

## 📝 **FOLDER-SPECIFIC CHANGE LOG**

---

## 🏁 **HP20K SPECIFIC GATES**

1: **film distortion check** / `Router` 📐
a: Evaluates thermal shrinkage properties of film substrates. - 🔍 Checks if `[API WTP Substrate Category]` contains **"Film"**.

2: **20K repeat validation** / `Router` 📏
a: Ensures the frame length matches the 20K's fixed repeat cycle. - 🔢 Validates that `[wfp.page size overall around]` is within **1.0mm** of the target repeat.

3: **ink saturation check** / `Router` 🎨
a: High-coverage check to prevent ink adhesion issues on film. - 🧪 Scans for total area coverage (TAC) exceeding **250%**.

test
