# Coffee machine in ARM assembly

Second lab assignment for **Arhitektura računala 1R** (Computer Architecture 1), 2023/24.

The program simulates a coffee machine on an ARM system with an LCD, three LEDs, a start button and a real-time clock (RTC). The RTC fires an IRQ every 30 seconds, and the interrupt handler moves the machine through its states.

## States

| State | LEDs | LCD |
|---|---|---|
| 1 – idle | all off | `SPREMNO` (ready) |
| 2 – preparing | red | `PRIPREMA` (preparing) |
| 3 | red + yellow | `PRIPREMA` |
| 4 | red + yellow + green | `PRIPREMA` |
| 5 – done | all three blink | `GOTOVO` (done) |
| 6 – error | red blinks | `GRESKA` (error) |

Pressing the button in state 1 starts the cycle. Pressing it again while a cycle is running triggers the error state. Both state 5 and state 6 last six RTC periods and then return the machine to state 1.

## Hardware map

| Device | Address | Use |
|---|---|---|
| GPIO1 | `0xFFFF0F00` | LCD (port B) |
| GPIO2 | `0xFFFF0B00` | LEDs on bits 5–7, button on bit 0 (port A) |
| RTC | `0xFFFF0E00` | 1 Hz clock, interrupt every 30 counts |
