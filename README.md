# Projetos com BitDogLab - Residência Embarcatech

Este repositório serve como um portfólio dos projetos que desenvolvi durante a Residência Tecnológica do programa Embarcatech. Todos os projetos utilizam a placa BitDogLab com o microcontrolador Raspberry Pi Pico W (RP2040) e incluem códigos-fonte, esquemas e documentação.

---

## Navegação Rápida

- [Projeto 1: Tranca Eletrônica](#projeto-1)
- [Projeto 2: Microfone RGB](#projeto-2)

---

<a id="projeto-1"></a>
## Projeto 1: Tranca Eletrônica

- **Repositório:** [marcosgabrielms/Tranca-Eletronica-BitDogLab](https://github.com/marcosgabrielms/Tranca-Eletronica-BitDogLab.git)

### Resumo

Este projeto implementa uma "Tranca Eletrônica" para a placa BitDogLab. O código-fonte foi desenvolvido de forma modular, com drivers de baixo nível escritos para controlar cada componente de hardware de forma independente.

### Funcionalidades Atuais

O projeto, no estado atual, funciona como um programa de teste de hardware que executa as seguintes ações:
- Inicializa e lê continuamente o estado de dois botões (A e B).
- Controla um LED RGB, alterando sua cor em resposta aos botões pressionados.
- Envia mensagens de status para um computador via USB para fins de depuração.

### Detalhes Técnicos

Um aspecto interessante do desenvolvimento é a abordagem de controle do LED RGB. O código atual utiliza **PWM (Modulação por Largura de Pulso)** para gerenciar um LED RGB padrão (4 pinos) conectado aos GPIOs 11, 12 e 13. Contudo, o sistema de build ainda contém arquivos de configuração (`ws2812.pio`) e regras no `CMakeLists.txt` preparadas para controlar uma matriz de LEDs endereçáveis (NeoPixel), que é uma tecnologia diferente e não utilizada na versão atual. Isso indica uma evolução ou mudança de abordagem durante o desenvolvimento do projeto.

---

<a id="projeto-2"></a>
## Projeto 2: Microfone RGB

- **Repositório:** [marcosgabrielms/microfone_rbg](https://github.com/marcosgabrielms/microfone_rbg.git)

### Resumo

Este projeto transforma a placa BitDogLab em um medidor de intensidade sonora visual. Utilizando um microfone para capturar o som ambiente, o sistema processa o sinal e controla uma matriz de LEDs para exibir o nível de ruído em tempo real, funcionando como um "ledbar" (barra de LEDs) que reage ao som.

### Funcionalidades Atuais

- **Captura de Áudio:** O sistema utiliza o conversor analógico-digital (ADC) para ler continuamente os dados de um microfone conectado ao pino GP28.
- **Calibração Automática:** Antes de iniciar a medição, o projeto executa uma rotina de calibração para definir os limites de som baixo e alto, adaptando-se ao ruído do ambiente.
- **Visualização em Matriz de LEDs:** Uma matriz de LEDs (conectada ao pino GP7) exibe a intensidade do som. A barra de LEDs muda de cor conforme o volume aumenta, passando de verde para amarelo e depois para vermelho.
- **Depuração Serial:** O nível de amplitude sonora é enviado para o monitor serial, permitindo a depuração e o acompanhamento dos dados em tempo real.

### Detalhes Técnicos

Um aspecto notável do projeto é a forma como o nível sonoro é medido e estabilizado. Em vez de usar leituras brutas do ADC, o código calcula a amplitude pico a pico (`max_val - min_val`) de um conjunto de amostras para determinar o volume. Além disso, aplica um filtro de suavização exponencial (`ALPHA = 0.3f`) para estabilizar as leituras e evitar oscilações bruscas na matriz de LEDs, resultando em uma visualização mais fluida e representativa do som ambiente. O projeto também é modular, separando a lógica do microfone (`mic.c`), da matriz (`matriz.c`) e de utilitários (`utils.c`), o que facilita a manutenção e expansão do código.
