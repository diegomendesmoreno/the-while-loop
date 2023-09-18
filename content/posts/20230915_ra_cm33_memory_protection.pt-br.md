+++
title = "Como proteger a memória flash dos MCUs RA (ARM Cortex M33) contra leitura"
date = "2023-09-15"
categories = ["renesas","ra","embarcados"]
type = ["posts","post"]
[ author ]
  name = "Diego Moreno"
+++

## Introdução

Durante a produção do produto eletrônico, é desejável que, após a gravação do firmware (binário) no microcontrolador, que haja uma proteção na memória contra leitura. Assim, o programa desenvolvido e gravado não fica acessível a concorrentes e/ou hackers mal intencionados.

Para proteger a memória dos microcontroladores da [família RA da Renesas](https://www.renesas.com/us/en/products/microcontrollers-microprocessors/ra-cortex-m-mcus), mais especificamente os que embarcam um core Arm Cortex-M33, é necessário usar o Device Lifecycle Management (DLM). Tanto a capacidade de debug quanto a interface de programação serial dependem das configurações dele [1]. É possível verificar os estados de _Device Lifecycle_ abaixo [2]:

![Device Lifecycle States](../../../../../assets/img/20230915_dlm_states.png)

As transições de um estado para o outro são descritas a seguir:

![Device Lifecycle State Transitions](../../../../../assets/img/20230915_dlm_transitions.png)

Veja que é possível fazer transições reversas, como passar do estado `DPL` (Deployed) de memória protegida contra leitura, de volta para o `SSD` (Secure Software Development) com acesso de leitura de memória, via um processo de autenticação por chaves [3] [4]. Se não tiver feito o processo de autenticação por chaves, ainda é possível recuperar o acesso de escrita e leitura (revertendo a transição), mas toda a memória do MCU é apagada. Assim, protegendo o conteúdo do programa.


## Exemplo

Veja abaixo um passo a paso de como fazer a gravação de um firmware de exemplo e fazer a proteção da memória sem injeção de chaves. Para isso vamos usar o _[Renesas Flash Programmer](https://www.renesas.com/us/en/software-tool/renesas-flash-programmer-programming-gui)_ para:
* Gravar o firmware de exemplo
* Verificar que é possível fazer a leitura do programa gravado
* Fazer a transição de `SSD` para `NSECSD` e ainda para `DPL`
* Verificar a proteção de memória
* Reveter para o estado `SSD` e verificar que a memória foi apagada

### Gravar o firmware de exemplo
Siga os passos do artigo _[Primeiros passos com o Renesas Flash Programmer](../primeiros-passos-com-o-renesas-flash-programmer/)_ para fazer a gravação do firmware de exemplo que pisca os LEDs da placa [EK-RA4M2](https://www.renesas.com/us/en/products/microcontrollers-microprocessors/ra-cortex-m-mcus/ek-ra4m2-evaluation-kit-ra4m2-mcu-group).


### Verificar que é possível fazer a leitura do programa gravado
Siga os passos do artigo _[Como ler a memória do microcontrolador com o Renesas Flash Programmer](../como-ler-a-mem%C3%B3ria-do-microcontrolador-com-o-renesas-flash-programmer/)_ para fazer a leitura do programa gravado.


### Fazer a transição de `SSD` para `NSECSD` e ainda para `DPL`
Vá em `Target Device` >> `Read Device Information`, e verifique que o microcontrolador está no estado inicial `SSD` (ou `CM`).

![RFP Read Device Information SSD](../../../../../assets/img/20230915_rfp_read_device_ssd.png)

![RFP Device SSD](../../../../../assets/img/20230915_rfp_device_ssd.png)

Vá em `Target Device` >> `DLM Transition...`, e escolha `NSECSD`.

![RFP DLM Transition](../../../../../assets/img/20230915_rfp_dlm_transition.png)

![RFP DLM Transition to NSECSD](../../../../../assets/img/20230915_rfp_dlm_transition_nsecsd.png)

Veja agora com o `Read Device Information` que o dispositivo está em `NSECSD`:

![RFP Device NSECSD](../../../../../assets/img/20230915_rfp_device_nsecsd.png)

Seguindo o mesmo processo, vá em `Target Device` >> `DLM Transition...`, e agora escolha `DPL`. E verifique também com o `Read Device Information`:

![RFP Device DPL](../../../../../assets/img/20230915_rfp_device_dpl.png)


### Verificar a proteção de memória
Apesar de ser possível fazer a leitura das informações gerais do dispositivo com o `Read Device Information`, não é mais possível fazer a leitura do firmware que foi programado. Assim como feito anteriormente, vá em `Target Device` >> `Read Memory...` e tente fazer a leitura dos primeiros 8KB (0x2000) da _Code Flash_. E, como esperado, não é possível fazer a leitura do programa gravado:

![RFP Read Memory Fail](../../../../../assets/img/20230915_rfp_read_memory_fail.png)
`Error(E100000E): A protection error occurred in the device. (Command: 15, Response: D5)
Operation failed`


### Reveter para o estado `SSD` e verificar que a memória foi apagada
Já que não configuramos o processo de autenticação por chaves, para voltarmos para o estado inicial `SSD`, é necessário ir em `Target Device` >> `Initialize Device`:

![RFP Initialize Device](../../../../../assets/img/20230915_rfp_initialize_device.png)

Após esse processo, os LEDs param de piscar, indicando que a memória do dispositivo foi apagada. E, ao fazermos um último `Read Device Information`, verificamos que o dispositivo voltou ao estado `SSD`, podendo ser programado de novo.

![RFP Device SSD](../../../../../assets/img/20230915_rfp_device_ssd.png)


## Referências

* [1]: [RA4 Quick Design Guide](https://www.renesas.com/us/en/document/apn/ra4-quick-design-guide)
* [2]: [Renesas RA Securing Data at Rest Using the Arm® TrustZone®](https://www.renesas.com/us/en/document/apn/renesas-ra-securing-data-rest-using-arm-trustzone)
* [3]: [Renesas RA Family Device Lifecycle Management Key Installation](https://www.renesas.com/us/en/document/apn/renesas-ra-family-device-lifecycle-management-key-installation)
* [4]: [How to Generate and Program DLM Keys for RA With SCE9](https://www.renesas.com/in/en/video/renesas-flash-programmer-tutorial-how-generate-and-program-dlm-keys-ra-sce9)
