# Deneb Guide - Intercompany Coverage Ratio %

This guide explains how to build a donut/progress visual in Deneb with a dynamic dot (endpoint marker), based on the measure **Intercompany Coverage Ratio %**.

## What You Get

- `0%` starts at `12:00`
- Progress moves clockwise
- The dot moves dynamically with the value
- Center label (percentage)
- Configurable font (default: `Tahoma`)
- Centrally configurable colors via signals

## Usage in Power BI + Deneb

1. Add a Deneb visual to your page.
2. Choose **Vega** (not Vega-Lite).
3. Add only the `Intercompany Coverage Ratio %` measure to Values.
4. Paste the full code below.

## Full Vega Code

```json
{
  "$schema": "https://vega.github.io/schema/vega/v5.json",
  "autosize": { "type": "fit", "contains": "padding" },
  "padding": 16,
  "data": [{ "name": "dataset" }],
  "signals": [
    {
      "name": "rawValue",
      "update": "length(data('dataset')) ? data('dataset')[0]['Intercompany Coverage Ratio %'] : 0"
    },
    {
      "name": "pct",
      "update": "clamp(rawValue > 1 ? rawValue / 100 : rawValue, 0, 1)"
    },

    { "name": "cx", "update": "width / 2" },
    { "name": "cy", "update": "height / 2" },
    { "name": "size", "update": "min(width, height)" },

    { "name": "rOuter", "update": "size * 0.38" },
    { "name": "rInner", "update": "size * 0.33" },
    { "name": "rMid", "update": "(rOuter + rInner) / 2" },
    { "name": "thickness", "update": "rOuter - rInner" },

    { "name": "startAngle", "value": 0 },
    { "name": "endAngle", "update": "startAngle + (2 * PI * pct)" },

    { "name": "colorRing", "value": "#1f70c1" },
    { "name": "colorDot", "value": "#1f70c1" },
    { "name": "colorText", "value": "#222222" },
    { "name": "colorDotStroke", "value": "#e6e6e6" },

    { "name": "fontFamily", "value": "Tahoma" }
  ],
  "marks": [
    {
      "type": "arc",
      "encode": {
        "update": {
          "x": { "signal": "cx" },
          "y": { "signal": "cy" },
          "startAngle": { "signal": "startAngle" },
          "endAngle": { "signal": "endAngle" },
          "innerRadius": { "signal": "rInner" },
          "outerRadius": { "signal": "rOuter" },
          "fill": { "signal": "colorRing" },
          "cornerRadius": { "signal": "thickness / 2" }
        }
      }
    },
    {
      "type": "symbol",
      "encode": {
        "update": {
          "x": { "signal": "cx + rMid * sin(endAngle)" },
          "y": { "signal": "cy - rMid * cos(endAngle)" },
          "shape": { "value": "circle" },
          "size": { "signal": "pow(thickness * 2.5, 2)" },
          "fill": { "signal": "colorDot" },
          "stroke": { "signal": "colorDotStroke" },
          "strokeWidth": { "value": 1 }
        }
      }
    },
    {
      "type": "text",
      "encode": {
        "update": {
          "x": { "signal": "cx" },
          "y": { "signal": "cy" },
          "align": { "value": "center" },
          "baseline": { "value": "middle" },
          "font": { "signal": "fontFamily" },
          "fontSize": { "signal": "size * 0.16" },
          "fill": { "signal": "colorText" },
          "text": { "signal": "format(pct, '.0%')" }
        }
      }
    }
  ]
}
```

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
