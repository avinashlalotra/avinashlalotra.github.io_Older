# 🚀 Optimizing `memset()` in RISC-V Assembly — From C to Bare Metal  
*By Abinash Singh | July 2025*  
📌 GitHub: [https://github.com/avinashlalotra/newlib-Optimization-Task](https://github.com/avinashlalotra/newlib-Optimization-Task) 

---

## 🌱 Background: A Challenge Worth Writing About

Earlier this year, I applied for the **LFX Mentorship (RISC-V Newlib Optimization Summer 2025)** program. The goal? To work on optimizing functions in **newlib**, the standard C library for embedded systems.

I didn’t make it to the final cohort — but I **did** complete the screening task, which involved writing an **optimized `memset()`** implementation using **hand-written RISC-V assembly**.

This blog walks through that journey:  
- Interfacing C with assembly  
- Writing architecture-specific code  
- Running it all on a bare-metal RISC-V environment using `spike`  

---

## 🔍 The Task: Rebuild `memset()` in Assembly

The `memset()` function is a core part of libc — it fills memory with a given byte value. A naive implementation in C might look like this:

```c
void *memset(void *ptr, int x, size_t n) {
    unsigned char *p = ptr;
    while (n--) *p++ = (unsigned char)x;
    return ptr;
}
```

But writing this in **assembly**, you can make it **smarter and faster** — especially by:
- Using 4-byte stores (`sw`) instead of byte-by-byte (`sb`)
- Aligning pointers for word-sized writes
- Minimizing loop instructions

---

## ⚙️ C ↔ Assembly Interface

### Step 1: Declaring `memset()` in C

In `memset.c`, I used the `extern` keyword to tell the C compiler that the function will be defined in assembly:

```c
extern void *memset(void *ptr, int x, size_t n);
```

This allows me to call the function in C tests just like any other C function.

---

### Step 2: Writing `memset()` in RISC-V Assembly

Here’s the basic (unaligned) version I wrote:

```asm
.section .text
.global memset
memset:                        # a0 = ptr, a1 = value, a2 = size
    mv t1, a0                  # t1 = pointer
    beqz a2, end               # if size == 0, return

    # Expand byte to 32-bit pattern
    slli t2, a1, 8
    or t2, t2, a1
    slli t3, t2, 16
    or t2, t2, t3              # now t2 = 0x01010101 if x = 0x01

    srli t3, a2, 2             # t3 = number of 4-byte chunks
    andi a2, a2, 3             # a2 = leftover bytes

word_loop:
    beqz t3, byte_loop
    sw t2, 0(t1)
    addi t1, t1, 4
    addi t3, t3, -1
    j word_loop

byte_loop:
    beqz a2, end
    sb a1, 0(t1)
    addi t1, t1, 1
    addi a2, a2, -1
    j byte_loop

end:
    ret
```

This version works well, but we can do better…

---

## 🧠 Bonus Version: Aligned `memset()`

To maximize performance, I wrote an **alignment-aware version** that first aligns the pointer to a 4-byte boundary using `sb`, and then proceeds with `sw`.

```asm
.align_loop:
    sb a1, 0(t1)
    addi t1, t1, 1
    addi a2, a2, -1
    andi t0, t1, 3
    bnez t0, align_loop
```

Once aligned, the optimized loop continues with `sw` instructions.

---

## 🧪 Testing with C

I wrote a test suite in C to verify the correctness of my `memset()`:

```c
void test_memset() {
    char buffer[10];
    memset(buffer, 'A', 10);

    char buffer2[50];
    memset(buffer2, 'B', 50);

    char buffer3[20];
    memset(buffer3, 'C', 0);  // edge case

    char buffer4[1000];
    memset(buffer4, 'D', 1000);
}
```

---

## 🔧 Building and Running with Spike

To run the program on a simulated RISC-V machine, I used the following script:

### `run.sh`

```bash
#!/bin/bash
filename=$1
testfile=$2

riscv64-unknown-elf-as -o memset.o $filename
riscv64-unknown-elf-gcc -o test.elf memset.o $testfile
spike pk test.elf
rm *.o 
```

### Run it:

```bash
chmod +x run.sh
./run.sh memset.S memset.c              # for basic version
./run.sh memset_aligned.S memset.c     # for aligned version
```

Make sure you have the following installed:
- `riscv64-unknown-elf-gcc`
- `spike` simulator
- `pk` (proxy kernel)

---

## 📊 Results

Both implementations passed all test cases. For large buffers (like 1000 bytes), the aligned version showed better performance and fewer instructions.

---

## 💡 Key Takeaways

✅ Learnings from this project:
- Cleanly interfacing C and assembly  
- Understanding calling conventions (RISC-V ABI)  
- Manual pointer alignment  
- Using `spike` to simulate embedded behavior  
- Gaining insight into how libc is implemented under the hood  

Even though this started as a screening task, it turned into a full learning experience in systems programming and optimization.

---

## 📌 Resources

- 💻 GitHub Repo: [https://github.com/avinashlalotra/newlib-Optimization-Task](https://github.com/avinashlalotra/newlib-Optimization-Task)  
- 📖 Newlib Source: https://sourceware.org/newlib/  
- 📚 RISC-V ELF ABI Spec: https://github.com/riscv-non-isa/riscv-elf-psabi-doc  
- 🛠 RISC-V GNU Toolchain: https://github.com/riscv-collab/riscv-gnu-toolchain  

---

## 🙏 Final Thoughts

I may not have gotten the mentorship, but I walked away with something just as valuable: confidence and clarity in writing system-level code.

If you're into embedded systems, toolchains, or want to break into open source — I highly recommend trying small tasks like these. Feel free to fork the repo and experiment!

**Thanks for reading!**  
— Abinash Singh
