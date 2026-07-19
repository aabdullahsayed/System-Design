# Day 1: How Computers Actually Work

**Week 1 · Data Representation & Complexity**

## 🎯 Learning Objectives
- Explain how data is represented and processed at the hardware level
- Understand the CPU-memory relationship
- Build the mental model everything else in this course sits on

## 📚 Topics
- Binary, hexadecimal, and number representation
- Bits, bytes, words, and memory addressing
- CPU registers, the fetch-decode-execute cycle
- Compilation vs interpretation, high-level code to machine code

## 💡 Core Ideas
- Every value in a computer is bits — the *type* is just an agreed-upon interpretation of those bits.
- Memory is a giant array of bytes; a 'pointer' is just an index into that array.
- The CPU can only do arithmetic, comparisons, and jumps — everything else is built from these.

## 🛠️ Practice Exercises
- [ ] By hand, convert 5 numbers between binary, hex, and decimal.
- [ ] Write a short program that prints the memory address of a few variables and observe how arrays lay out contiguously.
- [ ] Explain in your own words (write it down) why a `float` can't represent every decimal number exactly.

## 🎤 Interview-Style Questions
Try answering these out loud, without notes, before checking any reference:

- Why is integer overflow a security concern?
- What's the difference between a 32-bit and 64-bit system, practically speaking?
- Why does `0.1 + 0.2 != 0.3` in most languages?

## 🔗 Resources
- https://cs.fyi/guide/how-computer-works
- https://www.hpcwire.com (search: fetch decode execute)

## ✅ Daily Checklist
- [ ] Read/reviewed the topics and core ideas above
- [ ] Completed all practice exercises
- [ ] Answered the interview-style questions out loud or in writing
- [ ] Committed any code to your repo
- [ ] Wrote 2-3 sentences in your own words summarizing the day's key insight

---
[🏠 Home](README.md) | [Next Day ➡](day-02.md)
