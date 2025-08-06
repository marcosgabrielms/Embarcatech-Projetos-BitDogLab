# Projetos com BitDogLab - Residência Embarcatech

Este repositório serve como um portfólio dos projetos que desenvolvi durante a Residência Tecnológica do programa Embarcatech. Todos os projetos utilizam a placa BitDogLab com o microcontrolador Raspberry Pi Pico W (RP2040) e incluem códigos-fonte, esquemas e documentação.

---

## Navegação Rápida

- [Projeto 1: Tranca Eletrônica](#projeto-1)
- [Projeto 2: Microfone RGB](#projeto-2)
- [Projeto 3: Controle de Servos com Acelerômetro](#projeto-3)
- [Projeto 4: Análise de Som com Microfone](#projeto-4)
- [Projeto 5: Controle via Bluetooth](#projeto-5)
- [Projeto 6: Botões IoT Conectados à Nuvem](#projeto-6)
- [Projeto 7: Leitura de Joystick Analógico](#projeto-7)

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

---

<a id="projeto-3"></a>
## Projeto 3: Controle de Servos com Acelerômetro

- **Repositório:** [marcosgabrielms/bitdog-mpu-servo](https://github.com/marcosgabrielms/bitdog-mpu-servo.git)

### Resumo

Este projeto demonstra o controle de dois servo motores em tempo real com base nos dados de inclinação de um sensor de movimento MPU-6050 (acelerômetro e giroscópio). A placa BitDogLab atua como um controlador que traduz o movimento físico em movimento mecânico.

### Funcionalidades Atuais

- **Leitura do Sensor:** O sistema se comunica com o MPU-6050 via protocolo I2C para ler os dados brutos do acelerômetro nos eixos X e Y.
- **Controle de Servos:** Dois servo motores, conectados aos pinos GP15 e GP14, são controlados independentemente.
- **Mapeamento de Movimento:** A inclinação da placa no eixo X controla um servo, e a inclinação no eixo Y controla o outro.
- **Depuração Serial:** Os valores lidos do acelerômetro e os ângulos calculados para os servos são enviados ao monitor serial.

### Detalhes Técnicos

O ponto central do projeto é a conversão dos dados brutos do acelerômetro em ângulos de servo estáveis. O código utiliza a função `atan2` para calcular os ângulos de inclinação (roll e pitch) a partir dos vetores de aceleração. Para alimentar os servos, o sistema configura canais **PWM com uma frequência de 50 Hz**, padrão para a maioria dos servos analógicos. Uma função `map` converte a faixa de ângulos do sensor (-90° a 90°) para a faixa de operação do servo (geralmente 0° a 180°), que por sua vez é traduzida em um valor de *duty cycle* para o sinal PWM. Isso demonstra uma cadeia completa de processamento: sensor (I2C) -> processamento (matemática) -> atuador (PWM).

---

<a id="projeto-4"></a>
## Projeto 4: Análise de Som com Microfone

- **Repositório:** [marcosgabrielms/projeto_microfone.git](https://github.com/marcosgabrielms/projeto_microfone.git)

### Resumo

Este projeto foca nos fundamentos da captura e processamento de sinais de áudio. Ele utiliza um microfone para ler o som ambiente e calcula sua amplitude em tempo real, enviando os resultados para um computador via comunicação serial.

### Funcionalidades Atuais

- **Captura de Áudio:** Utiliza o ADC no pino GP26 para amostrar continuamente o sinal de um microfone.
- **Cálculo de Amplitude:** Processa um buffer de amostras para encontrar os valores de pico (mínimo e máximo) em uma janela de tempo.
- **Saída Serial:** Envia os valores de pico (mínimo, máximo e a amplitude `pico a pico`) para o monitor serial, permitindo a análise dos dados em um computador.

### Detalhes Técnicos

Este projeto serve como uma base essencial para aplicações de áudio mais complexas, como o "Microfone RGB". O destaque técnico é a abordagem de **medição de amplitude pico a pico (`max_val - min_val`)** a partir de um *buffer* de 512 amostras (`NSAMPLES`). Em vez de reagir a cada leitura instantânea do ADC, que seria muito volátil, o código analisa um bloco de amostras para determinar a "energia" do som em uma pequena janela de tempo. Essa técnica é fundamental para obter uma medição de volume estável e representativa, filtrando ruídos e flutuações momentâneas.

---

<a id="projeto-5"></a>
## Projeto 5: Controle via Bluetooth

- **Repositório:** [marcosgabrielms/embarcatech_bluetooth.git](https://github.com/marcosgabrielms/embarcatech_bluetooth.git)

### Resumo

Este projeto adiciona conectividade sem fio à placa BitDogLab utilizando um módulo Bluetooth (HC-05/HC-06). A placa pode receber comandos de um dispositivo pareado, como um smartphone ou computador, para controlar um LED.

### Funcionalidades Atuais

- **Comunicação Serial:** Configura uma interface UART para se comunicar com o módulo Bluetooth.
- **Recepção de Comandos:** O sistema aguarda e lê dados (caracteres) enviados via Bluetooth.
- **Controle de Atuador:** Controla o estado do LED integrado à placa (on/off) com base nos comandos recebidos.
- **Protocolo Simples:** Responde ao caractere 'L' para ligar o LED e a qualquer outro caractere para desligá-lo.

### Detalhes Técnicos

O aspecto técnico principal deste projeto é a implementação da **comunicação serial via UART** para interfacear com um módulo externo. O código demonstra a configuração dos pinos GPIO para as funções de UART (TX e RX), a inicialização do barramento com um *baud rate* específico (9600) e a lógica de leitura de dados do *buffer* de recepção. Este projeto é um exemplo clássico de como estender as capacidades de um microcontrolador adicionando módulos de comunicação sem fio, transformando um dispositivo local em um dispositivo controlável à distância.

---

<a id="projeto-6"></a>
## Projeto 6: Botões IoT Conectados à Nuvem

- **Repositório:** [marcosgabrielms/botoes_iot_nuvem.git](https://github.com/marcosgabrielms/botoes_iot_nuvem.git)

### Resumo

Este é um projeto de Internet das Coisas (IoT) que conecta a placa BitDogLab à nuvem. Ações físicas, como pressionar botões na placa, são convertidas em requisições web que podem acionar serviços e automações online, neste caso, através da plataforma IFTTT.

### Funcionalidades Atuais

- **Conectividade Wi-Fi:** Conecta o Raspberry Pi Pico W a uma rede Wi-Fi local utilizando as credenciais fornecidas.
- **Monitoramento de Botões:** Lê o estado de três botões conectados aos pinos GP18, GP19 e GP20.
- **Requisições HTTP:** Ao pressionar um botão, a placa monta e envia uma requisição HTTP GET para um *webhook* do IFTTT.
- **Tratamento de Bounce:** Implementa uma lógica de *debounce* com um `sleep` para evitar que um único pressionar de botão gere múltiplas requisições.

### Detalhes Técnicos

O destaque deste projeto é a implementação de um **cliente HTTP** em um microcontrolador para interagir com uma API web. O código utiliza a biblioteca `pico_cyw43_arch_lwip` para gerenciar a conexão Wi-Fi e a pilha de rede TCP/IP. A parte mais complexa é a formulação manual da requisição HTTP GET, que é escrita caractere por caractere em um *buffer* e enviada via soquetes TCP. Este processo de baixo nível demonstra o funcionamento fundamental da comunicação web e como um dispositivo com recursos limitados pode interagir com a vasta infraestrutura da internet para se tornar um verdadeiro dispositivo IoT.

---

<a id="projeto-7"></a>
## Projeto 7: Leitura de Joystick Analógico

- **Repositório:** [marcosgabrielms/atividade_joystick.git](https://github.com/marcosgabrielms/atividade_joystick.git)

### Resumo

Este projeto implementa a leitura e o processamento de dados de um joystick analógico de dois eixos (X e Y) e de um botão de pressão integrado. Os dados processados são enviados para um computador, permitindo que o joystick seja usado como um dispositivo de entrada.

### Funcionalidades Atuais

- **Leitura Analógica:** Utiliza dois canais do ADC para ler continuamente os valores de tensão dos eixos X (GP26) e Y (GP27) do joystick.
- **Leitura Digital:** Monitora o estado do botão de pressão do joystick (conectado ao GP28) como uma entrada digital.
- **Processamento de Dados:** Converte os valores brutos do ADC (0-4095) para uma faixa lógica mais intuitiva de -100 a 100.
- **Saída Serial:** Imprime os valores processados dos eixos X, Y e o estado do botão no monitor serial.

### Detalhes Técnicos

Um aspecto técnico notável deste projeto é a implementação de uma **"zona morta" (deadzone)** no software. Joysticks analógicos raramente retornam ao valor central exato quando em repouso, causando um "desvio" (drift). O código resolve isso ignorando pequenas variações em torno do ponto central (definido como 2047), tratando-as como zero. Isso evita movimentos indesejados e torna o controle mais estável e preciso. Além disso, a função de `map` utilizada para converter a faixa do ADC para a faixa de -100 a 100 é um exemplo prático e eficiente de normalização de dados, uma tarefa comum e essencial em sistemas embarcados que lidam com sensores analógicos.
