# CH9 — Momentum, Collisions, and Explosions

> [!abstract] Chapter focus
> Use conservation of momentum for collisions and explosions. For elastic collisions, combine momentum conservation with kinetic-energy conservation. When objects move under gravity after an explosion, also use kinematics.

## 1. Formula Sheet

### Momentum
$$
\boxed{\vec p=m\vec v}
$$

### Conservation of momentum
For an isolated system:
$$
\boxed{\sum \vec p_i=\sum \vec p_f}
$$
Apply this separately to each coordinate in two-dimensional problems:
$$
\sum p_x:\quad \sum m v_{x,i}=\sum m v_{x,f}
$$
$$
\sum p_y:\quad \sum m v_{y,i}=\sum m v_{y,f}
$$

### Perfectly inelastic collision
Objects stick together:
$$
\boxed{m_1v_1+m_2v_2=(m_1+m_2)v_f}
$$
Kinetic energy is generally not conserved.

### Elastic collision
Both momentum and kinetic energy are conserved:
$$
m_1u_1+m_2u_2=m_1v_1+m_2v_2
$$
$$
\frac12m_1u_1^2+\frac12m_2u_2^2
=\frac12m_1v_1^2+\frac12m_2v_2^2
$$
For a 1D elastic collision where mass $m_2$ is initially at rest:
$$
\boxed{v_1=\frac{m_1-m_2}{m_1+m_2}u_1}
$$
$$
\boxed{v_2=\frac{2m_1}{m_1+m_2}u_1}
$$

### Kinetic energy
$$
\boxed{K=\frac12mv^2}
$$

### Projectile motion after an explosion
Taking upward as positive:
$$
\boxed{y=y_0+v_{0y}t-\frac12gt^2}
$$
At the top of a vertical trajectory, the velocity immediately before the explosion is zero.

### Motion at constant speed
$$
\boxed{t=\frac{d}{v}}
$$

---

## 2. Question 9.26 — Neutron and Deuteron Collision

**Question:** A neutron of mass $1u$ with velocity $(7.9\hat e_x+1.4\hat e_y)$ Mm/s strikes a deuteron of mass $2u$, forming a tritium nucleus of mass $3u$ with velocity $(9.9\hat e_x+7.8\hat e_y)$ Mm/s. Find the deuteron's initial speed.

### Diagram

![[images/Momentum_Vectors_926.png]]

### Walkthrough

1. Conserve momentum as a vector:
   $$
   m_n\vec v_n+m_d\vec v_d=m_t\vec v_t
   $$
2. Solve for the deuteron's velocity:
   $$
   \vec v_d=\frac{m_t\vec v_t-m_n\vec v_n}{m_d}
   =\frac{3\vec v_t-\vec v_n}{2}
   $$
3. $x$ component:
   $$
   v_{dx}=\frac{3(9.9)-7.9}{2}=10.9\ \mathrm{Mm/s}
   $$
4. $y$ component:
   $$
   v_{dy}=\frac{3(7.8)-1.4}{2}=11.0\ \mathrm{Mm/s}
   $$
5. Find the magnitude:
   $$
   v_d=\sqrt{10.9^2+11.0^2}
   \approx15.5\ \mathrm{Mm/s}
   $$

**Answer: `15.5 Mm/s`**

---

## 3. Question 9.60 — Totally Inelastic Collision

**Question:** Two masses $m_1>m_2$ move in opposite directions with equal speed $v$. They stick together in a totally inelastic collision, losing half their initial kinetic energy. Find $m_1/m_2$.

### Diagram

![[images/Inelastic_Collision_960.png]]

### Walkthrough

1. Choose $m_1$'s direction as positive. Momentum conservation gives:
   $$
   (m_1-m_2)v=(m_1+m_2)v_f
   $$
   Hence:
   $$
   v_f=\frac{m_1-m_2}{m_1+m_2}v
   $$
2. Initial kinetic energy:
   $$
   K_i=\frac12(m_1+m_2)v^2
   $$
3. Since half the initial kinetic energy is lost, $K_f=K_i/2$:
   $$
   \frac12(m_1+m_2)v_f^2=\frac14(m_1+m_2)v^2
   $$
4. Substitute the final velocity:
   $$
   \left(\frac{m_1-m_2}{m_1+m_2}\right)^2=\frac12
   $$
5. Let $r=m_1/m_2$. Since $m_1>m_2$:
   $$
   \frac{r-1}{r+1}=\frac1{\sqrt2}
   $$
   Solving:
   $$
   r=3+2\sqrt2\approx5.83
   $$

**Answer: `5.83`**

---

## 4. Question 9.65 — Fireworks Rocket Explodes at Its Peak

**Question:** A rocket is launched vertically at $40$ m/s. At the peak, it explodes into two equal-mass fragments. One reaches the ground 2.87 s after the explosion. When does the second reach the ground?

### Diagram

![[images/Rocket_Explosion_965.png]]

### Walkthrough

1. Find the height at the peak:
   $$
   h=\frac{v_0^2}{2g}=\frac{40^2}{2(9.8)}
   \approx81.63\ \mathrm{m}
   $$
2. Let upward be positive. For the first fragment, $y=-h$ after $t_1=2.87$ s:
   $$
   -h=u_1t_1-\frac12gt_1^2
   $$
   Therefore:
   $$
   u_1=\frac{-h+\frac12gt_1^2}{t_1}
   \approx-14.38\ \mathrm{m/s}
   $$
3. At the explosion, the rocket's momentum is zero. The equal masses must have opposite velocities:
   $$
   u_2=-u_1=+14.38\ \mathrm{m/s}
   $$
4. For the second fragment:
   $$
   -h=u_2t-\frac12gt^2
   $$
   The positive root is:
   $$
   t=\frac{u_2+\sqrt{u_2^2+2gh}}{g}
   \approx5.81\ \mathrm{s}
   $$

**Answer: `5.81 s after the explosion`**

---

## 5. Question 9.66 — Explosion into Two Equal-Mass Pieces

**Question:** An object initially moving at speed $v$ explodes into two equal-mass pieces, each moving at $26.8v$. Find the two angles relative to the original direction.

### Walkthrough

1. Let the original mass be $M$. Each piece has mass $M/2$.
2. The initial momentum is $Mv$. Since the fragments have equal mass and equal speed, their transverse momentum components must cancel.
3. If the two directions are symmetric at angles $\pm\theta$:
   $$
   Mv=2\left(\frac M2\right)(26.8v)\cos\theta
   $$
4. Cancel $Mv$:
   $$
   \cos\theta=\frac1{26.8}
   $$
5. Thus:
   $$
   \theta=\cos^{-1}\left(\frac1{26.8}\right)
   \approx87.9^\circ
   $$

**Answer: `+87.9° and −87.9°`**

> [!warning] Check the given speed
> This answer assumes the text really says $26.8v$. If the original says $2.68v$, the answer changes to approximately $\pm68.1^\circ$.

---

## 6. Question 9.82 — Elastic Collision and a Second Meeting

**Question:** A $100$ g block starts from rest at height $b=4.0$ cm on a frictionless incline of angle $8.2^\circ$. It slides to the bottom and collides elastically with a stationary block of mass $4M$, located $d=1.8$ m from the bottom. How long after the first collision do the blocks meet again? Use $g=9.8$ m/s².

### Diagram

![[images/Elastic_Collision_Return_982.png]]

### Walkthrough

1. Find the speed at the bottom using energy conservation:
   $$
   Mgb=\frac12Mv^2
   $$
   $$
   v=\sqrt{2gb}=\sqrt{2(9.8)(0.040)}
   \approx0.8854\ \mathrm{m/s}
   $$
2. For an elastic collision with a stationary mass $4M$:
   $$
   v_1'=\frac{M-4M}{M+4M}v=-0.6v
   $$
   $$
   v_2'=\frac{2M}{M+4M}v=0.4v
   $$
   Numerically, $v_1'=-0.5313$ m/s and $v_2'=0.3542$ m/s.
3. The first block moves left across distance $d$ to the incline:
   $$
   t_{\mathrm{flat}}=\frac{d}{0.5313}\approx3.388\ \mathrm{s}
   $$
4. It rises a short distance up the incline and returns. Its initial uphill speed is $0.5313$ m/s and its acceleration along the slope has magnitude $g\sin\theta$:
   $$
   t_{\mathrm{up+down}}=\frac{2(0.5313)}{9.8\sin8.2^\circ}
   \approx0.760\ \mathrm{s}
   $$
5. It returns to the bottom at about:
   $$
   t_r=3.388+0.760=4.148\ \mathrm{s}
   $$
   It is then moving right at $0.5313$ m/s. The $4M$ block has moved right at $0.3542$ m/s since the first collision.
6. Let $t$ be the total time from the first collision. Taking the initial collision point as $x=0$, the first block's position after returning to the bottom is:
   $$
   x_1=-d+0.5313(t-t_r)
   $$
   The second block's position is:
   $$
   x_2=0.3542t
   $$
   Set $x_1=x_2$:
   $$
   -1.8+0.5313(t-4.148)=0.3542t
   $$
   Solving with the rounded intermediate values gives:
   $$
   \boxed{t\approx22.6\ \mathrm{s}}
   $$

**Answer: `22.6 s after the first collision`**

---

## 7. Question 9.89 — Astronaut Throws a Tool Kit

**Question:** A $66.2$ kg astronaut is $479$ m from the ISS and initially at rest relative to it. She throws a $21.7$ kg tool kit away from the station at $7.8$ m/s relative to herself. How long does it take her to return?

### Walkthrough

1. Initial total momentum is zero. Take toward the station as positive:
   $$
   m_av_a+m_kv_k=0
   $$
2. The kit moves away from the astronaut at $7.8$ m/s, so:
   $$
   v_a-v_k=7.8
   $$
3. Combining the equations:
   $$
   v_a=\frac{m_k}{m_a+m_k}(7.8)
   $$
   $$
   v_a=\frac{21.7}{66.2+21.7}(7.8)
   \approx1.926\ \mathrm{m/s}
   $$
4. Assuming constant speed:
   $$
   t=\frac{479}{1.926}\approx249\ \mathrm{s}
   $$

**Answer: `249 s`**

---

## 8. Answer Summary

| Problem | Topic | Answer |
|---|---|---:|
| 9.26 | Neutron–deuteron momentum conservation | $15.5$ Mm/s |
| 9.60 | Totally inelastic collision | $5.83$ |
| 9.65 | Rocket explosion and projectile motion | $5.81$ s |
| 9.66 | Explosion into equal-mass pieces | $\pm87.9^\circ$ |
| 9.82 | Elastic collision and return to collision point | $22.6$ s |
| 9.89 | Astronaut and thrown tool kit | $249$ s |

---

## 9. Exam Checklist

- [ ] Write momentum conservation before substituting numbers.
- [ ] For 2D problems, conserve $x$ and $y$ momentum separately.
- [ ] For a totally inelastic collision, objects stick together; kinetic energy is not conserved.
- [ ] For an elastic collision, conserve both momentum and kinetic energy.
- [ ] In explosions, total momentum immediately before and after is conserved.
- [ ] For explosions during vertical motion, combine momentum conservation with projectile-motion equations.
- [ ] In relative-speed problems, distinguish an object's speed relative to the ground from its speed relative to another object.
- [ ] Convert grams to kilograms and centimeters to meters before using SI equations.
