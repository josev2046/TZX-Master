# ZX81 Pro Studio: Zero-Loss Hex Edition

An iteration of previous work for creating and exporting low-resolution graphics for the **Sinclair ZX81** instead of the Speccy (a very different beast, that). It bridges the gap between modern hardware and 1980s computing by converting visual drawings into machine-readable BASIC and audio formats.

---

## 1. Purpose
The primary objective of this application is to simplify the creation of 1:1 pixel art for the ZX81's **64 x 48 "Plot" coordinate system**. It eliminates the manual drudgery of writing `PLOT X, Y` commands by automating the encoding process into a format that is safe for modern clipboards and ancient hardware.

---

## 2. Core Functionality
The application operates through three distinct stages of data transformation:

* **The Drawing Canvas:** Provides a 64x48 grid where users can "ink" pixels or import and down-sample modern image files into a high-contrast (black and white) format.
* **Hexadecimal Encoding:** To prevent data loss during copy-pasting—especially on modern systems like macOS—every pixel is converted into a 4-character Hex string (`XXYY`). 
* **Multi-Format Export:**
    * **BASIC Text:** Generates Sinclair BASIC code blocks.
    * **.P Binary:** A raw emulator-ready file.
    * **.WAV Audio:** A simulated tape signal that can be played back into an actual ZX81 via the "Ear" port.

---

## 3. Technical Implementation

### Drawing and Image Processing
The canvas uses a scaled 2D context. When an image is imported, the script calculates the scale to fit 64x48, then iterates through the pixel array. It applies a simple threshold (brightness average < 127) to determine if a pixel is "ink" or "paper."

### The Hex-Chunking Logic
Because the ZX81 has limited memory and string length constraints, the logic chunks the pixel data:
1.  It iterates through the grid.
2.  If a pixel is black, it records its coordinates.
3.  Coordinates are bundled into 400-character strings.
4.  Each chunk is assigned to a `LET A$` statement in BASIC.

### Tape Signal Simulation (`.WAV` Generation)
The most complex part of the code is the `downloadWav` function. It creates a Pulse Code Modulation (PCM) stream that mimics the ZX81's physical loading signal:
* **Pilot Tone:** A steady frequency to help the hardware sync.
* **Bit Encoding:** Each bit is represented by a specific number of pulses (9 for a '1', 4 for a '0').
* **Silence:** Inserted between pulses to ensure the hardware "hears" the gaps.

---

## 4. Operational Flow

1. **Input:** User draws or uploads a photo.
2. **Processing:** `generateHexStrings()` converts visual data to a coordinate list.
3. **Output:** The UI populates a Sinclair BASIC program template in the text area.

## 5. System Logic Diagrams

### Image Processing & Hex Conversion
This diagram illustrates how a modern image is down-sampled and converted into ZX81-compatible hexadecimal strings.

<img width="660" height="651" alt="image" src="https://github.com/user-attachments/assets/08874571-ad29-4de8-a723-754e2dec7761" />


### Binary and Audio Export Logic
This diagram shows the transformation of the BASIC text into a physical audio signal for tape loading.

<img width="671" height="884" alt="image" src="https://github.com/user-attachments/assets/d5145f3a-7231-4c5f-b5a0-0c8ff246ceea" />


