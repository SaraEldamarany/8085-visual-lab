<div align="center">

# 🧠 8085 Visual Lab

### See how assembly instructions become real CPU operations — one step at a time.

**An interactive, beginner-friendly Intel 8085 learning simulator**

Explore the journey from **Assembly → Machine Code → Memory → Fetch → Decode → Execute**, and watch registers, the ALU, and status flags respond to every instruction.

**No installation · No account · No backend required**

</div>

---

## Overview

**8085 Visual Lab** is a browser-based educational website designed to help students understand *what actually happens inside a processor* when an assembly program runs. Rather than showing only the final answer, it visualizes the relationship between **program instructions, memory addresses, stored data, CPU registers, the ALU, and the status flags**.

Its light, colorful interface and guided explanations are intended for first-time learners, classroom demonstrations, and self-paced revision.

> **Educational purpose:** This project is a learning simulator for a supported subset of Intel 8085 instructions, **not** a complete, cycle-accurate hardware emulator.

## Why learn the Intel 8085?

Although the Intel 8085 is a historical 8-bit microprocessor, it provides a particularly clear starting point for understanding computer architecture. Its small instruction set and visible registers make fundamental concepts easier to follow before progressing to more complex processors.

Studying the 8085 helps students understand:

- **How programs become machine instructions:** assembly mnemonics are encoded into instruction bytes (opcodes and, when needed, operand bytes).
- **How a CPU executes a program:** the Program Counter identifies the next instruction; the CPU fetches, decodes, and executes it.
- **How data is stored and moved:** instructions can access registers, immediate values, or memory addresses, and these mean different things.
- **How arithmetic works at the bit level:** 8-bit results may produce a carry or borrow, and 16-bit operations use multiple bytes or register pairs.
- **How flags control decisions:** condition flags influence branches, comparisons, and loops.
- **How complex tasks are built from simple instructions:** multiplication can be modeled with repeated addition, and division with repeated subtraction.

These ideas remain relevant to **computer organization, embedded systems, low-level programming, debugging, and processor design**.

## What makes this website different?

| Feature | What students see | Why it helps |
|---|---|---|
| **Highlighted assembly** | The current instruction is visually emphasized | Makes execution order obvious |
| **Assembly-to-machine-code view** | Instruction bytes and their addresses | Explains opcode encoding and instruction sizes |
| **Visual memory panel** | Program bytes, input data, and written results | Connects addresses to actual stored values |
| **CPU register display** | Register values update as instructions execute | Shows where data is moving |
| **ALU visualization** | Arithmetic or logical expressions and results | Makes each calculation understandable |
| **Live flags** | S, Z, AC, P, and CY, with changes highlighted | Connects an instruction's result to CPU status |
| **Fetch / Decode / Execute** | Guided stages for each instruction | Explains the CPU's execution process |
| **Explanation and prediction prompts** | Simple descriptions of what happened and what happens next | Supports independent learning |
| **Playback controls** | Automatic animation, pause, restart, and individual microsteps | Works for both classroom projection and self-study |
| **Ready-made examples** | Exercises including arithmetic and loops | Helps students get started without writing code |

### Important distinction: address vs. data

The website is designed to reinforce a foundational concept:

```asm
LDA 2050H
```

Here, **`2050H` is a memory address — not the value loaded into A**.

If `Memory[2050H] = A8H`, then executing the instruction yields `A = A8H`.

Compare three different meanings:

```asm
LDA 2050H       ; A <- Memory[2050H] (read data at an address)
MVI A, A8H      ; A <- A8H          (use an immediate value)
LXI H, 2050H    ; HL <- 2050H      (load an address/value into HL)
```

This difference is essential for correctly interpreting assembly programs.

## Getting started

### Option 1 — Open locally

1. Download or clone this repository.
2. Keep `index.html`, `style.css`, and `script.js` in the **same directory**.
3. Open `index.html` in a current browser (Chrome, Edge, or Firefox).
4. Select a built-in example or paste supported 8085 assembly code.
5. Press **Play the Story** to assemble and begin the guided animation.

The application is static and runs entirely in the browser. There is **no build step, external package, or server requirement** for the bundled files.

### Option 2 — Publish with GitHub Pages

1. Create a GitHub repository and upload the contents of this project to its root directory.
2. Open **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select branch `main` and folder `/ (root)`, then save.
5. Once deployment completes, open the published website. Its typical URL is:

   `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/`

Make sure the repository root contains `index.html`, `style.css`, and `script.js` — not another folder containing them.

## How to use the simulator in a lesson

**1. Start with a question.** For example, “How can an 8-bit accumulator represent the result of `A8H + F6H`?”

**2. Choose an example or enter a program.** Input data can be declared using the educational `DATA` directive:

```asm
DATA 2050H, A8H
DATA 2051H, F6H

LDA 2050H
MOV B, A
LDA 2051H
ADD B
HLT
```

**3. Observe assembly and machine code.** Point out the instruction bytes in program memory, separate from the data bytes at `2050H` and `2051H`.

**4. Follow each instruction.** The highlighted line and explanation tell students which command is running. The guided phases illustrate fetching the instruction, decoding its meaning, and executing its effect.

**5. Look at the CPU.** After the addition, the 8-bit accumulator contains `9EH`; the Carry flag (`CY`) is `1` because the full unsigned sum is `19EH`. The carry flag is **inside the CPU**; it is not automatically written into memory.

**6. Predict before continuing.** Pause the animation and ask which register, flag, or memory location should change next.

**7. Check the result.** For a program that explicitly saves the carry, inspect the output bytes to reconstruct the complete result.

> **Instructor tip:** Emphasize *address → contents → destination register*. Ask “Where is the value stored?”, “Which instruction reads it?”, and “Which flags does that instruction modify?”

## Suggested learning path

**Lab 1 — Fundamentals:** CPU and registers, opcodes, instruction lengths, immediate data, basic arithmetic, and status flags.

**Lab 2 — Memory and decisions:** HL memory pointer, conditional jumps, 8-bit addition and carry, subtraction and borrow, and comparisons with `CMP` / `CPI`.

**Lab 3 — Larger operations:** 16-bit low-byte-first storage, `LHLD` / `SHLD`, `ADD` / `ADC`, `SUB` / `SBB`, `DAD`, loops, repeated-addition multiplication, and repeated-subtraction division.

## Supported instruction subset

The learning engine supports the following instructions used in the supplied teaching exercises:

- **Data transfer and memory:** `MOV`, `MVI`, `LXI`, `LDA`, `STA`, `LHLD`, `SHLD`, `XCHG`
- **Arithmetic and logic:** `ADD`, `ADC`, `ADI`, `SUB`, `SBB`, `SUI`, `CMP`, `CPI`, `INR`, `DCR`, `INX`, `DCX`, `DAD`, `CMA`, `ORA`, `XRA`
- **Flow and flags:** `JMP`, `JC`, `JNC`, `JZ`, `JNZ`, `JM`, `JP`, `STC`, `CMC`, `HLT`, `NOP`
- **Educational extras:** labels and the `DATA address, value` directive for initializing memory

The current implementation limits executions to **600 instructions** to guard against runaway loops. Unsupported instructions produce an error rather than being silently approximated.

## Project structure

```text
8085-visual-lab/
├── index.html    # Main user interface
├── style.css     # Layout, visual highlighting, colors, animations
├── script.js     # Assembler, simulation, state updates, explanations
└── README.md     # Project introduction, instructions and scope
```

## Learning outcomes

After practicing with the website, students should be better able to:

1. Distinguish **memory addresses** from the **data stored at those addresses**.
2. Explain why some instructions use **1, 2, or 3 bytes** in memory.
3. Describe the **Fetch → Decode → Execute** sequence.
4. Trace register and memory changes through an 8085 program.
5. Predict **Carry, Zero, Sign, Parity, and Auxiliary Carry** behavior for supported operations.
6. Explain how carry/borrow propagates through **multi-byte arithmetic**.
7. Trace conditional branches and loops without memorizing outputs.
8. Build confidence before using full-featured tools such as GNUSim8085.

## Limitations and responsible use

This project is intended to **teach concepts visually**, not to replace formal instruction or a validated microprocessor emulator. Its phase animation is pedagogical and does not reproduce exact 8085 bus cycles or timings. It currently implements only the supported instruction subset listed above. Students should verify advanced, timing-sensitive, or unsupported exercises in a full simulator or against the official instruction reference.

---

## دليل سريع بالعربي

**ما هو 8085 Visual Lab؟** موقع تعليمي تفاعلي يوضح بالتدريج كيف يتحول كود Assembly إلى Machine Code، وكيف ينفذ المعالج الأوامر ويقرأ البيانات من الذاكرة ويغير قيم الـRegisters والـFlags.

**لماذا ندرس 8085؟** لأنه معالج بسيط نسبيًا يساعدنا على فهم المكونات الأساسية للـCPU، وحركة البيانات، وتنفيذ التعليمات، والجمع والطرح، والشروط والـLoops. الهدف هو فهم المبادئ التي تقوم عليها المعالجات، وليس حفظ تعليمات قديمة فقط.

**طريقة الاستخدام:**

1. افتح `index.html` أو رابط الموقع بعد نشره على GitHub Pages.
2. اختر مثالًا جاهزًا، أو اكتب كود 8085 المدعوم في محرر الـAssembly.
3. لإضافة مدخلات إلى الذاكرة، يمكنك كتابة `DATA 2050H, A8H`؛ هنا `2050H` **عنوان**، و`A8H` **القيمة المخزنة**.
4. اضغط **Play the Story** وشاهد السطر الحالي مميزًا بلون واضح.
5. تابع المراحل: **Fetch → Decode → Execute**، ثم راقب الـRegisters والـALU والـFlags والـMemory.
6. استخدم **Pause** و**Next Tiny Step** لطرح سؤال على نفسك: «إيه اللي المفروض يتغير بعد التعليمة دي؟»
7. قارن النتيجة المتوقعة بالنتيجة الفعلية، وجرّب تغيير الأرقام لفهم تأثيرها.

**مهم:** `LDA 2050H` معناها **اقرأ البيانات الموجودة داخل العنوان 2050H**، وليس «ضع الرقم 2050H في الـAccumulator».

**للمدرسين:** اعرض البرنامج على الشاشة، أوقف التنفيذ قبل كل خطوة، واطلب من الطلاب توقع قيمة الـRegister أو الـFlag التي ستتغير؛ ثم شغّل الخطوة التالية للمقارنة.

**تنويه:** الموقع محاكي تعليمي مبسط لمجموعة من أوامر 8085، وليس بديلًا كاملًا عن GNUSim8085.
