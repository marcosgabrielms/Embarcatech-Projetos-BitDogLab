# Projetos com BitDogLab - Residência Embarcatech

Este repositório serve como um portfólio dos projetos que desenvolvi durante a Residência Tecnológica do programa Embarcatech. Todos os projetos utilizam a placa BitDogLab com o microcontrolador Raspberry Pi Pico W (RP2040) e incluem códigos-fonte, esquemas e documentação.

---

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
