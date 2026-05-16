# ABCD Gaussian Beam Propagator

A browser-based tool for simulating Gaussian beam propagation through paraxial optical systems using the ABCD ray-transfer matrix (RTM) method.

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

Beam waists are found exactly by interpolating the *q*-parameter between adjacent free-space samples — the waist position is where Re(1/*q*) = 0.

## Usage

### Beam parameters
- **Wavelength** &mdash; in nanometres (e.g. 633 nm for HeNe)
- **Initial w₀ / w₀y** &mdash; beam half-widths for the x and y axes at z = 0 (mm); set w₀y = w₀ for a circular beam, or different values for an elliptical/astigmatic beam
- **M²** &mdash; beam propagation factor (1 = ideal Gaussian, >1 for real-world beams)
- **Initial R₀** &mdash; radius of curvature at z = 0; set to 0 for a plane wavefront (&infin;)
- **Total length** &mdash; propagation distance to simulate (mm)
- **Samples** &mdash; number of points along z (higher = smoother plots)

### Lenses
- Click **+ Add Lens** to add a lens (up to 10)
- Each lens has position, focal length (positive = converging), and an On/Off toggle
- A red × button removes a lens (disabled when only one remains)
- Press **Enter** in any input or click **Recalculate** to update

### Plot
- **Top panel**: beam widths *wₓ*(*z*) and *wᵧ*(*z*) (when asymmetric) as line curves with a filled envelope
- **Bottom panel**: radius of curvature *R*(*z*), inverse radius 1/*R*(*z*), or accumulated Guoy phase *φ*(*z*)
- **Show R(z)** — show/hide the radius panel
- **Show 1/R(z)** — switch the bottom panel to inverse radius
- **Show φ(z)** — plot the accumulated Guoy phase shift (takes priority over R/1/R when checked)
- **Grid** — toggle background grid lines
- **Points** — toggle data-point markers on both panels (useful for seeing the before/after lens discontinuity)
- Hover over the plot to see exact values at any z-position (both wₓ and wᵧ when asymmetric)
- **Drag horizontally** on the plot to zoom into a z-range (y-axes re-scale automatically)
- **Shift-click** to zoom out by 20 %
- **Double-click** to reset zoom to the full view

### Results table
The table below the plot shows *wₓ*, *wᵧ* (when asymmetric), *R*, and *φ* at the input plane, every active lens (before and after), detected beam waists, and the output plane. Waists are located to full precision by interpolating the *q*-parameter.

## Example

Default configuration (open the file to see it):
- HeNe laser (633 nm), M² = 1, initial waist 0.5 mm, plane wavefront
- Two converging lenses: f = 25 mm at z = 100 mm, f = 75 mm at z = 200 mm
- Total propagation distance 300 mm

Try adjusting M² to see how real beams diverge faster, or set w₀y to a different value to explore astigmatic propagation.

## References

- A. E. Siegman, *Lasers* (University Science Books, 1986)
- H. Kogelnik & T. Li, "Laser Beams and Resonators," *Applied Optics* **5**(10), 1550–1567 (1966)
- ISO 11146, "Lasers and laser-related equipment — Test methods for laser beam widths, divergence angles and beam propagation ratios"
