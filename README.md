<h1 align="center">TuringMachineSimulator</h1>
<p align="center">
  <strong>
    A deterministic multi-tape Turing Machine simulator with dynamic syntax parsing and interactive graph visualization.
  </strong>
</p>

<!--
<br>

# Table of Contents
* [Overview :sparkles:](#overview-sparkles)
  * [About](#about)
  * [Features](#features)
  * [Technologies](#technologies)
* [Details :scroll:](#details-scroll)
  * [ UI and application flow](#ui-and-application-flow)
  * [Technical challenges](#technical-challenges)

<br>
-->
# Overview :sparkles:

## About
This project is an advanced, fully functional Turing Machine simulator built as a modern Progressive Web App (PWA). It was developed as an engineering thesis to solve a major limitation in existing educational tools: rigid syntax rules. 

Instead of forcing users to adapt to one specific format, this simulator features a custom template system that allows you to define your own syntax, write code in a fully featured editor, and even transpile programs between different syntax styles.

Check out the [live version](https://pasek108.github.io/ConnectionGames/).

<br>

![preview](/_for_readme/preview.png)

### ⚙️ Advanced Logic Engine
* **Multi-Tape Support:** Accurately simulates deterministic multi-tape Turing machines.
* **Deep Simulation Control:** Users can run, pause, step forward, and even step backward through the simulation history.
* **Conditional Breakpoints:** The execution can be automatically paused when specific states are reached, specific values appear on the tape, or after a certain number of steps.
* **Wildcard Support:** Natively supports wildcard characters (`*`) in the code for creating default transitions and reducing code bloat.

### 🛠️ Developer Tooling & IDE
* **Custom Syntax Templates:** Write code using any syntax format you prefer. The dynamic parser extracts the transition rules regardless of the surrounding formatting.
* **Transpilation:** Automatically translate source code from one Turing machine syntax to another.
* **Integrated Code Editor:** Features a CodeMirror-based editor with dynamic syntax highlighting, multi-cursor editing, and line numbering.
* **Virtual File System:** Saves, imports, and exports programs and project folders directly in the browser using ZenFS and fflate compression.

### 🌍 UX & Visualization
* **Interactive Graph:** Generates a dynamic transition graph using a customized version of Neo4j-arc. Users can drag nodes, zoom, and watch the active state highlight in real-time during simulation.
* **PWA & Offline Mode:** Fully installable and runs flawlessly without an internet connection on both desktop and mobile devices.
* **Theming:** Includes full support for light and dark modes.

## Tech Stack

### **Core**
* **Framework:** React 19
* **Language:** TypeScript (v5.9.3) / JavaScript 
* **Build Tool:** Vite (with PWA plugin) 

### **State & Background Processing**
* **Multithreading:** Web Workers API
* **Storage:** IndexedDB 
* **File System:** ZenFS 

### **UI & Libraries**
* **Editor:** CodeMirror 6 (with custom `StreamParser` implementation) 
* **Graphing:** Neo4j-arc
* **Styling:** CSS / PostCSS

<br>

# Details 📜

## Architecture & Technical Challenges

Building a high-performance simulator in the browser required solving several complex architectural challenges:

* **Non-Blocking Multithreading:** To ensure the UI (like graph animations) remains smooth during heavy computations, all compilation, transpilation, and simulation steps are strictly isolated in background Web Workers.
* **Asynchronous Memory Dumps:** Passing massive tape states between threads via standard `postMessage` causes UI blocking. I engineered a snapshot system that uses IndexedDB as a transmission buffer, allowing fast, collision-free data exchange between the worker and the main thread.
* **Handling "Infinite" Tapes:** Standard JavaScript `Map` objects have allocation limits (max $2^{23}$ keys). I designed a custom `BigMap` structure that transparently fragments memory into smaller pools, enabling the simulation of tapes with tens of millions of cells and supporting negative indexing.
* **Dynamic Lexical Analysis:** Built a custom lexer using CodeMirror's `StreamParser` that analyzes code line-by-line and applies syntax highlighting on the fly, entirely based on the user's actively selected syntax template.

## UI & Application Flow

### Configuration & Editor
The configuration area allows users to write their Turing machine code. The editor provides real-time syntax highlighting that immediately reflects the rules of the selected template. Users can also manage templates, switch syntaxes, and transpile code in the adjacent settings section.

### Simulation & Tapes
The core simulation view presents a clean, dynamic representation of the machine's tapes and read/write heads. Users can initialize starting values on the tapes, track execution steps, and use the control bar to step backward, pause, or fast-forward the execution.

### Graph Visualization
A highly interactive node-based view of the machine's transition functions[cite: 490]. The layout is calculated automatically, but users can manually adjust node positions for better readability[cite: 492, 494].
