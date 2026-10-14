Here’s a clean Markdown version you can copy directly into Obsidian. I’ve kept the formulas in standard LaTeX, organized each question into steps, and added free-body diagram descriptions without the unusual formatting.

# CH5 — Circular Motion and Forces

## 1. Formula Sheet

### 1.1 Circular Motion

**Centripetal acceleration**

$$
a_c=\frac{v^2}{r}=r\omega^2
$$

**Centripetal force**

$$
F_c=ma_c=\frac{mv^2}{r}=mr\omega^2
$$

**Linear speed and angular speed**

$$
v=r\omega
$$

**Period and frequency**

$$
f=\frac{1}{T}
$$

$$
\omega=2\pi f=\frac{2\pi}{T}
$$

**Convert angular speed to revolutions per minute**

$$
\text{rpm}=\frac{\omega}{2\pi}\times60
$$

Where:

- $v$: linear speed (m/s)
- $r$: radius (m)
- $\omega$: angular speed (rad/s)
- $T$: period (s)
- $f$: frequency (Hz)

**Important:** Centripetal force is not a separate force. It is the net force directed toward the center of the circular path.

$$
\boxed{\sum F_{\text{radial}}=\frac{mv^2}{r}}
$$

---

### 1.2 Newton's Second Law and Gravity

Newton's second law:

$$
\boxed{\sum F=ma}
$$

Gravitational force (weight):

$$
F_g=mg
$$

Use $g=9.8\text{ m/s}^2$ unless the question specifies otherwise.

---

### 1.3 Tension in a String

For a string making an angle $\theta$ with the **horizontal**, while an object moves in a horizontal circle:

Vertical force balance:

$$
T\sin\theta=mg
$$

Horizontal centripetal force:

$$
T\cos\theta=\frac{mv^2}{r}
$$

Therefore,

$$
T=\frac{mg}{\sin\theta}
$$

Remember: The sine and cosine components depend on how the angle is measured.

---

### 1.4 Vertical Circular Motion

At the top of a loop, the center of the circle is below the object.

Taking downward as positive:

$$
mg+F_s=\frac{mv^2}{r}
$$

The signed force exerted by the seat and seatbelts is

$$
\boxed{F_s=\frac{mv^2}{r}-mg}
$$

A negative answer means the actual force points upward.

---

### 1.5 Banked Turns with Friction

For a banked curve designed for speed $v_0$:

$$
\tan\theta=\frac{v_0^2}{rg}
$$

If the car travels faster than the design speed, friction acts down the bank.

The minimum coefficient of friction is

$$
\boxed{
\mu_s=
\frac{rg(v^2-v_0^2)}
{r^2g^2+v^2v_0^2}
}
$$

Where:

- $v$: actual speed
- $v_0$: design speed
- $r$: radius of the turn
- $\theta$: banking angle
- $\mu_s$: coefficient of static friction

---

### 1.6 Spring Force

Hooke's law:

$$
F_{\text{spring}}=kx
$$

If the spring's unstretched length is $L_0$ and its current length is $r$:

$$
x=r-L_0
$$

When the spring provides the centripetal force:

$$
k(r-L_0)=\frac{mv^2}{r}
$$

Rearranging:

$$
\boxed{kr^2-kL_0r-mv^2=0}
$$

This is a quadratic equation in $r$.

---

### 1.7 Inclined Plane with Friction

Gravity's component down the slope:

$$
F_{\parallel}=mg\sin\theta
$$

Normal force:

$$
N=mg\cos\theta
$$

Kinetic friction:

$$
f_k=\mu_kN
$$

Therefore,

$$
f_k=\mu_kmg\cos\theta
$$

Acceleration down the slope:

$$
\boxed{a=g(\sin\theta-\mu_k\cos\theta)}
$$

For a frictionless slope:

$$
a=g\sin\theta
$$

For an object starting from rest and moving with constant acceleration:

$$
\boxed{s=\frac12at^2}
$$

---

# 2. Question 5.22 — Rock Whirled in a Horizontal Circle

## Question

An 8.0-kg rock is whirled in a horizontal circle at the end of a 1.30-m-long string. If the breaking strength of the string is 133.9 N, what is the minimum angle (in degrees) the string can make with the horizontal? Let $g=9.8\text{ m/s}^2$.

## Given

- Mass: $m=8.0\text{ kg}$
- String length: $L=1.30\text{ m}$
- Maximum tension: $T_{\max}=133.9\text{ N}$
- Gravitational acceleration: $g=9.8\text{ m/s}^2$

## Free-Body Diagram

Forces acting on the rock:

- **Tension $T$:** Along the string toward the pivot.
- **Weight $mg$:** Vertically downward.
- **Centripetal acceleration:** Horizontally toward the center of the circular path.

The tension has two components:

- Vertical: $T\sin\theta$
- Horizontal: $T\cos\theta$

## Walkthrough

**Step 1: Balance the vertical forces.**

Since the rock stays at a constant height,

$$
T\sin\theta=mg
$$

**Step 2: Use the maximum tension.**

The minimum angle occurs at the maximum allowable tension.

$$
T_{\max}\sin\theta=mg
$$

**Step 3: Solve for the angle.**

$$
\sin\theta=\frac{mg}{T_{\max}}
$$

Substitute:

$$
\sin\theta=\frac{8.0(9.8)}{133.9}
$$

$$
\theta=\sin^{-1}(0.5855)
$$

Therefore,

$$
\boxed{\theta\approx35.8^\circ}
$$

**Answer: $35.8^\circ$**

### Key concept

If the angle is measured from the horizontal, the vertical component is $T\sin\theta$. If it is measured from the vertical, the vertical component is $T\cos\theta$.

---

# 3. Question 5.45 — Roller Coaster at the Top of a Loop

## Question

Riders on the Great American Revolution roller coaster negotiate a loop of radius 9.8 m at 3.0 m/s. At the top of the loop, what is the signed downward force exerted on a 9.0-kg rider by the seat and seatbelts? Let $g=9.8\text{ m/s}^2$.

## Given

- Mass: $m=9.0\text{ kg}$
- Radius: $r=9.8\text{ m}$
- Speed: $v=3.0\text{ m/s}$
- Gravity: $g=9.8\text{ m/s}^2$

## Free-Body Diagram

Forces acting on the rider:

- **Gravity $mg$:** Downward.
- **Seat and seatbelt force $F_s$:** Can point upward or downward.
- **Centripetal acceleration:** Downward, toward the center of the loop.

Take downward as the positive direction.

## Walkthrough

**Step 1: Calculate the required centripetal force.**

$$
F_c=\frac{mv^2}{r}
$$

$$
F_c=\frac{9.0(3.0)^2}{9.8}
$$

$$
F_c\approx8.27\text{ N}
$$

**Step 2: Calculate the rider's weight.**

$$
mg=9.0(9.8)=88.2\text{ N}
$$

**Step 3: Apply Newton's second law.**

Taking downward as positive:

$$
mg+F_s=\frac{mv^2}{r}
$$

Rearrange:

$$
F_s=\frac{mv^2}{r}-mg
$$

Substitute:

$$
F_s=8.27-88.2
$$

Therefore,

$$
\boxed{F_s=-79.9\text{ N}}
$$

**Answer: $-79.9\text{ N}$**

### Why is the answer negative?

Gravity alone produces more downward force than the centripetal force required at this speed. The seat and seatbelts must exert an upward force on the rider.

Because downward was defined as positive, the upward force has a negative sign.

---

# 4. Banked Turn — Minimum Coefficient of Friction

## Question

A car moving at 46.3 m/s negotiates a 144-m-radius banked turn designed for 17.4 m/s. What is the minimum coefficient of friction needed if the car is to stay on the road?

## Given

- Actual speed: $v=46.3\text{ m/s}$
- Design speed: $v_0=17.4\text{ m/s}$
- Radius: $r=144\text{ m}$
- Gravity: $g=9.8\text{ m/s}^2$

## Free-Body Diagram

Forces acting on the car:

- **Weight $mg$:** Vertically downward.
- **Normal force $N$:** Perpendicular to the road.
- **Static friction $f_s$:** Down the bank because the car is traveling faster than the design speed.

Friction acts down the slope to prevent the car from sliding upward and outward.

## Walkthrough

**Step 1: Compare the actual speed with the design speed.**

Since

$$
v>v_0
$$

the car tends to slide up the bank. Friction must act down the slope.

**Step 2: Use the formula for a banked turn with friction.**

$$
\mu_s=
\frac{rg(v^2-v_0^2)}
{r^2g^2+v^2v_0^2}
$$

**Step 3: Substitute the values.**

$$
\mu_s=
\frac{144(9.8)(46.3^2-17.4^2)}
{144^2(9.8)^2+(46.3)^2(17.4)^2}
$$

**Step 4: Calculate.**

$$
\boxed{\mu_s\approx0.984}
$$

**Answer: $0.984$**

### Key concept

- If $v=v_0$, friction is not required in the ideal model.
- If $v>v_0$, friction acts down the bank.
- If $v<v_0$, friction acts up the bank.

The coefficient of friction has no units.

---

# 5. Question 5.64 — Space Station Simulating Earth's Gravity

## Question

A space station is shaped like a hollow ring with a diameter of 4.7 m. At how many revolutions per minute (r/min) should it rotate to simulate Earth's gravity, so that the normal force on an astronaut at the outer edge equals the astronaut's weight on Earth?

## Given

- Diameter: $d=4.7\text{ m}$
- Gravity: $g=9.8\text{ m/s}^2$

## Free-Body Diagram

Forces acting on the astronaut:

- **Normal force $N$:** Inward, toward the center of the rotating station.
- **Gravity:** In an idealized space station, Earth's gravitational effect is negligible compared with the simulated gravity from rotation.

The inward normal force provides the required centripetal force.

## Walkthrough

**Step 1: Calculate the radius.**

$$
r=\frac d2=\frac{4.7}{2}
$$

$$
r=2.35\text{ m}
$$

**Step 2: Set the normal force equal to the astronaut's Earth weight.**

$$
N=\frac{mv^2}{r}=mg
$$

Cancel the mass:

$$
\frac{v^2}{r}=g
$$

Since $v=r\omega$:

$$
r\omega^2=g
$$

Solve for angular speed:

$$
\omega=\sqrt{\frac gr}
$$

**Step 3: Calculate angular speed.**

$$
\omega=\sqrt{\frac{9.8}{2.35}}
$$

$$
\omega\approx2.042\text{ rad/s}
$$

**Step 4: Convert to revolutions per minute.**

$$
\text{rpm}=\frac{\omega}{2\pi}\times60
$$

$$
\text{rpm}=\frac{2.042}{2\pi}\times60
$$

Therefore,

$$
\boxed{\text{rpm}\approx19.5}
$$

**Answer: $19.5\text{ rpm}$**

### Key concept

To simulate Earth's gravity, the required centripetal acceleration must equal $g$.

$$
a_c=g
$$

Remember to convert diameter to radius before using the formula.

---

# 6. Spring on a Frictionless Air Table

## Question

A 2.4-kg mass is connected to a spring with spring constant $k=156\text{ N/m}$ and unstretched length 36.3 cm. The system is mounted on a frictionless air table, with the free end of the spring attached to a frictionless pivot. The mass moves in a circle at 3.0 m/s. Find the radius of its path.

## Given

- Mass: $m=2.4\text{ kg}$
- Spring constant: $k=156\text{ N/m}$
- Unstretched length: $L_0=36.3\text{ cm}=0.363\text{ m}$
- Speed: $v=3.0\text{ m/s}$

## Free-Body Diagram

Forces acting on the mass:

- **Spring force:** Radially inward, toward the pivot.
- **Normal force:** Vertically upward.
- **Weight $mg$:** Vertically downward.

The normal force and weight cancel vertically. The spring provides the centripetal force.

## Walkthrough

**Step 1: Set the spring force equal to the centripetal force.**

The spring stretches by

$$
x=r-L_0
$$

Hooke's law gives

$$
F_{\text{spring}}=k(r-L_0)
$$

Therefore,

$$
k(r-L_0)=\frac{mv^2}{r}
$$

**Step 2: Rearrange into a quadratic equation.**

Multiply both sides by $r$:

$$
kr(r-L_0)=mv^2
$$

Expand:

$$
kr^2-kL_0r-mv^2=0
$$

**Step 3: Substitute the values.**

$$
156r^2-156(0.363)r-2.4(3.0)^2=0
$$

Simplify:

$$
156r^2-56.628r-21.6=0
$$

**Step 4: Apply the quadratic formula.**

For $ar^2+br+c=0$:

$$
r=\frac{-b\pm\sqrt{b^2-4ac}}{2a}
$$

Substitute:

$$
r=\frac{56.628\pm\sqrt{56.628^2+4(156)(21.6)}}{312}
$$

Choose the positive root because the radius must be positive.

$$
\boxed{r\approx0.596\text{ m}}
$$

**Answer: $0.596\text{ m}$**

### Key concept

Use the spring's extension, not its total length:

$$
x=r-L_0
$$

Do not use $F=k r$ unless the spring's unstretched length is zero.

---

# 7. Inclined Slide — Coefficient of Friction

## Question

A slide inclined at $35^\circ$ takes bathers into a swimming pool. Water is sprayed onto the slide to make it essentially frictionless. A bather spends only one-third as much time on the slide as when it is dry. Find the coefficient of friction on the dry slide.

## Given

- Incline angle: $\theta=35^\circ$
- Wet-slide time: $t_w=\frac13t_d$
- Gravity: $g=9.8\text{ m/s}^2$

Assume the bather starts from rest and travels the same distance in both cases.

## Free-Body Diagram

Forces acting on the bather:

- **Weight $mg$:** Vertically downward.
- **Normal force $N$:** Perpendicular to the slide.
- **Kinetic friction $f_k$:** Up the slope, opposing the bather's downward motion.

Resolve gravity into two components:

$$
F_{\parallel}=mg\sin\theta
$$

$$
F_{\perp}=mg\cos\theta
$$

Thus,

$$
N=mg\cos\theta
$$

## Walkthrough

**Step 1: Relate distance, acceleration, and time.**

Since the bather starts from rest:

$$
s=\frac12at^2
$$

For the same distance, acceleration is inversely proportional to the square of time:

$$
a\propto\frac1{t^2}
$$

**Step 2: Use the time ratio.**

Given

$$
t_w=\frac13t_d
$$

we obtain

$$
\frac{a_d}{a_w}=\frac{t_w^2}{t_d^2}
$$

Therefore,

$$
\boxed{a_d=\frac19a_w}
$$

**Step 3: Find the acceleration on the wet slide.**

The wet slide is frictionless:

$$
a_w=g\sin\theta
$$

**Step 4: Find the acceleration on the dry slide.**

Friction opposes the motion:

$$
a_d=g\sin\theta-\mu_k g\cos\theta
$$

**Step 5: Substitute the acceleration ratio.**

$$
g\sin\theta-\mu_k g\cos\theta
=\frac19g\sin\theta
$$

Cancel $g$:

$$
\sin\theta-\mu_k\cos\theta
=\frac19\sin\theta
$$

Rearrange:

$$
\mu_k\cos\theta=\frac89\sin\theta
$$

Therefore,

$$
\mu_k=\frac89\tan\theta
$$

**Step 6: Calculate.**

$$
\mu_k=\frac89\tan35^\circ
$$

$$
\boxed{\mu_k\approx0.622}
$$

**Answer: $0.622$**

### Key concept

Time ratios are squared when converted into acceleration ratios for the same distance and initial conditions.

If the time is three times longer, the acceleration is nine times smaller.

---

# 8. Free-Body Diagram Checklist

Before solving a force problem, ask yourself:

1. **What object am I analyzing?** Draw only forces acting on that object.
2. **What real forces act on it?** Common forces include gravity, tension, normal force, friction, and spring force.
3. **Where is the center of the circle?** Centripetal acceleration always points toward it.
4. **Which direction is positive?** State the sign convention before writing the equations.
5. **Is friction opposing motion or preventing slipping?** Determine its direction from the situation.
6. **Have I resolved angled forces correctly?** Check whether the angle is measured from the horizontal or vertical.
7. **Are the units consistent?** Convert centimeters to meters and seconds to the correct units.

## 9. Answers at a Glance

| Problem | Answer |
|---|---:|
| Rock and string: minimum angle | $35.8^\circ$ |
| Roller coaster: signed seat force | $-79.9\text{ N}$ |
| Banked turn: minimum friction coefficient | $0.984$ |
| Space station: rotation rate | $19.5\text{ rpm}$ |
| Spring and circular motion: radius | $0.596\text{ m}$ |
| Inclined slide: friction coefficient | $0.622$ |

## 10. Highest-Priority Formulas for the Exam

Memorize these first:

**Circular acceleration**

$$
a_c=\frac{v^2}{r}
$$

**Net radial force**

$$
\sum F_{\text{radial}}=\frac{mv^2}{r}
$$

**Hooke's law**

$$
F_{\text{spring}}=kx
$$

**Friction**

$$
f_k=\mu_kN
$$

**Inclined-plane acceleration**

$$
a=g(\sin\theta-\mu_k\cos\theta)
$$

**Motion from rest with constant acceleration**

$$
s=\frac12at^2
$$

**Final reminder:** Draw the free-body diagram first, then apply Newton's second law. Centripetal force is the net inward force, not an additional force to include on the diagram.



