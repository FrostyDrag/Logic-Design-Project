# Logic Controller Board – LED Selector with Clock Divider

This project implements a digital logic controller board using basic ICs (555 timer, 74HC74, 74HC173, etc.) to control four LEDs based on four switches. It features a custom-built 1 Hz and 0.25 Hz clock divider using flip-flops and buffers, and a switch register with conditional memory retention.

---

## 🎯 Purpose

The objective is to design and simulate a logic board that:
- Uses a 555 timer to generate a 1 Hz clock.
- Switches to control the led
- Dynamic Variations of the sequences to create a magic effect
- Switch disabling special trick in sequence 3

---

## 📁 Project Structure

| Directory      | Contents                                                                 |
|----------------|--------------------------------------------------------------------------|
| `schematics/`  | Circuit schematics and timing diagrams (drawn in [your tool here])       |
| `src/`         | HDL/Quartus project files (`.qpf`, `.vhd` or `.v`) for synthesis         |
| `simulations/` | Testbenches, VCD/FSDB waveforms and simulation input/output              |
| `docs/`        | PDF documentation or final report                                        |
| `media/`       | Demo video thumbnail, GIFs, or other visual resources                    |

---

## ▶️ Demo Video

Click the thumbnail or [watch the demo](https://www.youtube.com/watch?v=YOUR_VIDEO_LINK) on YouTube.

## 🧪 Setup & Usage Instructions

1. **Hardware Setup**
   - Build the 555 timer using the formula:
     \[
     T = 0.693 \cdot (R1 + 2 \cdot R2) \cdot C
     \]
     Choose values to produce a 1 Hz clock with ~50% duty cycle.
   - Buffer the 555 output using a Schmitt inverter before feeding it into flip-flops.
   - Connect the two 74HC74 D flip-flops in series to divide the clock by 4.
   - Connect switches to the `74HC173` register with gating logic (OR of switch inputs).
   - Use an RC circuit on the `CLR` pin of flip-flops to ensure clean reset on power-up.
   - Use D flip-flops as enable flag for each switch and other selection circuit in order to detect the sequence

2. **Simulation**
   - Open `src/main.qpf` in Quartus.
   - Compile and simulate using `simulations/testbench.vhdl`.
   - Verify timing using waveform viewer with provided `.vwf` or `.vcd` files.

3. **Reset Behavior**
   - The flip-flops start in a known `Q = 0` state using an RC delay on the reset pin (`CLR`).
   - Manual reset is supported through a dedicated push button (active-low).

---

## 💡 Design Choices & Insights

- **555 Timer for Clock Generation**: Simple, robust, and configurable for 1 Hz output. Buffered with a logic inverter for clean rising edges.
- **Clock Division**: Achieved using two cascaded D flip-flops (74HC74) with Q'→D feedback.
- **Buffer Register (74HC173)**: Captures switch input only when any switch is HIGH; retains last state otherwise.
- **Power-On Reset**: Implemented using an RC delay on `CLR` pin to force flip-flops to 0 at startup.
- **Modular Design**: Allows reuse of each subsystem (clock, buffer, sequencer) independently.
