# Projeto 1 - PintOS 🐣🐣

Este repositório contém a implementação do **Projeto 1** do PintOS, um sistema operacional educacional utilizado para o ensino de conceitos fundamentais de sistemas operacionais. O foco deste projeto é o gerenciamento de threads, temporização e políticas de escalonamento.

## 🔧 Funcionalidades Implementadas

### ⏰ Alarm Clock

- **Descrição:**  
  Implementa o mecanismo para que threads possam dormir até um determinado número de ticks do sistema. Utilizado para simular atrasos ou esperas temporizadas de forma cooperativa.

- **Como foi feito:**  
  Foi adicionada uma lista ordenada (`sleeping_list`) contendo todas as threads dormindo, organizada por tick de acordar. No handler de timer, a cada tick, percorre-se essa lista acordando as threads cujo tempo expirou.

- **Arquivo principal:** `devices/timer.c`

- **Desafio enfrentado:**  
  Lidar com a interrupção de tempo e garantir que a lista estivesse sempre ordenada para minimizar o custo da verificação a cada tick.

---

### ⚙️ Scheduler com MLFQ (Multi-Level Feedback Queue)

- **Descrição:**  
  Substitui o escalonador por padrão por um esquema de filas múltiplas com feedback, ajustando dinamicamente as prioridades das threads com base em seu comportamento.

- **Características:**
  - Cada thread possui uma prioridade dinâmica.
  - Threads que usam mais CPU têm suas prioridades reduzidas.
  - Threads que esperam por mais tempo ou usam menos CPU são promovidas.
  - Integração com `nice` e `recent_cpu` para modelar uso da CPU.

- **Arquivos modificados:** `threads/thread.c`, `threads/thread.h`, `threads/fixed-point.h`

- **Funções importantes implementadas:**
  - `mlfqs_priority()` — calcula a nova prioridade de uma thread.
  - `mlfqs_recent_cpu()` — atualiza o uso recente de CPU.
  - `mlfqs_load_avg()` — atualiza a média de carga do sistema.
  - Substituição da `thread_yield()` e `next_thread_to_run()` para considerar prioridade.

- **Desafios enfrentados:**
  - Correta manipulação de números em ponto fixo (necessária devido à ausência de ponto flutuante).
  - Garantir que atualizações de prioridade aconteçam nos momentos certos: a cada tick, a cada segundo, ou na criação de uma nova thread.
  - Evitar starvation de threads de baixa prioridade com uma política justa de aging.

---

## 🧪 Testes

Todos os testes fornecidos pelo PintOS foram utilizados, incluindo:

- `alarm-single`, `alarm-multiple`, `alarm-priority`, `mlfqs-load-1`, `mlfqs-fair-2` e outros.

Para rodar todos os testes:



