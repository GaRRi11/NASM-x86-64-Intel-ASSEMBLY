| Register   | Typical use                                                      |
| ---------- | ---------------------------------------------------------------- |
| **RAX**    | Main register for calculations (add, subtract, multiply, etc.)   |
| **RBX**    | Often used to store data or base addresses                       |
| **RCX**    | Used for counting loops and passing function arguments           |
| **RDX**    | Used for extra data during multiplication/division and arguments |
| **RSI**    | Source index (for reading data from memory)                      |
| **RDI**    | Destination index (for writing data to memory)                   |
| **RSP**    | Stack pointer (points to top of the stack)                       |
| **RBP**    | Base pointer (used in stack frames)                              |
| **RIP**    | Instruction pointer (holds address of the next instruction)      |
| **R8–R15** | Additional general-purpose registers introduced in x86-64        |


## RAX 

### C

<pre>
int main() {
    return 2 + 3;
}
</pre>
### ASSEM

<pre>
section .text
global _start

_start:
    mov rax, 2
    add rax, 3
    ; RAX = 5
</pre>

## RBX

### C 

<pre>
int main() {
    long x = 10;
    return x;
}
</pre>

### ASSEM

<pre>
section .text
global _start

_start:
    mov rbx, 10
    ; RBX = 10 (holds data)
</pre>

## RCX

### C 

<pre>
int main() {
    for (int i = 0; i < 3; i++);
}
</pre>

### ASSEM

<pre>
section .text
global _start

_start:
    mov rcx, 3        ; loop count = 3
loop_start:
    loop loop_start   ; decrements RCX, jumps if not zero
</pre>

## RDX

### C 

<pre>
int main() {
    long a = 10, b = 3;
    long result = a * b;
    return result;
}
</pre>

### ASSEM

<pre>
section .text
global _start

_start:
    mov rax, 10
    mov rbx, 3
    mul rbx        ; unsigned multiply RAX * RBX → result in RDX:RAX
    ; RAX = low part, RDX = high part
</pre>

## RSI 

### C 

<pre>

int main() {
    char s[] = "A";
    char c = s[0];
    return c;
}
</pre>

### ASSEM

<pre>
section .data
    msg db 'A'

section .text
global _start

_start:
    mov rsi, msg     ; RSI points to source data
    mov al, [rsi]    ; read byte from [RSI] → AL = 'A'
</pre>

## RDI 

### C

<pre>
int main() {
    char src = 'A';
    char dest;
    dest = src;
    return dest;
}
</pre>

### ASSEM

<pre>

section .data
    dest db 0
    val  db 'A'

section .text
global _start

_start:
    mov rdi, dest    ; RDI points to destination
    mov al, [val]    ; read value
    mov [rdi], al    ; write to [RDI]
</pre>

## RSP

### C 

<pre>
int main() {
    int x = 5;
    return x;
}
</pre>

### ASSEM

<pre>
section .text
global _start

_start:
    push 5        ; push value onto stack → RSP moves down
    pop rax       ; pop value from stack → RSP moves up, RAX = 5
</pre>

## RPB

### C 

<pre>

int sum(int a, int b) {
    return a + b;
}
int main() {
    return sum(2, 3);
}
</pre>

### ASSEM

<pre>
section .text
global _start

_start:
    push rbp        ; save old base pointer
    mov rbp, rsp    ; set new base pointer
    mov rax, 2
    add rax, 3
    pop rbp         ; restore old base pointer
</pre>

## RIP

### C

<pre>


void func() {
    printf("Inside func\n");
}

int main() {
    func();  // calling function changes RIP behind the scenes
    return 0;
}
</pre>

### ASSEM

<rep>
section .text
global _start

_start:
    jmp label    ; jump → RIP updated to label
    nop          ; skipped

label:
    nop          ; RIP now points here
</rep>
