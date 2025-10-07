# Lesson 5: PID Loops – Gyro Turn

## Objective

Teachers will guide students through implementing a PID control loop to rotate the XRP robot accurately using the onboard gyro sensor.

## Prerequisites

* Completion of Lesson 4: Distance Sensor.
* Students understand proportional control concepts.
* XRP gyro is functional and calibrated.

## Teaching Notes

This lesson introduces closed-loop feedback control using WPILib’s `PIDController`. Focus on how proportional, integral, and derivative terms affect motion. Use this as a hands-on physics-meets-programming activity.

---

## Lesson Outline

### 1. Discuss PID Control

* **Proportional (P):** Corrects based on present error.
* **Integral (I):** Corrects accumulated past error.
* **Derivative (D):** Predicts future error trend.

Draw a feedback loop diagram on the board:

```
Setpoint → Controller → Motor → Gyro → Feedback → Error → Controller
```

---

### 2. Create GyroTurn Command

```java
package frc.robot.commands;

import edu.wpi.first.math.controller.PIDController;
import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.DriveSubsystem;
import edu.wpi.first.wpilibj.XRPGyro;

public class GyroTurn extends Command {
    private final DriveSubsystem drive;
    private final XRPGyro gyro = new XRPGyro();
    private final PIDController controller;
    private final double targetAngle;

    public GyroTurn(DriveSubsystem subsystem, double angle) {
        this.drive = subsystem;
        this.targetAngle = angle;
        controller = new PIDController(0.03, 0.0, 0.002);
        addRequirements(drive);
    }

    @Override
    public void initialize() {
        gyro.reset();
        controller.setSetpoint(targetAngle);
    }

    @Override
    public void execute() {
        double output = controller.calculate(gyro.getAngle());
        drive.drive(0, output); // Rotate in place
    }

    @Override
    public void end(boolean interrupted) {
        drive.drive(0, 0);
    }

    @Override
    public boolean isFinished() {
        return Math.abs(targetAngle - gyro.getAngle()) < 2.0; // within 2 degrees
    }
}
```

---

### 3. Test and Tune

In `RobotContainer.java`:

```java
new JoystickButton(controller, 4)
    .onTrue(new GyroTurn(driveSubsystem, 90)); // Turn 90 degrees
```

Run the program and watch the robot rotate. Adjust `kP`, `kI`, and `kD` for smoother motion:

* Increase `kP` → faster response, possible oscillation.
* Add `kD` → smooths and damps motion.
* Keep `kI` = 0 for now.

---

## Teaching Tips

* Have students record overshoot and response time.
* Use a visual marker to measure rotation accuracy.
* Discuss the importance of tuning for stability.

---

## Extensions

* Add dynamic tuning sliders in Shuffleboard.
* Combine this with the distance sensor for autonomous navigation.
* Create a `TurnToTarget` command that uses vision or sensor data.

---

## Estimated Duration

60 minutes (PID explanation + tuning)

## Learning Outcomes

* Understand the principles of PID control.
* Implement a gyro-based turning loop.
* Tune PID constants for stability and precision.
