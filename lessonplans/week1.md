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

### 1. Use the CommandXRP template, and Explain the Project Structure

* `Robot.java` controls the overall robot lifecycle (`autonomousInit`, `teleopPeriodic`, etc.).
* `RobotContainer.java` wires up subsystems and commands.
* `subsystems/` folder holds robot hardware logic.
* `commands/` folder holds behaviors.

### 2. Connect and Test the XRP

* Open the **Driver Station** or **XRP Web Interface**.
* Ensure WiFi connection is active.
* Verify motor control by running the default project.


### 3. Add Drive Command

Create a command to move forward:

```java
package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.XRPDrivetrain;

public class DriveForward extends Command {
    private final XRPDrivetrain drive;

    public DriveForward(XRPDrivetrain subsystem) {
        drive = subsystem;
        addRequirements(drive);
    }

    @Override
    public void execute() {
        drive.arcadeDrive(0.8, 0.0); // Forward at half speed
    }

    @Override
    public void end(boolean interrupted) {
        drive.arcadeDrive(0, 0);
    }

    @Override
    public boolean isFinished() {
        return false; // Runs until stopped
    }
}
```

### 4. Run Command in Teleop

In `RobotContainer.java`, bind the drive command as the default:

```java
public class RobotContainer {
  // The robot's subsystems and commands are defined here...
  private final XRPDrivetrain m_xrpDrivetrain = new XRPDrivetrain();

  private final CommandXboxController controller = new CommandXboxController(0);

  private final DriveForward driveForward = new DriveForward(m_xrpDrivetrain);

...

  private void configureButtonBindings() {
    m_xrpDrivetrain.setDefaultCommand(Commands.run(() ->m_xrpDrivetrain.arcadeDrive(controller.getLeftY(), controller.getRightX()), m_xrpDrivetrain));
    controller.a().whileTrue(driveForward);
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

* Add rotation: modify `drive(0.8, 0.3)`.
* Add joystick input for control.
* Have students experiment with different speed combinations.

---

## Estimated Duration

45 minutes (setup + first code run)

## Learning Outcomes

* Understand the role of `Subsystems` and `Commands`.
* Successfully connect and deploy code to the XRP.
* Control robot movement programmatically.
