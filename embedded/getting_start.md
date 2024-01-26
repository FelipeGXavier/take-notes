# ESP32 Pinout

1. Power pins: são pinos destinados a fornecer alimentação elétrica ao micro controlador. São eles: 

- Associados a alimentação de energia (VCC), pino de entrada principal de energia tipicamente 3.3V
- Associados ao aterramento (GND), conectado ao terra é utilizado como referência de tensão
- Utilizados para habilitar ou desabilitar o chip (EN), utilizado para reset
- Output de nergia (3V3), pode ser utilizado para alimentar periféricos externos.

2.  GPIO: são pinos que podem ser configurados para entrada ou saídas digitais como leitura de sensores, controle de dispositivos externos e assim por diante. 

Cada modelo de placa de pode variar a quantidade de GPIOs disponíveis. Cada GPIO pode ser configurado entrada, saída ou outras funções como específicas como saídas PWM, interfaces de comunicação serial. Um GPIO tem dois níveis lógicos (ALTO) com corrente e (LOW) sem corrente. 

Alguns GPIOs tem funções especiais como suporte a entrada analógicas.


# SPI, UART, I2C e JTAG

São formas de comunicação serial projetadas para certos tipos de comunicação. 

- UART (Universal Asynchronous Receiver-Transmitter): é uma comunicação serial assíncrona que utiliza dois pinos para transmitir (TX) e receber (RX). É frequentemente utilizado para comunicação simples entre o ESP32 e outros dispositivos como sensores, módulos de GPS etc. 

- SPI (Serial Periphal Interface): é uma interface síncrona de comunicação serial que permite transferência em alta velocidade entre o ESP32 e dispositivos periféricos como sensores, displays e outros microcontroladores. 

- I2C (Inter-Integrated Circuit): é uma barramento de comunicação serial multi-dispositivo que usa apenas dois fios SDA (dados) e SCL (relógico). É utilizdo para conectar vários dispositivos usando endereços únicos. 

- JTAG (Joint Test Action Group): é uma interface de teste utilizada para debug.

# PWM 

PWM, ou Modulação por Largura de Pulso em português, é uma técnica utilizada para controlar a potência média entregue a um dispositivo elétrico, como um motor, uma lâmpada ou um LED, variando a largura dos pulsos de um sinal digital.

# ttys0 e ttyACM0

São termos associados de terminal que fazem o intermédio entre a comunicação do sistema operacional com a entrada do microcontrolador.

- ttys0:

O prefixo "tty" representa um dispositivo de terminal.

O "s" indica que se refere a um dispositivo de terminal série (serial). O número "0" geralmente denota o primeiro dispositivo de série.

/dev/ttys0

- ttyACM0

"ACM" é uma abreviação de "Abstract Control Model" e é frequentemente associado a dispositivos USB que implementam o padrão de comunicação USB CDC (Communications Device Class).

/dev/ttyACM0

*Pode ser que seja necessário alterar a permissão ou user de /dev/ttyACM0 para conseguir se conectar

# tinyGSM

O TinyGSM é uma biblioteca Arduino que fornece uma interface simples e eficiente para comunicação com módulos GSM (Global System for Mobile Communications) e GPRS (General Packet Radio Service). 

# FreeRTOS

O microcontrolador ESP32 geralmente executa o FreeRTOS (Sistema Operacional de Tempo Real). O FreeRTOS é um kernel de sistema operacional de tempo real de código aberto popular projetado para sistemas embarcados. Ele fornece um ambiente de multitarefa para aplicações embarcadas, permitindo que os desenvolvedores gerenciem tarefas, manipulem interrupções e programem atividades em tempo real.

O framework de desenvolvimento do ESP32, fornecido pela Espressif Systems (a empresa por trás do ESP32), inclui o kernel FreeRTOS como parte de sua pilha de software. Essa integração permite que os desenvolvedores aproveitem as capacidades do FreeRTOS ao programar aplicativos para o ESP32.

# AT commands

Attention Commands (AT) são um conjunto de instruções utilizados para configurar e controlar certas funcionalidades do dispositivo geralmente sob uma comunicação serial. No ESP32 alguns AT podem ser utilizados para se comunicar com módulos de wifi e bluetooth como verificar a conexão wifi. Alguns AT comuns no ESP32: 

- AT: verificar se o módulo do ESP32 está respondendo
- AT+GMR: retorna a versão de firmware
- AT+CWMODE: define o modo do wifi (station, access point etc)
- AT+CWLAP: lista as redes wifi disponíveis


# COMs

# Framework de desenvolvimento

Há dois principais "frameworks" de desenvolvimento pro ESP32, o disponibilizado pela express-idf que contém abstrações de mais baixo nível e maior flexibilidade e o arduino core que provém uma biblitoteca de mais alto nível. Basicamente são dois frameworks desenvolvidos para interagir com o microcontrolador do ESP32.

