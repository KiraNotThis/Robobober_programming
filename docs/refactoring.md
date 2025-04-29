# Refactoring Auto mode

---

## 1. Safety & Error‑Handling

- **S‑1** – Implement a `failSafeStop()` method that sets every motor and servo to zero power or hold.  Call it from `catch` blocks, timeout branches, and whenever `!opModeIsActive()` becomes `false`.  *(Benefit: prevents uncontrolled motion on exceptions or aborts).*  
- **S‑2** – Wrap each major stage in `runOpMode()` inside its own `try / catch (Exception e)` block; log via telemetry and invoke `failSafeStop()`.  *(Benefit: clear diagnostics and safe recovery).*  
- **S‑3** – Add **timeouts** to all `while(isBusy())` or sensor‑poll loops (vertical, horizontal, drivetrain).  *(Benefit: avoids infinite loops if an encoder fails).*  
- **S‑4** – Apply `Range.clip()` to every computed motor power (`adjustedSpeed ± correction`).  *(Benefit: guarantees power never exceeds ±1.0).*  

---

## 2. Configuration & Calibration

- **C‑1** – Move every magic number (servo positions, powers, PID gains, distances) into `static final` constants with descriptive names.  *(Benefit: centralised tuning and self‑documenting code).*  
- **C‑2** – Persist calibration constants in JSON or CSV and load them at runtime, so adjustments don’t require reflashing.  *(Benefit: faster field tweaks).*  
- **C‑3** – Provide a service OpMode that drives 100 cm forward, reports encoder ticks, and recalculates `PULSES_PER_CM`.  *(Benefit: field recalibration in under two minutes).*  

---

## 3. Code Structure

- **A‑1** – Keep all hardware initialisation in `initRobot()`.  *(Benefit: `runOpMode()` stays readable).*  
- **A‑2** – Replace the long linear script with a **Finite‑State Machine**: `INIT → CHAMBER_1 → FETCH → OBS_ZONE_1 → … → PARK`.  *(Benefit: easier to interrupt, resume, or reorder.)*  
- **A‑3** – Factor the duplicated logic in `driveStraight`, `driveSide`, and `driveDiagonal` into one private template method that accepts a wheel‑power lambda.  *(Benefit: eliminates copy‑paste bugs).*  
- **A‑4** – Create a small **ChassisController** class that hides encoder reset, heading correction, and PID constants.  *(Benefit: encapsulation and reuse in Tele‑Op.)*  

---

## 4. Concurrency

- **P‑1** – Replace raw `Thread` usage with `ExecutorService` or a command‑based scheduler (e.g., SimpleFSM or Road Runner `Action`).  *(Benefit: cleaner shutdown and fewer race conditions).*  
- **P‑2** – Ensure no more than one thread writes to the same motor at a time; coordinate via flags or futures.  *(Benefit: prevents power conflicts).*  

---

## 5. Motion Control

- **M‑1** – Upgrade from proportional steering (`kP` only) to full **PIDF** using `PIDCoefficients`.  *(Benefit: straighter paths, less overshoot).*  
- **M‑2** – Implement trapezoidal or S‑curve motion profiles (`MotionProfile` class) instead of the ad‑hoc `rampUpTime` + `slowdownStartFactor`.  *(Benefit: smooth acceleration and reduced wheel slip).*  
- **M‑3** – Integrate odometry “dead wheels” or AprilTag localisation and apply closed‑loop correction every 500 ms.  *(Benefit: cumulative drift < ±1 cm across the field).*  

---

## 6. Testing & CI

- **T‑1** – Add unit tests for helper math methods (distance → ticks, PID, `clip`). Use JUnit in Android Studio.  *(Benefit: ensures future edits don’t break maths).*  
- **T‑2** – Configure GitHub Actions or GitLab CI to build the APK, run unit tests, and publish a code‑coverage badge.  *(Benefit: continuous verification).*  
- **T‑3** – Use FTC Dashboard or FTCSimulator to script autonomous regression runs (headless Webots) for every pull request.  *(Benefit: catches software regressions early).*  
- **T‑4** – Implement continuous test. Robot is running the same scenario again and again.  *(Benefit: catches software/hardware regressions early).*

---

### Implementation Order

1. Safety & Error‑Handling (S) – mandatory for inspection.  
2. Configuration & Calibration (C) – quick wins for field tuning.  
3. Code Structure & Concurrency (A & P) – lays a clean foundation.  
4. Motion Control (M) – measurable performance gains.  
5. Testing, CI, and Documentation (T) – long‑term maintainability.  

