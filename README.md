# Sprint 3 - Computer Organization and Architecture
## Controle Inteligente de Sessão de Recarga

**Integrantes:**
- Enzo Stahal Freitas | 569001
- Brenno Gomes | RM: 570525
- Eduardo Moreira | RM: 569923
- Matheus Bruno | RM: 572944

---

## Sobre o Projeto

O objetivo do projeto é simular o controle de uma sessão de recarga de veículo elétrico com base na energia disponível em uma residência. O protótipo, inspirado no conceito do GoodWe Smart Energy Controller, compara a geração de energia com o consumo da casa e decide se a recarga deve ser autorizada, reduzida ou bloqueada, sinalizando o resultado com LEDs e no terminal.

O projeto foi desenvolvido em MicroPython para o Raspberry Pi Pico e simulado no Wokwi. Os dados de geração e consumo são simulados, e o foco é demonstrar, na prática, a relação entre entrada de dados, processamento, memória e saída, conteúdos da disciplina de Arquitetura de Computadores.

## Objetivo

Desenvolver um sistema capaz de:

- Receber dados de geração de energia e consumo da residência.
- Calcular a energia disponível para a recarga.
- Identificar em qual das três situações a sessão de recarga se encontra.
- Sinalizar o estado da recarga por meio de LEDs (verde, amarelo e vermelho).
- Apresentar os dados e o status da sessão no terminal.
- Demonstrar a representação de dados em decimal, binário e hexadecimal.

## Funcionalidades

### ENTRADA DE DADOS

O sistema utiliza, para cada situação simulada:

- Geração de energia (W)
- Consumo da residência (W)

### PROCESSAMENTO

A energia disponível é calculada pela fórmula:

`Energia disponível = Geração - Consumo`

### ESTADOS DA RECARGA

O sistema identifica:

- Recarga autorizada (energia disponível >= 1400 W) - LED verde
- Recarga reduzida (energia disponível entre 0 W e 1400 W) - LED amarelo
- Recarga bloqueada (energia disponível <= 0 W) - LED vermelho

O limite de 1400 W foi definido pelo grupo e corresponde, aproximadamente, à menor potência usual de recarga (6 A em 230 V). Ele pode ser alterado na constante `LIMITE_RECARGA_PLENA` do arquivo `main.py`.

### TOMADA DE DECISÃO

O sistema realiza ações automatizadas como:

- Acender o LED verde e autorizar a recarga (energia suficiente)
- Acender o LED amarelo e reduzir a recarga (energia limitada)
- Acender o LED vermelho e bloquear a recarga (energia insuficiente)

### APRESENTAÇÃO DOS DADOS

A cada situação, o terminal exibe geração, consumo, energia disponível e status da recarga, além da representação da potência disponível em decimal, binário e hexadecimal (16 bits).

## Relação com Energias Renováveis

O projeto parte da ideia de que a recarga deve aproveitar a energia solar produzida pela própria residência. O sistema compara a geração com o consumo e só autoriza a recarga completa quando há excedente de energia, evitando sobrecarregar a rede e priorizando uma fonte renovável.

## Relação com Arquitetura de Computadores

- **Entrada:** os valores de geração e consumo representam os dados que, em um sistema real, viriam de sensores ou medidores.
- **Processamento:** a CPU ARM Cortex-M0+ do RP2040 executa a subtração e as comparações que definem o estado da sessão. A estrutura if/elif/else garante que apenas um estado (e um LED) seja ativado por vez.
- **Memória:** o programa fica armazenado na memória flash, e as variáveis, constantes e a lista de cenários ficam na RAM durante a execução.
- **Saída:** os LEDs, controlados pelos pinos GPIO, e o terminal serial apresentam o resultado ao usuário.
- **Sistemas numéricos:** a potência disponível é mostrada em decimal, binário e hexadecimal. Valores negativos são representados em complemento de dois, por exemplo: -800 W = `1111110011100000` = `0xFCE0`.

## Tecnologias Utilizadas

- MicroPython (linguagem de programação)
- Raspberry Pi Pico (microcontrolador RP2040)
- Wokwi (simulador online)
- LEDs verde, amarelo e vermelho com resistores de 220 Ω

## Ligação dos LEDs

| LED | Pino do Pico |
|---|---|
| Verde | GP15 |
| Amarelo | GP14 |
| Vermelho | GP13 |

Cada LED é ligado em série com um resistor de 220 Ω, e o cátodo de todos os LEDs vai ao GND.

## Como Executar

1. Acesse o [Wokwi](https://wokwi.com) e crie um projeto Raspberry Pi Pico com MicroPython.
2. Substitua o conteúdo do `diagram.json` e do `main.py` do projeto pelos arquivos deste repositório.
3. Inicie a simulação e acompanhe o Monitor Serial.

Em hardware físico, grave o `main.py` no Pico (por exemplo, com o Thonny) e monte o circuito conforme a tabela de ligação acima.

## Estrutura do Repositório

- `main.py` - código do programa em MicroPython
- `diagram.json` - circuito do Wokwi
- `README.md` - documentação do projeto

## Exemplo de Uso

Situação 1
- Geração: 4000 W
- Consumo: 1500 W
- Energia disponível: 2500 W
- Resultado: RECARGA AUTORIZADA (LED verde)

Situação 2
- Geração: 1800 W
- Consumo: 1500 W
- Energia disponível: 300 W
- Resultado: RECARGA REDUZIDA (LED amarelo)

Situação 3
- Geração: 1000 W
- Consumo: 1800 W
- Energia disponível: -800 W
- Resultado: RECARGA BLOQUEADA (LED vermelho)

Saída no terminal (situação 1):

```
GERACAO: 4000 W   CONSUMO: 1500 W   DISPONIVEL: 2500 W   STATUS: RECARGA AUTORIZADA
Potencia disponivel -> Decimal:   2500   Binario: 0000100111000100   Hexadecimal: 0x09C4
```

## Vídeo de Demonstração

[link do vídeo no YouTube]
