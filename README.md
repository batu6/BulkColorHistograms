# BulkColorHistograms — FlowJo Workspace Plugin

A FlowJo v10 workspace plugin that lets you **bulk-recolor histogram stacks** in the Layout Editor. FlowJo only allows you to change one histogram's color at a time; this plugin adds a dialog where you can select any number of histograms across all your layouts and recolor them in one step.

This has been a dream plugin for me. I've always wanted to have a tool that would allow me to change the colors easily. However, unfortunately my knowledge in R shiny was not much helpful in development of this plugin. In the end, I was able to realize this project with the Claude AI. I hope this plugin to be helpful for the scientist using FlowJo. -BA-

---

## Requirements

- **FlowJo** v10.x (tested on 10.10)

---

## Installation

1. Download the latest release from the [Releases](../../releases) page.
2. Copy it into your FlowJo plugins folder:
   - **macOS**: `/Applications/plugins/` (create it if it doesn't exist)
   - **Windows**: `C:\Program Files\FlowJo_v10\plugins\`
3. Open FlowJo and go to **Workspace tab → Plugins → Add Workspace Plugin**. You should see the plugin there.

---

## Usage

0. Ideally use it after you organized your data for one channel of interest.
1. After loading the plugin, save the file to trigger the plugin dialog.
2. The **BulkColorHistograms** dialog opens showing all histogram stacks grouped by layout.
3. Set your scope options

   **Apply to** — controls how broadly a single checkbox selection expands:
   - *Selected samples only* — recolors exactly what you checked, no expansion
   - *Same sample + fluorophore + gate* — also checks matching histograms with the same sample, channel, and gate (good for congenically marked samples)
   - *Same sample + fluorophore* — also checks matching histograms with the same sample and channel across any gate
  
   - Rowwise — Allows selection of samples belonging to same row in different histograms.

   **Layout scope** — controls whether matching extends across layouts or stays within the same one:
   - *Within same layout*
   - *Across all layouts*

4. Check the histograms you want to recolor. 
5. Choose a **fill color** (click the color swatch) and/or **opacity** from the dropdown.
6. Click **Apply**. The dialog stays open — repeat for other groups if needed.
7. Click **Done** to commit all changes, or **Cancel** to undo everything.
8. **Close and reopen** the `.wsp` file to see the updated colors in your layouts.

---

## Features

- Stress free, efficient coloring of histograms.
- Stacked, overlaid, and single histograms are all supported
- Histograms are grouped by layout in a 3-column grid with per-stack select-all checkboxes
- Stack labels show the antibody name alongside the fluorophore (e.g. `CD4 - FITC-A`).
- Each sample row shows a **color swatch** and **opacity %** that update live after Apply
- Sample name and gate path are shown on separate lines for easy reading
- **Multi-round apply** — apply different colors to different groups before committing.
- All scope and filter preferences are **saved between sessions**

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Dialog doesn't appear | Make sure the plugin is registered: Workspace tab → Plugins → Add Workspace Plugin |
| No histograms shown | The workspace must have at least one Layout with histograms in it |
| Colors revert after reopening | Make sure you clicked **Done** (not Cancel) and saved again after the dialog |
| Colors only visible after reopen | Expected — FlowJo caches layout renders; close and reopen the `.wsp` to refresh |

---

## Contributing

Feel free to open an issue if you find a bug or have a feature request.

---
