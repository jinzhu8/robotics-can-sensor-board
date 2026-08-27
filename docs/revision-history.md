## Initial Design Decisions

- Selected Interlink FSR 402 as the initial force-sensing element.
- Chose a resistive voltage-divider interface with a 10 kΩ fixed resistor as the starting design.
- Chose to prioritize closed-loop force regulation rather than high-accuracy absolute force measurement because of the FSR's nonlinear response.
Pimoroni's Servo 2040 was used as a reference architecture because it combines an RP2040 with servo outputs, analog sensor inputs, USB connectivity, and power/current monitoring in a platform intended for robotic applications. The project will implement the required functionality on a custom PCB rather than using the Servo 2040 directly.
