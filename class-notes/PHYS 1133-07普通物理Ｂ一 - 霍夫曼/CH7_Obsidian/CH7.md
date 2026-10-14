# CH7 — Potential Energy and Conservation of Energy

## 1. Formula Sheet

### Work–energy theorem
$$
W_{\text{net}}=\Delta K=K_f-K_i,\qquad K=\frac12mv^2
$$

### Kinetic friction and stopping distance
$$
f_k=\mu_kN
$$
On a horizontal surface, $N=mg$, so:
$$
f_k=\mu_kmg,\qquad a=-\mu_k g
$$
For an object that stops:
$$
\boxed{d=\frac{v_i^2}{2\mu_k g}}
$$

### Force from potential energy
$$
\boxed{F_x=-\frac{dU}{dx}}
$$
Force is the negative slope of a potential-energy graph.

### Potential energy from a force law
For a conservative force:
$$
\boxed{\Delta U=-\int_{x_i}^{x_f}F_x(x)\,dx}
$$
For a spring, $F=-kx$ gives $U=\frac12kx^2$.

### Conservation of mechanical energy
When there is no friction or other nonconservative work:
$$
K_i+U_i=K_f+U_f
$$

### Circular motion at the top of a loop
At the minimum speed to maintain contact, the normal force is zero:
$$
mg=\frac{mv^2}{R}
\quad\Rightarrow\quad
\boxed{v_{\text{top,min}}=\sqrt{gR}}
$$

### Useful unit conversions
$$
1\text{ aJ}=10^{-18}\text{ J},\quad 1\text{ nm}=10^{-9}\text{ m}
$$
$$
1\text{ Å}=10^{-10}\text{ m},\quad
1\frac{\text{aJ}}{\text{nm}}=1\text{ nN}
$$

---

## 2. Question 7.22 — Ice Skater Coasting to a Stop

**Question:** An 18.6-kg skater pushes off with an initial speed of 5.7 m/s and coasts with no further effort. If the friction coefficient is 0.049, how far does she travel?

### Free-body diagram

![[images/Skater_Friction.png]]

- Normal force $N$ upward.
- Weight $mg$ downward.
- Kinetic friction $f_k$ opposite the motion.
- Vertically, $N=mg$; horizontally, friction slows the skater.

### Walkthrough

1. Friction is $f_k=\mu_kN=\mu_kmg$.
2. Newton's second law gives $ma=-f_k$, so $a=-\mu_k g$.
3. Use $v_f^2=v_i^2+2ad$. At the stopping point, $v_f=0$:
   $$
   0=v_i^2-2\mu_k gd
   $$
4. Solve:
   $$
   d=\frac{v_i^2}{2\mu_k g}
   =\frac{5.7^2}{2(0.049)(9.8)}
   $$
   $$
   \boxed{d\approx33.8\text{ m}}
   $$

**Answer: `33.8 m`**

Mass cancels; the stopping distance depends on initial speed and friction coefficient.

---

## 3. Question 7.30 — Force from Potential Energy

**Question:** $U(x)=(1.27x^2-0.26x^4)$ aJ, where $x$ is in nm. Find $F_x$ at $x=2.26$ nm, in nN.

### Potential-energy diagram

![[images/Potential_Energy_730.png]]

### Walkthrough

1. Use $F_x=-dU/dx$.
2. Differentiate:
   $$
   \frac{dU}{dx}=2(1.27)x-4(0.26)x^3
   =2.54x-1.04x^3
   $$
3. Add the negative sign and substitute:
   $$
   F_x=-[2.54(2.26)-1.04(2.26)^3]
   $$
4. Since $1\text{ aJ/nm}=1\text{ nN}$:
   $$
   \boxed{F_x\approx+6.26\text{ nN}}
   $$

**Answer: `6.26 nN`**

The positive sign means the force points in the positive $x$ direction.

---

## 4. Question 7.37 — Energy Stored in a Stretched Rope

**Question:** The rope's restoring force is $F(x)=-kx+bx^2-cx^3$, with $k=200$ N/m, $b=4.2$ N/m$^2$, and $c=9.5$ N/m$^3$. Find the energy stored when stretched by 4.4 m.

### Diagram

![[images/Rope_Energy_737.png]]

### Walkthrough

1. Stored potential energy is the negative integral of the restoring force:
   $$
   U(x)=-\int_0^xF(x')\,dx'
   $$
2. Substitute:
   $$
   U(x)=\int_0^x(kx'-b{x'}^2+c{x'}^3)\,dx'
   $$
3. Integrate:
   $$
   U(x)=\frac12kx^2-\frac13bx^3+\frac14cx^4
   $$
4. At $x=4.4$ m:
   $$
   U=\frac12(200)(4.4)^2-\frac13(4.2)(4.4)^3+\frac14(9.5)(4.4)^4
   $$
   $$
   \boxed{U\approx2706.9\text{ J}\approx2707\text{ J}}
   $$

**Answer: `2707 J`**

Use $-\int F\,dx$ because the given force is the rope's restoring force.

---

## 5. Question 7.45 — Block Between Two Springs

**Question:** A 1-kg block moves on a frictionless surface between two springs. The left spring has $k_1=9.0$ N/m and maximum compression $x=5.9$ m. Find the block's speed while it moves freely between the springs.

### Diagram

![[images/Two_Springs_745.png]]

### Walkthrough

1. At maximum compression, the block momentarily stops, so its energy is all spring potential energy:
   $$
   E_i=\frac12k_1x^2
   $$
2. Between the springs, the block is not in contact with either spring, so its energy is kinetic:
   $$
   E_f=\frac12mv^2
   $$
3. Conservation of energy:
   $$
   \frac12k_1x^2=\frac12mv^2
   $$
4. Solve:
   $$
   v=x\sqrt{\frac{k_1}{m}}
   =5.9\sqrt{\frac{9.0}{1}}
   $$
   $$
   \boxed{v=17.7\text{ m/s}}
   $$

**Answer: `17.7 m/s`**

The right spring's constant is not needed for the speed as the block first moves freely between the springs.

---

## 6. Question 7.52 — Equilibrium Separation of Two Ions

**Question:** The ionic potential energy is $U(r)=br^{-n}-ar^{-1}$, with $a=3.0\times10^{-28}$, $b=8.8\times10^{-98}$, and $n=9$. Find the equilibrium separation in angstroms.

### Potential-energy diagram

![[images/Ionic_Potential_752.png]]

### Walkthrough

1. Equilibrium occurs at the minimum of $U(r)$, so $dU/dr=0$:
   $$
   \frac{dU}{dr}=-nbr^{-n-1}+ar^{-2}=0
   $$
2. Rearrange:
   $$
   ar^{-2}=nbr^{-n-1}
   \quad\Rightarrow\quad
   r^{n-1}=\frac{nb}{a}
   $$
3. Therefore:
   $$
   r=\left(\frac{nb}{a}\right)^{1/(n-1)}
   $$
4. Substitute:
   $$
   r=\left(\frac{9(8.8\times10^{-98})}{3.0\times10^{-28}}\right)^{1/8}
   $$
   $$
   r=(2.64\times10^{-69})^{1/8}
   \approx2.6773\times10^{-9}\text{ m}
   $$
5. Convert to angstroms:
   $$
   r=\frac{2.6773\times10^{-9}}{10^{-10}}
   $$
   $$
   \boxed{r\approx26.77\text{ Å}}
   $$

**Answer: `26.77 Å`**

This is the corrected numerical answer. Note that $26.4\times10^{-70}=2.64\times10^{-69}$.

---

## 7. Question 7.59 — Minimum Spring Compression for a Roller-Coaster Loop

**Question:** A 914-kg roller-coaster car is launched by a spring with $k=30045$ N/m into a frictionless loop of radius $R=7.9$ m. Find the minimum spring compression that keeps the car on the track. Use $g=9.8$ m/s$^2$.

### Diagram

![[images/Rollercoaster_Loop_759.png]]

### Walkthrough

1. At the top of the loop, the minimum speed occurs when the normal force is zero. Gravity alone supplies the centripetal force:
   $$
   mg=\frac{mv_{\text{top}}^2}{R}
   \quad\Rightarrow\quad v_{\text{top}}^2=gR
   $$
2. Take the bottom/start level as zero gravitational potential energy. The spring's initial energy becomes gravitational potential energy and kinetic energy at the top:
   $$
   \frac12kx^2=mg(2R)+\frac12mv_{\text{top}}^2
   $$
3. Substitute $v_{\text{top}}^2=gR$:
   $$
   \frac12kx^2=2mgR+\frac12mgR=\frac52mgR
   $$
4. Solve:
   $$
   x=\sqrt{\frac{5mgR}{k}}
   =\sqrt{\frac{5(914)(9.8)(7.9)}{30045}}
   $$
   $$
   \boxed{x\approx3.43\text{ m}}
   $$

**Answer: `3.43 m`**

The car must still have speed $\sqrt{gR}$ at the top; merely reaching the top is not enough.

---

## 8. Answers at a Glance

| Problem | Answer |
|---|---:|
| 7.22 — Skater stopping distance | $33.8$ m |
| 7.30 — Force from potential energy | $+6.26$ nN |
| 7.37 — Energy stored in rope | $2707$ J |
| 7.45 — Block speed between springs | $17.7$ m/s |
| 7.52 — Ionic equilibrium separation | $26.77$ Å |
| 7.59 — Minimum loop spring compression | $3.43$ m |

## 9. Exam Checklist

1. **Friction stopping distance:** $d=v_i^2/(2\mu_k g)$ for a horizontal surface.
2. **Force from potential energy:** differentiate and include the minus sign: $F_x=-dU/dx$.
3. **Energy from a restoring force:** $U=-\int F\,dx$.
4. **Frictionless motion:** mechanical energy is conserved.
5. **Equilibrium position:** solve $dU/dr=0$ and check that it is a minimum.
6. **Loop-the-loop:** at the top, use $N+mg=mv^2/R$; at the minimum-contact threshold, $N=0$.
7. **Units:** convert aJ/nm to nN, meters to angstroms, and keep spring constants in SI units.
