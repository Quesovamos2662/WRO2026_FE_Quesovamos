# WRO2026_FE_QuesoVamos


[banner + team photo]

## Meet the Team    
Welcome to the official GitHub repository for Team QuesoVamos from Panama, participating in the WRO 2026 San Miguelito Regional in the Future Engineers category, Open Challenge.

Team QuesoVamos is made up of three unlikely friends who somehow decided that building an autonomous robot, learning GitHub, using BrickLink, programming, documenting, troubleshooting, and surviving WRO all at the same time was a good idea. This is our first time working with many of these tools and technologies, so this repository is not only a place for our source code, materials, and robot documentation, but also proof of our learning process.

Even if we do not achieve something huge in this competition, our goal is to step out of our comfort zone, learn as much as possible, and use this experience to come back stronger in the future. We may be beginners now, but we are determined to improve, keep building, and go beyond what we thought we were capable of.

## Robot Overview     

Length: ~28cm (280mm)

Height: ~9.5cm (95mm)

Width: ~13cm (130mm)

Weight: ~73.61g (0.07361kg)

(~200 chars — specs rápidos)

---

## 1. Mobility & Mechanical Design   
### Driving base & chassis            

Our driving base and chassis are constructed entirely from the official LEGO Mindstorms EV3 Kit. This was decided beacuse of several technical factors we took into consideration.

First, LEGO components offer native compatibility with the EV3 brick, which alleviates the difficulty of mounting sensors and motors. And second, LEGO builds allows structural issues to be identified and corrected rapidly during testing while not needing any externally sourced or 3D printed pieces.

But an all-LEGO build introduces limitations. These come in the form of the plastic frame having measurable flex at higher speeds, and connection points being able to loosen if the frame withstands powerful impacts. Despite these possible shortcomings, we still believed that we could make our car work, so we chose to build in this manner.



### Motor selection & torque reasoning 

Our vehicle uses two LEGO Mindstorms EV3 motors: one large and one medium. These are connected directly to the EV3 brick via ports A and B. Both motors were chosen for their native compatibility with the rest of out components and their ability to provide reliable speed and torque on their own.

The large motor, used for moving back and forth, has a top speed of 170 RPM, a running and stall torque of 20Nxcm and 40 Nxcm respectively, an operating voltage of 9V, and a weight of 76g. For a Lego component, these specs are very respectable and are just what we were looking for in our robot.

Finally, the medium motor, which we decided to use for steering, has a top speed of 250 RPM, a running torque of 8Nxcm, a stall torque of 12Nxcm, an operating voltage of 9V and a weight of only 36g. This motor's higher RPM and lower torque make it better suited for steering. Its low weight also helps on making our robot lighter, benefiting its speed overall.


### Steering mechanism (Ackermann)   

Our robot uses a simple Ackermann steering mechanism to achieve stable and controlled turning. Since this is our first time competing in the WRO, we decided to not make things harder on ourselves by using a more complex steering system. For this reason, we did not implement a servo-based steering mechanism, as we considered it more difficult to build, and troubleshoot with our current experience.

← AMPLIAR
### Chassis iterations                 (~300 chars)  ← FALTA (Lamina mandara el previous iteration)

## 2. Power & Sensor Architecture      [Criterion 2]
### Power supply & EV3 brick specs     (~300 chars)

For our controller, we used a standard EV3 Lego Mindstorms Brick. We decided on using this Brick as our controller because it can also serves as a battery for all of the sensors and components that our robot utilises.

It sports a flash memory of 16 MB, has 64 MB of RAM, is able to output any voltage value in between 0V and 9V, and has a rechargable battery with a maximum capacity of 2000 mAh

### Wiring diagram                     [imagen + tabla de puertos]  ← FALTA
### Sensor selection & placement       (~600 chars) 

In the sensors department, we settled on utilising ultrasonic sensors to their full extent. Since we had to make up for not having a camera, we mounted three ultrasonic sensors on either side and on the front of our car. These will make sure to detect any nearby walls or obstacles, making our robot able to correct its route on the fly.

We also decided on choosing ultrasonic sensors because of their ease of calibration and efficiency. They are more intuitive to adjust than Lego EV3 infrared sensors and have almost no competition in terms of efficiency and accuracy.

### Sensor calibration                 (~200 chars)  ← FALTA

## 3. Software Architecture            

### Algorithm description

The vehicle's navigation software is written in Python 3 using the ev3dev2 library, and runs directly on the EV3 brick. The program follows a *priority-based threshold algorithm* — on every loop iteration, the robot reads all three ultrasonic sensors and reacts according to a strict hierarchy of conditions.

The core logic works as follows: the robot drives forward continuously while constantly polling the left (INPUT_1), front (INPUT_2), and right (INPUT_3) ultrasonic sensors. Each sensor has two distance thresholds — a *WARN* threshold that triggers a gentle steering correction while still moving, and a *CRASH* threshold that triggers a full stop and recovery sequence. Side sensors warn at 20 cm and crash at 6 cm. The front sensor warns at 55 cm and crashes at 25 cm.

A key feature of the algorithm is *turn direction locking* (TURN_DIR). The first time the robot naturally navigates a corner (front wall detected at warn range), it records whether it steered left or right and locks that direction for the entire run. Since all four corners of a WRO track share the same handedness, this guarantees that every subsequent corner and every recovery always steers the robot back into the correct lane.

Lap completion is tracked by counting corners: every time the front sensor transitions from below CORNER_ENTRY_DIST (70 cm) back above CORNER_EXIT_DIST (90 cm), one corner is counted. After every 4 corners, one lap is registered. The robot stops automatically after 3 laps.

### Flowchart                         

faltan dos 

### Obstacle & corner handling         (~400 chars)  ← FALTA


### Tuning process                     (~300 chars)  ← FALTA

## 4. Engineering Decisions           

### Design decision log    

Every major component choice our team made involved weighing alternatives 
against our constraints as first-time competitors with limited experience 
and a fixed timeline. This section documents the reasoning behind our most 
significant decisions.

*EV3 over Arduino*

Early in the planning phase, we considered building the robot around an 
Arduino microcontroller, as it initially seemed like a flexible option for 
connecting multiple components. However, we quickly determined that Arduino 
was not the right fit for our team at this stage. As first-time competitors 
with no prior robotics experience, the process of wiring individual 
components, managing voltage levels, writing low-level driver code, and 
debugging hardware connections within our available time was beyond what we 
could realistically execute. The LEGO Mindstorms EV3 ecosystem offered a 
fully integrated solution — motors, sensors, brick, and software all 
designed to work together — which allowed us to focus our limited time on 
solving the actual navigation problem rather than on hardware setup.

*Three ultrasonics over two*

Our initial sensor layout used only two ultrasonic sensors on the left and 
right sides of the vehicle. During early testing we identified a blind spot 
directly in front of the robot — the side sensors could not detect a wall 
ahead until the robot was already too close to correct in time. This led us 
to add a third ultrasonic sensor facing forward, which became the primary 
trigger for corner detection and is responsible for the UF_WARN (55 cm) 
threshold that gives the robot enough time to begin steering before reaching 
the wall.

*Medium motor for steering over a servo*

We considered using a servo motor for the Ackermann steering mechanism, as 
servos are commonly used in RC car designs for precise angle control. 
However, given our fully LEGO-based chassis, integrating an external servo 
would have required custom mounting solutions and additional wiring outside 
the EV3 ecosystem. The EV3 medium motor provided sufficient steering 
response through timed pulses and remained fully compatible with our 
structure and ev3dev2 library.

### What didn't work       

*Infrared sensor as front obstacle detector*

Our original design used an infrared sensor mounted at the front of the 
robot to detect the distance between the vehicle and the wall ahead. In 
theory, the infrared sensor offered a narrower detection beam than an 
ultrasonic sensor, which we believed would reduce false positives from 
angled surfaces. In practice, the sensor performed poorly under repeated 
collision conditions — after several impacts during testing, its readings 
became inconsistent and unreliable, causing the robot to either fail to 
detect walls or trigger corrections at incorrect distances. We replaced it 
with a third ultrasonic sensor (EV3 model, INPUT_2), which proved 
significantly more robust and consistent across all testing sessions.

*Threshold-based steering without cooldown*

Our first version of the navigation loop fired a steering correction every 
single cycle whenever a sensor read below its warn threshold. This caused 
the robot to oscillate violently in straight sections — it would correct 
right, then immediately correct left on the next cycle, then right again, 
producing a zigzag pattern instead of a straight line. Introducing a 0.50 
second cooldown after each correction (STEER_COOLDOWN) resolved the 
oscillation entirely and allowed the robot to maintain a stable forward 
trajectory between corrections.

*Fixed front warn threshold of 35 cm*

Our initial front sensor warn distance was set to 35 cm. During corner 
testing, the robot consistently clipped the outer wall because it began 
steering too late — 35 cm did not give the motors enough time to turn the 
front wheels and change the vehicle's heading before the wall was reached. 
Raising the threshold to 55 cm gave the robot enough advance warning to 
complete the steering maneuver before reaching the corner wall.

## 5. Reproducibility                  [Criterion 5]
### Bill of Materials                  (~200 chars)

Electronic Materials

- X1 Lego Mindstorms EV3 Brick
- X1 Lego Mindstorms EV3 Large Motor
- X1 Lego Mindstorms EV3 Medium Motor
- X3 Lego Mindstorms EV3 Ultrasonic Sensor
- 
### Build instructions                 (~300 chars)  ← FALTA

## Vehicle Photos
## Team Photos
## Performance Videos
## Resources



  
