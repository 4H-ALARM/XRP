# Lesson 6: PID Loops with Feedforward

## Objective

Teachers will help students understand how to improve robot control by combining PID feedback with feedforward control to predict and compensate for system behavior.

## Prerequisites

* Completion of Lesson 5: PID Loops – Gyro Turn.
* Familiarity with motor control and tuning `kP`, `kI`, `kD`.
* XRP drivetrain and sensors are calibrated.

## Teaching Notes

This lesson extends PID control by introducing feedforward — predicting how much power is needed to achieve desired motion without waiting for error feedback. Emphasize how this improves responsiveness.

---

## Lesson Outline

### 1. Concept Review

Draw a comparison between feedback and feedforward:

| Term                 | Description       | Example                                        |
| -------------------- | ----------------- | ---------------------------------------------- |
| **Feedback (PID)**   | Responds to error | Corrects when robot is off-target              |
| **Feedforward (FF)** | Predicts output   | Applies voltage proportional to expected speed |

---

### 2. Introduce Feedforward Formula

WPILib’s `SimpleMotorFeedforward` models voltage:

[ V = kS + kV \times v + kA \times a ]

* `kS` — static friction voltage
* `kV` — velocity gain
* `kA` — acceleration gain

---

### 3. Creating FeedforwardDrive Command

```java
package frc.robot.commands;

import edu.wpi.first.math.controller.PIDController;
import edu.wpi.first.math.controller.SimpleMotorFeedforward;
import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.DriveSubsystem;

public class FeedforwardDrive extends Command {
    private final DriveSubsystem drive;
    private final PIDController pid = new PIDController(0.1, 0, 0);
    private final SimpleMotorFeedforward ff = new SimpleMotorFeedforward(0.2, 1.2, 0.1);
    private final double targetSpeed;

    public FeedforwardDrive(DriveSubsystem subsystem, double speed) {
        this.drive = subsystem;
        this.targetSpeed = speed;
        addRequirements(drive);
    }

    @Override
    public void execute() {
        double measuredSpeed = drive.getWheelSpeed(); // method to be implemented in DriveSubsystem
        double pidOutput = pid.calculate(measuredSpeed, targetSpeed);
        double ffOutput = ff.calculate(targetSpeed);
        drive.drive(pidOutput + ffOutput, 0);
    }

    @Override
    public void end(boolean interrupted) {
        drive.drive(0, 0);
    }

    @Override
    public boolean isFinished() {
        return false;
    }
}
```

---

### 4. Updating DriveSubsystem

Add a method to measure wheel speed (approximate for XRP):

```java
public double getWheelSpeed() {
    // Replace with encoder reading when available
    return 0.0; // Placeholder for simulation
}
```

---

### 5. Discussion and Testing

Explain that in a real robot with encoders, feedforward improves motion smoothness.
Run command at various speeds and observe the smooth start/stop transitions.

---

## Teaching Tips

* Emphasize that feedforward compensates before error occurs.
* Demonstrate how tuning `kS`, `kV`, and `kA` changes motor responsiveness.
* Use logging to compare raw PID vs PID + FF performance.

---

## Extensions

* Add encoder feedback to XRP to compute actual speed.
* Visualize control outputs in AdvantageScope.
* Apply feedforward to arm position or velocity control.

---

## Estimated Duration

60–75 minutes (concept explanation + tuning)

## Learning Outcomes

* Understand the purpose of feedforward in control systems.
* Implement combined PID and feedforward control.
* Recognize how predictive control improves performance.
