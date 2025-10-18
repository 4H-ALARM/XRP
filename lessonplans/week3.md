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
import edu.wpi.first.wpilibj.xrp.XRPServo;

public class ArmSubsystem extends SubsystemBase {
    private final XRPServo servo = new XRPServo(2); // Channel 2

    public void moveArm(double angle) {
      servo.setAngle(angle);
    }

    public double getArm() {
      return servo.getAngle();
    }

}
```

Explain: The `XRPServo` object controls one servo. Each servo is assigned a port number, basedon on what is on the board.

---

### 3. Creating Arm Commands

Create two commands: `Arm90.java` and `Arm0.java`.

#### ArmUp.java

```java
package frc.robot.commands;

import edu.wpi.first.wpilibj2.command.Command;
import frc.robot.subsystems.ArmSubsystem;

public class Arm90 extends Command {
    private final ArmSubsystem arm;

    public Arm90(ArmSubsystem subsystem) {
        arm = subsystem;
        addRequirements(arm);
    }

    @Override
    public void execute() {
        arm.moveArm(90);
    }

    @Override
    public void end(boolean interrupted) {
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

public class Arm0 extends Command {
    private final ArmSubsystem arm;

    public Arm0(ArmSubsystem subsystem) {
        arm = subsystem;
        addRequirements(arm);
    }

    @Override
    public void execute() {
        arm.moveArm(0);
    }

    @Override
    public void end(boolean interrupted) {

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

private void configureBindings() {
    controller.leftbumper().onTrue(Arm0);
    controller.rightbumper().onTrue(Arm90);
}
```

---

## Teaching Tips

* Reinforce that Subsystems = Hardware, Commands = Actions.
* Show how multiple commands can use the same subsystem safely.
* Have students add print statements to confirm code flow.

---

## Extensions

* Create a command to move the arm to a specific position.

---

## Estimated Duration

60 minutes (coding + testing)

## Learning Outcomes

* Create and manage new subsystems.
* Bind commands to controller buttons.
* Understand hardware abstraction in WPILib.
