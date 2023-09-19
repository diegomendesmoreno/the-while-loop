+++
title = "Bibliotecas em C, Object files e Shared Objects"
date = "2023-09-18"
categories = ["Programação","C"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
draft = true
+++

## Introdução

Este documento mostra as diferentes maneiras de usar bibliotecas em C.
- Compilando os arquivos da biblioteca junto com o código de aplicação
- Usando um arquivo de objeto
- Usando um arquivo de objeto compartilhado (`.so` ou `.dll`)


## Executando o exemplo de código

Você pode executar o código e ver os resultados [neste repositório Replit](https://replit.com/@diegommoreno/C-Libraries-Object-files-Shared-Objects). Após abrir a página, ao clicar no botão "Run", o terminal será aberto e você poderá executar os comandos bash listados aqui.

Ao longo dos exercícios, após executar o executável (`./bin/main.out`), você deve ver a seguinte mensagem no console:
```bash
Wow, it works!
```

E a qualquer momento, você pode excluir os arquivos executaveis/binários/objetos com o comando:
```bash
make clean
```


## Como usar suas próprias bibliotecas em C
Este exemplo mostra como usar uma biblioteca em seu código C de 3 maneiras diferentes. Como exemplo, usaremos uma biblioteca simples que apenas imprime uma string no console.

### Compilando os arquivos da biblioteca
Isso é feito compilando os arquivos de código de biblioteca junto com o código da aplicação. E, finalmente, executando um executável que contém ambos o código da aplicação e a biblioteca.
- Se quiser usar a toolchain diretamente pela linha de comando, execute:
```bash
gcc src/main.c src/hello.c -o bin/main.out
./bin/main.out
```
- Se quiser usar o Makefile com `make`, execute:
```bash
make
./bin/main.out
```

### Usando um arquivo de objeto
Isso é feito pré-compilando a biblioteca em um arquivo de objeto (.o) e compilando-a junto com o código da aplicação. O resultado também é um executável final autocontido com o arquivo de objeto vinculado na compilação. Apesar desse também ser um executável que contém tanto o código da aplicação quanto a biblioteca, esta forma pode resultar em um arquivo executável maior. Isso pode acontecer se o arquivo objeto tiver funções que não são utilizadas no programa e estão incluídas no executável mesmo assim, porque a otimização do compilador não atua no objeto pré-compilado.
- Se quiser usar a toolchain diretamente pela linha de comando, execute:
```bash
gcc -Wall -g -c src/hello.c -o lib/hello.o
gcc src/main.c lib/hello.o -o bin/main.out
./bin/main.out
```
- Se quiser usar o Makefile com `make`, execute:
```bash
make object
./bin/main.out
```

### Usando um arquivo de objeto compartilhado
Isso é feito pré-compilando a biblioteca em um arquivo de objeto compartilhado (.so) ou uma biblioteca linkado dinamicamente (.dll no Windows). Em seguida, podemos compilar com o código da aplicação, mas o objeto compartilhado será carregado em tempo de execução e não será empacotado junto no executável. Saiba de que isso também pode resultar em um executável maior, pois há algum overhead associado à linkagem dinâmica. Isto inclui a necessidade de resolver símbolos em tempo de execução, o que requer informações adicionais no executável.

Primeiro, criamos o arquivo de objeto compartilhado:
```bash
gcc -Wall -g -fPIC -shared -o lib/libhello.so src/hello.c -lc
```
- Se quiser usar o Makefile com `make`, execute:
```bash
make shared
```

Agora, precisamos especificar onde o local desse arquivo de objeto compartilhado para ser carregado em tempo de execução. Podemos fazer isso de duas maneiras:

- Usando a opção de linker `-rpath`: é usada para especificar uma pasta/diretório onde o sistema operacional vai procurar as bibliotecas compartilhadas necessárias para serem carregadas e linkadas em tempo de execução. Execute:
```bash
gcc -Wall -g -o bin/main.out src/main.c -L./lib -lhello -Wl,-rpath,./lib
./bin/main.out
```
- Se quiser usar o Makefile com `make`, execute:
```bash
make shared_rpath
./bin/main.out
```

- Usando a variável de ambiente `LD_LIBRARY_PATH`: é uma variável de ambiente que informa ao sistema operacional onde procurar bibliotecas compartilhadas ao executar um executável. Portanto, podemos adicionar a localicação do objecto compartilhado (`.so`) ao LD_LIBRARY_PATH e simplificar o prompt de comando. Execute:
```bash
gcc -Wall -g -o bin/main.out src/main.c -L./lib -lhello
LD_LIBRARY_PATH=./lib:$LD_LIBRARY_PATH
./bin/main.out
```
- Se quiser usar o Makefile com `make`, execute:
```bash
make shared_ldpath
LD_LIBRARY_PATH=./lib:$LD_LIBRARY_PATH
./bin/main.out
```

## Conclusão
Essas são diferentes estratégias para usar bibliotecas em C. Elas variam em velocidade, consumo de memória, tamanho e complexidade. Cabe a você analisar qual caminho se adapta aos requisitos que você possui.


## Links
- [Shared libraries with GCC on Linux](https://www.cprogramming.com/tutorial/shared-libraries-linux-gcc.html)
- [How to write your own code libraries in C - Jacob Sorber YouTube video](https://youtu.be/JbHmin2Wtmc)
