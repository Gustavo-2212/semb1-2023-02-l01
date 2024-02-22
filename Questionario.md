# Questionário Sistemas Embarcados I

## 1. Explique brevemente o que é compilação cruzada (***cross-compiling***) e para que ela serve.

> Compilação cruzada ocorre quando um programa é compilado em um sistema que possui uma determinada arquitetura de hardware, para um outro sistema com outra arquitetura de hardware. \
Sendo assim, temos um sistema **HOST** que compilará um programa de modo a poder ser executado em um outro sistema completamente diferente, denominado **TARGET**.
No nosso caso, temos o sistema do nosso computador, que baixamos as ferramentas de compilação para compilarmos um programa para ser executado no sistema com processador CORTEX M4.

## 2. O que é um código de inicialização ou ***startup*** e qual sua finalidade?

> Um código inicialização é o código responsável por:
    **1)** definição da tabela de vetores de interrupções;\
    **2)** chamar a função **main** do nosso programa principal **\*.c**;\
    **3)** inicializar as seções **.data** (copiar da memória **FLASH** para a memória **RAM**) e **.bss** (Zerar / Remover o lixo e colocar na **RAM**);\
    **4)** Inicializar a **STACK**;\
<br>Em sistemas em que tenha um sistema operacional e meios de entrada e saída:
    **5)** Preparar os meios de saída e entrada;\
    **6)** Adicionar a stack os valores passador pela linha de comando as variáveis **argc** e **argv**;\
    **7)** Inicialização da **HEAP** (Alocação dinâmica)

## 3. Sobre o utilitário **make** e o arquivo **Makefile responda**:

#### (a) Explique com suas palavras o que é e para que serve o **Makefile**.

> O arquivo Makefile é um passo a passo de como nosso programa será compilado. São as instruções que "automatizam" (reduzem a complexidade) da compilação manual do nosso projeto.

#### (b) Descreva brevemente o processo realizado pelo utilitário **make** para compilar um programa.

> O utilitário **make** irá compilar o nosso programa baseando-se em regras. Essas regras são instruções de como um ou mais arquivos alvos **(target)** são obtidos, a partir de outros arquivos **(pre-requisitos)**.\
Então, se precisamos compilar um arquivo **\*.c** e gerar o arquivo objeto **\*.o** (antes da linkedição), temos por alvo e pré-requisitos, respectivamente, o arquivo objeto que será gerado e o arquivo fonte **\*.c** e possíveis outros arquivos que ele dependa. E com base nas instruções de como é gerado **(receita/recipe)**, o arquivo objeto será gerado:\
**(1) se ele não existir**; OU\
**(2) se ele for mais antigo que qualquer um dos arquivos que ele dependa**

#### (c) Qual é a sintaxe utilizada para criar um novo **target**?

> A sintaxe para criar um target é da seguinte forma:

```Makefile
target: prerequisites
    recipe
```

#### (d) Como são definidas as dependências de um **target**, para que elas são utilizadas?

> As dependências de um ou mais **targets** são definidos logo a frente da definição dos targets, separando os por dois pontos.\
Elas são utilizadas para informar quando o target será construído/reconstruído e a partir de quais arquivos.

#### (e) O que são as regras do **Makefile**, qual a diferença entre regras implícitas e explícitas?

> As regras do **Makefile** são instruções que indicam como um arquivo alvo será criado ou atualizado.\
As **regras explícitas** indicam como atualizar ou criar um ou mais arquivos targets listados, com base em uma lista de arquvios que os tragets dependam;
```Makefile
main.o: main.c
    arm-none-eabi-gcc -c -g -mthumb -mcpu=cortex-m4 -O0 -Wall main.c -o main.o
```
As **regras implícitas**, de modo semelhante, indicam como atualizar ou criar os arquivos targets, porém tanto os arquivos target como os arquivos pré-requisitos não são listados, mas um caractere coringa **(%)** é utlizado para abstrair os arquivos, fazendo uma regra que gera uma classe de arquivos targets com base em uma classe de arquvios pré-requisitos.
```Makefile
%.o: %.c
    arm-none-eabi-gcc -c -g -mthumb -mcpu=cortex-m4 -O0 -Wall $< -o $@
```

## 4. Sobre a arquitetura **ARM Cortex-M** responda:

### (a) Explique o conjunto de instruções ***Thumb*** e suas principais vantagens na arquitetura ARM. Como o conjunto de instruções ***Thumb*** opera em conjunto com o conjunto de instruções ARM?



### (b) Explique as diferenças entre as arquiteturas ***ARM Load/Store*** e ***Register/Register***.



### (c) Os processadores **ARM Cortex-M** oferecem diversos recursos que podem ser explorados por sistemas baseados em **RTOS** (***Real Time Operating Systems***). Por exemplo, a separação da execução do código em níveis de acesso e diferentes modos de operação. Explique detalhadamente como funciona os níveis de acesso de execução de código e os modos de operação nos processadores **ARM Cortex-M**.



### (d) Explique como os processadores ARM tratam as exceções e as interrupções. Quais são os diferentes tipos de exceção e como elas são priorizadas? Descreva a estratégia de **group priority** e **sub-priority** presente nesse processo.



### (e) Qual a diferença entre os registradores **CPSR** (***Current Program Status Register***) e **SPSR** (***Saved Program Status Register***)?



### (f) Qual a finalidade do **LR** (***Link Register***)?



### (g) Qual o propósito do Program Status Register (PSR) nos processadores ARM?



### (h) O que é a tabela de vetores de interrupção?



### (i) Qual a finalidade do NVIC (**Nested Vectored Interrupt Controller**) nos microcontroladores ARM e como ele pode ser utilizado em aplicações de tempo real?



### (j) Em modo de execução normal, o Cortex-M pode fazer uma chamada de função usando a instrução **BL**, que muda o **PC** para o endereço de destino e salva o ponto de execução atual no registador **LR**. Ao final da função, é possível recuperar esse contexto usando uma instrução **BX LR**, por exemplo, que atualiza o **PC** para o ponto anterior. No entanto, quando acontece uma interrupção, o **LR** é preenchido com um valor completamente  diferente,  chamado  de  **EXC_RETURN**.  Explique  o  funcionamento  desse  mecanismo  e especifique como o **Cortex-M** consegue fazer o retorno da interrupção. 



### (k) Qual  a  diferença  no  salvamento  de  contexto,  durante  a  chegada  de  uma  interrupção,  entre  os processadores Cortex-M3 e Cortex M4F (com ponto flutuante)? Descreva em termos de tempo e também de uso da pilha. Explique também o que é ***lazy stack*** e como ele é configurado. 



---
## Referências

### Básicas

[1] [STM32F411xC Datasheet](https://www.st.com/resource/en/datasheet/stm32f411ce.pdf)

[2] [RM0383 Reference Manual](https://www.st.com/resource/en/reference_manual/rm0383-stm32f411xce-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)

[3] [Using the GNU Compiler Collection (GCC)](https://gcc.gnu.org/onlinedocs/gcc/index.html)

[4] [GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

### Vídeos Microsoft Teams

[5] [05 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[6] [06 - Arquitetura Cortex-M Parte 1/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[7] [07 - Arquitetura Cortex-M Parte 2/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[8] [08 - Microcontroladores STM32](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[9] [10 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

### Material Suplementar

[5] [A General Overview of What Happens Before main()](https://embeddedartistry.com/blog/2019/04/08/a-general-overview-of-what-happens-before-main/)
 
[6] [Bare metal embedded lecture-1: Build process](https://youtu.be/qWqlkCLmZoE?si=mn5yDnJYudQ1PpZH)
 
[7] [Bare metal embedded lecture-2: Makefile and analyzing relocatable obj file](https://youtu.be/Bsq6P1B8JqI?si=yuNLPj3JQ-2IT1yo)
 
[8] [Bare metal embedded lecture-3: Writing MCU startup file from scratch](https://youtu.be/2Hm8eEHsgls?si=c27MpZ47ApiMSwHR)
 
[9] [Lecture 15: Booting Process](https://youtu.be/3brOzLJmeek?si=MsHRUEJP8zofjwJQ)