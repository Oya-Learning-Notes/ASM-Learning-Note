This post will introduce one method to print the positive integer value inside `AX`.

Basic the thought of this implementation is inspired by the basic method to get number digit from a decimal number, that is loop dividing:

```
init bit_array
while number > 0
    bit_arr.push_front(number % 10)
    number /= 10

for i : bit_array.size()
    print bit_array[i]
```

Below is an implementation of this thought.

# Code Implementation

```asm
; print the value inside ax in decimal
; 
; Notice:
; This method require user to set a 7 byte arr inside es, and store the arr offset inside 
; di in advance.
; This arr will be used to store intermediate result
; 
; Notice:
; This method do NOT support negative number
define_print_ax_dec macro  

local skip, calc_one, print_out, print_one_bit, print_zero, end_print  

jmp skip
    
print_ax_dec proc      
    push di
    push dx 
    push cx 
    push bx
    push ax
    
    ; check if the initial value is already zero
    cmp ax, 0
    jz print_zero                               
    
    ; loop and store inside
    mov bx, 10          
    mov cx, 0
    
    calc_one:
    cmp ax, 0
    jz print_out
    mov dx, ax
    mod_ax_bx   
    mov es:[di], ax
    inc di
    inc cx           
    mov ax, dx     
    mov dx, 0
    div bx
    jmp calc_one
    
    print_out:       
    print_one_bit:
    sub di, 1
    mov al, es:[di]   
    add al, 48
    mov ah, 0eh
    int 10h
    loop print_one_bit
    jmp end_print
    
    
    ; the value inside ax is zero
    print_zero:
    mov al, '0'
    mov ah, 0eh
    int 10h
    jmp end_print
    
    
    end_print:                  
    pop ax
    pop bx
    pop cx 
    pop dx
    pop di
    ret
print_ax_dec endp

skip:

define_print_ax_dec endm
```

One thing to notice is that when we use `MUL` and `DIV`, **if we only want to do this arithmetic on `AX`, then we shall set another implicitly used register `DX` to `0`**, otherwise we may faced some unexpected error. For example when `DX` is not zero then the result will not be `AX * OPAND`, but `(DX*0AAAAH + AX) * OPAND` which is unexpected.

The code of `mod_ax_bx` is:

```asm
; calclate ax % bx, result stored in ax
mod_ax_bx macro 
    push cx
    push dx 
    mov dx, 0  
    
    mov cx, ax
    div bx
    mul bx
    sub cx, ax
    mov ax, cx
    
    pop dx
    pop cx
mod_ax_bx endm
```

Here are several things that we need to noticed when writing this proc.

**Negative Number Incapability**. Since we have used both `MUL` and `DIV` inside this function and in the utility funccion `mod_ax_bx`, which is the unsigned arithmatic instruction, it would become impossible for this proc to print negative number.

**Extra Segment Configuration Needed**. Before calling this function, we must already distribute memory for _Extra Segment_ and load its address into `ES`. Also, we need at least _7 bytes_ memory which will be used by proc to store intermediate number.

# Example Usecase

Below is an example to use this proc.

```asm
include 'print_ax.inc'

extra segment
    arr db 7 dup(0)
ends           

data segment
    test_arr dw 363, 4533, 7092, 19602, 0, 5
ends

code segment
    start:
    mov ax, data
    mov ds, ax    
    mov ax, extra
    mov es, ax
    mov ax, 0
    mov di, 0
                      
    mov cx, 6  
    lea si, test_arr
    loop_print: 
    lodsw
    call print_ax_dec
    print_char ','
    loop loop_print
    
    mov ah, 4ch
    int 21h
ends                 

define_print_ax_dec

end start
```

Notice that we used `include` pre-process keyword. It would be the same that you directly paste the proc code there, which should also work as intend.

Below is the output screenshot:

![image](https://github.com/Oya-Learning-Notes/ASM-Learning-Note/assets/61616918/2eca7e35-165c-4d11-a6c9-1f372c0c523d)

All value is successfully printed on the screen.