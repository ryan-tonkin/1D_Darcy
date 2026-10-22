# Faults & fluid flow

A browser teaching lab adapted from `darcy_serial.py` and `darcy_parallel_mass.py`.
Students need only a modern browser. There are no accounts, package installs,
external libraries, fonts, network requests, or server-side calculations.

## Open or distribute

Open **`docs/index.html`** by double-clicking it. It works directly from disk,
including without an internet connection. Share that single file by USB, email,
or a learning platform. It contains the complete application.

The same single file can be hosted on GitHub Pages and distributed directly.
There is no download control in the student interface.

## Publish on GitHub Pages

1. Create a GitHub repository and upload the files, keeping `docs/index.html`
   inside the `docs` folder. A public repository supports Pages on GitHub Free.
2. Open the repository's **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select the branch containing the files (usually `main`) and the **`/docs`**
   folder, then **Save**.
5. Once GitHub finishes publishing, share the website URL shown on that page.

No build command, Python hosting, GitHub Actions configuration, or API key is
needed. See [GitHub's publishing-source instructions](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).

## Teaching controls

- **Across the fault:** two equal rock lengths and a fault in series. Flow is
  the same in every unit; hydraulic resistances add.
- **Along the fault:** two rock paths and a fault in parallel. Pressure drop is
  the same in every path; hydraulic conductances add. Fault thickness partitions
  a fixed 200 m × 200 m cross-section. The diagram reports permeability, mass
  flow, and area for each path separately.
- Hold **pressure drop** or **mass flow** fixed in either arrangement. Both
  arrangements start with 1 mD rock and 1 mD fault permeability. Across the fault
  starts with 10 bar pressure drop; along the fault starts with fixed mass flow.
  Fixed mass flow defaults to 1 kg/s, with a range of 0.01–20 kg/s in both
  arrangements. Each arrangement retains its inputs when switching views;
  Reset restores that arrangement's defaults.
- Adjust the sliders or type exact input values. Permeability sliders span
  0.001–1,000 mD logarithmically; number fields show permeability directly in mD.
- Click inside the sensitivity plot to select a fault permeability. The slider
  provides the same control by keyboard. Plot axes automatically rescale.
- Read **Behind the model** for equations, constants, and assumptions.

The pressure-versus-distance plot has two scale options. **Auto** follows the
current inlet pressure continuously, without snapping to rounded axis bounds.
**Fixed range** covers the full selected input range (1–100 bar, or the pressure
needed for up to 20 kg/s at the current geometry and permeabilities), so its
vertical axis stays steady while the pressure or mass-flow input changes.
Slider redraws are limited to one per animation frame to keep interaction smooth.

The right-hand permeability plot also has **Auto** and **Fixed range** vertical
scaling. Auto fits the current curves. Fixed range covers the full pressure or
mass-flow input range at the current geometry and rock permeability, keeping
the axis steady as that input changes. Its horizontal axis always spans
0.001–1,000 mD logarithmically.

For flow across the fault, the sensitivity plot changes only fault permeability;
all other inputs stay fixed. At fixed pressure, the rock and fault curves are
limiting flows through each resistance alone, rather than additive flows. At
fixed mass flow, the serial pressure contributions add.

For flow along the fault at **fixed pressure**, the right-hand plot shows total
mass flow versus fault permeability, with separate rock and fault flow
contributions that add to the total.

At **fixed mass flow**, it shows combined-path, rock-only, and fault-only
reference pressures, matching `darcy_parallel_mass.py`. The reference cases use
the existing partitioned areas and carry the **entire total flow** through only
the selected paths. These pressure curves do not add. The diagram always shows
the actual shared pressure and the actual division of flow among all three paths.

The page uses the original model constants: density
1,000 kg/m³, viscosity 5.5e-4 Pa·s, and 1 mD = 9.869233e-16 m². Pressure is
relative to a zero-pressure outlet. It is a steady, single-phase, 1D Darcy
model with constant fluid properties, without gravity or transient storage.
Very thin layers are enlarged in the geometry sketch only; pressure-profile
distances and calculations use the exact input dimensions.

## Development and verification

Edit `docs/index.html`; all CSS, SVG rendering, and JavaScript are inline so
the distributed file remains self-contained. The original Python files are
retained as supplied. Students do not need Python or Node.js.

For a maintainer with Python, NumPy, and Node.js installed:

```sh
python3 tests/check_models.py
```

The check compares the browser solver with the actual calculation functions
extracted from both supplied Python files, including pressure nodes, forward
and inverse operation, boundary values, and deterministic random inputs.
It also checks conservation, monotonicity, and sensitivity-curve consistency,
including the fixed-mass reference-curve expressions extracted from
`darcy_parallel_mass.py`, fixed-pressure flow contributions, and stable bounds
for the fixed vertical scale.
