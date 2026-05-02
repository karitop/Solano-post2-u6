# Solano-post2-u6

Programa en NASM (.COM) que demuestra cuatro modos de direccionamiento
de la arquitectura x86 en modo real de 16 bits, verificados con DEBUG en DOSBox.

**Tabla resumen de modos de direccionamiento**
| Modo | Fórmula dirección efectiva | Instrucción NASM usada | Valor observado en DEBUG |
|---|---|---|---|
| INMEDIATO | EA = ninguna (el valor está embebido en el opcode) | `MOV ax, 100` | U 100 : `JMP 124` - U 124 : `MOV AX,0064` (AX = 0064h = 100 decimal) |
| DIRECTO | EA = dirección fija codificada en la instrucción | `MOV ax, [var_x]` | U 124 : `MOV AX,[0112]` - D DS:0112 : `FF FF` |
| INDIRECTO POR REGISTRO | EA = [SI] o [BX] (contenido del registro) | `MOV ax, [si]` | T : SI=010Ch - AX=0055h (85 decimal) |
| INDEXADO (BASE + ÍNDICE + DESPLAZAMIENTO) | EA = BX + SI + desplazamiento | `MOV ax, [bx + si]` | T : BX=0102h, SI=4 - AX=001Eh (30 decimal) |

**Trazado del modo indirecto**
| Instrucción | Registro modificado | Valor resultante |
|---|---|---|
| `MOV si, nota1` | SI | 010C (dirección de nota1) |
| `MOV ax, [si]` | AX | 0055h (85 decimal) |
| `MOV si, nota2` | SI | 010E (dirección de nota2) |
| `MOV bx, [si]` | BX | 0049h (73 decimal) |
| `ADD ax, bx` | AX | 009Eh (158 decimal) |
| `SHR ax, 1` | AX | 004Fh (79 decimal) |
| `MOV si, promedio` | SI | 0110 (dirección de promedio) |
| `MOV [si], ax` | mem[0110] | 004Fh escrito en memoria |
