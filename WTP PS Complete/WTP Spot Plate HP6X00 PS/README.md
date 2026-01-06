# WTP Spot Plate HP6X00 PS

## 🎯 Purpose

This workflow automates the creation of technical spot plates (White, Silver, or Varnish) for Pressure Sensitive labels on the HP 6000 series presses.

## 🛠 Logic Breakdown

- **Plate Name**: Define exactly what the spot plate is called (e.g., `Opaque White` or `Technical_1`).
- **Mapping Rules**:
  - _Rule 1_: If [Attribute], then create plate from [Inks].
  - _Rule 2_: Handle transparent vs. opaque substrate logic.
- **Pull-back/Choke**: Document any specific choke values (e.g., 0.1mm) used to prevent white ink peeking.

## 🚀 Automation Engine Nodes

1. **Create Spot Plate**: Uses [Insert specific Esko task name] to generate the separation.
2. **Ink Map**: Ensures the new plate is placed in the correct printing order.

[⬅️ Back to Master Menu](../../README.md)
