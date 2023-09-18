+++
title = "Como ler a memória do microcontrolador com o Renesas Flash Programmer"
date = "2023-09-10"
categories = ["renesas","ra","embarcados"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
+++

## Introdução

Esse artigo pressupõe que você tenha feito o passo a passo descrito no artigo _[Primeiros passos com o Renesas Flash Programmer](../primeiros-passos-com-o-renesas-flash-programmer/)_ que grava um firmware de exemplo que pisca os LEDs da placa [EK-RA4M2](https://www.renesas.com/us/en/products/microcontrollers-microprocessors/ra-cortex-m-mcus/ek-ra4m2-evaluation-kit-ra4m2-mcu-group).


## Fazendo a leitura de memória do microcontrolador
Com o Renesas Flash Programmer aberto, e conectado a um microcontrolador já programado (sem proteção de memória contra leitura), vá em `Target Device` >> `Read Memory...` e dê um nome (`read.mot`) para o arquivo binário que será lido:

![RFP Read Memory](../../../../../assets/img/20230910_rfp_read_memory.png)

Como o programa de exemplo é pequeno, vamos ler apenas os primeiros 8KB da _Code Flash_:

![RFP Read Memory Config](../../../../../assets/img/20230910_rfp_read_memory_config.png)

E podemos ver que a leitura foi feita com sucesso:

![RFP Read Memory Success](../../../../../assets/img/20230910_rfp_read_memory_success.png)
