# BulkColorHistograms — FlowJo Workspace Plugin

A FlowJo v10 workspace plugin that lets you **bulk-recolor histogram stacks** in the Layout Editor. FlowJo only allows you to change one histogram's color at a time; this plugin adds a dialog where you can select any number of histograms across all your layouts and recolor them in one step.

---

## Features

- Works with **stacked, overlaid, and single histograms** in the Layout Editor
- Change **fill color and/or opacity** independently
- **Multi-round apply** — the dialog stays open so you can apply different colors to different groups before committing
- **Cancel** undoes all changes made in the current session
- Three **scope modes** control how broadly a color change is applied:
  - *All layouts & gates* — applies to every histogram showing the same sample and fluorophore, anywhere in the workspace
  - *Same layout only* — applies within the current layout only
  - *Selected only* — applies strictly to the histograms you checked
- Histograms are **grouped by layout** and displayed in a 3-column grid with per-stack select-all checkboxes
- Colors and opacity **persist across saves** — no need to re-apply after Cmd/Ctrl+S

---

## Requirements

| FlowJo | v10.x (tested on 10.10) |

## Installation

1. Download `BulkColorHistograms-1.1.jar` from the [Releases](../../releases) page.
2. Copy it into your FlowJo plugins folder
3. Open FlowJo and go to **Workspace tab → Plugins → Add Workspace Plugin**.

---


## Usage

1. Open a workspace (`.wsp`) that contains one or more layouts with histograms.
2. **Press Cmd+S / Ctrl+S** to save — this is what triggers the plugin dialog.
3. The **BulkColorHistograms** dialog opens showing all histograms grouped by layout.
4. Check the histograms you want to recolor. Use the **Select all** checkbox in each stack panel, or the global **Select/Deselect All** at the top.
5. Choose your **fill color** (click the color swatch) and/or **opacity** from the dropdown.
6. Set the **scope** using the Apply to dropdown:
   - *All layouts & gates* — recolors matching histograms everywhere
   - *Same layout only* — recolors within the same layout
   - *Selected only* — recolors exactly what you checked
7. Click **Apply**. The dialog stays open — repeat steps 4–6 for other groups if needed.
8. Click **Done** to commit all changes, or **Cancel** to undo everything.
9. Close and reopen the workspace file to see the updated colors — FlowJo caches its layout renders and a full reload is required.

> **Note:** The plugin re-applies your color choices on every subsequent save, so your colors won't be lost if you save multiple times.

---

## How It Works

FlowJo workspace files (`.wsp`) are XML. Each histogram stack in the Layout Editor is a `<ChartData>` element containing one `<DataLayer>` per sample, each with a `<LegendSpec>` that holds the color and fill:

```xml
<ChartData figID="..." offsetHistograms="1">
  <Graph type="Histogram">
    <Axis dimension="x" name="PE-A"/>
  </Graph>
  <PopModelList>
    <DataLayer path="Lymphocytes" sampleID="3">
      <LegendSpec color="#FF0000" chartFill="le.chartfill.tinted.40"/>
    </DataLayer>
  </PopModelList>
</ChartData>
```

The plugin implements `WorkspacePluginInterface` and intercepts the `save()` call. It:

1. Walks the workspace XML tree to collect all histogram `<ChartData>` blocks
2. On the first save, shows the selection dialog
3. On **Apply/Done**, updates the `color` and `chartFill` attributes on the relevant `<LegendSpec>` elements — both in FlowJo's live in-memory XML tree and by patching the `.wsp` file on disk after a short delay (to survive FlowJo's own save overwrite)
4. On subsequent saves, silently re-applies the stored color specs

---

## Opacity Values

The `chartFill` attribute uses FlowJo's internal string values:

| Display | XML value |
|---------|-----------|
| 0% (outline only) | `le.chartfill.none` |
| 20% | `le.chartfill.tinted.20` |
| 40% | `le.chartfill.tinted.40` |
| 60% | `le.chartfill.tinted.60` |
| 80% | `le.chartfill.tinted.80` |
| 100% (solid) | `le.chartfill.filled` |

---

## Project Structure

```
FlowJoBulkColorPlugin/
├── pom.xml
└── src/
    └── main/
        └── java/
            └── com/flowjo/plugin/bulkcolor/
                └── BulkColorHistograms.java
```

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Dialog doesn't appear | Make sure the plugin is registered: Workspace tab → Plugins → Add Workspace Plugin |
| No histograms shown in dialog | The workspace must have at least one Layout with histograms in it |
| Colors revert after reopen | Make sure you clicked **Done** (not Cancel) and saved again after the dialog closed |
| Colors visible only after reopen | This is expected — FlowJo caches layout renders; close and reopen the `.wsp` file |


---

## Contributing

Bug reports and pull requests are welcome. If you have a use case not covered — e.g. bulk-changing line weight, line style, or supporting dot plot colors — feel free to open an issue.

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Contact

For questions about the FlowJo SDK: [flowjo@bd.com](mailto:flowjo@bd.com)  
FlowJo plugin developer guide: https://docs.flowjo.com/flowjo/plugins-2/so-you-want-to-become-a-plugin-developer/
