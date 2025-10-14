# Lesson 7: AdvantageScope

## Objective

Teachers will show students how to use **AdvantageScope** to visualize, record, and debug robot data in real time. Students will learn to view PID loops, sensor readings, and command states.

## Prerequisites

* Completion of Lesson 6: PID Loops with Feedforward.
* Robot code includes sensors or PID control data.
* AdvantageScope installed on laptops.

## Teaching Notes

This lesson introduces the concept of data visualization for robotics debugging. Show how telemetry helps tune systems more effectively than guesswork.

---

## Lesson Outline

### 1. Overview: What Is AdvantageScope?

Explain that AdvantageScope is a WPILib-compatible tool that displays:

* Real-time graphs of sensor data, motor outputs, and PID errors.
* Command and subsystem states.
* Logged events and system messages.

Discuss its role in **data-driven debugging**.

---

### 2. Logging Custom Data

Demonstrate logging data manually from subsystems:

```java
import edu.wpi.first.util.datalog.DataLog;
import edu.wpi.first.util.datalog.DoubleLogEntry;
import edu.wpi.first.wpilibj.DataLogManager;

public class DistanceSubsystem extends SubsystemBase {
    private final XRPDistanceSensor distanceSensor = new XRPDistanceSensor();
    private final DoubleLogEntry distanceLog;

    public DistanceSubsystem() {
        DataLog log = DataLogManager.getLog();
        distanceLog = new DoubleLogEntry(log, "/distance/cm");
    }

    @Override
    public void periodic() {
        distanceLog.append(distanceSensor.getDistanceCM());
    }
}
```

This allows AdvantageScope to visualize the robot’s distance readings over time.

---

### 3. Viewing PID and Feedforward Data

Show students how to plot multiple entries:

* `targetAngle`
* `gyroAngle`
* `PIDOutput`

In AdvantageScope:

1. Add graph panel.
2. Select log entries by name.
3. Adjust scale and smoothing.

---

## Teaching Tips

* Demonstrate using AdvantageScope **live** while running commands.
* Encourage students to correlate sensor data with robot motion.
* Use saved logs to review autonomous runs.

---


## Learning Outcomes

* Learn to configure and connect AdvantageScope.
* Record and visualize live robot telemetry.
* Use graphs to refine PID and feedforward control performance.
