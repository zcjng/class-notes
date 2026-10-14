# CH5 — Circular Motion and Forces

## 1. Formula Sheet

### Circular motion
$$a_c=\frac{v^2}{r}=r\omega^2$$
$$\sum F_{\text{radial}}=\frac{mv^2}{r}$$
$$v=r\omega,\qquad f=\frac1T,\qquad \omega=2\pi f=\frac{2\pi}{T}$$
$$\text{rpm}=\frac{\omega}{2\pi}\times60$$
$$\sum F=ma,\qquad F_g=mg$$

### Tension: angle measured from horizontal
$$T\sin\theta=mg,\qquad T\cos\theta=\frac{mv^2}{r}$$

### Vertical circular motion (top of loop; downward positive)
$$mg+F_s=\frac{mv^2}{r}$$
$$F_s=\frac{mv^2}{r}-mg$$

### Banked turn with friction
$$\tan\theta=\frac{v_0^2}{rg}$$
$$\mu_s=\frac{rg(v^2-v_0^2)}{r^2g^2+v^2v_0^2}$$

### Spring force
$$F_{\text{spring}}=kx,\qquad x=r-L_0$$
$$k(r-L_0)=\frac{mv^2}{r}$$
$$kr^2-kL_0r-mv^2=0$$

### Inclined plane with friction
$$N=mg\cos\theta,\qquad f_k=\mu_kN$$
$$a=g(\sin\theta-\mu_k\cos\theta)$$
$$a_{\text{frictionless}}=g\sin\theta,\qquad s=\frac12at^2$$

---

## 2. Question 5.22 — Rock Whirled in a Horizontal Circle

**Question:** An 8.0-kg rock is whirled in a horizontal circle at the end of a 1.30-m string. The string's breaking strength is 133.9 N. Find the minimum angle the string can make with the horizontal. Use $g=9.8\text{ m/s}^2$.

### Free-body diagram

![[images/FBD_Rock.png]]

- Tension $T$ acts along the string toward the pivot.
- Weight $mg$ acts vertically downward.
- The horizontal component of tension provides centripetal force.
- Since the angle is measured from horizontal, the vertical component is $T\sin\theta$.

### Walkthrough

1. Balance vertical forces:
   $$T\sin\theta=mg$$
2. Use maximum tension:
   $$T_{\max}\sin\theta=mg$$
3. Solve:
   $$\theta=\sin^{-1}\left(\frac{mg}{T_{\max}}\right)$$
   $$\theta=\sin^{-1}\left(\frac{8.0(9.8)}{133.9}\right)$$
   $$\boxed{\theta\approx35.8^\circ}$$

**Answer: $35.8^\circ$**

---

## 3. Question 5.45 — Roller Coaster at the Top of a Loop

**Question:** A 9.0-kg rider is at the top of a loop with radius 9.8 m, moving at 3.0 m/s. Find the signed downward force exerted on the rider by the seat and seatbelts. Use $g=9.8\text{ m/s}^2$.

### Free-body diagram

![[images/FBD_Rollercoaster.png]]

- Weight $mg$ points downward.
- The seat/seatbelt force can point upward or downward.
- Centripetal acceleration points downward, toward the loop's center.
- Take downward as positive.

### Walkthrough

1. Centripetal force:
   $$F_c=\frac{mv^2}{r}=\frac{9.0(3.0)^2}{9.8}\approx8.27\text{ N}$$
2. Weight:
   $$mg=9.0(9.8)=88.2\text{ N}$$
3. Apply Newton's second law:
   $$mg+F_s=\frac{mv^2}{r}$$
4. Solve:
   $$F_s=\frac{mv^2}{r}-mg=8.27-88.2$$
   $$\boxed{F_s\approx-79.9\text{ N}}$$

**Answer: $-79.9\text{ N}$.** The negative sign means the seat and seatbelts exert an upward force.

---

## 4. Banked Turn — Minimum Coefficient of Friction

**Question:** A car moving at 46.3 m/s negotiates a 144-m-radius banked turn designed for 17.4 m/s. Find the minimum coefficient of friction.

### Free-body diagram

![[images/FBD_BankedTurn.png]]

- Weight $mg$ acts vertically downward.
- Normal force $N$ acts perpendicular to the road.
- Since $v>v_0$, friction acts down the bank to prevent sliding up and outward.

### Walkthrough

1. Compare speeds: $46.3>17.4$, so friction acts down the bank.
2. Use:
   $$\mu_s=\frac{rg(v^2-v_0^2)}{r^2g^2+v^2v_0^2}$$
3. Substitute:
   $$\mu_s=\frac{144(9.8)(46.3^2-17.4^2)}{144^2(9.8)^2+46.3^2(17.4)^2}$$
   $$\boxed{\mu_s\approx0.984}$$

**Answer: $0.984$**

---

## 5. Question 5.64 — Space Station Simulating Earth's Gravity

**Question:** A hollow ring-shaped space station has diameter 4.7 m. Find the rotation rate in rpm so the normal force on an astronaut equals their Earth weight.

### Free-body diagram

![[images/FBD_SpaceStation.png]]

- Normal force $N$ points inward toward the center.
- The normal force provides the centripetal force.

### Walkthrough

1. Radius:
   $$r=\frac{4.7}{2}=2.35\text{ m}$$
2. Set the normal force equal to Earth weight:
   $$\frac{mv^2}{r}=mg$$
   $$r\omega^2=g$$
3. Angular speed:
   $$\omega=\sqrt{\frac{9.8}{2.35}}\approx2.042\text{ rad/s}$$
4. Convert to rpm:
   $$\text{rpm}=\frac{2.042}{2\pi}\times60$$
   $$\boxed{\text{rpm}\approx19.5}$$

**Answer: $19.5\text{ rpm}$**

---

## 6. Spring on a Frictionless Air Table

**Question:** A 2.4-kg mass is attached to a spring with $k=156\text{ N/m}$ and unstretched length 36.3 cm. It moves in a circle at 3.0 m/s on a frictionless air table. Find the radius.

### Free-body diagram

![[images/FBD_Spring.png]]

- Spring force points inward toward the pivot.
- Normal force and weight cancel vertically.
- There is no friction.

### Walkthrough

1. Convert length:
   $$L_0=36.3\text{ cm}=0.363\text{ m}$$
2. Equate spring and centripetal force:
   $$k(r-L_0)=\frac{mv^2}{r}$$
3. Rearrange:
   $$kr^2-kL_0r-mv^2=0$$
4. Substitute:
   $$156r^2-56.628r-21.6=0$$
5. Take the positive quadratic root:
   $$r=\frac{56.628+\sqrt{56.628^2+4(156)(21.6)}}{312}$$
   $$\boxed{r\approx0.596\text{ m}}$$

**Answer: $0.596\text{ m}$**

---

## 7. Inclined Slide — Coefficient of Friction

**Question:** A slide is inclined at $35^\circ$. The frictionless wet slide takes a bather one-third as long to descend as the dry slide. Find the coefficient of friction on the dry slide.

### Free-body diagram

![[images/FBD_InclinedSlide.png]]

- Weight $mg$ acts vertically downward.
- Normal force $N$ is perpendicular to the slide.
- Friction acts up the slope, opposing downward motion.
- Gravity's component down the slope is $mg\sin\theta$.

### Walkthrough

1. From rest over the same distance:
   $$s=\frac12at^2$$
2. Since $t_w=\frac13t_d$:
   $$\frac{a_d}{a_w}=\frac{t_w^2}{t_d^2}=\frac19$$
   $$a_d=\frac19a_w$$
3. Wet slide:
   $$a_w=g\sin\theta$$
4. Dry slide:
   $$a_d=g\sin\theta-\mu_k g\cos\theta$$
5. Equate:
   $$g\sin\theta-\mu_k g\cos\theta=\frac19g\sin\theta$$
   $$\mu_k=\frac89\tan\theta$$
6. Substitute $\theta=35^\circ$:
   $$\boxed{\mu_k=\frac89\tan35^\circ\approx0.622}$$

**Answer: $0.622$**

---

## 8. Free-Body Diagram Checklist

1. Identify the object being analyzed.
2. Draw only real forces acting on it.
3. Locate the center of the circular path.
4. Point centripetal acceleration toward the center.
5. Choose a positive direction.
6. Resolve angled forces into components.
7. Determine friction's direction from the motion or tendency to slip.
8. Apply Newton's second law and check units.

**Remember:** Centripetal force is the net inward force, not an extra force to draw.

## 9. Answers at a Glance

| Problem | Answer |
|---|---:|
| Rock and string: minimum angle | $35.8^\circ$ |
| Roller coaster: signed seat force | $-79.9\text{ N}$ |
| Banked turn: minimum friction coefficient | $0.984$ |
| Space station: rotation rate | $19.5\text{ rpm}$ |
| Spring and circular motion: radius | $0.596\text{ m}$ |
| Inclined slide: friction coefficient | $0.622$ |

## 10. Highest-Priority Formulas

$$a_c=\frac{v^2}{r}$$

$$\sum F_{\text{radial}}=\frac{mv^2}{r}$$

$$F_{\text{spring}}=kx$$

$$f_k=\mu_kN$$

$$a=g(\sin\theta-\mu_k\cos\theta)$$

$$s=\frac12at^2$$
