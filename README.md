# ABCD Gaussian Beam Propagator

A browser-based tool for simulating Gaussian beam propagation through paraxial optical systems using the ABCD ray-transfer matrix (RTM) method, supporting elliptical/astigmatic beams and astigmatic lenses.

Open `abcd-beam-propagator.html` in any modern browser — no server or build step required.

## How it works

Every paraxial optical element can be described by a 2×2 matrix:

| Element | Matrix | Description |
|---------|--------|-------------|
| Free space of length *d* | ⎡1 &nbsp; *d*⎤<br>⎣0 &nbsp; 1⎦ | Propagation through a uniform medium |
| Thin lens of focal length *f* | ⎡1 &nbsp; 0⎤<br>⎣-1/*f* &nbsp; 1⎦ | Ideal thin lens (sign convention: converging = positive) |

For a Gaussian beam, the complex beam parameter *q* encodes the beam width *w*(*z*) and radius of curvature *R*(*z*):

```
1/q = 1/R - i·M²·λ / (π·w²)
```

where *M²* is the beam propagation factor (1 for an ideal Gaussian, >1 for real beams). After an ABCD matrix, *q* transforms as:

```
q' = (A·q + B) / (C·q + D)
```

The tool walks along the optical axis, composes the cumulative ABCD matrix from each sample point back to the input plane, and extracts *w*(*z*) and *R*(*z*) from the transformed *q*-parameter. The accumulated Guoy phase is computed as *φ*(*z*) = −Arg(*A*(*z*) + *B*(*z*)/*q*₀), where (*A*, *B*) are elements of the cumulative ABCD matrix and *q*₀ is the input *q*-parameter.

Beam waists are found exactly by interpolating the *q*-parameter between adjacent free-space samples — the waist position is where Re(1/*q*) = 0, solved to sub-sample precision.

### Elliptical / astigmatic beams

When the initial *w₀ᵧ* differs from *w₀ₓ* (or *R₀ᵧ* differs from *R₀ₓ*), the beam is elliptical. The tool propagates the x and y axes independently through the same optical system, producing separate *wₓ*(*z*), *wᵧ*(*z*) curves and radius panels.

### Astigmatic lenses

Each lens has independent focal lengths *fₓ* and *fᵧ* (shown when the beam is asymmetric or when any lens has *fₓ* ≠ *fᵧ*). Setting *fₓ* or *fᵧ* to 0 means infinite focal length (no optical power along that axis).

### Waist-tracking comparison

As a cross-check, the tool computes an independent propagation of the Gaussian beam using the standard waist-tracking formulas (waist position → free-space q → lens transformation → free-space q). This independent calculation is overlaid as dashed curves on both the width and radius panels, and its values appear in gray in the tooltip — any discrepancy with the solid ABCD curves indicates a bug or numerical issue.

## Usage

### Beam parameters
- **Wavelength** &mdash; in nanometres (e.g. 633 nm for HeNe)
- **Initial w₀ / w₀y** &mdash; beam half-widths for the x and y axes at z = 0 (mm); set w₀y = w₀ for a circular beam, or different values for an elliptical/astigmatic beam
- **M²** &mdash; beam propagation factor (1 = ideal Gaussian, >1 for real-world beams)
- **Initial R₀ / R₀y** &mdash; radius of curvature at z = 0 for each axis; set to 0 for a plane wavefront (&infin;)
- **Total length** &mdash; propagation distance to simulate (mm)
- **Samples** &mdash; number of points along z (higher = smoother plots)

### Lenses
- Click **+ Add Lens** to add a lens (up to 10)
- Each lens has **z**, **fₓ**, and **fᵧ** positions (fᵧ shown when beam is asymmetric or lens is astigmatic); set f = 0 for infinite focal length
- A red × button removes a lens (disabled when only one remains)
- All inputs auto-recalculate on change or Enter key

### Plot
- **Top panel**: beam widths *wₓ*(*z*) and *wᵧ*(*z*) (when asymmetric) as line curves with a filled envelope. Dashed overlay curves for waist-tracking (WT) comparison
- **Bottom panel**: radius of curvature *R*(*z*), inverse radius 1/*R*(*z*), or accumulated Guoy phase *φ*(*z*). Dashed WT overlay shown when enabled
- **Show R(z)** — show/hide the radius panel
- **Show 1/R(z)** — switch the bottom panel to inverse radius (default on)
- **Show φ(z)** — plot the accumulated Guoy phase shift (takes priority over R/1/R when checked)
- **Grid** — toggle background grid lines (default on)
- **Points** — toggle data-point markers on both panels (useful for seeing the before/after lens discontinuity; default off)
- Hover over the plot to see exact values at any z-position (wₓ, wᵧ, Rₓ, Rᵧ or 1/Rₓ, 1/Rᵧ, φ when enabled, plus WT comparison values in gray)
- **Drag horizontally** on the plot to zoom into a z-range (y-axes re-scale automatically)
- **Shift-click** to zoom out by 20 %
- **Double-click** to reset zoom to the full view
- Dashed green vertical lines and double-headed arrows mark lens positions
- Waist markers (× symbols) on both panels, coloured per axis

### Results table
The table below the plot shows *wₓ*, *wᵧ* (when asymmetric), *R*, and *φ* at the input plane, every active lens (before and after), detected beam waists, and the output plane. Waists are located to full precision by interpolating the *q*-parameter. When the beam is asymmetric, waist and output-plane columns show separate *wₓ*/*wᵧ* and *Rₓ*/*Rᵧ* values.

## Example

Default configuration (open the file to see it):
- HeNe laser (633 nm), M² = 1, initial waist 0.5 mm, plane wavefront
- Two converging lenses: fₓ = fᵧ = 25 mm at z = 100 mm, fₓ = fᵧ = 75 mm at z = 200 mm
- Total propagation distance 300 mm, 4096 samples

Try adjusting M² to see how real beams diverge faster, or set w₀y to a different value to explore astigmatic propagation.

## References

- A. E. Siegman, *Lasers* (University Science Books, 1986)
- H. Kogelnik & T. Li, "Laser Beams and Resonators," *Applied Optics* **5**(10), 1550–1567 (1966)
- ISO 11146, "Lasers and laser-related equipment — Test methods for laser beam widths, divergence angles and beam propagation ratios"
