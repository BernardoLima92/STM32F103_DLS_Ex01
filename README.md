# STM32F103_DLS_Ex01
Foftware developed for performing DLS experiments

O software desenvolvido tem o seguinte objetivo:
- Realizar a aquisição de Ns valores analógicos, com uma frequência de amostragem fs, durante um intervalo de tempo T.

O programa mostrado neste tutorial realiza a leitura de 1500 valores analógicos através do ADC do STM32F103 (Bluepill) sempre que um comando é dado.
Neste tutorial, o comando é dado através de um push-button conecntado em modo pull-up à porta PB9 do microcontrolador.
Sempre que uma leitura analógica é realizada, o DMA do microcontralor transfere o valor lido para um buffer. 
Após a realização das 1500 leituras (e consequentemente, após o preenchimento do buffer) os dados são transferidos via porta serial para um PC.
O softwate Termite foi usado como Terminal Serial para a leitura dos dados.

O fluxograma abaixo ilustra as etapas do programa:
![Fluxograma](https://user-images.githubusercontent.com/114233216/231753766-15def680-fcae-447d-a019-66e462f130b8.png)














O intervalo de tempo total no qual o sinal é adquirido depende diretamente da frequência de amostragem escolhida e da maemória do microcontrolador.

