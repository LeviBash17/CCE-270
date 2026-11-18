# Overhanging Beam Validation Cases

Use downward loads as positive. Measure all load positions from the physical left end. Let the physical beam length be `L = a + S + c`, where `a` and `c` are the left and right overhang lengths and `S` is the support-to-support span. The supports are at `x = a` and `x = a + S`.

For total load `W` and total moment about the physical left end `M0`, check reactions with:

- `Rright = (M0 - a W) / S`
- `Rleft = W - Rright`

Negative support reactions are valid and indicate a downward restraint force.

## Hand-calculated cases

### 1. Symmetric overhangs with full-length uniform load

- Left overhang: 5 ft; support span: 20 ft; right overhang: 5 ft; total length: 30 ft.
- Uniform load: 1 kip/ft from x = 0 to 30 ft.
- Total load: 30 kip at x = 15 ft.
- Expected reactions: `Rleft = 15 kip`; `Rright = 15 kip`.
- Expected maximum absolute shear: 10 kip, immediately beside either support.
- Expected moments: -12.5 kip-ft at each support and +37.5 kip-ft at x = 15 ft; maximum absolute moment is 37.5 kip-ft.
- Boundary condition: deflection is zero at x = 5 ft and x = 25 ft. The L/240 allowable uses the 20 ft support span, not the 30 ft physical length.

### 2. Left-tip point load with a negative support reaction

- Left overhang: 2 ft; support span: 10 ft; right overhang: 0 ft.
- Point load: 1 kip at x = 0 ft.
- Expected reactions: `Rleft = 1.2 kip`; `Rright = -0.2 kip`.
- Expected maximum absolute shear: 1 kip.
- Expected maximum absolute moment: 2 kip-ft at the left support (x = 2 ft).
- Boundary condition: deflection is zero at x = 2 ft and x = 12 ft; allowable deflection is `120 in / 240 = 0.5 in`.

### 3. Partial uniform load on the left overhang

- Left overhang: 2 ft; support span: 10 ft; right overhang: 1 ft.
- Uniform load: 2 kip/ft from x = 0 to 2 ft.
- Resultant: 4 kip at x = 1 ft.
- Expected reactions: `Rleft = 4.4 kip`; `Rright = -0.4 kip`.
- Expected maximum absolute shear: 4 kip immediately before the left support.
- Expected maximum absolute moment: 4 kip-ft at the left support.
- Boundary condition: deflection is zero at x = 2 ft and x = 12 ft.

### 4. Self-weight spans the physical beam

- Use the geometry from Case 3 and select W12x26 (26 lb/ft).
- Enable self-weight. It must add `0.026 kip/ft` from x = 0 to 13 ft, for a total of `0.338 kip`.
- With self-weight alone, expected reactions are approximately `Rleft = 0.1859 kip` and `Rright = 0.1521 kip`.
- Disable self-weight and confirm those reactions no longer include the member weight.

## Existing-support regressions

### Simply supported

- Span: 20 ft; uniform load: 1 kip/ft over 0–20 ft.
- Expected reactions: 10 kip each; maximum absolute moment: 50 kip-ft at midspan.
- With W12x26 and self-weight off, the existing elastic calculation reports approximately 0.609 in maximum deflection.

### Cantilever

- Span: 10 ft; point load: 1 kip at the free end; self-weight off.
- Expected fixed reaction: 1 kip; fixed-end moment: 10 kip-ft; maximum absolute moment: 10 kip-ft at the fixed end.
- With W12x26, the existing elastic calculation reports approximately 0.097 in maximum deflection. The allowable remains span/240 (0.5 in).

## Input validation

- Overhang support mode with both overhangs set to zero must show validation errors and suppress analysis updates.
- A support span plus overhang lengths above 100 ft must be rejected.
- A point load or uniform-load endpoint may be located anywhere from the physical left end through the physical right end, including either overhang.
