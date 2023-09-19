+++
title = "Primeiros passos com o Renesas Flash Programmer"
date = "2023-09-05"
categories = ["Renesas","RA","Embarcados"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
draft = false
+++

## Introdução

O _[Renesas Flash Programmer](https://www.renesas.com/us/en/software-tool/renesas-flash-programmer-programming-gui)_ é uma ferramenta de programação de firmware que permite a gravação de dados na memória flash dos microcontroladores da Renesas.

## Download e instalação
Acesse a [página do software na seção _Downloads_](https://www.renesas.com/us/en/software-tool/renesas-flash-programmer-programming-gui#download), faça o login na sua conta Renesas (crie uma, se necessário) e faça o download e instalação da ferramenta.


## Gravando um firmware de exemplo
Para validar a instalação, vamos gravar um firmware de exemplo em uma placa com um microcontrolador da família _RA_ (ARM Cortex M33). Abra o _Renesas Flash Programmer_, crie um novo projeto com as configurações abaixo e conecte:

![RFP Configuration](../../../../../assets/img/20230905_rfp_config.png)

Veja que a placa conectou com sucesso:

![RFP Connected](../../../../../assets/img/20230905_rfp_connected.png)

Escolha a imagem de firmware que será gravada:

![RFP Choosing .srec](../../../../../assets/img/20230905_rfp_choosing_srec.png)

Clique em _Start_:

![RFP Start](../../../../../assets/img/20230905_rfp_start.png)

Verifique que o firmware foi gravado com sucesso:

![RFP Firmware Download success](../../../../../assets/img/20230905_rfp_firmware_download_success.png)
