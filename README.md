# IPiils — Organizador Semanal Inteligente de Gavetas IoT

O **CitizenTech** nasceu para ajudar pessoas a manterem rotinas que exigem atenção no dia a dia — como tomar medicação nos dias certos ou organizar compartimentos ao longo da semana — sem depender apenas da memória. Desenvolvido para a disciplina de **Software Experience** na **FIAP**, o projeto combina um **ESP32**, um relógio em tempo real (**RTC DS1307**), avisos físicos imediatos e envio de dados por **MQTT** para tornar o uso simples, claro e confiável.

## Teste no Wokwi

Dá para testar o circuito e acompanhar o funcionamento na simulação online:
* **Link do projeto:** [https://wokwi.com/projects/480778332770808833](https://wokwi.com/projects/480778332770808833)

---

## O que o sistema faz na prática

* **Acompanha 14 gavetas:** Lê cada compartimento pelos pinos digitais do ESP32 (com *pull-up* interno) e identifica na hora o que está aberto ou fechado.
* **Sabe qual é o dia certo:** Usa o módulo **RTC DS1307** para acompanhar data, hora e dia da semana, sabendo quais gavetas correspondem ao dia atual (por exemplo, as gavetas `13` e `14` aos domingos).
* **Reage de acordo com a situação (3 estados):**
  * **Tudo calmo (`ESTADO_REPOUSO`):** Nenhuma gaveta aberta; o sistema fica em espera sem disparar avisos.
  * **Gaveta certa (`ESTADO_SUCESSO`):** Ao abrir uma gaveta do dia correspondente, o sistema reconhece a ação sem acionar alerta de erro.
  * **Ops, gaveta errada (`ESTADO_ERRO_ALERTA`):** Se alguém abrir uma gaveta que não é do dia, o **LED** acende, o **buzzer** apita e o **LCD** mostra o aviso para corrigir na hora.
* **Mostra tudo no visor:** O **LCD 16x2 I2C** informa a data, a hora e o estado do sistema de forma direta.
* **Envia dados via MQTT:** Publica um JSON com o status das gavetas, `timestamp` e estado atual em `citizentech/organizer/status`, tanto quando há mudança quanto de tempos em tempos usando `millis()` (sem travar o código com `delay`).

---

## O que vai no hardware

| Componente | O que faz aqui | Onde liga no ESP32 |
| :--- | :--- | :--- |
| **ESP32** | O "cérebro" do projeto: lê os sensores, aplica a lógica, controla os avisos e manda os dados | Placa principal |
| **Módulo RTC DS1307** | Mantém a data, a hora e o dia da semana atualizados | Barramento I2C (`SDA` / `SCL`) |
| **Display LCD 16x2 I2C** | Mostra data, hora e mensagens de status ou alerta | Endereço `0x27` (`SDA` / `SCL`) |
| **14 Chaves / Sensores de Gaveta** | Avisam quando cada gaveta é aberta ou fechada | `13, 14, 16, 17, 18, 19, 23, 27, 32, 33, 2, 4, 12, 15` (`INPUT_PULLUP`) |
| **LED** | Dá o aviso visual quando uma gaveta fora do dia é aberta | Pino `25` |
| **Buzzer** | Dá o aviso sonoro quando uma gaveta fora do dia é aberta | Pino `26` |

---

## Quem fez

* **Anna Giazzi** — RM 569846
* **Erick Diego** — RM 574064
* **Giulia Stella** — RM 568888
* **Lucas Octaviano** — RM 573368
* **Rafael Willian** — RM 570738
