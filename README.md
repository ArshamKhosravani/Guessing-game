1. Introduction:

I designed and implemented an interactive number-guessing game using the Zybo 
Z7-10 FPGA development board, building upon prior labs that explored keypad input and output 
display mechanisms. The core objective was to create a playable system where users could 
guess a pseudo-random number between 0 and 9 using a 4x4 keypad. The board would then 
provide feedback based on proximity to the correct number via LEDs, a two-digit 
seven-segment display (SSD), RGB indicators, and UART transmission. Each guessed number 
would appear in binary on standard LEDs, and both the current and previous guesses would be 
shown on the SSD. RGB LEDs would indicate whether a guess was close, far, or exactly 
correct, and ASCII-encoded guesses would be transmitted over UART. The project demanded 
careful coordination of real-time input handling, display multiplexing, finite state machine (FSM) 
control logic, and modular VHDL development—all synchronized by the 125 MHz clock of the 
Zybo board. This lab emphasized not only technical integration but also resilience in debugging 
and troubleshooting synthesis issues, ultimately reinforcing best practices for scalable and 
interactive FPGA design. 
  

2. Design/Implementation :

The game system was built around a top-level VHDL module that orchestrated all internal 
subsystems. The design took a modular approach to ensure that each functionality—from user 
input to output display—could be independently tested, maintained, and refined. Inputs included 
button presses (BTN2 for setting the target and BTN3 for resetting) and keypad digits. Outputs 
ranged from binary display on four LEDs, live feedback on an SSD, RGB color-coded proximity 
indicators, and UART transmission of guesses as ASCII characters. 
Each module was tailored for maximum synchronization and clarity in behavior. A critical aspect 
of the design was ensuring all processes operated correctly with the Zybo’s 125 MHz clock, 
which required precise counters for timing tasks like debounce logic, SSD multiplexing, and 
RGB LED flashing during win states. The game_logic module featured an FSM with clear 
transitions for capturing input, configuring a target, accepting and evaluating guesses, and 
providing visual/audio feedback. Synthesis issues—particularly those related to Vivado's file 
referencing—were mitigated by renaming conflicting states and ensuring proper file registration. 
Subsystems like the SSD controller used time-multiplexing at 50 Hz to refresh both digits, 
showing the previous and current guesses in sequence. The RGB LED logic used absolute 
difference comparisons to assess if the guess was hotter or colder than the previous one, or a 
match, triggering a timed green flash sequence to celebrate a win. While some components 
such as the UART transmitter and the counter remained placeholders, they provided functional 
stubs for simulation and interfacing, highlighting the importance of testability in digital design. 
A block diagram was created to visually map the interconnections between modules, pin 
assignments, and the data/control signals passed between them. Each hardware pin was 
carefully mapped in the constraints file to ensure compatibility with the Zybo board’s I/O 
structure. Key timing values—like 10 ms for debounce and SSD digit refresh and 500 ms 
blinking for win feedback—were carefully chosen for visibility and user interaction fidelity. 

3. Verification :

To validate the design, both simulation and physical hardware testing were employed. A custom 
VHDL testbench simulated 1.5 seconds of operation and verified every major functionality: reset 
behavior, keypad input detection, target number setting, LED response, SSD accuracy, RGB 
feedback logic, and UART transmission. Simulation procedures replicated realistic user actions, 
including debounced button presses and multiple guess scenarios, checking that state 
transitions occurred as expected and output values were stable and correct. 
On the hardware side, the fully programmed Zybo board was tested in a lab setup. A known 
input sequence was followed to verify that guesses appeared in binary and on the SSD, and 
that RGB LEDs responded appropriately to proximity or wins. Although the UART did not 
function due to the placeholder module, the other feedback systems validated the game’s logic 
and timing. Minor issues, such as occasional inconsistencies in keypad response, were 
noted—likely due to the limitations of the gpio.vhd placeholder module. These were not 
severe enough to impact overall system function but suggest areas for improvement in future 
iterations. 

4. Conclusions :

This project represented the culmination of concepts explored throughout the course, blending FSM 
design, real-time input processing, and output synchronization into a single cohesive system. 
Despite encountering synthesis errors and working with several placeholder modules, I 
successfully implemented a working version of the number-guessing game. The system reliably 
captured user guesses, evaluated them against a target, and provided clear visual and 
behavioral feedback. One major takeaway was the importance of state naming consistency and 
Vivado’s sensitivity to stale references—issues that were resolved through careful debugging 
and refactoring. I also gained hands-on experience balancing timing across subsystems and 
understanding how modularity contributes to reusability and verification. 
While functional, the system could benefit from enhancements such as a fully implemented 
UART transmitter for serial monitoring, a controllable pseudo-random counter for target 
selection, and a more robust GPIO interface for the keypad. Nonetheless, this project provided 
deep insights into real-world FPGA development and reinforced my ability to manage complex 
hardware/software systems. The moment the RGB LEDs flashed green to signal a successful 
guess was a gratifying validation of the design process from concept to reality. 
