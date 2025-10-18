# Lesson 2: How to Create Commands

## Objective

Teachers will guide students through the process of creating and managing Commands in the Command-Based WPILib structure for XRP. Students will learn how to design modular robot actions.

## Prerequisites

* Completion of Lesson 1: Set Up and Move the Robot.
* Students understand how subsystems interact with commands.
* XRP and VS Code are set up and connected.

## Teaching Notes

This lesson teaches students how to use the command-based model to separate logic cleanly. The focus is understanding the lifecycle of commands and why modular design matters in FRC programming.

---

## Lesson Outline

### 1. Command Lifecycle Overview

Explain the key phases of a Command:

* **initialize()** — Runs once when command starts.
* **execute()** — Runs repeatedly while active.
* **end()** — Runs once when finished or interrupted.
* **isFinished()** — Determines when command ends.

Illustrate this lifecycle with a diagram on the board.

---

### 2. Creating a DriveTime Command

```java
package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.XRPDrivetrain;
import edu.wpi.first.wpilibj.Timer;

public class DriveTime extends Command {
    private final XRPDrivetrain drive;
    private final double speed;
    private final double duration;
    private final Timer timer = new Timer();

    public DriveTime(XRPDrivetrain subsystem, double speed, double duration) {
        this.drive = subsystem;
        this.speed = speed;
        this.duration = duration;
        addRequirements(drive);
    }

    @Override
    public void initialize() {
        timer.reset();
        timer.start();
    }

    @Override
    public void execute() {
        drive.arcadeDrive(speed, 0);
    }

    @Override
    public void end(boolean interrupted) {
        drive.arcadeDrive(0, 0);
        timer.stop();
    }

    @Override
    public boolean isFinished() {
        return timer.hasElapsed(duration);
    }
}
```

---

### 3. Running Commands from RobotContainer

Add a test binding to `RobotContainer.java`:

```java
public class RobotContainer {
    private final DriveSubsystem driveSubsystem = new DriveSubsystem();

    public RobotContainer() {
        configureBindings();
    }

    private void configureBindings() {
        new JoystickButton(new Joystick(0), 1)
            .onTrue(new DriveTime(driveSubsystem, 0.5, 2)); // Button 1 drives for 2s
    }
}
```

---

## Teaching Tips

* Have students experiment with different speeds and durations.
* Explain how the `addRequirements()` method prevents hardware conflicts.
* Show how Commands can be composed into autonomous routines later.

---

## Extensions

* Write a `TurnTime` command that spins in place.
* Combine commands into a `SequentialCommandGroup`.
* Challenge students to create a simple autonomous routine.

---

## Estimated Duration

45 minutes (concept introduction + coding)

## Learning Outcomes

* Understand how Commands encapsulate robot actions.
* Learn to use `initialize`, `execute`, `end`, and `isFinished` methods.
* Be able to create and trigger Commands via joystick input.
