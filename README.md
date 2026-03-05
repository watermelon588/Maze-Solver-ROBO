
# Autonomous Line Follower & Maze Solver Robot

An **autonomous robotics project** that combines **PID-based line following** with **maze exploration and shortest-path solving**.
The robot is designed to compete in **line maze solving competitions** where the robot must first **map the maze** and then execute the **fastest optimized path**.

The robot uses **Arduino Nano, an 8-sensor IR array, TB6612FNG motor driver, and N20 metal gear motors**.

---

# Features

* PID-controlled high-speed line following
* Junction detection (T, cross, dead-end)
* Autonomous maze exploration
* Path recording during exploration
* Shortest-path optimization
* Final run using optimized route
* LCD status display
* Modular firmware architecture

---

# Competition Objective

The robot is designed for a **two-stage robotics competition**:

### Day 1 — Qualifier Track

Robot must follow a **black line on a white background** which may include:

* curves
* S-bends
* 90° turns
* small line gaps
* straight sections

Top teams qualify for Day 2.

---

### Day 2 — Maze Solver

The robot performs two phases:

#### Phase 1 — Mapping Run

Robot explores the maze and records decisions.

#### Phase 2 — Final Run

Robot executes the **shortest path discovered during mapping**.

Maze may contain:

* T junctions
* dead ends
* loops
* intersections
* multiple branches

---

# Hardware Components

## Controller

* Arduino Nano (ATmega328P)

## Sensors

* 8 × TCRT5000 IR sensors

## Motor System

* 2 × N20 600RPM metal gear motors
* TB6612FNG motor driver
* 42mm rubber wheels

## Power

* 7.4V Li-ion battery (1200–1500mAh)
* LM2596 buck converter (5V regulation)

## Interface

* 16×2 LCD display with I2C module
* Mode selection toggle switch
* Start/reset push button

## Chassis

* Custom acrylic chassis (14 × 12 cm)

---

# System Architecture

```
Battery (7.4V)
      │
      ├── TB6612FNG → Motors
      │
      └── Buck Converter → 5V
              │
              ├── Arduino Nano
              ├── IR Sensor Array
              └── LCD Display
```

---

# Software Architecture

The firmware is divided into three major functional stages:

```
1. Line Following
2. Maze Exploration
3. Maze Solving
```

Each module is implemented separately for modular development.

---

# PID Line Following

The robot uses a **PID control system** to maintain alignment with the line.

PID components:

```
P → Proportional correction
I → accumulated error correction
D → future error prediction
```

Correction formula:

```
correction = P * error
           + I * total_error
           + D * (error - previous_error)
```

Motor speeds are adjusted accordingly.

---

# Maze Exploration Algorithm

During exploration the robot follows a decision priority rule:

```
Left → Straight → Right → Backtrack
```

Each decision is stored in a path array:

```
L = Left
S = Straight
R = Right
B = Back
```

Example recorded path:

```
L S R S L B
```

---

# Path Optimization

Dead-end loops are simplified using path compression rules.

Example:

```
L B R → S
L B S → R
R B L → S
```

After optimization:

```
L S R S L B
→
S S R
```

The optimized path is executed during the final run.

---

# Repository Structure (Demo)

```
maze-line-follower-robot/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── event-rules.md
│   ├── robot-architecture.md
│   ├── wiring-diagram.md
│   └── algorithm-explanation.md
│
├── hardware/
│   ├── chassis/
│   │     chassis-design.png
│   │
│   ├── circuit/
│   │     wiring-diagram.png
│   │
│   └── components.md
│
├── firmware/
│   ├── main.ino
│   │
│   ├── config.h
│   ├── pins.h
│   │
│   ├── sensors/
│   │     sensors.cpp
│   │     sensors.h
│   │
│   ├── motors/
│   │     motor.cpp
│   │     motor.h
│   │
│   ├── control/
│   │     pid.cpp
│   │     pid.h
│   │
│   ├── maze/
│   │     explorer.cpp
│   │     solver.cpp
│   │     optimizer.cpp
│   │
│   ├── display/
│   │     lcd.cpp
│   │     lcd.h
│   │
│   └── utils/
│         helpers.cpp
│         helpers.h
│
└── tests/
    ├── sensor_test.ino
    ├── motor_test.ino
    └── pid_test.ino
```

---

# Firmware Modules

### Sensors

Handles IR sensor readings and line position detection.

```
readSensors()
calculateLinePosition()
detectJunction()
```

---

### Motor Control

Controls motor direction and speed.

```
setMotorSpeed()
stopMotors()
turnLeft()
turnRight()
```

---

### PID Controller

Maintains robot alignment with the line.

```
computePID()
```

---

### Maze Explorer

Handles junction detection and path recording.

```
detectNode()
chooseDirection()
storePath()
```

---

### Maze Solver

Optimizes the recorded path and performs the final run.

```
compressPath()
executePath()
```

---

### LCD Display

Displays robot status.

Example outputs:

```
Mapping Mode
Final Run
Path Optimized
```

---

# Development Workflow

Recommended implementation order:

```
1. Sensor reading
2. Motor control
3. PID line following
4. Junction detection
5. Maze exploration
6. Path optimization
7. Final run execution
```

---

# Testing

Separate test sketches are included:

```
tests/sensor_test.ino
tests/motor_test.ino
tests/pid_test.ino
```

These help verify hardware before full integration.

---

