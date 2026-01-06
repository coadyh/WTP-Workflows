# 📐 NEW WTP S&R (Step & Repeat)

## 🎯 Workflow Overview

This workflow serves as the primary automation engine for WTP Pressure Sensitive (PS) labels. It includes automated cleanup, die-line verification, and press-specific marking logic.

---

1: Base Workflow / Router
a: Evaluates the name of the originating workflow to set material-specific flags. - Checks if the SmartName [wfp.Base Workflow Name] contains the text "Sandwich".

2: sandwich distortion option / Modify Workflow Parameter Values
a: Adjusts internal variables to account for sandwich-style print scaling. - Modifies the "Distort?" parameter to the value provided by <<wfp.Distort?>>.

3: checking die1 / Router
a: First gate in a multi-step check for specific physical die dimensions. - Performs a numerical comparison on [API WTP Die #1] to check for value 30942.

4: checking die2 / Router
a: Second gate in the die validation logic to identify a problematic combination. - Performs a numerical comparison on [API WTP Die #2] to check for value 2034.

5: stop / Wait for Action (Checkpoint)
a: Pauses the automation if the specific die combo from steps 3 and 4 is met. - Displays a custom message to the Task Owner instructing them to use the MANUAL workflow.

6: white ink check / Router
a: Scans the file separations to identify if white ink is present for specialized marking. - Routes based on Separation Name containing the strings "HPI-White" or "WHT".

7: background color check / Router
a: Evaluates the substrate type to determine if high-contrast marks are required for press sensors. - Checks the SmartName [API WTP Background Color] for matches containing "Metallic", "Clear", or "white".

8: eyemark wht / Modify Workflow Parameter Values
a: Signals the marking engine to apply a white base to the registration marks. - Sets the internal workflow parameter "eyemark wht" to "Yes".

9: customer legend layer check / Router
a: Identifies if the file contains specific layers that require removal for certain customer standards. - Scans for any Layer Name that contains the string "Customer Legend".

10: remove customer legend / Optimize PDF Document (Classic)
a: Cleans up the technical layers to prevent printing interference in the legend area. - Utilizes a remapping logic to isolate and remove the "Customer Legend" and "Technical - Legend" layers.

11: max length check / Router
a: Performs a final physical dimension check to ensure the stepped layout fits the press repeat. - Compares the numerical value of [wfp.page size overall around] to see if it is greater than 38.58.

12: over max around / Wait for Action (Checkpoint)
a: Triggers a notification when the calculated layout exceeds press capabilities. - Displays a message to the Task Owner: "OVERALL WIDTH TOO LARGE FOR MARKS".

13: 1. Send E-mail / Send E-mail
a: Automated alert sent to the operator when step and repeat data is missing from the job. - Includes a custom message instructing the user to check the graphics menu and update repeat data.

14: remap dieline and techinfo / Optimize PDF
a: Standardizes separation names and removes unnecessary technical layers. - Remaps separations: "dieline" to "dieline", "techinfo" to "techinfo", and "invisible ink" to "HPI-White". - Specifically removes layers named "Legend" and "Technical - Legend".

15: Special Finish LILO / Sub-Workflow
a: Internal placeholder for specialty varnish or holographic embellishment logic.

16: domino check / Router
a: Determines if the job is slated for the Domino digital press instead of the HP.

17: WTP Domino S&R / Sub-Workflow
a: Branch for Domino-specific stepping and mark requirements.

18: Reorder Inks / Reorder Inks
a: Ensures ink stack order is correct for the press RIP. - Utilizes the "WTP/wtp reorder inks" Action List.

19: sheet check / Router
a: Evaluates if the job is a sheet-fed vs. roll-fed layout. - Checks the SmartName [DNS_Sheet_WTP] for numerical values.

20: Combo Set Check / Router
a: Initial gate to identify multi-label combined runs. - Evaluates SmartName [API WTP Combined Set Check] for "SET" or "Combo Set".

21: Combined Set Check / Router
a: Secondary validation for combined set logic.

22: combined set stepping / Step & Repeat
a: Applies specialized stepping logic for ganged/combined labels.

23: dynamic stepping / Step & Repeat
a: The main stepping engine for standard single-label production runs.

24: HPI-White? / Router
a: Final check for white ink remapping before export. - If "HPI-White" is present, it routes to the remapping node.

25: map wht-plate as WHT / Optimize PDF
a: Final separation cleanup to ensure the white plate is named correctly for the HP DFE. - Remaps "HPI-White" specifically to "WHT".

26: distortion / Router
a: Determines if the "Distort?" flag set at the start requires a specific scaling value.

27: checking width across / Router
a: Validates that the total stepped width does not exceed the web width. - Checks if the Trim Box Width (converted to inches) is less than 12.592.

28: HENKEL BEAUTY / Router
a: Customer-specific logic gate for Henkel brand standards. - Checks [API WTP Customer Name] for "HENKEL BEAUTY".

29: dual finish? / Router
a: Checks for jobs requiring two different finish types (e.g., Matte and Gloss). - Evaluates [API WTP Feature List] for value "10" (Dual Finish).

30: Stepped File TOO wide / Wait for Action (Checkpoint)
a: Final fail-safe for physical width violations. - Displays detailed breakdown of size, including gap and marks, for operator review.

31: copy to stepped folder / Copy File
a: Moves the finished, stepped PDF to the final production directory. - Renames file to [Job_URL]/Stepped/[File_wE].pdf.

32: OK / Terminal Node
a: Final success status for the workflow.

- **[Special Finish LILO](./Sub-Workflows/LILO.md)**: (Documentation Pending)
- **[WTP Domino S&R](./Sub-Workflows/Domino.md)**: (Documentation Pending)
