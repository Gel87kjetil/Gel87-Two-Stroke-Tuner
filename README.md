# Gel87 Two-Stroke Tuner

Gel87 is a free browser-based engineering and tuning tool
for small high-performance two-stroke engines.

The project was originally developed around a heavily modified
Suzuki TS50X, but all major engine geometry is user-defined.

## Main features

- Engine geometry and port timing
- Generated portmap
- Individual T1-Tn transfer geometry
- Booster ports B1-Bn
- Transfer direction and duct analysis
- STA analysis
- Squish and combustion chamber modelling
- Crankcase compression and flow estimates
- Piston-port, reed, combined and rotary intake
- Intake tract resonance analysis
- Expansion chamber designer
- Direct segment exhaust-wave analysis
- RPM sweep against EO / TO / BDC / TC / EC
- Ignition and performance analysis
- Exhaust cone cutting templates
- Gear and vehicle-speed calculator
- Save / Upload complete projects as JSON

License
Gel87 Two-Stroke Tuner is licensed under the GNU General Public License v3.0 (GPL-3.0).

You are free to use, study, modify and redistribute the software under the terms of the GPL-3.0 license.

If you distribute a modified version, the corresponding source code and the same license freedoms must remain available.

See the LICENSE file for the full license terms.

## Running Gel87

No installation is required.

Download `Gel87_Two_Stroke_Tuner.html`
and open it in a modern web browser.

# Gel87 Two-Stroke Tuner - README

For use with Gel87 1.8.2 and later revisions based on the same calculation model.

> Important: Gel87 is an engineering/tuning aid, not CFD, dyno testing, cylinder-pressure measurement, or a guarantee against detonation or mechanical failure. Calculated and empirical values are deliberately separated where possible.

---

# PART 1 - QUICK START / HOW TO USE

## What Gel87 does

Gel87 combines the engine, ports, combustion chamber, transfer ducts, crankcase, exhaust, intake, bottom-end dynamics, gearing and vehicle speed into one connected model.

The key rule is:

**A physical value should normally be edited in one place only. Other sections reuse that value automatically.**

Blue fields are user inputs. Grey/locked fields are calculated or imported from another section.

## Recommended workflow

### 1. Motor, compression and squish
Enter the engine's basic geometry first:

- Bore
- Stroke
- Rod length
- Measured TDC volume
- Target dynamic compression ratio
- Squish area
- Inner/outer squish clearance

The program calculates displacement and compression-related results from these values.

### 2. Port timing and intake/reed
Measure port heights from the cylinder top/deck reference and enter the physical values.

Gel87 calculates opening/closing angles and durations from engine geometry instead of requiring duplicate manual duration inputs.

For rotary intake, use the rotary opening/closing inputs. Rotary valve calculations still use the intake-area model, so the port profile should represent the real rotary opening.

### 3. Port map and STA
Describe exhaust, transfer and intake geometry. The generated portmap uses the same geometry that feeds the area and STA calculations.

Where a value was already entered earlier, Gel87 should reuse it instead of creating a second editable copy.

### 4. Transfer direction and ducts
Use the transfer module for duct-specific information:

- Left and right edge directions
- Side-entry angle
- Entry area
- Duct length
- Shared-entry / branch point
- Cd and pressure-flow analysis

Center arc, port width and window height belong to the port geometry and are therefore references here.

### 5. Crankcase / transfer pressure
Enter measured crankcase free volume if available. Gel87 distinguishes between:

- geometric crankcase compression
- dynamic crankcase compression after effective intake closure

Use measured values when possible. The pressure curve remains a simplified polytropic model.

### 6. Squish, chamber and Singh grooves
This section uses the motor dimensions already entered. Relevant motor values may be shown again as locked references.

Edit only the geometry that belongs to the combustion-chamber model, spark plug and Singh grooves.

### 7. Bottom-end / wrist-pin analysis
Use the local analysis RPM if a specific RPM should be inspected without changing Design Peak RPM.

### 8. Exhaust design
Start with the **First estimate exhaust-wave target distance** as a construction starting point, not as an absolute final answer.

Then build realistic header, diffuser, belly and baffle sections and use the Direct Segment Wave model to verify physical timing.

Recommended exhaust workflow:

1. First estimate exhaust-wave target distance
2. Build initial pipe geometry
3. Verify direct physical segment timing
4. Place the diffuser negative peak around BDC
5. Examine the scavenging tail toward TC
6. Examine wave-geometry crossover and combined zero crossing
7. Place useful positive baffle/stuffing pressure in the TC-EC window
8. Check positive combined wave at EC
9. Fine-tune cone distribution and temperature assumptions

### 9. Analysis RPM slider
The exhaust amplitude section has a quick RPM sweep around Design Peak RPM.

Use the slider for a fast visual scan and the Analysis RPM field for exact values. Analysis RPM does not redesign the pipe.

### 10. Intake resonance
Measure the intake along actual flow centerlines.

For combined piston-port + reed systems, Gel87 separates:

- common tract: carb exit to split
- piston branch: split to piston port
- reed branch: split to reed cage

The resonance regions are estimates. Reed stiffness, pressure pulsation and crankcase coupling can shift the real response.

### 11. Cone cutting templates
Select any diffuser or baffle section. Gel87 creates:

- finished cone side view
- flat annular-sector cutting pattern
- inner/outer development radii
- sector angle
- seam allowance
- 100 mm print-scale check

Always physically measure the 100 mm reference line before cutting sheet metal.

### 12. Gear and vehicle speed
Enter actual tooth counts for each gear, primary drive and sprockets. Tire format 110/80-18 means the sidewall height is 80% of 110 mm.

---

# PART 2 - MEASUREMENT NOTES

## Port heights
Use one consistent reference, normally cylinder top/deck to the port edge.

## Crankcase volume
A practical method is to assemble and seal the crankcase in a known piston position and fill the free volume from a calibrated burette using a suitable, non-volatile calibration liquid. Vent the high point and avoid trapped air.

Do not use gasoline as a measuring liquid.

Transfer duct volumes can be measured separately by sealing their openings and filling from a burette, or obtained from a sealed CAD volume.

## Exhaust length
Measure along the approximate gas-flow / pipe centerline, including:

- exhaust window to flange/header start
- bends/adapters
- header
- every diffuser section
- belly
- every baffle section

Do not substitute straight-line distance for a curved centerline path.

## EGT
Record both the measured EGT and the sensor distance from the exhaust window. Gel87 uses the temperature profile to calculate local sound speed and return times.

---

# PART 3 - MATHEMATICS AND MODEL DETAILS

## 1. Displacement

For bore B and stroke S:

Vd = (pi/4) * B^2 * S

Use consistent units. With B and S in mm, divide mm^3 by 1000 for cc.

## 2. Slider-crank position

For crank radius r, rod length L and crank angle theta, piston travel from TDC is based on standard slider-crank geometry:

x(theta) = r(1-cos(theta)) + L - sqrt(L^2 - r^2 sin^2(theta))

Gel87 numerically inverts this relationship when a measured port height must be converted to crank angle.

## 3. Port duration

If a symmetric port opens at angle theta_open after TDC on the downward stroke:

duration = 360 - 2*theta_open

Real intake models can be asymmetric, especially rotary valves.

## 4. Port area

The program uses the entered/generated port profile rather than assuming a rectangular port wherever the profile data are available.

For circular-cylinder wall geometry, chord and arc conversions use cylinder radius/circumference geometry.

## 5. Static and dynamic compression

Static compression ratio:

CR_static = (Vd + Vc) / Vc

Dynamic compression uses only the trapped/effective swept volume after the selected exhaust-closing/trapping reference used by the model.

Measured TDC volume and geometrically calculated chamber volume are intentionally kept as separate references.

## 6. Crankcase compression

Geometric crankcase compression:

CCR_geo = V_TDC / V_BDC

For displacement Vd and measured free crankcase volume at TDC:

V_BDC ~= V_TDC - Vd

Dynamic crankcase compression starts at effective intake closure:

CCR_dyn = V_at_intake_close / V_BDC

The closed-volume pressure estimate is polytropic:

P2 = P1 * (V1/V2)^n

where n is the selected polytropic exponent.

This is not a complete gas-exchange simulation. Open reeds, piston ports and transfer flow exchange mass with the volume.

## 7. Transfer-flow estimate

A simplified orifice-style estimate uses:

Q = Cd * A * sqrt(2*DeltaP/rho)

where:

- Cd = discharge coefficient
- A = effective flow area
- DeltaP = pressure difference
- rho = mixture density

Cd is not a universal constant. The default is an estimate until flow-bench data are available.

## 8. Mixture density

For an air approximation:

rho = P / (R*T)

with absolute pressure P, temperature T in kelvin and specific gas constant R.

Gel87 may apply an AFR-based mass correction for fresh mixture. Real carburetted mixture can include droplets, so excessive apparent precision should be avoided.

## 9. Exhaust sound speed

Gel87's current exhaust model uses a temperature-dependent approximation of the form:

c ~= 20.05 * sqrt(273.15 + T_C)

Each physical pipe segment can therefore have its own local sound speed.

## 10. Segment return time

For segment length DeltaL and local sound speed c_i:

Delta t_i = DeltaL / c_i

Accumulated round-trip return time to a reflection point:

t_return = 2 * sum(DeltaL_i / c_i)

The exhaust-window-to-header-start distance is included in the travel path.

## 11. First estimate exhaust-wave target distance

This is an initial construction estimate based on average/compensated wave travel and the historical average exhaust-wave travel multiplier.

The default multiplier 1.35 is empirical. It changes the initial suggested target distance, not physical sound speed.

Final design judgement should prioritize Direct Segment physical timing over a small percentage error in first-estimate total length.

## 12. Exhaust pulse-width estimate

The simplified width reference derived from the historical workbook is:

PulseWidth = (W_exhaust,max / Bore) * (800 / PeakRPM) * 1000 * 0.017

Gel87 can derive the exhaust width/area from the physical port model instead of relying only on displacement-based tables.

## 13. Pulse phase increment

The historical phase construction uses:

PhaseIncrement = 180 / (PulseWidth / (PhaseTravel / c_comp))

where PhaseTravel has historically been about 9.95 mm in the reference model and c_comp is compensated average sound speed.

This parameter is a pulse-model parameter, not a physical pipe segment length.

## 14. Base pulse models

### Calculated windowed pulse

Base(n) = sin(n*PhaseIncrement) * Envelope(n)

The window limits later artificial sine lobes.

### Continuous reference sine

Base(n) = sin(n*PhaseIncrement)

This mode is retained for comparison with the historical spreadsheet.

### Damped sine

Base(n) = sin(n*PhaseIncrement) * exp(-D*cycles)

where D is the selected damping coefficient.

## 15. Exhaust segment reflection coefficient

The Direct Segment model uses adjacent cross-sectional areas:

R_i = |(A_i - A_(i-1)) / (A_i + A_(i-1))|

Diffuser contributions are negative; baffle contributions are positive.

For each segment:

A_diff(t) = -sum(R_i * Base(t - t_return,i))

A_baffle(t) = +sum(R_j * Base(t - t_return,j))

Optional propagation-loss and baffle-calibration multipliers are explicit. Default propagation loss is none and default baffle correction is 1.000.

## 16. Combined exhaust response

A_combined(t) = A_diff(t) + A_baffle(t)

The displayed amplitudes are relative model values, not calibrated kPa/bar cylinder pressure.

Timing, shape and comparison between designs are currently more trustworthy than absolute pressure magnitude.

## 17. Wave centers, peaks and crossover

### Reflection center
A strength-weighted center of physical segment return times. This is a reference point and does not have to equal the actual wave peak.

### Actual diffuser negative peak
The time where the calculated diffuser response reaches its most negative value.

A useful design reference is the error relative to BDC.

### Actual baffle positive peak
The time where the calculated baffle response reaches its maximum positive value.

This is compared with the TC-EC stuffing window.

### Wave geometry crossover

GeometryCrossover = (DiffuserWaveEnd + BaffleWaveEnd) / 2

This is the structural/reference crossover used by the historical spreadsheet method.

### Combined zero crossing
The actual calculated time after BDC where the combined wave crosses from negative to positive.

The two crossover definitions deliberately remain separate.

## 18. Scavenging and stuffing windows

Gel87 calculates average/point response in important engine windows:

- TO to BDC
- BDC to TC
- TC to EC
- combined value at TC
- combined value at EC

Conceptually:

ScavengingIndex ~ - integral of negative combined amplitude while transfers are open

StuffingIndex ~ integral of positive combined amplitude from TC toward EC

These remain relative indices.

## 19. Intake acoustics

For a simple quarter-wave acoustic reference:

f1 = c / (4*L_eff)

RPM_equivalent = 60*f1

Engine-order coincidence regions are then shown as:

RPM_3rd = RPM_equivalent / 3
RPM_5th = RPM_equivalent / 5

These are possible reinforcement regions, not guaranteed torque peaks.

### Piston-port open-period reference

A simplified travel-length comparison is:

L_open = c*theta / (12*N)

where theta is intake-open duration in crank degrees and N is RPM.

### Combined piston/reed system

Gel87 treats common tract, piston branch and reed branch separately. Reed resonance remains approximate because reed stiffness, motion and crankcase coupling are not fully modeled.

## 20. Intake flow-quality index

The flow-quality index combines explicit user-described geometry such as:

- throttle opening
- bend severity
- bridge transition quality
- reed-transition smoothness
- bellmouth profile

It is a comparison index, not a measured flow coefficient.

## 21. PWK venturi / jet-area comparison

Venturi/slide area is calculated geometrically.

Needle annulus area:

A_annulus = pi/4 * (D_needleJet^2 - D_needle^2)

Main jet and needle annulus are treated as serial restrictions using an equivalent-area comparison:

A_main_eff = 1 / sqrt(1/A_main^2 + 1/A_annulus^2)

Pilot area is then a parallel contribution.

The result is deliberately called a metering-area index, not calculated AFR.

## 22. Intake flow area split

For combined piston-port/reed systems, carburettor area can be compared with the automatically calculated downstream piston-port plus reed effective area.

A smaller reed branch should naturally carry a smaller share, but the program does not yet solve a full transient branch-network CFD model.

## 23. Gear and vehicle speed

Primary ratio:

R_primary = DrivenPrimary / DrivingPrimary

Gear ratio:

R_gear = DrivenGear / DrivingGear

Final ratio:

R_final = RearSprocket / FrontSprocket

Total reduction:

R_total = R_primary * R_gear * R_final

Wheel RPM:

RPM_wheel = RPM_engine / R_total

For tire W/P-Rim:

Sidewall = W * P/100

WheelDiameter_mm = Rim_in * 25.4 + 2*Sidewall

Circumference_m = pi * WheelDiameter_mm / 1000

Vehicle speed:

km/h = RPM_wheel * Circumference_m * 60 / 1000

An optional tire-growth percentage modifies circumference for comparison.

## 24. Cone cutting-template mathematics

For finished small radius r1, large radius r2 and axial length L:

Delta r = r2-r1

Slant length:

s = sqrt(L^2 + Delta r^2)

Development outer radius:

R_outer = s*r2/(r2-r1)

Development inner radius:

R_inner = s*r1/(r2-r1)

Sector angle:

alpha = 360*r2/R_outer

The flat pattern is therefore an annular sector.

Sheet thickness can be used to shift finished inside/outside diameters toward an approximate neutral diameter before development.

Seam allowance is added as extra tangential angle. Edge trim modifies the inner/outer cut radii.

Always verify the physical 100 mm reference line before manufacturing.

---

# PART 4 - HOW TO INTERPRET THE COLORS AND RESULTS

- Blue input: editable here
- Grey input: imported/calculated reference; edit at its owner section
- Green result: generally close to the current model target
- Orange: review / possible compromise
- Red: strong mismatch or invalid geometry

Do not treat green as a guarantee of mechanical safety or maximum power.

---

# PART 5 - WHAT IS CALCULATED VS ESTIMATED

## Strongly geometry-based

- displacement
- slider-crank port timing
- port area/profile
- duct/window ratios
- pipe section area/volume
- cone angles
- local exhaust segment travel distance
- temperature-dependent sound-speed timing
- gear speed
- cone development geometry

## Engineering estimates / simplified physical models

- dynamic crankcase pressure
- intake resonance regions
- transfer flow from Cd and DeltaP
- exhaust relative reflection strength
- pre-ignition estimate
- tire growth

## Empirical/calibration parameters

- historical average exhaust-wave travel multiplier
- optional exhaust propagation attenuation
- optional baffle correction
- generic Cd when not flow-bench measured
- intake bend/transition quality factors

The program should expose these values rather than hiding them.

---

# PART 6 - PRACTICAL VALIDATION

For best results, calibrate the model against a known real engine/pipe combination.

Useful measurements include:

- exact pipe centerline dimensions
- cone diameters and lengths
- EGT plus probe distance
- RPM where torque rise begins
- RPM at strongest midrange
- power peak RPM
- over-rev falloff
- known ignition curve
- measured crankcase volume
- measured transfer duct volumes
- real carb/needle/jet dimensions

For the current TS50X project, the stock Yasuni R3 Short is an excellent future calibration reference because its real riding behavior is already well characterized by the user.

---

# PART 7 - FINAL SAFETY / LIMITATIONS

Gel87 does not currently solve full compressible 1D/3D gas dynamics, combustion chemistry, detonation margin, reed structural dynamics, fuel atomization, or measured cylinder/exhaust pressure.

Use the tool to understand relationships, compare designs and choose sensible test directions. Final ignition, jetting, compression and mechanical limits must be validated on the actual engine using appropriate temperature, mixture, knock/pressure and dyno/riding evidence.

