# Lesson 1: How to Set Up and Move the Robot

## Objective

Teachers will guide students through setting up their XRP robot and writing their first Java program to drive it forward and backward using WPILib.

## Prerequisites

* Students have installed **WPILib VS Code** and the **XRP project template**.
* XRP is assembled, connected via WiFi, and flashed with the latest firmware.
* Students are familiar with the Java class structure and basic syntax.

## Teaching Notes

This lesson introduces the XRP’s environment and simple robot movement. Emphasize the structure of WPILib robot projects, focusing on the `Robot`, `RobotContainer`, and `Subsystem` concepts at a high level.

---

## Lesson Outline

### 1. Explain the Project Structure

* `Robot.java` controls the overall robot lifecycle (`autonomousInit`, `teleopPeriodic`, etc.).
* `RobotContainer.java` wires up subsystems and commands.
* `subsystems/` folder holds robot hardware logic.
* `commands/` folder holds behaviors.

### 2. Connect and Test the XRP

* Open the **Driver Station** or **XRP Web Interface**.
* Ensure WiFi connection is active.
* Verify motor control by running the default project.

### 3. Creating a Drive Subsystem

Create a new file: `DriveSubsystem.java`

```java
package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.XRPDrivetrain;

public class DriveSubsystem extends SubsystemBase {
    private final XRPDrivetrain drivetrain = new XRPDrivetrain();

    public void drive(double speed, double rotation) {
        drivetrain.arcadeDrive(speed, rotation);
    }
}
```

### 4. Add Drive Command

Create a command to move forward:

```java
package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.DriveSubsystem;

public class DriveForward extends Command {
    private final DriveSubsystem drive;

    public DriveForward(DriveSubsystem subsystem) {
        drive = subsystem;
        addRequirements(drive);
    }

    @Override
    public void execute() {
        drive.drive(0.5, 0.0); // Forward at half speed
    }

    @Override
    public void end(boolean interrupted) {
        drive.drive(0, 0);
    }

    @Override
    public boolean isFinished() {
        return false; // Runs until stopped
    }
}
```

### 5. Run Command in Teleop

In `RobotContainer.java`, bind the drive command as the default:

```java
public RobotContainer() {
    driveSubsystem = new DriveSubsystem();
    driveSubsystem.setDefaultCommand(new DriveForward(driveSubsystem));
}
```

Deploy the code and confirm the XRP moves forward.

---

## Teaching Tips

* Have students predict what happens when changing the speed sign.
* Let them try using negative values to reverse direction.
* Emphasize stopping motors in `end()`.

---

## Extensions

* Add rotation: modify `drive(0.5, 0.3)`.
* Add joystick input for control.
* Have students experiment with different speed combinations.

---

## Estimated Duration

45 minutes (setup + first code run)

## Learning Outcomes

* Understand the role of `Subsystems` and `Commands`.
* Successfully connect and deploy code to the XRP.
* Control robot movement programmatically.
