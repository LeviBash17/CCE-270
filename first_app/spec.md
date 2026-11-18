# App Specification
# Beam Designer

## Purpose
Shear, moment, and deflection diagrams for a beam, with a check of
whether the beam is strong and stiff enough.

## Inputs
- **Support type:** Simply supported or cantilever; default is simply supported. Control: selector. Range: those two options; units: not applicable.
- **Span:** 0.1–100 ft; default is 20 ft. Control: numeric field with range validation.
- **Beam:** Choose steel W-shape or dimensional lumber. Control: material selector, then a beam selector. Range: entries in the built-in catalog. Default: W12x26 for steel and the first catalog entry when lumber is selected. Units: catalog designation (W-shape label or nominal lumber dimensions in inches). Lumber entries identify size, species, and grade.
- **Applied loads:** 0–20 point-load or uniform-load rows; the startup example has one uniform-load row. Control: load-type selector, numeric fields, and add/remove buttons. Positive loads act downward; upward loads are not supported.
  - **Point load:** 0–1,000 kip; new-row default is 0 kip. Position is 0–span ft; default is span/2. Control: numeric fields.
  - **Uniform load:** 0–100 kip/ft; new-row default is 0 kip/ft. Start and end positions are each 0–span ft; defaults are 0 and span, and start must be less than end. Control: numeric fields.
- **Beam self-weight:** Default is included. Control: checkbox. Range: on or off; units: not applicable. When on, weight per length comes from the selected catalog entry and is applied over the full span.
- **Startup example:** Simply supported, 20 ft, W12x26, with a 1 kip/ft uniform load from 0 to span and self-weight included.

The built-in beam catalog supplies section modulus, elastic modulus, allowable bending stress, and weight per length; users do not enter these properties directly.

## Calculations
- Reactions from statics.
- Shear V(x) and moment M(x) along the beam, at 1 ft spacing or finer,
  plus exactly at every load point and load start/end.
- Deflection y(x) by integrating M/EI twice with the boundary conditions
  for the support type (y = 0 at both supports; or y = 0 and slope = 0 at
  the fixed end).
- Bending stress fb = |M max| x 12 / Sx (ksi). Bending check passes if
  fb <= Fb.
- Deflection check passes if |y max| <= L/240 or L/360 (L in inches).
- Lightest passing section: of the sections in the chosen material, the
  lightest steel section that passes both checks (if two weigh the same,
  the one with the larger Ix), or the first lumber size in the table
  that passes.
- Units: kips, ft, and kip-ft on screen; inches for deflection.


## Outputs
- A beam drawing with the supports, load arrows (point loads as single
  arrows, uniform loads as a row of small arrows), and the reactions.
- Three diagrams stacked under the beam and lined up with it on the same
  x scale: shear (kips), moment (kip-ft), and deflected shape (in). Each
  diagram is filled and shows its maximum value labeled at its location.
- Moving the mouse over any diagram shows a vertical line across all three
  with the values of V, M, and y at that x.
- Two check cards: Bending and Deflection. Each shows the actual value, the
  allowable value, a bar for the ratio (green under 70%, amber 70-100%,
  red over 100%), and PASS or FAIL.
- A line suggesting the lightest passing section, with a button to use it.
- Everything updates immediately whenever any input changes.

## Look and feel
- Opens with an example already loaded (20 ft simply supported W12x26
  with a 1 kip/ft uniform load over the full span), so it is useful the
  moment it opens.
- Clean and professional, like a commercial engineering web app: light
  background, white cards with soft shadows, one accent color, a modern
  system font, generous spacing. No clutter.
- Inputs in a panel on the left; drawing, diagrams, and checks on the
  right. On a phone, the panels stack.
- Every input has a short label with units. Hovering over a check card
  explains the formula used.

## Validation
1. Simply supported, 20 ft, W12x26, uniform 1 kip/ft over the full span:
   reactions 10 k and 10 k; max shear 10 k; max moment 50.0 kip-ft at
   10 ft; fb = 17.96 ksi; max deflection 0.609 in at 10 ft; L/360 =
   0.667 in; both checks pass.
2. Simply supported, 20 ft, W12x26, point load 10 k at 5 ft: reactions
   7.5 k (left) and 2.5 k (right); max moment 37.5 kip-ft at 5 ft;
   fb = 13.47 ksi; max deflection 0.340 in at 8.82 ft.
3. Cantilever, 10 ft, W12x26, point load 5 k at the free end: reaction
   5 k and fixed-end moment 50 kip-ft; max deflection 0.487 in at the free
   end (PL^3/3EI); L/360 = 0.333 in, so deflection fails.
4. Simply supported 12 ft 2x10 joist, uniform 0.05 kip/ft: max moment
   0.90 kip-ft; fb = 0.505 ksi; max deflection 0.147 in; both pass.

## Out of scope
- Self-weight, shear stress, lateral-torsional buckling, load
  combinations, and continuous beams.
- Saving designs.

## Build stages

1. **Static page mockup — no calculations.** Lay out the input panel, beam drawing, three diagram areas, check cards, and sizing suggestion with clearly marked placeholder values.
   - **Check:** Open at desktop and phone widths. Confirm everything is readable, stacks appropriately, and placeholders are clearly distinguished from calculated results.
2. **Inputs and beam/load drawing.** Add validated inputs, load-row controls, and a drawing that reflects current inputs. Keep calculation results as placeholders.
   - **Check:** Test input limits and invalid values, add/remove rows, change span and support type, and confirm the drawing updates.
   - **Before this stage:** Resolve the conflict between self-weight being enabled by default in Inputs and listed as out of scope.
3. **Reactions, shear, and moment.** Calculate statics and draw shear and moment diagrams.
   - **Check:** Compare against hand calculations for a full-span uniform load, an off-center point load, and a cantilever end load. Verify peaks and point-load jumps are not missed by the plotting grid.
4. **Deflection and design checks.** Calculate deflection and bending stress, then show the pass/fail checks.
   - **Check:** Compare values and locations against hand calculations; test just below, at, and above each limit.
   - **Before this stage:** Define whether the deflection limit is L/240 or L/360, add Ix to the beam catalog, and settle the unit conventions.
5. **Lightest-section recommendation and final validation.** Search the catalog, show a passing recommendation, and make “use it” update the selected beam.
   - **Check:** Compare recommendations against hand-ranked candidates, including ties and no passing section; then run the validation cases and recheck the responsive layout.
   - **Before this stage:** Define catalog ranking and reconcile validation cases with the final self-weight rule.