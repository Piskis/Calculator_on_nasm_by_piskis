%macro printf 2
mov rax, 1
mov rdi, 1
mov rsi, %1
mov rdx, %2
syscall
%endmacro

%macro name_input 2
mov rax, 0
mov rdi, 0
mov rsi, %1
mov rdx, %2
syscall
%endmacro


section .data
first_message db "Hello and welcome to", 0
              db " the calculator on asm!", 0, 10

len_first equ $ - first_message

second_message db "Enter a number to sum it:", 0, 10

len_second equ $ - second_message

message_3 db "A second number:", 0, 10

len_3 equ $ - message_3


section .bss
result resw 256
num1 resb 256
num2 resb 256

section .text
global _start
_start:

; Сбор необходимых данных
printf first_message, len_first
printf second_message, len_second
name_input num1, 256
printf message_3, len_3
name_input num2, 256
; ---------------------------------

lea rdi, [num1]
call string_to_line
mov r8, rax

lea rdi, [num2]
call string_to_line
add rax, r8

; Операции

mov rbx, rax

mov rsi, result
mov rax, rbx

mov rcx, 10
call itoa

mov rax, 1
mov rdi, 1
mov rdx, 4
syscall

jmp _exit

; Функции

string_to_line:

push rbx
xor rax, rax
xor rcx, rcx
xor rbx, rbx

.skip_spaces:
  mov cl, byte [rdi]
  cmp cl, ' '
  jne .check_sing
  inc rdi
  jmp .skip_spaces

.check_sing:
  mov cl, byte [rdi]
  cmp cl, '-'
  jne .check_plus
  mov bl, 1
  inc rdi
  jmp .parse_digits

.check_plus:
  mov cl, byte [rdi]
  cmp cl, '+'
  jne .parse_digits
  inc rdi

.parse_digits:
  mov cl, byte [rdi]
  cmp cl, 0
  je .done

  cmp cl, '0'
  jb .done
  cmp cl, '9'
  ja .done

  imul rax, rax, 10
  sub cl, '0'
  movzx rcx, cl
  add rax, rcx

  inc rdi
  jmp .parse_digits

.done:
  cmp bl, 1
  jne .positive
  neg rax

.positive:
  pop rbx
  ret

itoa:
  add rsi, 3
  mov byte [rsi], 0

itoa_loop:
  dec rsi
  xor rdx, rdx
  div rcx
  add dl, '0'
  mov [rsi], dl
  test rax, rax
  jnz itoa_loop
  ret


_exit:
mov rax, 60
xor rdi, rdi
syscall
