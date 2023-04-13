# STM32F103_DLS_Ex01
Foftware developed for performing DLS experiments

O software desenvolvido tem o seguinte objetivo:
- Realizar a aquisição de Ns valores analógicos, com uma frequência de amostragem fs, durante um intervalo de tempo T.

Este programa pode ser usado para diversas aplicações onde se deseje uma aquisição analógica com alta velocidade. No nosso caso, usaremos para 
experiments de DLS (Espalhamento Dinâmico da Luz).

O programa mostrado neste tutorial realiza a leitura de 1500 valores analógicos através do ADC do STM32F103 (Bluepill) sempre que um comando é dado.
No nosso experimento, o comando é dado através de um push-button conecntado em modo pull-up à porta PB9 do microcontrolador.
Sempre que uma leitura analógica é realizada, o DMA do microcontralor transfere o valor lido para um buffer. 
Após a realização das 1500 leituras (e consequentemente, após o preenchimento do buffer) os dados são transferidos via porta serial para um PC.
O softwate Termite foi usado como Terminal Serial para a leitura dos dados.

O fluxograma abaixo ilustra as etapas do programa:
![Fluxograma](https://user-images.githubusercontent.com/114233216/231753766-15def680-fcae-447d-a019-66e462f130b8.png)

Os prints abaixo mostram as configurações realizadas no software STM32CubeIDE.
Estão nesta ordem:
1. Clock Tree ![Fig1 - Clock Tree](https://user-images.githubusercontent.com/114233216/231754788-f5d3c02e-6e2a-4317-a7b3-0cf42e7f2d65.png)

2. Pinout![Fig2 - Pinout](https://user-images.githubusercontent.com/114233216/231754822-89ebfc80-efb6-452e-837e-8888e4cc1ebd.png)

3. RCC![Fig3 - RCC](https://user-images.githubusercontent.com/114233216/231755315-e333a7a4-4fc3-4cf6-be1b-99dd861624e1.png)

4. GPIO![Fig4 - GPIO](https://user-images.githubusercontent.com/114233216/231755340-c4524ae0-bcb7-4591-b56b-a3f53990b912.png)

5. DMA1![Fig5 - DMA](https://user-images.githubusercontent.com/114233216/231755364-59a9ecec-695f-4425-aeb0-b164e9284a22.png)

6. ADC1![Fig6 - ADC](https://user-images.githubusercontent.com/114233216/231755381-31798905-ed84-4edb-b664-4421a44e0c52.png)

7. TIMER1![Fig7 - Timer1](https://user-images.githubusercontent.com/114233216/231755405-44e29a09-1d77-4139-afde-b8b959da5273.png)

8. USART1![Fig8 - USART1](https://user-images.githubusercontent.com/114233216/231755415-42268849-18a0-4004-91de-ad1b927dc8e8.png)


O código escrito está no arquivo main.c presente na pasta principal deste repositório. Além deste código em C, há também um arquivo .HEX e um arquivo .IOC.
O arquivo HEX pode ser usado para gravar diretamente no STM32, sem a necessidade de se usar o STM32CubeIDE.
O Arquivi IOC é o arquivo gerado pelo STM32CubeIDE, e possui as configurações visuais do software gerado.

Para gravar no microcontrolador o arquivo HEX gerado pelo STM32CubeIDE usei o software STM32 ST-Link Utility.


O intervalo de tempo total no qual o sinal é adquirido depende diretamente da frequência de amostragem escolhida e da memória do microcontrolador.
Em testes realizados com o Bluepill, ao se adquirir 8500 valores analógicos, foi preenchido 93% da memória RAM disponível.
