# CHIP-8 Emulator

A fully functional CHIP-8 emulator built with JavaScript and p5.js, featuring a retro green terminal aesthetic. This emulator can run classic CHIP-8 games and programs from the 1970s.

![CHIP-8 Emulator](https://img.shields.io/badge/CHIP--8-Emulator-brightgreen)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow)
![p5.js](https://img.shields.io/badge/p5.js-1.7.0-red)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Architecture](#architecture)
- [API Documentation](#api-documentation)
- [CHIP-8 Specification](#chip-8-specification)
- [Controls](#controls)
- [ROM Loading](#rom-loading)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)

## Overview

CHIP-8 is an interpreted programming language developed in the 1970s for early microcomputers. This emulator recreates the CHIP-8 virtual machine, allowing you to run classic games like Pong, Tetris, Space Invaders, and many others.

### What is CHIP-8?

- **CPU**: Simple 8-bit processor with 35 instructions
- **Memory**: 4KB RAM (4096 bytes)
- **Display**: 64x32 monochrome pixels
- **Input**: 16-key hexadecimal keypad
- **Sound**: Single beep tone
- **Timers**: 60Hz delay and sound timers

## Features

- ✅ Complete CHIP-8 instruction set (35 opcodes)
- ✅ Accurate memory management and timing
- ✅ 64x32 pixel display with retro green aesthetic
- ✅ Full keyboard input support
- ✅ ROM file loading (.ch8, .rom)
- ✅ Built-in test ROM
- ✅ Pause/Resume functionality
- ✅ Step-by-step debugging
- ✅ System reset capability
- ✅ Responsive p5.js rendering

## Getting Started

### Prerequisites

- No installation required - runs directly in browser

### Quick Start

1. **Download** the HTML file
2. **Open** in any modern web browser
3. **Click** "Load Test ROM" to see it in action
4. **Load** your own CHIP-8 ROM files using the file input

### Loading ROMs

You can find CHIP-8 ROMs at:
- [CHIP-8 Games Pack](https://www.zophar.net/pdroms/chip8.html)
- [Awesome CHIP-8](https://github.com/tobiasvl/awesome-chip-8)
  
## Usage

### Controls

| Action | Button |
|--------|--------|
| Load ROM | File input button |
| Test ROM | "Load Test ROM" button |
| Reset System | "Reset" button |
| Pause/Resume | "Pause/Resume" button |
| Step Debug | "Step" button (when paused) |

### Keyboard Mapping

The CHIP-8's 16-key hexadecimal keypad is mapped to your keyboard:

```
CHIP-8 Keypad    →    Your Keyboard
┌─────────────┐       ┌─────────────┐
│ 1 │ 2 │ 3 │ C │     │ 1 │ 2 │ 3 │ 4 │
├───┼───┼───┼───┤     ├───┼───┼───┼───┤
│ 4 │ 5 │ 6 │ D │  →  │ Q │ W │ E │ R │
├───┼───┼───┼───┤     ├───┼───┼───┼───┤
│ 7 │ 8 │ 9 │ E │     │ A │ S │ D │ F │
├───┼───┼───┼───┤     ├───┼───┼───┼───┤
│ A │ 0 │ B │ F │     │ Z │ X │ C │ V │
└───┴───┴───┴───┘     └───┴───┴───┴───┘
```

## Architecture

### Core Components

```
┌─────────────────┐
│   HTML/CSS UI   │ ← User Interface
├─────────────────┤
│   p5.js Canvas  │ ← Graphics Rendering
├─────────────────┤
│   CHIP-8 CPU    │ ← Emulation Core
├─────────────────┤
│ Memory & I/O    │ ← System Components
└─────────────────┘
```

### File Structure

```
chip8-emulator/
├── index.html          # Main emulator file
├── README.md          # This documentation
└── roms/              # ROM files (user-provided)
    ├── pong.ch8
    ├── tetris.ch8
    └── ...
```

## API Documentation

### Chip8 Class

The main emulator class that implements the CHIP-8 virtual machine.

#### Constructor

```javascript
const chip8 = new Chip8();
```

Creates a new CHIP-8 emulator instance with initialized memory, registers, and system state.

#### Properties

| Property | Type | Description |
|----------|------|-------------|
| `memory` | Uint8Array(4096) | 4KB system memory |
| `V` | Uint8Array(16) | 16 general-purpose registers (V0-VF) |
| `I` | Number | Index register (12-bit) |
| `pc` | Number | Program counter |
| `stack` | Uint16Array(16) | Call stack (16 levels) |
| `sp` | Number | Stack pointer |
| `dt` | Number | Delay timer (60Hz) |
| `st` | Number | Sound timer (60Hz) |
| `keys` | Uint8Array(16) | Keypad state |
| `display` | Uint8Array(2048) | 64x32 display buffer |
| `drawFlag` | Boolean | Screen update flag |

#### Methods

##### `loadROM(rom)`
Loads a ROM into memory starting at address 0x200.

**Parameters:**
- `rom` (Uint8Array): ROM data to load

**Example:**
```javascript
const rom = new Uint8Array([0xA2, 0x2A, 0x60, 0x0C, ...]);
chip8.loadROM(rom);
```

##### `reset()`
Resets the emulator to initial state.

```javascript
chip8.reset();
```

##### `cycle()`
Executes one CPU cycle (fetch-decode-execute).

```javascript
chip8.cycle();
```

##### `executeInstruction(instruction)`
Executes a single CHIP-8 instruction.

**Parameters:**
- `instruction` (Number): 16-bit instruction to execute

### p5.js Functions

#### `setup()`
Initializes the p5.js canvas and rendering settings.

```javascript
function setup() {
    let canvas = createCanvas(640, 320);
    canvas.parent('canvas-container');
    noSmooth();
    frameRate(500);
}
```

#### `draw()`
Main rendering loop that runs the emulator and updates display.

```javascript
function draw() {
    chip8.cycle();
    if (chip8.drawFlag) {
        // Render display
    }
}
```

#### `keyPressed()` / `keyReleased()`
Handle keyboard input for CHIP-8 keypad.

```javascript
function keyPressed() {
    const key = keyMap[window.key.toLowerCase()];
    if (key !== undefined) {
        chip8.keys[key] = 1;
    }
}
```

### Utility Functions

#### `loadTestROM()`
Loads a built-in test ROM that displays "HELLO".

#### `reset()`
Resets the emulator system.

#### `togglePause()`
Toggles pause/resume state.

#### `step()`
Executes single instruction when paused.

## CHIP-8 Specification

### Memory Map

```
┌─────────────────┐ 0xFFF (4095)
│                 │
│   Program ROM   │ 0x200-0xFFF
│                 │
├─────────────────┤ 0x200 (512)
│   ETI 660 Data  │ 0x600-0x1FF
├─────────────────┤ 0x600 (96)
│   Font Data     │ 0x50-0x9F
├─────────────────┤ 0x50 (80)
│   Interpreter   │ 0x000-0x1FF
└─────────────────┘ 0x000
```

### Instruction Set

CHIP-8 has 35 instructions, all 2 bytes long:

| Opcode | Description | Example |
|--------|-------------|---------|
| `0nnn` | Call RCA 1802 program | `0x0230` |
| `00E0` | Clear screen | `cls` |
| `00EE` | Return from subroutine | `ret` |
| `1nnn` | Jump to address | `jp 0x200` |
| `2nnn` | Call subroutine | `call 0x300` |
| `3xkk` | Skip if Vx == kk | `se V0, 0x44` |
| `4xkk` | Skip if Vx != kk | `sne V1, 0x55` |
| `5xy0` | Skip if Vx == Vy | `se V0, V1` |
| `6xkk` | Set Vx = kk | `ld V0, 0x88` |
| `7xkk` | Add kk to Vx | `add V0, 0x04` |

*[Full instruction set documentation available in code comments]*

### Timers

- **Delay Timer (DT)**: General purpose timer, decrements at 60Hz
- **Sound Timer (ST)**: When non-zero, emits beep sound, decrements at 60Hz

### Display

- **Resolution**: 64x32 pixels
- **Colors**: Monochrome (on/off)
- **Sprites**: XOR-based drawing
- **Collision**: VF register set when pixels are erased

## Controls

### Game Controls
Use the mapped keyboard keys (see [Keyboard Mapping](#keyboard-mapping)) to control games.

### Emulator Controls
- **Space**: Pause/Resume (when implemented)
- **Escape**: Reset system (when implemented)

### Debug Controls
- **Step Mode**: Execute one instruction at a time
- **Memory Viewer**: Inspect system state (planned feature)

## ROM Loading

### Supported Formats
- `.ch8` - Standard CHIP-8 ROM format
- `.rom` - Generic ROM file

### File Size Limits
- Maximum: ~3.5KB (4096 - 512 bytes for system area)
- Typical games: 1-2KB

### Popular ROMs
1. **Pong** - Classic paddle game
2. **Tetris** - Block puzzle game  
3. **Space Invaders** - Shoot-em-up arcade game
4. **Breakout** - Ball and paddle game
5. **Pac-Man** - Maze navigation game

## Troubleshooting

### Common Issues

#### ROM Won't Load
- **Check file format**: Ensure file is .ch8 or .rom
- **File size**: Must be under 3.5KB
- **Browser security**: Some browsers block local file access

#### No Display Output  
- **Check ROM**: Try the built-in test ROM first
- **Graphics instructions**: ROM must use display opcodes (Dxyn)
- **Clear screen**: ROM should clear screen initially (00E0)

#### Input Not Working
- **Focus**: Click on the emulator area first
- **Key mapping**: Use the correct keyboard mapping
- **Caps lock**: Input is case-insensitive

#### Performance Issues
- **Refresh rate**: Emulator runs at 500Hz
- **Browser**: Use modern browser for best performance
- **Background tabs**: Performance may decrease when tab is inactive

### Debug Mode

Use the step function for debugging:

1. Click "Pause/Resume" to pause
2. Click "Step" to execute one instruction
3. Monitor the status display for current state

## Contributing

### Development Setup

1. Clone or download the project
2. Open `index.html` in a browser
3. Make changes and refresh to test

### Code Style

- Use ES6+ JavaScript features
- Follow existing naming conventions
- Comment complex instruction implementations
- Maintain p5.js integration patterns

### Adding Features

Popular enhancement ideas:
- Sound support (Web Audio API)
- Save states
- Memory viewer
- Assembler/disassembler
- Game-specific fixes
- Super-CHIP extensions

### Testing

Test with various ROMs:
- Simple programs (test ROM)
- Classic games (Pong, Tetris)
- Homebrew programs
- Edge cases and illegal instructions

## License

This project is open source. The CHIP-8 specification is public domain.

### Third-Party Libraries

- **p5.js** (v1.7.0): MIT License
- **Font data**: Public domain CHIP-8 font set

---

## Appendix

### CHIP-8 History

CHIP-8 was created by Joseph Weisbecker in 1977 for the COSMAC VIP computer. It was designed as an easy way to create simple games and programs for early microcomputers.

### Technical Notes

- **Endianness**: Big-endian instruction format
- **Timing**: Original systems ran at ~500-1000Hz
- **Quirks**: Various implementations have subtle differences
- **Extensions**: Super-CHIP, CHIP-48, XO-CHIP add features

### Resources

- [CHIP-8 Technical Reference](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM)
- [Cowgod's CHIP-8 Reference](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM)
- [CHIP-8 Wikipedia](https://en.wikipedia.org/wiki/CHIP-8)
- [ROM Archive](https://www.zophar.net/pdroms/chip8.html)

---

**Happy Emulating!** 🎮
