# ABCD Gaussian Beam Propagator

A browser-based tool for simulating Gaussian beam propagation through paraxial optical systems using the ABCD ray-transfer matrix (RTM) method.

Open `abcd-beam-propagator.html` in any modern browser — no server or build step required.

## How it works

Every paraxial optical element can be described by a 2×2 matrix:

| Element | Matrix | Description |
|---------|--------|-------------|
| Free space of length *d* | ⎡1 &nbsp; *d*⎤<br>⎣0 &nbsp; 1⎦ | Propagation through a uniform medium |
| Thin lens of focal length *f* | ⎡1 &nbsp; 0⎤<br>⎣-1/*f* &nbsp; 1⎦ | Ideal thin lens (sign convention: converging = positive) |

For a Gaussian beam, the complex beam parameter *q* = *z* + i*z*<sub>R</sub> encodes both the beam width *w*(*z*) and radius of curvature *R*(*z*):

```
1/q = 1/R - i·λ / (π·w²)
```

After an ABCD matrix, *q* transforms as:

```
q' = (A·q + B) / (C·q + D)
```

The tool walks along the optical axis, composes the cumulative ABCD matrix from each sample point back to the input plane, and extracts *w*(*z*) and *R*(*z*) from the transformed *q*-parameter.

## Usage

### Beam parameters
- **Wavelength** &mdash; in nanometres (e.g. 633 nm for HeNe)
- **Initial width w₀** &mdash; beam half-width at z = 0 (mm)
- **Initial R₀** &mdash; radius of curvature at z = 0; set to 0 for a plane wavefront (&infin;)
- **Total length** &mdash; propagation distance to simulate (mm)
- **Samples** &mdash; number of points along z (higher = smoother plots)

### Lenses
- Click **+ Add Lens** to add a lens (up to 10)
- Each lens has position, focal length (positive = converging), and an On/Off toggle
- A red × button removes a lens (disabled when only one remains)
- Press **Enter** in any input or click **Recalculate** to update

### Plot
- **Top panel**: beam width *w*(*z*) as a filled envelope
- **Bottom panel**: radius of curvature *R*(*z*) (toggleable to 1/*R*(*z*))
- **Show R(z)** — show/hide the bottom panel
- **Show 1/R(z)** — switch the bottom panel to inverse radius
- **Grid** — toggle background grid lines
- Hover over the plot to see exact values at any z-position

### Results table
The table below the plot shows *w* and *R* at the input plane, every active lens, and the output plane.

## Example

Default configuration (open the file to see it):
- HeNe laser (633 nm), initial waist 0.5 mm, plane wavefront
- One converging lens (*f* = 60 mm) at z = 80 mm
- Total propagation distance 300 mm

Try adding a second lens, changing signs to explore diverging lenses, or toggling 1/*R* near a beam waist.

## References

- A. E. Siegman, *Lasers* (University Science Books, 1986)
- H. Kogelnik & T. Li, "Laser Beams and Resonators," *Applied Optics* **5**(10), 1550–1567 (1966)
