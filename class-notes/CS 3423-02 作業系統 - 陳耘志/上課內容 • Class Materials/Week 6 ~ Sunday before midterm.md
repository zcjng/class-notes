---
title: 第六週 • 週日
course: CS 3423-02 作業系統
week: 6
day: Sunday
tags:
  - os
---

# 第六週 • 週日 — CPU Scheduling (Worksheet 6, Midterm Review)

> [!abstract] Main idea
> After every timer interrupt or blocking system call, the kernel must answer one question: **who runs next?** The code that answers it is the **scheduler**, and the rule it follows is the **scheduling policy**.
> - **Metrics** → turnaround, response, waiting time, throughput, utilization, fairness
> - **Preemptive vs cooperative** → when is the scheduler allowed to decide?
> - **FIFO / SJF / STCF** → short-first policies minimize average turnaround
> - **Round robin** → trades turnaround for response time
> - **Priority + aging** → interactive responsiveness without starvation
> - **CFS** → run the process with the smallest vruntime; nice values change how fast vruntime grows

Midterm: **Monday, Oct. 19, 10:00–12:00**, Weeks 1–6 including this worksheet. Prepare by studying the worksheets and redoing Quiz 1–4.

## 1. What does a scheduler decide?

One CPU core can run only one process at a time. After the kernel regains control (timer interrupt or a blocking system call), the **scheduler** picks the next process from the ready queue.

Homework analogy (Capoo = CPU, Dogdog = professor, assignments = processes, tray = ready queue):

- Picking the next assignment is the scheduler's main job.
- Grade weight (30% vs 1%) is like a **priority**.
- A time slice is how long you may focus before being interrupted.
- Deciding takes time, and **switching has a cost**: save state (PCB), load state (PCB), reload memory/cache, lose momentum.

## 2. CPU bursts and I/O bursts

A program alternates between two phases:

- **CPU burst** — it computes. The CPU time a burst needs is its **burst time** (e.g. a 6 ms burst).
- **I/O burst** — it waits for disk/keyboard/etc., sleeps, and needs **no CPU at all**.

| Type | Pattern | Examples |
| --- | --- | --- |
| **I/O-bound** | many short CPU bursts + long waits | VS Code (waits for you to type), editors, most interactive programs |
| **CPU-bound** | long CPU bursts, rarely waits | compiling (`gcc`, `make -j1`), rendering video, training a model, `ffmpeg` transcode |

The compiler wants lots of CPU; the editor wants only a little CPU but wants it **right now**. A good scheduler favors waking interactive (I/O-bound) processes for response time, while CPU-bound jobs soak up the remaining CPU.

**Classify:** `ffmpeg` 4K transcode → CPU-bound · `bash` prompt waiting for input → I/O-bound · web server sending small files → I/O-bound · `make -j1` building the Linux kernel → CPU-bound.

## 3. Scheduling metrics

A **job** = a process, or one CPU burst. Burst = the CPU time it needs to finish.

| Metric | Formula | Meaning, who cares |
| --- | --- | --- |
| **Turnaround time** | done − arrive | Enter-to-finish; you care waiting for a build |
| **Response time** | first run − arrive | Arrive-to-first-CPU; you care when typing |
| **Waiting time** | turnaround − burst | Total time spent in the ready queue (CPU "wasted" for this job) |
| **Throughput** | jobs done ÷ time | Work finished per second; server-farm owner cares |
| **CPU utilization** | useful time ÷ total time | Share of time doing real work (not idle, not switching) |
| **Fairness** | — | Every job gets its share; no job waits forever (starving = waiting forever) |

### Worked example (timeline reading)

Capoo arrives at t = 2 while Dog is running. First runs at t = 5, preempted at 7, runs 9→11, finishes at 11. Burst = 4 ms.

- Response = 5 − 2 = **3 ms**
- Turnaround = 11 − 2 = **9 ms**
- Waiting = 9 − 4 = **5 ms** (the two grey ready-queue stretches: 3 + 2)

> [!note] Waiting ≠ response in general
> Here waiting is 5 but response is 3, because Capoo was preempted and waited again mid-run.

### Response time vs throughput (the fundamental tradeoff)

To respond fast, the scheduler must interrupt long jobs often — but every interruption is a context switch, and switches cost time, lowering throughput.

- **Big slices (8 ms):** key press waits 7.35 ms, only 3 switches, 22.4 of 24 ms useful.
- **Small slices (2 ms):** key press waits 1.35 ms, 10 switches, only 20 of 24 ms useful.

Response time is also critical in **real-time systems** (e.g. a car's braking controller): miss the deadline and people can die.

**Worked example:** Job A arrives at 0, needs 3 ms; Job B arrives at 1, needs 2 ms. CPU runs A 0→3, B 3→5. B: response = 3 − 1 = **2**, turnaround = 5 − 1 = **4**, waiting = 4 − 2 = **2 ms**. (B never preempted, so waiting = response here — not true in general.)

## 4. Preemptive vs non-preemptive scheduling

- **Preemptive:** the kernel can stop a running process at any time (timer interrupt) and give the CPU to another, even mid-work.
- **Non-preemptive (cooperative):** a process keeps the CPU until it gives it up itself — finishes, blocks on I/O, or calls `yield()`. If a process 站著毛坑不拉屎 (`while(1){}`), the whole system hangs.

### The four scheduling points

| # | Moment | Process transition |
| --- | --- | --- |
| ① | `read()`, `sleep()`, `wait()` — block | running → waiting |
| ② | Timer tick: slice used up | running → ready (preempted) |
| ③ | I/O done: process wakes up | waiting → ready |
| ④ | `exit()` | running → terminated |

- **Cooperative scheduler decides only at ① and ④** — the moments the running process leaves the CPU by itself.
- **Preemptive scheduler can also decide at ② and ③** — the kernel takes (or may take) the CPU away.

### Why early systems were cooperative — and why they can't be today

Early personal computers (Apple Mac System 6/7, DOS, Windows 3.x) had < 1 MB RAM and ran one main app at a time. Cooperative scheduling avoided timer-interrupt and switch costs on slow hardware. The price: the foreground app got everything — a busy Photoshop froze your mail; a stuck Word froze the machine; only a reboot helped.

On **shared** Unix machines the kernel couldn't trust all users' programs: one user's infinite loop would lock out everyone. So Unix **had to** use preemptive scheduling — still true today (a crypto-mining customer must not take over a 100-core server).

- Cooperative + `while(1){}`: kernel never runs again, key press waits forever.
- Preemptive: at the next tick the kernel runs `vim`, which shows the key.

> [!warning] Cost of preemption
> A process can be stopped mid-update of data shared with another process — you will meet this again in **synchronization**.

## 5. FIFO, SJF and STCF

Assume one CPU, jobs that only compute (no I/O), and known bursts.

### FIFO (FCFS)

First arrived runs first, until done. Simple, feels fair — but the **convoy effect**: short jobs stuck behind a long one.

Dog = 60 ms, Capoo = 6 ms, Mika = 6 ms, all arrive at 0:

- FIFO (Dog first): turnarounds 60, 66, 72 → **average 66 ms**. Capoo and Mika wait 60 ms for 6 ms of work.

### SJF (shortest job first)

Run the smallest burst first. Same three jobs: Capoo, Mika, Dog → turnarounds 6, 12, 72 → **average 30 ms**. Dog finishes a bit later (72 vs 60), everyone else much sooner.

**Why short-first wins:** each job's turnaround includes the bursts of all jobs before it. A long burst at the front is added into many turnarounds; at the back it's added only once. With simultaneous arrivals, SJF gives the **smallest possible average turnaround**.

### STCF (shortest time-to-completion first) — SJF with preemption

Jobs don't arrive together. If Capoo and Mika arrive at t = 6 (Dog already running), non-preemptive SJF can't stop Dog → average back up to **62 ms**.

STCF re-decides on every arrival: compare the new job with the **remaining** time of the running job; preempt if the new job is shorter. Average back to **30 ms**, average response just **2 ms**.

| Policy | arrivals | avg turnaround |
| --- | --- | --- |
| FIFO | all at 0 | 66 |
| SJF | all at 0 | 30 |
| SJF | Capoo, Mika at 6 | 62 |
| STCF | Capoo, Mika at 6 | 30 |

> [!warning] The long job pays for everyone
> Under STCF with simultaneous arrivals, Dog's turnaround doubles (60 → 72). Short-first wins the averages but hurts the long job. SJF can also **starve** a long job if short jobs keep arriving.

### Worked example: the 7-Eleven checkout (Walkthrough 1)

Dog = 8 min of items (starts at t = 0), Capoo = 4 items (arrives t = 1), Mika = 2 items (arrives t = 2):

| Policy | Dog TR | Capoo TR | Mika TR | avg TR | avg response |
| --- | --- | --- | --- | --- | --- |
| FIFO | 8 | 11 | 12 | 10.33 | 5.67 |
| SJF | 8 | 13 | 8 | 9.67 | 5 |
| STCF | 14 | 6 | 2 | 7.33 | 0 |

- FIFO: Dog 0→8, Capoo 8→12, Mika 12→14. Responses 0, 7, 10.
- SJF: Dog 0→8 (only job at t=0), then Mika 8→10, Capoo 10→14. Responses 0, 9, 6.
- STCF: Dog 0→1, Capoo 1→2 (preempts), Mika 2→4 (preempts), Capoo 4→7, Dog 7→14. Every arrival runs immediately → all responses 0.
- **Best for Dog? FIFO or SJF** (done at 8). STCF doubles Dog's turnaround to 14.

### Worked example: STCF with a late Mika (Predict)

Dog 60, Capoo 6, Mika 6. Capoo arrives t = 6 → preempts Dog (6 < 54), runs 6→12. Dog resumes. At t = 30 Dog still has 36 left; Mika needs 6 → preempts, runs 30→36. **Mika's response = 0.** Dog finishes at 72. Turnarounds: Dog 72, Capoo 6, Mika 6 → **average 28 ms**.

### But how does the scheduler know the burst?

It doesn't! It **guesses** from the past. Common guess — exponential average, newer bursts count more:

$$\text{next guess} = \alpha \times \text{last burst} + (1-\alpha) \times \text{previous guess}$$

Example, α = ½: previous guess 10 ms, last burst 6 ms → ½ × 6 + ½ × 10 = **8 ms**. A process that keeps doing short bursts quickly gets a short guess.

## 6. Round robin (RR)

For interactive use, **response time** matters more than turnaround. RR: each job runs for at most one **time slice (quantum)**; when it's used up, the timer fires and the job goes to the **back** of the ready queue. Everyone gets a little CPU very soon.

Dog, Capoo, Mika each need 4 ms, all arrive at 0:

| | avg response | avg turnaround |
| --- | --- | --- |
| FIFO / SJF | 4 ms | 8 ms |
| RR, q = 1 ms | 1 ms | 11 ms |
| RR, q = 2 ms | 2 ms | 10 ms |

RR improves response but **stretches turnaround** (every job is spread over the whole run). It sits between FIFO and tiny-quantum RR, as expected.

### How long should a slice be?

Every switch costs time (save registers, run scheduler, load next process, then run with a cold cache). Worksheet numbers — four jobs (8, 4, 12, 2 ms), switch = 0.25 ms:

- q = 0.5 ms: response 11.4 ms... (fast) but **29% of the CPU wasted on switches**.
- q = 12 ms: little waste, but response as bad as FIFO.

Rule: slice ≫ switch cost (a real switch is a few µs, typical slices a few **ms** → overhead stays a few percent), but short enough that a human doesn't notice the wait.

### RR with I/O

When an I/O-bound job blocks, it **leaves** the ready queue entirely (sleeps), so a CPU-bound job uses the CPU meanwhile — the CPU isn't idle while the editor waits for your key. But plain RR treats a just-woken job like everyone else (back of the queue), so the editor may still wait out the compiler's slice. Part 6 fixes this with a priority boost.

## 7. Priority scheduling

Every process has a priority; the scheduler always picks the **highest-priority** ready process. A newly ready higher-priority process **preempts** the running one.

### 累擱 (lag) and the interactive boost

Playing a CPU-bound shooting game while typing chat: each keypress is a tiny ~0.5 ms task. If the scheduler doesn't pause the game right away, the letters appear slowly. So schedulers **raise the priority of interactive programs**: when you press a key, the kernel wakes the editor at high priority, the scheduler preempts the game immediately, the editor runs its tiny burst and sleeps again. The game barely notices; the computer feels responsive.

### Starvation and aging

If high-priority processes keep arriving, a low-priority process may **never** run = **starvation**.

**Aging**: dynamic priorities — a process waiting a long time gets its priority raised step by step (e.g. +1 every 2 ms of waiting). Example: L (priority 1) beside a stream of H jobs (priority 3–4); with aging L climbs to 4 by t = 6, runs, finishes at t = 9. Without aging, L never runs.

Two goals:

- **Fairness**: every process gets CPU in proportion to its priority.
- **Liveness**: even the lowest-priority process runs within a finite time — free from starvation.

### `nice`: priority in Linux

A process's **nice value** sets its priority. Higher nice = nicer to others = **less important**:

- **−20** = highest priority (least nice)
- **+19** = lowest priority (nicest)
- **0** = default; a child inherits the parent's nice value

```bash
$ nice -n 15 ./my-video-encoder     # compile/render without lagging the desktop
$ renice -n 19 -p 4242             # make a running process even nicer
```

A normal user may only make processes **nicer**; negative nice needs root (otherwise everyone would self-assign −20).

## 8. The Completely Fair Scheduler (CFS)

CFS was Linux's standard scheduler until 6.6 (2023); its ideas remain. Goal: **fairness** — one runnable process gets 100%, two get 50% each, four get 25% each. (The CPU can't literally run at 25% speed; CFS tracks who is behind and runs the process that is **most behind**.)

### vruntime

Instead of fixed slices, CFS gives each process a **vruntime** (virtual runtime = CPU time received so far, weighted). CFS always picks the process with the **lowest vruntime**.

Nice values map to **weights** (nice 0 = 1024; each level ≈ 1.25×). While running:

$$\Delta vruntime = \Delta runtime \times \frac{1024}{weight}$$

| nice | weight | vruntime growth per 1 ms on CPU |
| --- | --- | --- |
| −5 (more important) | 3121 | +0.33 ms |
| 0 (default) | 1024 | +1.00 ms |
| +5 (nicer) | 335 | +3.06 ms |
| +10 (even nicer) | 110 | +9.31 ms |

- Important process (negative/low nice) → big weight → vruntime grows **slowly** → stays among the smallest → picked more often.
- Nice process → small weight → vruntime grows **quickly** → picked less often.
- **No starvation**: a waiting process's vruntime stops growing, so the others eventually catch up.

Worked example: A and B nice 0, C nice 5, sched_latency = 6 ms, over 12 ms → CPU share **A 43% · B 43% · C 14%** (C's vruntime line is 3× steeper, so all lines meet after C has used much less CPU).

### Scheduling latency and slices

CFS tries to run every runnable process once per **scheduling latency** (base 6 ms, scaled up on many-CPU machines). Each process's slice = its share:

$$slice = latency \times \frac{weight}{total\ weight}$$

More runnable processes → shorter slices.

**Run queue** = the set of ready processes (not sleeping on I/O, not running).

### Red-black tree pick (optional reading)

The run queue is a red-black tree sorted by vruntime; the smallest vruntime is always the **leftmost** node, and the kernel caches a pointer to it → picking is **O(1)**, insertion **O(log n)**.

Preemption happens when: the running process used up its slice and someone else waits, **or** another process becomes ready with a **clearly smaller** vruntime.

### What replaced CFS? EEVDF (Linux 6.6+, optional reading)

EEVDF keeps vruntime, nice weights, and the red-black tree. The pick rule changes: each process gets a **virtual deadline** from the slice it asks for; among processes **not ahead of their fair share** (eligible), run the one with the **earliest deadline**. This gives latency-sensitive processes short, frequent turns without CFS's accumulated special cases.

### Worked example: CPU shares under CFS (Predict)

Two CPU-bound processes on one core, nice 0 vs nice 5: weights 1024 and 335 → shares 1024/1359 ≈ **75%** and 335/1359 ≈ **25%**. Both nice 5: equal weights → **50/50**. Nice values only matter **relative to other processes** — a lone process with nice 19 still gets the whole CPU.

## 9. Multi-core scheduling (optional reading)

On multi-core CPUs the scheduler also picks **which core** runs a process — tricky because of caches.

- Process A ran on Core 0 (its data is hot in Core 0's L1/L2). A is preempted, B runs; Core 1 is idle. Should A **migrate** to Core 1? On Core 1, A starts with a cold cache — every access misses to main memory. Often waiting a moment for Core 0 beats migrating.
- **One shared run queue**: simple, all cores busy, but every decision locks the same queue (slow with many cores), and processes jump between cores (caches suffer).
- **One run queue per core** (Linux): a core only locks its own queue; processes stay put → warm caches. Cost: **load imbalance** (one core busy, its neighbor 躺平).
- **Load balancing / work stealing**: periodically move work from busy cores to idle ones; goal is **work-conserving** (no core idle while work waits) — but balancing costs time and fights **cache affinity**. The paper *"The Linux Scheduler: a Decade of Wasted Cores"* (EuroSys 2016) found bugs that left cores idle while runnable threads waited elsewhere.
- **Soft affinity**: the scheduler's best-effort preference to keep a process on the same core (may be overridden). **Hard affinity (pinning)**: force it yourself:

```bash
$ taskset -c 0 ./my-process      # only ever run on core 0
```

Pinning can raise the cache hit rate, but the scheduler can no longer move the process even when another core is idle.

## 10. Midterm drill — all worked numbers in one place

| Problem | Answer |
| --- | --- |
| Capoo timeline (arrive 2, first run 5, preempted 7, run 9→11, burst 4) | response 3, turnaround 9, waiting 5 |
| B after A (A: 0, 3 ms; B: 1, 2 ms) | B: response 2, turnaround 4, waiting 2 |
| Dog 60 / Capoo 6 / Mika 6, all at 0 — FIFO avg TR | 66 ms |
| same, SJF avg TR | 30 ms |
| same, Capoo+Mika arrive 6 — SJF avg TR | 62 ms |
| same, STCF avg TR / avg response | 30 ms / 2 ms |
| Exponential average, α = ½, guess 10, burst 6 | next guess 8 ms |
| 7-Eleven FIFO avg TR / avg response | 10.33 / 5.67 |
| 7-Eleven SJF avg TR / avg response | 9.67 / 5 |
| 7-Eleven STCF avg TR / avg response | 7.33 / 0 (STCF worst for Dog: 14 vs 8) |
| STCF, Mika arrives 30 | Mika response 0; Dog done 72; avg TR 28 |
| RR q = 1, three 4 ms jobs | avg response 1, avg TR 11 |
| RR q = 2, three 4 ms jobs | avg response 2, avg TR 10 |
| nice 0 vs nice 5 CPU shares | ≈ 75% vs 25% (weights 1024 vs 335) |
| A, B nice 0 + C nice 5 over 12 ms | 43% / 43% / 14% |
| α = ½ weight relation | each nice level ≈ 1.25× weight; Δvruntime = Δruntime × 1024/weight |

## 11. Cheat sheet — one-line mental model

```
FIFO    → first come first served; simple; convoy effect; short jobs suffer
SJF     → smallest burst first; min avg turnaround (simultaneous arrivals); non-preemptive
STCF    → SJF + preempt on arrival; min avg turnaround/response; long job pays; can starve
RR      → everyone gets a slice; best response, worse turnaround; slice ≫ switch cost
Priority→ highest priority runs; interactive boost kills lag; starvation → aging (liveness)
CFS     → lowest vruntime runs; nice = weight = vruntime speed; fair share, no starvation
EEVDF   → CFS + virtual deadlines among eligible processes (Linux ≥ 6.6)
```

```
PREEMPTIVE     → timer can take the CPU anytime; decides at block, tick, wakeup, exit
COOPERATIVE    → process must give up the CPU; decides only at block and exit; one bad
                 loop freezes the machine
RESPONSE ↑ ⟂ THROUGHPUT ↑ → more preemption = faster response, more switch overhead
TURNAROUND = done − arrive · RESPONSE = first run − arrive · WAITING = turnaround − burst
```

---

## Flashcards

What is a scheduler, and what question does it answer?
??
The scheduler is the part of the kernel that picks which ready process runs next on the CPU. Every time the timer interrupt fires or a process blocks, it answers: "who runs next?"

What is a scheduling policy?
??
The rule the scheduler follows when choosing the next process (e.g. FIFO, SJF, RR, priority, CFS).

What is a CPU burst?
??
A phase in which a program is computing and needs the CPU. Its length is the job's burst time.

What is an I/O burst?
??
A phase in which a program waits for something like disk or keyboard input; it sleeps and needs no CPU at all.

How do I/O-bound and CPU-bound programs differ?
??
I/O-bound programs have many short CPU bursts and long waits (editors, interactive programs); CPU-bound programs have long CPU bursts and rarely wait (compilers, video rendering).

Why can't a scheduler treat all programs the same way?
??
They behave differently: I/O-bound programs want little CPU but want it immediately for responsiveness, while CPU-bound programs want as much CPU as possible. A good scheduler must serve both demands.

Why should a scheduler favor a just-woken interactive process?
??
Because humans perceive delay: a keypress task that needs only ~0.5 ms must run almost immediately or the user feels lag. A CPU-bound background job barely notices giving up the CPU briefly.

What is turnaround time?
??
Turnaround = done − arrive. The total time from job submission to completion, including waiting and running.

What is response time?
??
Response = first run − arrive. How long until the job gets the CPU for the first time.

What is waiting time?
??
Waiting = turnaround − burst. The total time the job spent sitting in the ready queue — time the scheduler "wasted" for that job.

What is throughput?
??
Jobs completed per unit of time (jobs done ÷ time).

What is CPU utilization?
??
The fraction of time the CPU does useful work, not idling and not switching (useful time ÷ total time).

What is starvation?
??
When a job waits forever because other jobs are always chosen ahead of it.

Why do response time and throughput conflict?
??
To respond fast, the scheduler must interrupt long jobs often, but each interruption is a context switch that costs time — reducing useful work. Small slices give fast response but more switch overhead; large slices waste little but respond slowly.

Roughly how do real slice lengths and switch costs compare?
??
A real context switch takes a few microseconds and typical time slices are a few milliseconds, so switch overhead stays at a few percent.

What is preemptive scheduling?
??
The kernel can stop a running process at any time (via timer interrupt) and give the CPU to another process, even if the first isn't finished.

What is non-preemptive (cooperative) scheduling?
??
A process keeps the CPU until it gives it up itself: it finishes, blocks on I/O, or calls yield().

What is the main danger of cooperative scheduling?
??
A process stuck in a loop (e.g. while (1) {}) never gives up the CPU, so the kernel never runs again and the whole system hangs.

Name the four moments at which the scheduler may run.
??
1. A process blocks on I/O (running → waiting).
2. A timer tick uses up the slice (running → ready).
3. I/O completes and wakes a process (waiting → ready).
4. A process exits (running → terminated).

At which scheduling points does a cooperative scheduler decide?
??
Only when the running process leaves the CPU by itself: blocking (①) and exiting (④).

At which additional points does a preemptive scheduler decide?
??
At the timer tick (②) and at wakeup (③) — the moments the kernel can take the CPU away or a new competitor appears.

Why did early personal computers use cooperative scheduling?
??
They were slow with < 1 MB of RAM, users ran one app at a time, and cooperative scheduling avoided the cost of frequent timer interrupts and context switches on such hardware.

Why was cooperative scheduling unacceptable on shared Unix machines?
??
The kernel couldn't trust users' programs: one user's infinite loop would lock out every other user on the machine. Unix needed preemption to keep control of the CPU.

What price does preemption pay?
??
A process can be stopped halfway through updating shared data, which later causes synchronization problems.

What is FIFO (FCFS) scheduling?
??
The job that arrived first runs first, until it is done. Simple and intuitively fair, but short jobs can wait a long time behind a long job (the convoy effect).

What is the convoy effect?
??
Short jobs stuck behind one long job at the front of the queue: the long burst is added into everyone's turnaround, so average turnaround is bad.

What is SJF scheduling?
??
Shortest Job First: always run the ready job with the smallest burst. Non-preemptive — it only decides when the CPU becomes free.

Why does SJF minimize average turnaround when all jobs arrive together?
??
Each job's turnaround includes the bursts of all jobs before it. Putting long bursts at the back adds them to the fewest turnarounds, so short-first minimizes the sum.

What is STCF?
??
Shortest Time-to-Completion First: SJF with preemption. Whenever a new job arrives, compare its burst with the running job's remaining time; run whichever is shorter.

Why does STCF need preemption?
??
Jobs don't all arrive at once. Without preemption a short job that arrives while a long one runs is stuck behind it; STCF must be able to pause the long job on arrival.

What is the main fairness problem with SJF/STCF?
??
If short jobs keep arriving, a long job may never be selected — it starves.

How can the kernel estimate a job's burst if it cannot know the future?
??
By guessing from the past, typically an exponential average of recent bursts where newer bursts count more: next guess = α × last burst + (1 − α) × previous guess.

Compute: α = ½, previous guess 10 ms, last burst 6 ms.
??
½ × 6 + ½ × 10 = 8 ms.

What is round robin?
??
Each ready job runs for at most one time slice (quantum); when the slice is used up the timer fires and the job goes to the back of the ready queue, so everyone gets a turn soon.

How does RR compare to FIFO on response time and turnaround?
??
RR improves average response time dramatically (everyone starts quickly) but worsens average turnaround (every job is stretched over the whole run).

For three 4 ms jobs arriving at 0, compare FIFO and RR q = 1.
??
FIFO: avg response 4 ms, avg turnaround 8 ms. RR (q = 1): avg response 1 ms, avg turnaround 11 ms.

For three 4 ms jobs and RR with q = 2, what are the average response and turnaround?
??
Average response 2 ms (first runs at 0, 2, 4), average turnaround 10 ms — between RR q = 1 and FIFO, as expected.

How does RR interact with I/O-bound processes?
??
When an I/O-bound job blocks it leaves the ready queue entirely, so a CPU-bound job uses the CPU meanwhile; but a just-woken job rejoins at the back, so it may still wait for the rest of someone's slice.

Why must the time slice be much longer than a context-switch cost?
??
Otherwise a large fraction of the CPU is spent switching instead of doing useful work (in the worksheet example, q = 0.5 ms with 0.25 ms switches wastes 29% of the CPU).

What is priority scheduling?
??
Every process has a priority; the scheduler always picks the ready process with the highest priority, preempting a lower-priority running process when a higher-priority one becomes ready.

Why do schedulers boost the priority of interactive processes?
??
So tiny latency-sensitive tasks (like displaying a keypress) run immediately even when CPU-hungry background jobs exist; the editor runs its short burst and sleeps again, and the game barely notices.

What is aging?
??
Dynamically raising a waiting process's priority step by step (e.g. +1 per 2 ms of waiting) so that eventually even a low-priority process is important enough to run.

What problem does aging solve?
??
Starvation of low-priority processes under a stream of high-priority arrivals.

What are fairness and liveness as scheduling goals?
??
Fairness: every process gets CPU time in proportion to its priority. Liveness: even the lowest-priority process runs within a finite amount of time (free from starvation).

What is a Linux nice value?
??
A per-process priority setting: −20 is the highest priority (least nice), +19 the lowest (nicest), 0 the default. A child inherits its parent's nice value.

Which direction is "more important" in Linux nice values, and why the confusing name?
??
Smaller (more negative) = more important. It is called "nice" because a higher nice value means the process voluntarily runs at a lower priority — it is being nice to everyone else.

Who may set a negative nice value?
??
Only root (or with CAP_SYS_NICE). A normal user may only make processes nicer (raise the nice value); otherwise everyone would give themselves −20.

What is CFS?
??
The Completely Fair Scheduler, Linux's default scheduler until version 6.6: it gives every runnable process a fair CPU share by always running the process with the smallest vruntime.

What is vruntime?
??
Virtual runtime: how much (weighted) CPU time a process has received so far. CFS always picks the ready process with the lowest vruntime — the one most behind.

How does the nice value affect vruntime growth?
??
It sets the process's weight. Δvruntime = Δruntime × 1024 / weight. Nice 0 has weight 1024 so vruntime grows 1:1; an important (negative nice) process has a big weight and its clock runs slow; a nice process has a small weight and its clock runs fast.

How much does weight change per nice level, and what are some exact weights?
??
About 1.25× per level. Nice −5: 3121, nice 0: 1024, nice +5: 335, nice +10: 110.

How does CFS give important processes more CPU without starving others?
??
Their vruntime grows slowly, so they stay among the smallest vruntimes and are picked more often. But while a process waits its vruntime stops growing, so the others catch up and it is eventually picked too.

Two processes, nice 0 and nice 5 — what CPU shares does CFS give?
??
Weights 1024 vs 335: approximately 75% and 25%. If both are nice 5, weights are equal and each gets 50%. Nice only matters relative to other processes; a lone process with nice 19 still gets the whole CPU.

What is the scheduling latency in CFS?
??
A period (base 6 ms, scaled up with more CPUs) within which CFS tries to run every runnable process once. Each process's slice = latency × weight / total weight.

What is a run queue?
??
The set of processes that are ready to run on a CPU — not running, and not sleeping/waiting for I/O.

How does CFS pick the next process with its red-black tree?
??
The run queue is a red-black tree sorted by vruntime; the smallest vruntime is always the leftmost node, and the kernel caches a pointer to it — so picking is O(1) and inserting a process is O(log n).

When does CFS preempt the running process?
??
When the process has used up its slice and another process is waiting, or when another process becomes ready with a clearly smaller vruntime.

What is EEVDF, and how does it differ from CFS?
??
EEVDF (Earliest Eligible Virtual Deadline First), Linux's default since 6.6, keeps vruntime, nice weights and the red-black tree, but picks the process with the earliest virtual deadline among those not ahead of their fair share — giving latency-sensitive processes short frequent turns without CFS's special cases.

What is task migration, and why is it expensive?
??
Moving a process from one core to another. The process loses its warm L1/L2 cache on the old core and starts with cache misses on the new one, which can cost more than waiting a few microseconds to resume on the same core.

What is the tradeoff between a shared run queue and one run queue per core?
??
A shared queue keeps every core busy and is simple, but every decision locks it (slow with many cores) and processes jump between cores. Per-core queues (Linux) are fast and cache-friendly, but can become load-imbalanced.

What is load balancing (work stealing) in Linux?
??
Periodically moving processes from busy cores to idle ones so that no core sits idle while work waits somewhere (work-conserving) — at the cost of balancing overhead and lost cache affinity.

What is soft affinity vs hard affinity?
??
Soft affinity is the scheduler's best-effort preference to keep a process on the same core (it can override it). Hard affinity (pinning) forces a process onto specific cores, e.g. with taskset.

What did the paper "The Linux Scheduler: a Decade of Wasted Cores" find?
??
Load-balancing bugs that left CPU cores idle while runnable threads waited on other cores' queues.

What is the overall mental model of Worksheet 6?
??
The kernel's scheduler decides who runs next whenever control returns to it. Metrics are turnaround, response, waiting, throughput, utilization and fairness. Preemptive scheduling is required on shared systems. Short-first policies (SJF/STCF) minimize average turnaround, round robin maximizes responsiveness, priority scheduling with aging balances both without starvation, and Linux's CFS implements fairness by running the smallest vruntime, with nice values weighting how fast vruntime grows.
