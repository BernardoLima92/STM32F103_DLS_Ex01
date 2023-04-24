# STM32F103_DLS_Ex01
Software developed for performing DLS experiments

O software desenvolvido tem o seguinte objetivo:
- Realizar a aquisição de Ns valores analógicos, com uma frequência de amostragem fs, durante um intervalo de tempo T.

Este programa pode ser usado para diversas aplicações onde seja necessária uma aquisição analógica com alta velocidade. No nosso caso, usaremos para 
experimentos de DLS (Espalhamento Dinâmico da Luz).

O programa mostrado neste tutorial realiza a leitura de 1500 valores analógicos através do ADC do STM32F103 (Bluepill) sempre que um comando é dado.
No nosso experimento, o comando é dado através de um push-button conecntado em modo pull-up à porta PB9 do microcontrolador.
Sempre que uma leitura analógica é realizada, o DMA do microcontrolador transfere o valor lido para um buffer. 
Após a realização das 1500 leituras (e consequentemente, após o preenchimento do buffer) os dados são transferidos via porta serial para um PC.
O softwate Termite foi usado como Terminal Serial para a leitura dos dados.

O fluxograma abaixo ilustra as etapas do programa:
![Fluxograma](https://user-images.githubusercontent.com/114233216/231769513-7364bbb1-f5a8-4c5a-abe3-303d25f1a685.png)


Os prints abaixo mostram as configurações realizadas no software STM32CubeIDE.

1. Clock Tree ![Fig1 - Clock Tree](https://user-images.githubusercontent.com/114233216/231754788-f5d3c02e-6e2a-4317-a7b3-0cf42e7f2d65.png)

2. Pinout![Fig2 - Pinout](https://user-images.githubusercontent.com/114233216/231754822-89ebfc80-efb6-452e-837e-8888e4cc1ebd.png)

3. RCC![Fig3 - RCC](https://user-images.githubusercontent.com/114233216/231755315-e333a7a4-4fc3-4cf6-be1b-99dd861624e1.png)

4. GPIO![Fig4 - GPIO](https://user-images.githubusercontent.com/114233216/231755340-c4524ae0-bcb7-4591-b56b-a3f53990b912.png)

5. DMA1![Fig5 - DMA](https://user-images.githubusercontent.com/114233216/231755364-59a9ecec-695f-4425-aeb0-b164e9284a22.png)

6. ADC1![Fig6 - ADC](https://user-images.githubusercontent.com/114233216/231755381-31798905-ed84-4edb-b664-4421a44e0c52.png)

7. TIMER1![Fig7 - Timer1](https://user-images.githubusercontent.com/114233216/231755405-44e29a09-1d77-4139-afde-b8b959da5273.png)

8. USART1![Fig8 - USART1](https://user-images.githubusercontent.com/114233216/231755415-42268849-18a0-4004-91de-ad1b927dc8e8.png)


O código escrito está no arquivo **main.c** presente na pasta principal deste repositório. Além deste código em C, há também um arquivo .HEX e um arquivo .IOC.
O arquivo HEX pode ser usado para gravar diretamente no STM32, sem a necessidade de se usar o STM32CubeIDE.
O Arquivo IOC é o arquivo gerado pelo STM32CubeIDE, e possui as configurações visuais do software gerado.

Para gravar no microcontrolador o arquivo HEX gerado pelo STM32CubeIDE usei o software STM32 ST-Link Utility, mostrado no print abaixo:
![STLink](https://user-images.githubusercontent.com/114233216/231759696-82f94c8f-6c49-44d0-a65a-3dd6f5c0e469.png)



Para a transferência Serial entre o STM32F103 (BluPill) e o PC, usei um módulo conversor CH340, mostrado na imagem abaixo.

![CH340](https://user-images.githubusercontent.com/114233216/231759344-0dbe064b-0a9d-40d6-9a7c-bffa4d09ceb2.png)

O resultado é mostrado no software Free Termite Serial. (Pode ser usado em conjunto com qualquer software, inclusive o LabView).

![Termite](https://user-images.githubusercontent.com/114233216/231761066-cb1a0929-d0c5-4945-83f6-b6d4e1087287.png)


Na imagem acima, foi realizada a leitura de 8500 valores. A porta analógica estava conectada ao pino de 3.3V do bluepill. 
A última linha, que mostra o número de pulsos de clock, serve para contagem do tempo necessário para a realização das 8500 leituras analógicas.
O Cálculo realizado para o cálculo do tempo é:

Tempo total da operação = (Prescaler * Pulsos de Clock Total) / PCLK2

Os valores usados neste código foram: Prescaler = 100, Pulsos = 4728, PCKL2 = 56MHz.
Assim, temos que o Tempo total da operação = (100 x 4728)/56x10^6 = 8.44mS.
Ou seja, a aquisição dos 8500 valores analógicos durou 8.44mS.

Quanto à frequência de amostragem o cálculo é outro.
A Imagem abaixo, retirada do datasheet, mostra o total de clocks usados para uma conversão ADC.

![ADCClock](https://user-images.githubusercontent.com/114233216/231763456-2fd9d519-3a5d-4bdc-9483-513a3387724b.png)

Assim, temos um tempo total de conversão de 1uS.

Se fizermos uma regra de três simples com o resultado das 8500 leituras, chegaremos a um tempo de conversão parecido:
Se 8500 leituras durou 8.44mS. Quanto tempo dura uma única leitura? 8.44ms/8500 = 0,99uS.

Em testes realizados com o Bluepill, ao se adquirir 8500 valores analógicos, foi preenchido 93% da memória RAM disponível.

Ou seja, se for necessário um tempo maior de aquisição, por exemplo 20mS, é necessário diminuir a frequência de amostragem, caso contrário, a memória será totamente
preenchida antes de se completar os 20mS de operação. 


## Alterar Número Total de Aquisições
Para alterar o tempo total de aquisição é necessário alterar o define NS, presente na linha 20 do arquivo *main.c*
```
/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include <stdbool.h>
#define Ns 8500 // Número de Leituras Realizadas
/* USER CODE END Includes */
```

## Alterar a Frequência de Amostragem
Para alterar a frequência de amostragem há dois  caminhos:
- Alterar a frequência do ADCClock. Neste trabalho usamos 14MHz. Podemos configurar valores menores.

- Alterar a quantidade de ciclos usados no Sampling Time, mostrado no print abaixo:
![Fig9 - Sampling Time](https://user-images.githubusercontent.com/114233216/231772740-2b1bb5f3-3d32-4915-ba69-d2e7cc49a7c9.png)




