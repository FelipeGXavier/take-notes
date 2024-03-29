### Endians

O conceito chamado de *endianess* se refere a forma um valor multi-byte deve ser interpretado. 

Existem dois tipos de referência: big-endian e little-endian. No caso do big-endian o byte mais importante, o que carrega mais "peso" fica por primeiro. Enquanto no little-endian é o contrário, o byte com menor relevância vai por primeiro. 

O que define a utilização de big-endian ou little-endian é a arquitetura do computador. 

Por exemplo, no caso do número 16909060:

Na representação big-endian ficaria `00000001 00000010 00000011 00000100`

Enquanto na representação little-endian ficaria `00000100 00000011 00000010 00000001` 

Como se invertessa a ordem da leitura. 

Importante ressaltar que o conceito de endianess só é funcional para informações multi-byte e é sobre ordem de bytes e não bits. 

Na comunicação entre computadores como uma rede é concenso através de RFC's na maioria dos casos o uso do formato big-endian, então sempre que necessário um computador little-endia formato o valor enviado para que a comunicação possa ocorrer. 

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