# Sat-OBDH-Sim: Simulação de Sistema de Manipulação de Dados de Bordo (OBDH)

![C++](https://img.shields.io/badge/C++-17-blue.svg)
![CMake](https://img.shields.io/badge/CMake-Build-success.svg)
![Linux Embedded](https://img.shields.io/badge/Platform-Embedded_Linux-lightgrey.svg)
![Status](https://img.shields.io/badge/Status-MVP_in_Progress-orange.svg)

## 🛰️ Visão Geral do Projeto

Este projeto é uma simulação de software embarcado para o **OBDH (On-Board Data Handling)**, atuando como o computador principal de um satélite com carga útil SAR (Synthetic Aperture Radar). 

O objetivo principal deste repositório é demonstrar boas práticas de arquitetura de software, proficiência em **C++ moderno**, e o uso de padrões de projeto voltados para sistemas de missão crítica em um ambiente **Linux Embarcado**.

Atualmente, o projeto foca no MVP (Mínimo Produto Viável): a comunicação e o agendamento de tarefas entre o módulo OBDH e um subsistema simulado de Payload via barramento **CAN (Controller Area Network)**.

## 📐 Arquitetura e Padrões de Projeto (C++)

O design do sistema foi estruturado para garantir alta coesão, baixo acoplamento e testabilidade (TDD). Os seguintes padrões e conceitos são aplicados:

* **State Machine (State Pattern):** Gerencia os modos de operação do satélite (ex: `Boot`, `Nominal`, `SafeMode`, `Imaging`). O OBDH reage a falhas de telemetria alterando seu estado de forma segura.
* **Hardware Abstraction Layer (HAL):** Interfaces puramente virtuais separam a lógica de negócios do OBDH da implementação de hardware. O acesso ao barramento CAN via `SocketCAN` no Linux é isolado por esta camada, permitindo a injeção de dependências (Mocking) nos testes unitários.
* **Observer Pattern (Pub-Sub):** Utilizado para o roteamento de telemetria. O OBDH "assina" as mensagens de batimento cardíaco (heartbeat) dos subsistemas.
* **RAII e Gerenciamento de Memória:** Uso estrito de *smart pointers* (`std::unique_ptr`, `std::shared_ptr`) para evitar vazamentos de memória (memory leaks), essencial em sistemas de longa duração em órbita.
* **Thread-Safety:** O agendador de tarefas (Task Scheduler) utiliza concorrência moderna do C++ (`std::thread`, `std::mutex`, `std::condition_variable`) para processar comandos assíncronos sem condições de corrida.

## 🛠️ Tecnologias e Ambiente de Build

* **Linguagem:** C++17 (foco em features modernas e seguras).
* **Build System:** CMake.
* **Testes Unitários:** Google Test (GTest) e Google Mock (GMock).
* **Comunicação:** SocketCAN (utilizando `vcan` do Linux para simulação de barramento embarcado).
* **Alvo:** Linux Embarcado (Ubuntu Server VM / Yocto/Buildroot compatível).

## 🚀 Como Compilar e Executar

### Pré-requisitos (Linux)
Certifique-se de ter o compilador C++, CMake e as ferramentas do CAN instaladas:
```bash
sudo apt-get update
sudo apt-get install build-essential cmake can-utils
