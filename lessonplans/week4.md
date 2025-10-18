# Lesson 4: Distance Sensor

## Objective

Teachers will teach students how to read and use the XRP’s built-in distance sensor to measure distances and control robot behavior based on sensor input.

## Prerequisites

* Completion of Lesson 3: Subsystems and Button Bindings.
* XRP’s distance sensor is connected and functional.
* Students understand Java conditionals and the command-based structure.

## Teaching Notes

This lesson introduces basic sensor integration. Focus on reading real-time data from hardware, interpreting it, and reacting to environmental changes.

---

## Lesson Outline

### 1. Discuss Sensor Purpose

Explain what the distance sensor does:

* Emits infrared or ultrasonic signals.
* Measures time until signal returns.
* Reports distance in centimeters.

Show how sensors allow the robot to make autonomous decisions.

---

### 2. Creating DistanceSubsystem.java

```java
package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.xrp.XRPRangefinder;

public class DistanceSubsystem extends SubsystemBase {
    private final XRPRangefinder distanceSensor = new XRPRangefinder();

    public double getDistanceIn() {
        return distanceSensor.getDistanceInches();
    }
}
```

---

### 3. Creating StopAtWall Command

This command drives forward until the robot is within a threshold distance from an obstacle.

```java
package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.XRPDrivetrain;
import frc.robot.subsystems.DistanceSubsystem;

public class StopAtWall extends Command {
    private final XRPDrivetrain drive;
    private final DistanceSubsystem sensor;

    public StopAtWall(XRPDrivetrain drive, DistanceSubsystem sensor) {
        this.drive = drive;
        this.sensor = sensor;
        addRequirements(drive, sensor);
    }

    @Override
    public void execute() {
        drive.arcadeDrive(0.6, 0);
    }

    @Override
    public void end(boolean interrupted) {
        drive.arcadeDrive(0, 0);
    }

    @Override
    public boolean isFinished() {
        return sensor.getDistanceIn() < 8.0; // Stop within 8 in of obstacle
    }
}
```

---

### 4. Binding to Button

In `RobotContainer.java`:

```java
private final DistanceSubsystem distanceSubsystem = new DistanceSubsystem();

private final StopAtWall stopAtWall= new StopAtWall(m_xrpDrivetrain, distanceSubsystem);

private void configureBindings() {
    controller.rightTrigger().onTrue(stopAtWall);
}
```

---

## Teaching Tips

* Have students log distance readings using `System.out.println()`.
* Place obstacles at different distances and test stopping accuracy.
* Discuss sensor noise and real-world variability.

---

## Extensions

* Use the sensor for automatic parking or alignment.
* Plot sensor readings in AdvantageScope (preview for Lesson 7).
* Introduce averaging or filtering for more accurate readings.

---

## Estimated Duration

45 minutes (concept + testing)

## Learning Outcomes

* Understand how to interface with a distance sensor.
* Use sensor feedback to stop or adjust robot behavior.
* Build foundational logic for autonomous navigation.
