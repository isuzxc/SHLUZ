# Ship-Lock Gate Protection System

A working scale model of a ship lock with an automatic system that protects the lock gates from a vessel ramming into them. Built by team **Hydroguards** (Гидростражи) for the **Moscow Predprofessional Olympiad 2024/25** (Product sector, Engineering profile). The team won with **90/100 points**.

▶ **Demo video:** [https://drive.google.com/file/d/1UjxNKrvoZrJ4bb8MGSVHhdbNzYJcA9Ue/view?usp=sharing]

## What the project does

Large vessels hitting lock gates can damage the gates and endanger ships passing through. Our system protects the lock gates with a **cable (boom) that is raised across the lock before a vessel arrives and lowered afterwards**, and automates the whole locking cycle:

- Automatic forward and backward movement of a model vessel
- Raising and lowering the protective cable at the upper and lower gates
- Opening and closing the lock gates
- Filling and draining the lock chamber, with water-level control
- Traffic-light signals for the locking process
- A web dashboard for monitoring and manual control

## The model

- Lock base made of foam insulation board (Penoplex): cheap, light and easy to assemble. About 1.5 m long and 0.5 m wide.
- Custom-designed parts: upstream and downstream sections, swing gates on hinges, and a reel for the protective cable.
- Model vessel with a small motor, propeller and an ESP8266 (NodeMCU) control board.

## Hardware

| Part | Purpose |
|---|---|
| Arduino Mega | Main controller, runs all the lock logic |
| 4 servo motors | Opening and closing the gates |
| DC motors with L298N drivers | Raising and lowering the protective cable |
| 3 HC-SR04 ultrasonic sensors | Detecting the vessel and measuring distances (upstream, chamber, downstream) |
| 2 water-level sensors | "Chamber empty" and "chamber full" detection |
| Water flow sensor | Measuring the volume of water moved |
| Relay modules and 3 pumps | Filling and draining the chamber, recirculating water |
| LED traffic lights | Showing the state of the lock |
| 12 V power supply | Pumps and motors |

## Software

Firmware is written in C++ in the Arduino IDE, using `Servo.h` and `Wire.h`.

- **Control by commands over Serial**, for example `MOTORA1A2_UP`, `SERVO2_3_OPEN`, `PUMP_ON`, `LED1_ON`
- **Flow sensor** pulses are counted with interrupts, so measurement does not block the main loop
- **Automatic locking cycle:** when a vessel is detected closer than 10 cm, the lock drains (`full_shluz()`); when the vessel leaves, it fills again (`empty_shluz()`)
- **Web dashboard** to control the cable protection, gates, pumps and traffic lights, and to see the water-level and distance sensors in real time

### Sequence of one locking cycle

1. The operator switches the system on
2. The protective cable is raised
3. The vessel receives a signal and starts moving
4. If the vessel hits the cable, the boom absorbs the impact instead of the gates
5. Water levels are equalised, then the gates open

## Testing and results

We tested gate opening and closing under load, automatic vessel movement and maneuvering. All stages of locking (installing the protection, vessel movement, opening the gates, removing the protection) worked as intended.

Ideas for improvement:

- More accurate water-level and obstacle sensors
- A more stable boom design for waves and changing water levels
- Stronger materials (composites or light metals)
- A faster microcontroller for larger installations
- Richer data visualisation for the operator


## Team

| Name | Role |
|---|---|
| Timofey Shchelkunov | Electrical engineer: wiring and configuring the equipment |
| Dmitry Antonyuk | Programmer |
| Sofia Volchkova, Maria Luzanova | Design engineers (lock and vessel models) |
| Ilya Sedov | Coordinator and documentation |

Supervisors: Anton Ignatov, Tatiana Shapovalenko. School No. 548 "Tsaritsyno", Moscow.
