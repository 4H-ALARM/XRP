# Lesson 6: PID Loops (Feedforward for Friction Compensation)

## Objective

Teachers will guide students through implementing a **PID controller with feedforward** to handle frictional resistance in the XRP robot’s drivetrain. Students will learn to fine-tune PID constants and use feedforward terms to improve motion smoothness.

## Prerequisites

* Completion of Lesson 5: PID Loops - Gyro Turn.
* Robot code that can already perform basic PID turning using the gyro.

## Teaching Notes

This lesson builds on PID control by showing how **feedforward compensation** can offset predictable frictional forces that cause sluggish or inconsistent performance. This mirrors real-world FRC control strategies for mechanisms like drivetrains and arms.

---

## Lesson Outline

### 1. Recap: What Is Feedforward?

Feedforward adds a predictable control term that helps the PID controller overcome known system resistance.

**Formula:**

```
Output = kP * error + kI * errorSum + kD * errorRate + kS + kV * velocity
```

Where:

* `kS` is the **static friction compensation** (voltage needed to overcome stiction).

Explain that while PID corrects *errors*, feedforward anticipates the required effort to move smoothly.

---

### 2. Example: Accounting for High Friction

Use this concept to improve the **gyro turn** from Lesson 5.

```java
public class GyroTurnCommand extends CommandBase {
    private final Drivetrain drivetrain;
    private final ADXRS450_Gyro gyro = new ADXRS450_Gyro();
    private final double targetAngle;
    private final PIDController pid = new PIDController(0.02, 0.0, 0.001);

    // Feedforward constants
    private final double kS = 0.15; // static friction voltage
    private final double kV = 0.0;  // optional if you want velocity scaling

    public GyroTurnCommand(Drivetrain drivetrain, double angleDegrees) {
        this.drivetrain = drivetrain;
        this.targetAngle = angleDegrees;
        addRequirements(drivetrain);
        pid.setTolerance(2.0);
    }

    @Override
    public void execute() {
        double error = targetAngle - gyro.getAngle();
        double pidOutput = pid.calculate(gyro.getAngle(), targetAngle);

        // Add static friction feedforward
        double output = pidOutput;
        if (Math.abs(pidOutput) > 0.01) {
            output += Math.copySign(kS, pidOutput);
        }

        drivetrain.arcadeDrive(0, output);
    }

    @Override
    public boolean isFinished() {
        return pid.atSetpoint();
    }

    @Override
    public void end(boolean interrupted) {
        drivetrain.arcadeDrive(0, 0);
    }
}
```

---

### 3. Tuning Process

1. Start with feedforward only (`kS`). Increase until the robot starts moving smoothly at low speeds, then turn it down so any PID output will move it.
2. Then add PID tuning from Lesson 5 to refine accuracy.
3. Observe how feedforward reduces overshoot and startup lag.

---

### 4. Teaching Demo

* Have students compare turns **with** and **without feedforward**.
* Record both runs using **AdvantageScope** (next lesson) to visualize PID output and angle tracking.

---

## Discussion Points

* How does friction affect turning precision?
* Why is static feedforward especially useful on small robots like the XRP?
* What other mechanisms (like arms or elevators) might need feedforward in FRC?

---

## Estimated Duration

60–75 minutes (including testing and tuning)

## Learning Outcomes

* Understand feedforward’s role in overcoming system resistance.
* Apply friction compensation to improve control smoothness.
* Tune combined PID + feedforward loops for reliable motion.
