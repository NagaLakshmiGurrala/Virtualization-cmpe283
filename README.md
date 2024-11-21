# Virtualization-cmpe283
 Assignment 2



### Questions and Answers



#### Q1: Did you complete this lab alone or as part of a team?
Answer: I completed this lab by myself.



#### Q2: Steps to Complete the Lab

1. Have a Working Assignment 01 Configuration:
   - This lab builds on the setup and configurations from Assignment 01, so it is essential to have a functional Assignment 01 configuration in place.

2. Modify the CPUID Leaf (0x4FFFFFFE) Function:
   - I modified the function to provide exit counts for individual VM exit types. The following files were updated:
     - `assignment02/linux/arch/x86/kvm/cpuid.c`:
       - Added the `query_exits()` function to query exit statistics.
     - `assignment02/linux/arch/x86/kvm/vmx/vmx.c`:
       - Modified `cmpe283_increase()` and `handle_exits()` to handle the incrementing of exit counters.

3. Start the Nested Virtual Machine (L1):
   - In my setup, I configured a headless nested virtual machine. To access the console of the nested VM, use the following commands:
     ```bash
     sudo virsh start <vm-name>
     sudo virsh console <vm-name>
     ```
   - When prompted with `ESC ^]` on the console, press the Enter key to enter the VM console.

4. Create a User-Mode Program to Call CPUID (0x4FFFFFFE):
   - Developed a user-mode program to call the CPUID instruction at leaf `0x4FFFFFFE` and observe the exit counts.

---

#### Q3: Frequency of Exits

Answer:
- The frequency of exits does **not** increase at a stable rate. Instead, it varies depending on the VM operations being performed.
- During certain operations, such as external interrupts, I observed spikes in exit counts.
- For a complete VM boot, the total number of exits was approximately **515,155**, as measured using the `dmesg` command.
- Commonly observed exit types during boot included:
  - External Interrupt Exit
  - Interrupt Window Exit
  - Control-Register Access Exit
  - I/O Instruction Exit**

---

#### Q4: Most and Least Frequent Exit Types

Answer:
- Most Frequent Exit Types:
  - External Interrupt (Exit Number: 1): 34,266 exits
  - CPUID (Exit Number: 10): 139,717 exits
  - Control-Register Access (Exit Number: 28): 102,212 exits
  - I/O Instruction (Exit Number: 30): 216,954 exits

- Least Frequent Exit Types:
  - MOV DR (Exit Number: 29): 2 exits
  - Exception (Exit Number: 0): 9 exits
  - APIC Access (Exit Number: 44): 22 exits

Below is a snapshot of the exit statistics from the lab:

```
cpuid(0x4FFFFFFFE), exit number = 0, exits = 9
cpuid(0x4FFFFFFFE), exit number = 1, exits = 34,266
cpuid(0x4FFFFFFFE), exit number = 7, exits = 8,709
cpuid(0x4FFFFFFFE), exit number = 10, exits = 139,717
cpuid(0x4FFFFFFFE), exit number = 12, exits = 50,734
cpuid(0x4FFFFFFFE), exit number = 28, exits = 102,212
cpuid(0x4FFFFFFFE), exit number = 29, exits = 2
cpuid(0x4FFFFFFFE), exit number = 30, exits = 216,954
cpuid(0x4FFFFFFFE), exit number = 31, exits = 385
cpuid(0x4FFFFFFFE), exit number = 32, exits = 86,133
cpuid(0x4FFFFFFFE), exit number = 44, exits = 22
cpuid(0x4FFFFFFFE), exit number = 48, exits = 1,095
cpuid(0x4FFFFFFFE), exit number = 49, exits = 1,953
cpuid(0x4FFFFFFFE), exit number = 54, exits = 9
Remaining exit counts are zero.
