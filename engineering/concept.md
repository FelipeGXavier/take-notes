### Endians

### Most and least signficant bit

### Bitwise operations

### System calls

É uma forma onde uma máquina partindo de um programa requisita um serviço ao kernel onde o sistema operacional está rodando. *System calls* funcionam como uma interface entre a aplicação a nível de usuário e o sistema operacional realizando operações que podem vir a interagir com o hardware.

Por exemplo, quando em Go abrimos um arquivo `os.Open(path)` no fim das contas o runtime da linguagem tem um código em assembly especifico para a arquitetura que está rodando que vai fazer as chamadas para o kernel. 

Quando chamamos então `os.Open(path)` o runtime chama um arquivo em assembly como `sys_linux_amd64.s` que tem a implementação da chamada ao kernel, algo como: 

<pre>
#include "textflag.h"

// func open(name string, mode int32, perm int32) int32
TEXT ·open(SB), NOSPLIT, $0
	MOVQ	name+0(FP), DI	// name
	MOVL	mode+8(FP), SI	// mode
	MOVL	perm+12(FP), DX	// perm
	LEAQ	errortable<>(SB), AX
	CALL	runtime·open(SB)
	RET

// func read(fd int32, p []byte) int32
TEXT ·read(SB), NOSPLIT, $0
	MOVL	fd+0(FP), DI	// fd
	MOVQ	p+8(FP), SI	// p
	LEAQ	errortable<>(SB), AX
	CALL	runtime·read(SB)
	RET
</pre>