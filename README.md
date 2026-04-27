# Single-Layer Protective-Coating Reflectivity Calculator

A self-contained HTML calculator for estimating the reflectivity of a single thin-film coating on common optical substrates.

The calculator is in [`bbo_p_coating_reflectivity.html`](./bbo_p_coating_reflectivity.html). It runs fully in the browser and does not require a build step, server, or external JavaScript dependencies.

## Usage

Open `bbo_p_coating_reflectivity.html` directly in a browser.

The main inputs are:

- **Center wavelength**: sets the quarter-wave coating thickness.
- **Start / end wavelength**: sets the plotted wavelength range.
- **Incidence angle**: angle in the ambient medium.
- **Ambient index**: usually air, default `1.0003`.
- **Coating material**: selects the single-layer film material.
- **Substrate material**: selects the optical substrate.
- **Substrate index / optical axis**: selects the isotropic index or a principal crystal index.
- **Surface Fresnel polarization**: choose `p`, `s`, or unpolarized average.

Hover over the plot to read the wavelength, coated reflectivity, and uncoated reflectivity at the cursor position.

## Supported Materials

Substrates:

- BBO
- Fused silica
- KTA
- Lithium niobate, LN
- LBO
- KTP
- Sapphire
- N-BK7
- CaF2

Single-layer coating materials:

- MgF2
- SiO2 / fused silica
- Al2O3 / alumina
- CaF2
- HfO2, constant-index approximation
- Ta2O5, constant-index approximation
- TiO2, constant-index approximation
- Custom constant index

## Model

The calculation uses the thin-film Fresnel interference expression for one coating layer:

```text
r = (r01 + r12 exp(i 2 beta)) / (1 + r01 r12 exp(i 2 beta))
R = |r|^2
```

where the phase term is evaluated as:

```text
2 beta = 4 pi n_layer d cos(theta_layer) / lambda
```

The coating thickness is set by the center wavelength:

```text
d = lambda0 / (4 n_layer(lambda0) cos(theta_layer, lambda0))
```

For `p` and `s` polarization, the corresponding Fresnel amplitude coefficients are used. For unpolarized light, the calculator plots the average of `Rp` and `Rs`.

## Brewster Angle

Use **Surface Fresnel polarization = p-polarized** to see the Brewster-angle minimum. The unpolarized average will not go to zero at Brewster angle because the s-polarized component remains reflective.

The displayed Brewster angle is for the **uncoated ambient-substrate interface** at the center wavelength:

```text
theta_B = atan(n_substrate / n_ambient)
```

The coated reflectivity does not generally reach zero at that same angle because the coating introduces an additional interface and interference phase.

## Limitations

- This is a scalar thin-film model. It does not calculate full birefringent propagation through arbitrary crystal cuts.
- For anisotropic crystals, the axis selector chooses one principal Sellmeier index such as `no`, `ne`, `nx`, `ny`, or `nz`.
- Effective refractive index for a real beam/crystal geometry must be calculated separately and is not inferred here.
- HfO2, Ta2O5, and TiO2 are included as constant-index approximations because thin-film indices depend strongly on deposition process and wavelength.
- Absorption is not modeled; refractive indices are treated as real.
- Wavelengths outside each material's listed Sellmeier range are extrapolated and should be treated cautiously.

## Files

- [`bbo_p_coating_reflectivity.html`](./bbo_p_coating_reflectivity.html): standalone calculator.
- [`README.md`](./README.md): this documentation.

## References

The calculator uses common Sellmeier equations from vendor data sheets and RefractiveIndex.INFO entries, including:

- BBO: Newlight Photonics BBO data
- Fused silica, N-BK7, CaF2, sapphire, MgF2: RefractiveIndex.INFO
- KTA, LN, KTP, LBO: vendor data sheets / material references

