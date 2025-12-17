Este projeto foi criado como parte do currículo da 42 por **pdiniz-l**.

## get_next_line
### Descrição

**get_next_line** é uma biblioteca em C que fornece uma função para ler um file descriptor **linha por linha**, retornando uma linha a cada chamada da função.

O objetivo deste projeto é implementar uma solução robusta e segura em termos de memória, que utilize leitura bufferizada, preserve o estado entre chamadas e trate corretamente casos extremos como fim de arquivo (EOF), ausência de caractere de nova linha, file descriptors inválidos e falhas de alocação de memória.

Esta implementação segue as especificações do **Get Next Line v13** do currículo da 42 e foi escrita em conformidade com as restrições do projeto e as funções permitidas.

### Instruções

#### Compilação

O projeto deve ser compilado definindo a macro BUFFER_SIZE, que controla quantos bytes são lidos a cada chamada da função read.

Exemplo:

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line.c get_next_line_utils.c
```

Você pode alterar o valor de `BUFFER_SIZE`para testar diferentes comportamentos de buffer.

#### Descrição da Biblioteca

Este projeto implementa a seguinte função pública:
```c
char *get_next_line(int fd);
```

#### Comportamento

* Lê do file descriptor fornecido e retorna uma linha por chamada

* Retorna a linha incluindo \n caso um caractere de nova linha esteja presente

* Retorna a última linha sem \n se o EOF for alcançado sem uma nova linha ao final

* Retorna NULL em caso de erro ou quando não houver mais linhas para ler

* Preserva os dados ainda não lidos entre chamadas utilizando uma variável estática interna

* O chamador é responsável por liberar a memória da string retornada

#### Estrutura dos Arquivos

`get_next_line.h` - Arquivo de cabeçalho contendo os protótipos e include guards

`get_next_line.c` - Implementação principal da função get_next_line

`get_next_line_utils.c` - Funções utilitárias usadas internamente pela função principal

#### Funções Utilitárias

As seguintes funções auxiliares são reimplementadas e utilizadas internamente:

`ft_strlen(const char *str)` — retorna o tamanho da string

`ft_strdup(const char *s)` — duplica uma string

`ft_substr(char const *s, unsigned int start, size_t len)` — cria uma substring

`ft_strjoin(char const *s1, char const *s2)` — concatena duas strings

`ft_strchr(const char *s, int c)` — busca um caractere em uma string

#### Lógica Interna

A implementação utiliza funções auxiliares internas para gerenciar estado e memória:

`memorize_line` — adiciona o buffer lido à string acumulada

`line_with_nl` — extrai uma linha até o \n e armazena os dados restantes

`make_line` — trata os valores de retorno do read e constrói a saída final

`safe_free_dp` — libera ponteiros alocados dinamicamente de forma segura

#### Tratamento de Erros e Proteções

A função retorna `NULL` se:

* `fd < 0`

* `BUFFER_SIZE <= 0`

* Ocorrer erro na chamada de read

* Ocorrer falha na alocação de memória

* Toda a memória alocada é liberada corretamente em caso de falha

* Nenhum memory leak é introduzido

### Bônus

O projeto **get_next_line** possui uma parte bônus opcional, que só deve ser avaliada caso a parte obrigatória esteja perfeitamente funcional e sem erros.

#### Requisitos do Bônus

Implementar uma versão de **get_next_line** capaz de lidar com múltiplos **file descriptors** simultaneamente

O comportamento deve ser correto mesmo quando chamadas intercaladas são feitas com diferentes fds

Cada file descriptor deve manter seu próprio estado interno de leitura

#### Restrições

* É permitido o uso de apenas uma variável estática

* O gerenciamento de memória deve ser feito de forma segura, sem memory leaks

* O comportamento deve permanecer idêntico ao da parte obrigatória para um único fd

#### Estrutura do Bônus

A implementação do bônus deve estar nos seguintes arquivos:

`get_next_line_bonus.c` - Implementação da função get_next_line com suporte a múltiplos file descriptors

`get_next_line_bonus.h` - Arquivo de cabeçalho do bônus

`get_next_line_utils_bonus.c` - Funções utilitárias usadas pela versão bônus

#### Compilação do Bônus

Exemplo de compilação da parte bônus:
```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line_bonus.c get_next_line_utils_bonus.c
```
Assim como na parte obrigatória, o valor de `BUFFER_SIZE` pode ser alterado para testar diferentes comportamentos de leitura.

### Recursos

42 School — enunciado do Get Next Line v13

Documentação POSIX:

* read(2)

Páginas de manual:

* man read

* man open

* Documentação da biblioteca padrão C

# English Version

*This project has been created as part of the 42 curriculum by **pdiniz-l**.*

## get_next_line

### Description

**get_next_line** is a C library that provides a function to read a file descriptor **line by line**, returning one line per function call.

The goal of this project is to implement a robust, memory-safe solution that handles buffered reading, preserves state between calls, and correctly manages edge cases such as end-of-file, missing newline characters, invalid file descriptors, and allocation failures.

This implementation follows the specifications of **Get Next Line v13** from the 42 curriculum and is written in compliance with the project constraints and allowed functions.

---

### Instructions

#### Compilation

The project must be compiled while defining the `BUFFER_SIZE` macro, which controls how many bytes are read per call to `read`.

Example:

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line.c get_next_line_utils.c
```
You may change the value of `BUFFER_SIZE` to test different buffer behaviors.

#### Library Description

This project implements the following public function:
```c
char *get_next_line(int fd);
```

#### Behavior

* Reads from the given file descriptor and returns one line per call

* Returns the line including `\n` if a newline is present

* Returns the last line without `\n` if EOF is reached without a trailing newline

* Returns NULL on error or when there are no more lines to read

* Preserves unread data between calls using an internal static variable

* The caller is responsible for freeing the returned string

#### File Structure

`get_next_line.h` - Header file containing prototypes and include guards

`get_next_line.c` - Core implementation of get_next_line

`get_next_line_utils.c` - Utility functions used internally by the main function

#### Utility Functions

The following helper functions are reimplemented and used internally:

`ft_strlen(const char *str)` — returns string length

`ft_strdup(const char *s)` — duplicates a string

`ft_substr(char const *s, unsigned int start, size_t len)` — creates a substring

`ft_strjoin(char const *s1, char const *s2)` — concatenates two strings

`ft_strchr(const char *s, int c)` — searches for a character in a string

#### Internal Logic

The implementation relies on internal static helpers to manage state and memory:

`memorize_line` — appends the read buffer to the accumulated string

`line_with_nl` — extracts a line up to \n and stores the remaining data

`make_line` — handles read return values and builds the final output

`safe_free_dp` — safely frees dynamically allocated pointers

#### Error Handling and Guards

Returns `NULL` if:

* `fd < 0`

* `BUFFER_SIZE <= 0`

* read fails

* Memory allocation fails

* All allocated memory is properly freed on failure

* No memory leaks are introduced

### Bonus

The **get_next_line project** includes an optional bonus part, which is evaluated only if the mandatory part is perfectly functional and free of errors.

#### Bonus Requirements

Implement a version of **get_next_line that** can handle multiple **file descriptors** simultaneously

* The function must behave correctly even when calls are interleaved across different file descriptors

* Each file descriptor must maintain its own internal reading state

#### Constraints

* Only one static variable is allowed

* Memory management must be safe, with no memory leaks

* The behavior must remain identical to the mandatory part when using a single fd

#### Bonus File Structure

The bonus implementation must be placed in the following files:

`get_next_line_bonus.c` - Implementation of get_next_line with support for multiple file descriptors

`get_next_line_bonus.h` - Bonus header file

`get_next_line_utils_bonus.c` - Utility functions used by the bonus version

#### Bonus Compilation

Example compilation command for the bonus part:
```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line_bonus.c get_next_line_utils_bonus.c
```
As in the mandatory part, the `BUFFER_SIZE` value can be changed to test different buffering behaviors.

### Resources

42 School — Get Next Line v13 subject

POSIX documentation:

* read(2)

Manual pages:

* man read

* man open

* C standard library documentation
