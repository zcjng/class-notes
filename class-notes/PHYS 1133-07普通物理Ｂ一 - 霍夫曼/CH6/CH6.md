# CH6 — Work, Power, and Variable Forces

## 1. Formula Sheet

### Work by a constant force
$$
W=\vec F\cdot\vec d=Fd\cos\theta
$$
Here, $\theta$ is the angle between the force and displacement.

- Same direction: $W>0$
- Opposite direction: $W<0$
- Perpendicular: $W=0$

### Work by a variable force
$$
W=\int_{x_i}^{x_f}F_x(x)\,dx
$$
For a force in two dimensions:
$$
W=\int \vec F\cdot d\vec r
=\int_{x_i}^{x_f}F_x\,dx+\int_{y_i}^{y_f}F_y\,dy
$$
If the path is $y=y(x)$:
$$
W=\int_{x_i}^{x_f}\left[F_x(x,y(x))+F_y(x,y(x))\frac{dy}{dx}\right]dx
$$

### Power
$$
P=\frac{dW}{dt},\qquad W=\int_{t_i}^{t_f}P(t)\,dt
$$
At constant speed, when force is parallel to motion:
$$
P=Fv,\qquad F=\frac Pv
$$
Average power:
$$
P_{\text{avg}}=\frac{W}{t}
$$

### Motion at steady speed up a slope
If a car travels uphill at constant speed:
$$
F_{\text{drive}}=mg\sin\theta+F_{\text{resistance}}
$$
Since $P=F_{\text{drive}}v$:
$$
\sin\theta=\frac{P/v-F_{\text{resistance}}}{mg}
$$

### Useful integrals
$$
\int x^n\,dx=\frac{x^{n+1}}{n+1}+C\quad(n\ne-1)
$$
$$
\int x^{-1/2}\,dx=2\sqrt{x}+C
$$

---

## 2. Question 6.51 — Work Done by an Angled Rope

**Question:** You pull a box 47 m horizontally using a rope. The rope tension is 196 N, and the rope does 2610 J of work on the box. What angle (in radians) does the rope make with the horizontal?

### Diagram

![[images/Work_Angled_Rope.png]]

### Walkthrough

1. Use the constant-force work formula:
   $$
   W=Fd\cos\theta
   $$

2. Rearrange:
   $$
   \cos\theta=\frac{W}{Fd}
   $$

3. Substitute:
   $$
   \theta=\cos^{-1}\left(\frac{2610}{196(47)}\right)
   $$

4. Evaluate in radians:
   $$
   \boxed{\theta\approx1.284\text{ rad}}
   $$

**Answer: `1.284 rad`**

**Common mistake:** The displacement is horizontal, so only the horizontal component $F\cos\theta$ does work. Make sure the requested answer is in radians.

---

## 3. Question 6.70 — Work Done by a Long-Distance Runner

**Question:** A runner's power output is approximated by $P=m(bv-c)$, where $m$ is mass, $v$ is speed, $b=5.31$ and $c=4.34\text{ W/kg}$. Find the work done (kJ) by a 44-kg runner moving at 5.4 m/s over a 9-km race.

### Walkthrough

1. Find the power. Treat $b$ and $c$ as the constants with the units given in the question:
   $$
   P=m(bv-c)
   $$
   $$
   P=44[5.31(5.4)-4.34]
   $$
   $$
   P=1070.696\text{ W}
   $$

2. Find the time:
   $$
   t=\frac{d}{v}=\frac{9000}{5.4}=1666.67\text{ s}
   $$

3. For constant speed and constant power over the race:
   $$
   W=Pt
   $$
   $$
   W=1070.696(1666.67)\approx1.7845\times10^6\text{ J}
   $$

4. Convert joules to kilojoules:
   $$
   \boxed{W\approx1784.5\text{ kJ}}
   $$

**Answer: `1784.5 kJ`**

---

## 4. Question 6.72 — Car Ascending a Mountain Road

**Question:** A 1380-kg car ascends a mountain road at a steady 61 km/h against a 655-N force of air resistance. The engine supplies power to the drive wheels at 36 kW. Find the slope angle in radians. Use $g=9.8\text{ m/s}^2$.

### Diagram

![[images/Forces_Mountain_Road.png]]

### Walkthrough

1. Convert speed and power:
   $$
   v=61\frac{1000}{3600}=16.9444\text{ m/s}
   $$
   $$
   P=36\,000\text{ W}
   $$

2. Find the driving force using $P=Fv$:
   $$
   F_{\text{drive}}=\frac Pv=\frac{36000}{16.9444}
   \approx2124.59\text{ N}
   $$

3. The car moves at steady speed, so the net force along the slope is zero:
   $$
   F_{\text{drive}}=mg\sin\theta+655
   $$

4. Solve for the angle:
   $$
   \sin\theta=\frac{F_{\text{drive}}-655}{mg}
   $$
   $$
   \theta=\sin^{-1}\left(\frac{2124.59-655}{1380(9.8)}\right)
   $$

   $$
   \boxed{\theta\approx0.109\text{ rad}}
   $$

**Answer: `0.109 rad`**

---

## 5. Question 6.83 — Work by a Position-Dependent Force

**Question:** A force in the $x$-direction is given by $F(x)=1.2/\sqrt{x}$. Find the work done in moving an object from $x=0$ to $x=5.9\text{ m}$.

### Walkthrough

1. For a variable force:
   $$
   W=\int_{x_i}^{x_f}F(x)\,dx
   $$

2. Substitute:
   $$
   W=\int_0^{5.9}\frac{1.2}{\sqrt{x}}\,dx
   =1.2\int_0^{5.9}x^{-1/2}\,dx
   $$

3. Integrate:
   $$
   W=1.2[2\sqrt{x}]_0^{5.9}
   $$

4. Evaluate:
   $$
   W=2.4\sqrt{5.9}
   $$
   $$
   \boxed{W\approx5.83\text{ J}}
   $$

**Answer: `5.83 J`**

**Key idea:** A variable force requires integration. The integral is finite even though the expression grows very large near $x=0$.

---

## 6. Question 6.87 — Work Along a Curved Path

**Question:** A particle moves from the origin to $(x,y)=(3\text{ m},6\text{ m})$ along $y=ax^2-bx$, where $a=2\text{ m}^{-1}$ and $b=4$. The force is $\vec F=cxy\,\hat i+d\,\hat j$, where $c=10\text{ N/m}^2$ and $d=15\text{ N}$. Calculate the work done.

### Path diagram

![[images/Path_Particle_687.png]]

### Walkthrough

1. Write the path:
   $$
   y=2x^2-4x
   $$

2. Differentiate:
   $$
   \frac{dy}{dx}=4x-4
   $$
   Hence, $dy=(4x-4)dx$.

3. Use the dot product:
   $$
   dW=\vec F\cdot d\vec r=F_x\,dx+F_y\,dy
   $$
   With $F_x=cxy$ and $F_y=d$:
   $$
   W=\int_0^3[cxy+d(dy/dx)]\,dx
   $$

4. Substitute all expressions:
   $$
   W=\int_0^3[10x(2x^2-4x)+15(4x-4)]dx
   $$

5. Expand and integrate:
   $$
   W=\int_0^3(20x^3-40x^2+60x-60)dx
   $$
   $$
   W=\left[5x^4-\frac{40}{3}x^3+30x^2-60x\right]_0^3
   $$

6. Evaluate:
   $$
   W=405-360+270-180
   $$
   $$
   \boxed{W=135\text{ J}}
   $$

**Answer: `135 J`**

**Key idea:** For a curved path, include both components of the force in the dot product. The vertical contribution is $F_y\,dy$, not $F_y\,dx$.

---

## 7. Question 6.77 — Two Machines Doing Equal Work

**Question:** One machine works at constant power $P_0$. A second machine's power is
$$
P_2(t)=2P_0\left(1-\frac{(t-t_0)^2}{t_0^2}\right)
$$
Both start at $t=0$. Find the earliest time when both machines have done the same amount of work, in units of $t_0$.

### Walkthrough

1. Work is the integral of power over time:
   $$
   W(t)=\int_0^tP(\tau)\,d\tau
   $$

2. First machine:
   $$
   W_1=P_0t
   $$

3. Expand the second machine's power:
   $$
   P_2(t)=2P_0\left(1-\frac{(t-t_0)^2}{t_0^2}\right)
   =\frac{4P_0t}{t_0}-\frac{2P_0t^2}{t_0^2}
   $$

4. Integrate:
   $$
   W_2=\frac{2P_0t^2}{t_0}-\frac{2P_0t^3}{3t_0^2}
   $$

5. Set $W_1=W_2$:
   $$
   P_0t=\frac{2P_0t^2}{t_0}-\frac{2P_0t^3}{3t_0^2}
   $$

6. For the nonzero solution, let $x=t/t_0$ and divide by $P_0t$:
   $$
   1=2x-\frac23x^2
   $$
   $$
   2x^2-6x+3=0
   $$

7. Solve:
   $$
   x=\frac{3\pm\sqrt3}{2}
   $$

   Choose the smaller root for the earliest time:
   $$
   \boxed{\frac{t}{t_0}=\frac{3-\sqrt3}{2}\approx0.634}
   $$

**Answer: `0.634`** (meaning $t=0.634t_0$)

---

## 8. Answers at a Glance

| Problem | Answer |
|---|---:|
| 6.51 — Rope angle | $1.284\text{ rad}$ |
| 6.70 — Runner's work | $1784.5\text{ kJ}$ |
| 6.72 — Mountain road angle | $0.109\text{ rad}$ |
| 6.83 — Variable-force work | $5.83\text{ J}$ |
| 6.87 — Work along a curve | $135\text{ J}$ |
| 6.77 — Equal work time | $0.634t_0$ |

## 9. Exam Checklist

1. **Constant force:** use $W=Fd\cos\theta$.
2. **Variable force:** integrate $W=\int F_x(x)\,dx$.
3. **Curved path:** use $dW=F_x\,dx+F_y\,dy$.
4. **Power:** use $P=dW/dt$ or $P=Fv$ when force is along motion.
5. **Steady speed:** net force along the direction of motion is zero.
6. **Units:** convert km to m, km/h to m/s, kW to W, and J to kJ when requested.
7. **Earliest time:** if a quadratic gives two positive roots, choose the smaller one.
