# CH8 — Orbital Motion and Gravitation

> [!abstract] Chapter focus
> Use Newton's law of gravitation and Kepler's laws to connect orbital period, orbital radius, speed, and mass. Use conservation of mechanical energy for objects falling toward Earth.

## 1. Formula Sheet

### Newton's law of gravitation
$$
F_g=\frac{GMm}{r^2}
$$
Here, $r$ is measured from the center of the planet or other central body.

### Circular orbital speed
Set gravity equal to centripetal force:
$$
\frac{GMm}{r^2}=\frac{mv^2}{r}
$$
Therefore:
$$
\boxed{v=\sqrt{\frac{GM}{r}}}
$$

### Kepler's third law for a small object orbiting a much larger mass
$$
\boxed{T^2=\frac{4\pi^2r^3}{GM}}
$$
Rearrangements:
$$
\boxed{M=\frac{4\pi^2r^3}{GT^2}}
\qquad
\boxed{r\propto T^{2/3}}
\qquad
\boxed{T\propto r^{3/2}}
$$

### Conservation of mechanical energy in a gravitational field
$$
K+U=\text{constant},\qquad K=\frac12mv^2,\qquad U=-\frac{GMm}{r}
$$
Between two points:
$$
\boxed{v_f^2=v_i^2+2GM\left(\frac1{r_f}-\frac1{r_i}\right)}
$$

### Differentiated Kepler's third law
Starting from $T^2\propto r^3$:
$$
2\frac{dT}{T}=3\frac{dr}{r}
$$
Thus:
$$
\boxed{\frac{dr}{dt}=\frac{2r}{3T}\frac{dT}{dt}}
$$

### Useful constants and conversions
$$
G=6.67\times10^{-11}\ \mathrm{N\,m^2/kg^2}
$$
$$
M_E=5.97\times10^{24}\ \mathrm{kg},\quad
R_E=6.37\times10^6\ \mathrm{m}
$$
$$
1\ \mathrm{km}=10^3\ \mathrm{m},\qquad
1\ \mathrm{year}\approx3.156\times10^7\ \mathrm{s}
$$

> [!warning] Most common mistake
> Orbital radius $r$ is measured from the center of Earth, not from its surface. Altitude is $h=r-R_E$.

---

## 2. Question 8.45 — Estimate the Mass of the Galaxy

**Question:** The Sun orbits the galaxy with a period of 200 million years at a distance of $8.4\times10^{20}$ m from the galactic center. Assuming the galaxy is effectively spherical and most of its mass lies inside the Sun's orbit, estimate the galaxy's mass.

### Diagram

![[images/Galaxy_Orbit_845.png]]

### Walkthrough

1. Use Kepler's third law:
   $$
   T^2=\frac{4\pi^2r^3}{GM}
   $$
2. Rearrange for mass:
   $$
   M=\frac{4\pi^2r^3}{GT^2}
   $$
3. Convert the period to seconds:
   $$
   T=200\times10^6(3.156\times10^7)
   \approx6.31\times10^{15}\ \mathrm{s}
   $$
4. Substitute $r=8.4\times10^{20}$ m:
   $$
   M=\frac{4\pi^2(8.4\times10^{20})^3}
   {(6.67\times10^{-11})(6.31\times10^{15})^2}
   $$
   $$
   \boxed{M\approx8.8\times10^{42}\ \mathrm{kg}}
   $$

**Answer: `8.8e42 kg`**

---

## 3. Question 8.59 — Altitude Decrease When Orbital Speed Increases

**Question:** A spacecraft is in a circular orbit 3586 km above Earth's surface. How much does its altitude decrease if it moves to a new circular orbit where its orbital speed is 10% higher? Earth's radius is 6370 km.

### Diagram

![[images/Satellite_Altitude_859.png]]

### Walkthrough

1. Convert altitude to orbital radius:
   $$
   r_1=R_E+h_1=6370+3586=9956\ \mathrm{km}
   $$
2. For circular orbits, $v=\sqrt{GM_E/r}$, so $v\propto r^{-1/2}$.
3. Since $v_2=1.10v_1$:
   $$
   \frac{v_2}{v_1}=\sqrt{\frac{r_1}{r_2}}
   \quad\Rightarrow\quad
   r_2=\frac{r_1}{(1.10)^2}
   $$
4. Calculate:
   $$
   r_2=\frac{9956}{1.21}\approx8228.1\ \mathrm{km}
   $$
5. The altitude decrease equals the orbital-radius decrease:
   $$
   \Delta h=r_1-r_2=9956-8228.1
   $$
   $$
   \boxed{\Delta h\approx1728\ \mathrm{km}}
   $$

**Answer: `1728 km`**

---

## 4. Question 8.56(a) — Meteoroid Striking Earth

**Question:** A meteoroid starts $5.4\times10^5$ km from Earth's center at $3.6\times10^3$ m/s and heads straight toward Earth. Find its speed when it strikes Earth.

### Diagram

![[images/Meteoroid_Approach_856.png]]

### Walkthrough

1. Use gravitational potential energy $U=-GM_Em/r$ and conservation of energy:
   $$
   v_f^2=v_i^2+2GM_E\left(\frac1{r_f}-\frac1{r_i}\right)
   $$
2. Convert distances:
   $$
   r_i=5.4\times10^8\ \mathrm{m},\qquad
   r_f=R_E=6.37\times10^6\ \mathrm{m}
   $$
3. Substitute $v_i=3.6\times10^3$ m/s and $GM_E\approx3.982\times10^{14}$ m$^3$/s$^2$:
   $$
   v_f=\sqrt{(3.6\times10^3)^2+
   2(3.982\times10^{14})
   \left(\frac1{6.37\times10^6}-\frac1{5.4\times10^8}\right)}
   $$
4. This gives:
   $$
   \boxed{v_f\approx1.17\times10^4\ \mathrm{m/s}
   =11.7\ \mathrm{km/s}}
   $$

**Answer: `11.7 km/s`**

Use Earth's radius as the final distance because the meteoroid strikes the surface.

---

## 5. Question 8.56(b) — Meteoroid at Closest Approach

**Question:** A second meteoroid starts $2.7\times10^5$ km from Earth's center at $8.0\times10^3$ m/s. Its path passes within 8500 km of Earth's center. Find its speed at closest approach.

### Diagram

![[images/Meteoroid_Approach_856.png]]

### Walkthrough

1. Use the same energy equation:
   $$
   v_f^2=v_i^2+2GM_E\left(\frac1{r_f}-\frac1{r_i}\right)
   $$
2. Convert distances:
   $$
   r_i=2.7\times10^8\ \mathrm{m},\qquad
   r_f=8.5\times10^6\ \mathrm{m}
   $$
3. Substitute:
   $$
   v_f=\sqrt{(8.0\times10^3)^2+
   2(3.982\times10^{14})
   \left(\frac1{8.5\times10^6}-\frac1{2.7\times10^8}\right)}
   $$
4. Calculate:
   $$
   \boxed{v_f\approx1.24\times10^4\ \mathrm{m/s}
   =12.4\ \mathrm{km/s}}
   $$

**Answer: `12.4 km/s`**

For the speed, conservation of energy is sufficient; no angular-momentum calculation is needed.

---

## 6. Question 8.67 — Satellite Descends to Catch Up

**Question:** Two satellites are in geostationary orbit at diametrically opposite positions. One descends into a lower circular orbit to catch the other in 10 orbits. Given $r=42{,}200$ km, how far must it descend?

### Diagram

![[images/Satellite_Catchup_867.png]]

### Walkthrough

1. The descending satellite is faster and must gain half an orbit. For the catch-up time, the descending satellite completes 10 orbits while the original satellite completes 9.5.
2. Let $T'$ be the lower satellite's period and $T$ the original period:
   $$
   10T'=9.5T
   \quad\Rightarrow\quad
   \frac{T'}{T}=0.95
   $$
3. Kepler's third law gives $T\propto r^{3/2}$:
   $$
   \frac{r'}{r}=\left(\frac{T'}{T}\right)^{2/3}
   $$
4. Find the new radius:
   $$
   r'=42200(0.95)^{2/3}\approx40781\ \mathrm{km}
   $$
5. Find the descent:
   $$
   \Delta r=42200-40781
   $$
   $$
   \boxed{\Delta r\approx1419\ \mathrm{km}}
   $$

**Answer: `1419 km`**

---

## 7. Question 8.71 — Rate of Change of Earth–Moon Distance

**Question:** Tidal effects cause the Moon's orbital period to increase at 36 ms per century. Assuming a circular orbit, find the rate at which the Earth–Moon distance increases.

### Diagram

![[images/Moon_Distance_871.png]]

### Walkthrough

1. Kepler's third law states:
   $$
   T^2\propto r^3
   $$
2. Differentiate:
   $$
   2\frac{dT}{T}=3\frac{dr}{r}
   $$
   Hence:
   $$
   \frac{dr}{dt}=\frac{2r}{3T}\frac{dT}{dt}
   $$
3. Use the mean lunar orbital radius $r=3.84\times10^8$ m and period $T=27.3$ days $=2.36\times10^6$ s. The period increase is:
   $$
   \frac{dT}{dt}=0.036\ \mathrm{s/century}
   $$
4. Substitute:
   $$
   \frac{dr}{dt}=
   \frac{2(3.84\times10^8)}
   {3(2.36\times10^6)}(0.036)
   $$
   $$
   \boxed{\frac{dr}{dt}\approx3.9\ \mathrm{m/century}}
   $$

**Answer: `3.9 m/century`**

The Moon's orbital period increases, so its average orbital radius increases too.

---

## 8. Answers at a Glance

| Problem | Topic | Answer |
|---|---|---:|
| 8.45 | Estimate the mass of the galaxy | $8.8\times10^{42}$ kg |
| 8.59 | Decrease in satellite altitude | 1728 km |
| 8.56(a) | Meteoroid impact speed | 11.7 km/s |
| 8.56(b) | Meteoroid closest-approach speed | 12.4 km/s |
| 8.67 | Satellite descent for catch-up | 1419 km |
| 8.71 | Moon's distance increase | 3.9 m/century |

## 9. Exam Checklist

- [ ] Convert all distances to meters before using SI equations.
- [ ] Measure orbital radius from the center of the planet, not its surface.
- [ ] Use $v=\sqrt{GM/r}$ for circular orbital speed.
- [ ] Use $T^2=4\pi^2r^3/(GM)$ to relate period, radius, and central mass.
- [ ] Use $K+U=\text{constant}$ for gravitational acceleration and meteoroid speed.
- [ ] Differentiate Kepler's third law for rates of change.
- [ ] In catch-up problems, identify which satellite completes the specified number of orbits.
