# OS Scheduling Visualizer

An interactive, browser-based learning tool that animates how an operating system schedules **disk requests** and **CPU processes**. Built for *Assignment 2 – Operating Systems* as an open learning aid for students who find static scheduling diagrams hard to follow.

**SDG mapping:** SDG 4 (Quality Education) · SDG 9 (Industry, Innovation & Infrastructure) · link to SDG 7 (energy-aware scheduling)

> Live demo: `<add GitHub Pages link>`  ·  Demo video: `<add link>`

---

## Features

### Part A – Disk scheduling
- Algorithms: **FCFS, SSTF, SCAN, C-SCAN, LOOK**
- Inputs: disk size (cylinders), initial head position, request queue, direction
- Outputs: animated **seek graph** (cylinder vs request order), **service sequence**, **total head movement**
- **Comparison chart:** runs all five algorithms on the same input and shows an animated bar chart of total head movement
- Built-in test cases: assignment sample and an SSTF-starvation case
- Option to count or ignore the C-SCAN return jump

### Part B – CPU scheduling (two algorithms)
- **Round Robin** with a user-set time quantum (with optional 1 ms context-switch overhead)
- **SRTF** (Shortest Remaining Time First, preemptive)
- Inputs: process name, arrival time, burst time (add / remove processes)
- Outputs: animated **Gantt chart**, plus completion, turnaround, waiting and response time per process and their averages

### Common
- **Step-by-step mode** (Previous / Next, keyboard ← →) and **auto-play** with speed control
- **Input validation** (e.g. head position outside the disk range, non-numeric or out-of-range requests, empty queue)
- No installation, no dependencies, works offline

---

## Setup

No build step is needed.

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

Then open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari).

Optional: enable **GitHub Pages** (Settings → Pages → deploy from `main` branch) to host it online.

---

## How to use

1. Choose the **Part A** or **Part B** tab.
2. Enter your input, or pick a test case from the dropdown.
3. Press **Auto-play**, or use **Next / Previous** to walk through one move at a time.
4. Check the comparison bar chart (Part A) or the metrics table (Part B).

---

## Sample inputs

### Part A – assignment sample
| Field | Value |
| --- | --- |
| Disk size | 200 (cylinders 0–199) |
| Head position | 50 |
| Requests | 82, 170, 43, 140, 24, 16, 190 |

**Expected total head movement**

| Algorithm | Direction | Total movement |
| --- | --- | --- |
| FCFS | – | **642** |
| SSTF | – | **208** |
| SCAN | towards higher | 332 |
| SCAN | towards lower | 240 |
| C-SCAN (jump counted) | towards higher | 391 |
| C-SCAN (jump counted) | towards lower | 366 |
| LOOK | towards higher | 314 |
| LOOK | towards lower | 208 |

> SCAN and C-SCAN travel to the disk ends (0 and 199). LOOK only goes as far as the last request.

### Part A – SSTF starvation case
| Field | Value |
| --- | --- |
| Disk size | 200 |
| Head position | 100 |
| Requests | 199, 101, 99, 102, 98, 103, 97, 104, 96 |

Requests keep arriving close to the head, so SSTF serves the far request **199 last**, while FCFS serves it first. Select SSTF and watch the service sequence.

### Part B – CPU scheduling sample (Round Robin, quantum 2 ms)
| Process | Arrival | Burst |
| --- | --- | --- |
| P1 | 0 | 5 |
| P2 | 1 | 3 |
| P3 | 2 | 4 |
| P4 | 3 | 2 |

Try the same table with **SRTF** and compare average waiting time using the *Compare* button.

---

## Screenshots

Add your screenshots to a `screenshots/` folder and link them here:

```markdown
![Seek graph](screenshots/seek-graph.png)
![Disk comparison chart](screenshots/disk-comparison.png)
![CPU Gantt chart](screenshots/gantt.png)
```

Suggested shots: seek graph mid-animation, comparison bar chart, validation error, Round Robin Gantt chart, SRTF Gantt chart.

---

## How the algorithms work (short)

| Algorithm | Idea | Trade-off |
| --- | --- | --- |
| FCFS | Serve in arrival order | Fair but lots of head movement |
| SSTF | Always go to the nearest request | Low movement, can starve far requests |
| SCAN | Sweep to one end, then reverse | Bounded wait, travels to the ends |
| C-SCAN | Sweep one way, jump back, sweep again | More uniform wait times |
| LOOK | Like SCAN but turns at the last request | Less wasted movement than SCAN |
| Round Robin | Each process gets a fixed quantum | Good response time, overhead from switches |
| SRTF | Run the process with the least remaining time | Low average waiting, can starve long jobs |

---

## Project structure

```
.
├── index.html        # the whole app (HTML + CSS + JavaScript)
├── README.md
└── screenshots/      # add your images here
```

---

## Limitations and future work

- Disk requests are all known in advance; no requests arrive while the head is moving.
- SSTF ties are broken by queue order.
- SRTF does not charge context-switch overhead.
- Only two CPU algorithms are implemented (Round Robin, SRTF).
- Ideas: add SJF, Priority and FCFS for CPU; add C-LOOK; export results as CSV or PNG; estimate energy use from head movement; add dynamic request arrival.

---

## Author

`<Your name / team>` · `<Roll number>` · `<College>`

Course: Operating Systems · Assignment 2

## License

MIT (or your choice) – free to use as a learning aid.
