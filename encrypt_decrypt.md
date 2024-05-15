'''
section .text
	global _start
_start:
;encryption
mov eax, 'O'		;first letter of plain text
mov ebx, 'f'		;first letter of key word
push ebx
xor eax, ebx		;encrypt plain text and key with xor
mov [result], eax	;mov encryption to result

mov eax, 'R'		;second letter plain text
mov ebx, 'l'		;second key word
xor eax, ebx		;xor encryption
mov [result2], eax	;mov second encryption to result2

mov eax, 'A'		;third letter plain text
mov ebx, 'o'		;third key word
xor eax, ebx		;xor encryption
mov [result3], eax	;mov third encryption to result3

mov eax, 'N'		;fourth letter plain text
mov ebx, 'w'		;fourth key word
xor eax, ebx		;xor encryption
mov [result4], eax	;mov fourth encryption to result4

mov eax, 'G'		;fifth letter plain text
mov ebx, 'e'		;fifth key word
xor eax, ebx		;xor encryption
mov [result5], eax	;mov fifth encryption to result5

mov eax, 'E'		;sixth letter plain text
mov ebx,'r'		;sixth key word
xor eax, ebx		;xor encryption
mov [result6], eax	;mov sixth encryption to result6

;file created
mov eax,8		;create new file (sys_creat)
mov ebx, filename	;create filename('output.txt')
mov ecx, 0777o		;set permission to read, write, and execute (octual format)
int 0x80		;call kernal

;file open
mov eax, 5		;open file (sys_open)
mov ebx, filename	;file
mov ecx, 1		;
mov edx, 0777o		;read, write, and execute (octual format)
int 0x80		;call kernal

mov [fd_out],eax	;file decriptor

;file write
mov eax, 4		;write file (sys_write)
mov ebx, [fd_out]
mov ecx, message	;plain text
mov edx,19		;number of bytes written
int 0x80

mov eax,4
mov ebx, [fd_out]
mov ecx, key		;key word
mov edx, 17		;number of bytes
int 0x80

;encryption output
mov eax, 4
mov ebx, [fd_out]
mov ecx, text		;encryption
mov edx,11		;bytes
int 0x80

mov eax,4
mov ebx, [fd_out]
mov ecx, result		;first encryption())
mov edx,1
int 0x80

mov eax, 4
mov ebx, [fd_out]
mov ecx, result2	;second encryption(>)
mov edx,1
int 0x80

mov eax, 4
mov ebx, [fd_out]
mov ecx, result3	;third encryption(.)
mov edx,1
int 0x80

mov eax, 4
mov ebx, [fd_out]
mov ecx, result4	;fourth encryption(9)
mov edx, 1
int 0x80

mov eax, 4 
mov ebx, [fd_out]
mov ecx, result5	;fifth encryption(")
mov edx,1
int 0x80

mov eax,4
mov ebx, [fd_out]
mov ecx, result6	;sixth encryption(7)
mov edx,1
int 0x80

;new line
mov eax,4
mov ebx, [fd_out]
mov ecx, next		;create new line after encryption
mov edx,1
int 0x80

mov eax, 4
mov ebx, [fd_out]
mov ecx, text2		;decryption 
mov edx, 8
int 0x80

;decryption
mov eax,[result]  	;first encryption())
pop ebx 		;pop key(f)
xor eax, ebx		;xor encryption and key(O)
mov [result], eax	;decrypt(O) result

mov eax, [result2]	;second encryption(>)
mov ebx, 'l'		;key word
xor eax, ebx		;xor encryption and key(R)
mov [result2], eax	; mov decryption(R) to result2

mov eax, [result3]	;third encryption (.)
mov ebx, 'o'		;key word
xor eax, ebx		;xor encryption and key(A)
mov [result3], eax	;mov decryption(A) to result3

mov eax,[result4]	;fourth enctyption(9)
mov ebx, 'w'		;key word
xor eax, ebx		;xor encryption and key(N)
mov [result4], eax	;mov decryption(N) to result4

mov eax, [result5]	;fifth encryption(")
mov ebx, 'e'		;key word
xor eax, ebx		;xor encryption and key(G)
mov [result5], eax	;mov decryption(G) to result5

mov eax, [result6]	;sixth encryption(7)
mov ebx, 'r'		;key word
xor eax, ebx		;xor encyprtion and key(E)
mov [result6], eax	;mov decryption(E) to result6

mov eax, 4
mov ebx, [fd_out]
mov ecx, result		;decryption (O)
mov edx,1
int 0x80

mov eax,4
mov ebx, [fd_out]
mov ecx, result2	;decryption 2 (R)
mov edx,1
int 0x80

mov eax, 4
mov ebx, [fd_out]
mov ecx, result3	;decryption 3 (A)
mov edx,1
int 0x80

mov eax,4
mov ebx, [fd_out]
mov ecx, result4	;decryption 4 (N)
mov edx,1
int 0x80

mov eax,4
mov ebx, [fd_out]
mov ecx, result5	;decryption 5 (G)
mov edx,1
int 0x80

mov eax,4
mov ebx, [fd_out]
mov ecx, result6	;decryption 6 (E)
mov edx,1
int 0x80

;file close
mov eax, 6		;close file(sys_close)
mov ebx,[fd_out]	;descriptor
int 0x80		;call kernal

mov eax,1		;reset to 1
int 0x80		;call kernal

section .data
filename db 'output.txt', 0h		;filename to create
message db 'Plain Text: ORANGE', 0xa	;plain text
key db 'Key Word: flower', 0xa		;key word
text db 'Encryption:'
text2 db 'Decrypt:'
result db 0h				;first letter result
result2 db 0h				;second result
result3 db 0h				;third result
result4 db 0h				;fourth result
result5 db 0h				;fifth result
result6 dd 0h				;sixth result
next dd 0xa				;create next line after last letter(result6)

segment .bss
fd_out resb 1				;descriptor
'''
