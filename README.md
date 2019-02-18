# grbl1-1g-Servo
modified spindle_control.c to introduce RC Servo control into grbl 1.1g

The modified 'spindle_control.c' is based on the current grbl (1.1g) version and provides full support for an RC Servo on Arduino Pin D11.

To add RC Servo capability to grbl (1.1g), download the latest version of grbl (1.1g) from github and replace 'spindle_control.c' with the version provided above. You will also need to select/modify SPINDLE_TCCRB_INIT_MASK in 'cpu_map.h' to select the appropriate (61Hz) servo frequency rate and, of course, select/modify VARIABLE_SPINDLE in 'config.h' . . . all other required #define(s) are contained with the modified 'spindle_control.c'. All lines of code that have been added to 'spindle_control.c' are marked with a comment in column 100+ of their respective lines. Apart from these 31 new lines of code (and an additional SPINDLE_TCCRB_INIT_MASK in 'cpu_map.h'), in order to add RC Servo capability, the rest of the grbl (1.1g) source code remains unchanged.

The #define RC_SERVO_SHORT (15) and RC_SERVO_LONG (31), in the modified 'spindle_control.c', set the PWM duty cycle for the RC Servo to 2.05ms and 1.03ms respectively (these are recommended values for standard RC Servos). There is also a #define that allows the servo to be inverted if movement of the arm is in the wrong direction.

Note:  the added RC Servo functionality will only operate in 'non laser' ($32=0) mode and will be ignored if you have $32 set to 1 (however, the frequency SPINDLE_TCCRB_INIT_MASK set 'cpu_map.h' will still be 61Hz rather than the default 0.98kHz used by most diode laser PWM/TTL inputs). To use this functionality use G-Code M3 to turn on the RC Servo and G-Code M5 to turn off the servo. The amount the RC Servo moves if controlled with G-Code S commands in the range 0-255.

    M3 S255     (turn servo full on)
    M5          (turn servo off)
    M3 S125     (turn servo half way)
    M3 S0       (turn servo on full off - similar to M5)
