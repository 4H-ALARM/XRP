# Lesson 3: Creating Subsystems (Arm) and Binding to Buttons

## Objective

Teachers will lead students through creating a new subsystem (an Arm) and binding its actions to controller buttons using WPILib’s command-based structure.

## Prerequisites

* Completion of Lesson 2: Creating Commands.
* XRP is connected and tested.
* Students understand how Commands interact with Subsystems.

## Teaching Notes

This lesson introduces multiple subsystems and shows how to use button bindings to control them. Emphasize that subsystems represent hardware and commands represent behavior.

---

## Lesson Outline

### 1. Concept Discussion

Review the command-based structure:

* **Subsystems** → Define hardware components and methods.
* **Commands** → Tell subsystems what to do.
* **Button Bindings** → Connect controller inputs to commands.

---

### 2. Creating the Arm Subsystem

Create `ArmSubsystem.java`:

```java
package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.XRPMotor;

public class ArmSubsystem extends SubsystemBase {
    private final XRPMotor armMotor = new XRPMotor(2); // Channel 2

    public void moveArm(double speed) {
        armMotor.set(speed);
    }

    public void stopArm() {
        armMotor.stopMotor();
    }
}
```

Explain: The `XRPMotor` object controls one motor. Each motor is assigned a port number.

---

### 3. Creating Arm Commands

Create two commands: `ArmUp.java` and `ArmDown.java`.

#### ArmUp.java

```java
package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.ArmSubsystem;

public class ArmUp extends Command {
    private final ArmSubsystem arm;

    public ArmUp(ArmSubsystem subsystem) {
        arm = subsystem;
        addRequirements(arm);
    }

    @Override
    public void execute() {
        arm.moveArm(0.5);
    }

    @Override
    public void end(boolean interrupted) {
        arm.stopArm();
    }

    @Override
    public boolean isFinished() {
        return false; // Hold until button released
    }
}
```

#### ArmDown.java

```java
package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.ArmSubsystem;

public class ArmDown extends Command {
    private final ArmSubsystem arm;

    public ArmDown(ArmSubsystem subsystem) {
        arm = subsystem;
        addRequirements(arm);
    }

    @Override
    public void execute() {
        arm.moveArm(-0.5);
    }

    @Override
    public void end(boolean interrupted) {
        arm.stopArm();
    }

    @Override
    public boolean isFinished() {
        return false;
    }
}
```

---

### 4. Binding Commands to Buttons

In `RobotContainer.java`:

```java
private final ArmSubsystem armSubsystem = new ArmSubsystem();
private final Joystick controller = new Joystick(0);

private void configureBindings() {
    new JoystickButton(controller, 1)
        .whileTrue(new ArmUp(armSubsystem)); // Button 1 raises arm

    new JoystickButton(controller, 2)
        .whileTrue(new ArmDown(armSubsystem)); // Button 2 lowers arm
}
```

---

## Teaching Tips

* Reinforce that Subsystems = Hardware, Commands = Actions.
* Show how multiple commands can use the same subsystem safely.
* Have students add print statements to confirm code flow.

---

## Extensions

* Add a sensor to stop arm movement at upper/lower limits.
* Combine drive and arm commands to practice concurrency.
* Create a command to move the arm to a specific position.

---

## Estimated Duration

60 minutes (coding + testing)

## Learning Outcomes

* Create and manage new subsystems.
* Bind commands to controller buttons.
* Understand hardware abstraction in WPILib.
