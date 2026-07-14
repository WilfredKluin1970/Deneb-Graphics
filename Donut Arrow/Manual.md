# Deneb Guide - Custom Donut Chart

This guide explains how to build a donut/progress visual in Deneb with a dynamic dot (endpoint marker). .

## What You Get

- `0%` starts at `12:00`
- Progress moves clockwise
- The dot moves dynamically with the value
- Center label (percentage)
- Configurable font (default: `Tahoma`)
- Centrally configurable colors via signals

## Code
get code from file in this repoitory

## Usage in Power BI + Deneb
1. Add a Deneb visual to your page.
2. Choose **Vega** (not Vega-Lite).
3. Add only the `Intercompany Coverage Ratio %` measure to Values.
4. Paste the full code below.

## Full Vega Code



## Where to Change What

### 1) Colors

Update these signals:

- `colorRing`: ring color
- `colorDot`: dot color
- `colorText`: text color
- `colorDotStroke`: dot border color

### 2) Font

Update this signal:

- `fontFamily`: for example `Tahoma`, `Segoe UI`, `Arial`

### 3) Ring Thickness

Ring thickness is `rOuter - rInner`.

- Thinner: lower `rOuter` or increase `rInner`
- Thicker: increase `rOuter` or lower `rInner`

### 4) Dot Size

Adjust this line:

- `size`: `pow(thickness * 2.5, 2)`

To make it larger: increase `2.5`.
To make it smaller: decrease `2.5`.

## Troubleshooting

- Error about `mark/layer/hconcat/...`:
  - You are likely using Vega-Lite instead of Vega.
  - In Deneb, explicitly choose the **Vega** template.

- Value does not respond to slicers:
  - Verify the measure in Values is `Intercompany Coverage Ratio %`.

- Value appears 100x too large/small:
  - The code supports both `0-1` and `0-100` input via:
    - `rawValue > 1 ? rawValue / 100 : rawValue`
