# task1ELEC-WEEK4-

This project involves designing a robotic mouth using an 8x32 LED matrix. The mouth can display different facial expressions such as happy, neutral, and sad.

- Fully simulated on: https://wokwi.com/projects/405803428359855105

![Screenshot (4)](https://github.com/user-attachments/assets/fd300e40-2120-4d6c-83f1-c7f393c64257)


## Requirements

- Arduino Uno 
- LED matrix -MAX7219-
- Jumper wires

## Circuit Diagram

Here's a brief explanation of how to wire the components:

1. **DIN** (Data Input) from the MAX7219 to **Pin 11** on the Arduino.
2. **CLK** (Clock) from the MAX7219 to **Pin 13** on the Arduino.
3. **CS** (Chip Select) from the MAX7219 to **Pin 10** on the Arduino.
4. **VCC** from MAX7219 to **5V** on the Arduino.
5. **GND** from MAX7219 to **GND** on the Arduino.

## Code Explanation

The code controls the LED matrix by setting specific rows to display the desired pattern for each expression. The following functions handle the different expressions:

- `displayHappy()`: Displays a smiling face.
- `displayNeutral()`: Displays a neutral face.
- `displaySad()`: Displays a sad face.

Each expression is created by setting the binary pattern for each row in the 8x32 matrix. The code uses the `LedControl` library to control the matrix and manage the LED states.

### Example Code

```cpp
#include <LedControl.h>

// Pin connections: DIN, CLK, CS
LedControl lc = LedControl(11, 13, 10, 4);

void setup() {
  for (int i = 0; i < 4; i++) {
    lc.shutdown(i, false);       // Wake up the MAX7219
    lc.setIntensity(i, 8);       // Set brightness level
    lc.clearDisplay(i);          // Clear the display
  }
}

void loop() {
  displayHappy();
  delay(1000);
  displayNeutral();
  delay(1000);
  displaySad();
  delay(1000);
}

void displayHappy() {
  byte happy[8][4] = {
    {B00000000, B00000000, B00000000, B00000000},
    {B00000000, B00000000, B00000000, B00000000},
    {B01111111, B00000000, B00000000, B11111110},
    {B10000000, B11111111, B11111111, B00000001},
    {B10000100, B00000000, B00000000, B00100001},
    {B10000010, B00000000, B00000000, B01000001},
    {B01000001, B11111111, B11111111, B10000010},
    {B00111110, B00000000, B00000000, B01111100}
  };
  displayMouth(happy);
}

void displayNeutral() {
  byte neutral[8][4] = {
    {B00000000, B00000000, B00000000, B00000000},
    {B00000000, B00000000, B00000000, B00000000},
    {B01111111, B00000000, B00000000, B11111110},
    {B10000000, B11111111, B11111111, B00000001},
    {B10000000, B00000000, B00000000, B00000001},
    {B10000000, B00000000, B00000000, B00000001},
    {B01000000, B11111111, B11111111, B00000010},
    {B00111111, B00000000, B00000000, B11111100}
  };
  displayMouth(neutral);
}

void displaySad() {
  byte sad[8][4] = {
    {B00000000, B00000000, B00000000, B00000000},
    {B00000000, B00000000, B00000000, B00000000},
    {B01111111, B00000000, B00000000, B11111110},
    {B10000000, B11111111, B11111111, B00000001},
    {B10000010, B00000000, B00000000, B01000001},
    {B10000100, B00000000, B00000000, B00100001},
    {B01000001, B11111111, B11111111, B10000010},
    {B00111110, B00000000, B00000000, B01111100}
  };
  displayMouth(sad);
}

void displayMouth(byte mouth[8][4]) {
  for (int row = 0; row < 8; row++) {
    for (int section = 0; section < 4; section++) {
      lc.setRow(section, row, mouth[row][section]);
    }
  }
}
