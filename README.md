# Relatório da NOME DA ATIVIDADE

**Disciplina:** 
**Aluno(s):**
**Turma:**
**Professor:**
**Data:**

---

# 1. Descrição do Problema

O programa resolve o problema de Identificação de Pares de Perguntas Duplicadas utilizando o dataset "Quora Question Pairs".

Objetivo: Reduzir o tempo de processamento de uma tarefa com alta carga computacional ($O(n^2)$), onde cada par de perguntas deve ser comparado para extração de similaridade.

Volume de dados: 5.000 perguntas, gerando 12.497.500 comparações.

Algoritmo: Comparação exaustiva por força bruta com distribuição de índices via MPI (Message Passing Interface).

---

# 2. Ambiente Experimental

Descreva o ambiente em que os experimentos foram realizados.

## Orientações

Informar as características do hardware e software utilizados na execução dos testes.

| Item                        | Descrição |
| --------------------------- | --------- |
| Processador                 | 12th Gen Intel(R) Core(TM) i5-12500          |
| Número de núcleos           |  12 nucleos    |
| Memória RAM                 |  16,0 GB  |
| Sistema Operacional         |  Windows 11 Pro         |
| Linguagem utilizada         |  Python   |
| Biblioteca de paralelização | `mpi4py` integrada ao Microsoft MPI (MS-MPI) v10.1          |
| Compilador / Versão         | Python 3.13.2      |

---

---

# 3. Metodologia de Testes

Os experimentos foram conduzidos para comparar o desempenho da versão serial (baseline) com a versão paralelizada via MPI, utilizando diferentes níveis de concorrência no sistema operacional Windows 11.

## Procedimento Experimental

* **Medição do Tempo:** O tempo de execução foi medido utilizando a função `time.time()` do Python. O cronômetro foi iniciado imediatamente antes do início da carga de dados e encerrado após o processo mestre (Rank 0) consolidar os resultados finais de todos os trabalhadores.
* **Tamanho da Entrada:** Foi utilizado um subconjunto do dataset "Quora Question Pairs", totalizando **5.000 perguntas**. Essa amostragem resultou em **12.497.500 comparações de pares**, garantindo carga computacional suficiente para observar o comportamento do escalonamento.
* **Execuções:** Foram realizadas execuções controladas para cada configuração de processos. Os tempos registrados referem-se ao tempo total de parede (*wall-clock time*).
* **Condições de Execução:** Os testes foram realizados em uma estação de trabalho local. Para garantir a fidelidade dos dados, as execuções ocorreram com o mínimo de processos de terceiros em segundo plano, embora mantendo a carga típica do Sistema Operacional (Windows 11).

## Configurações Testadas

O experimento foi segmentado nas seguintes configurações de processos:

1.  **Versão Serial:** Execução única (1 processo) para estabelecer o tempo de referência ($T_1$).
2.  **Configurações MPI:** Execuções distribuídas utilizando a biblioteca `mpi4py` com:
    * 2 Processos
    * 4 Processos
    * 8 Processos
    * 12 Processos

## Cálculo de Métricas

Para a análise de eficiência, os dados coletados foram submetidos aos seguintes cálculos:

* **Speedup ($S$):** Calculado pela razão $S = T_1 / T_p$, onde $T_1$ é o tempo serial e $T_p$ o tempo com $p$ processos.
* **Eficiência ($E$):** Calculada pela razão $E = S / p$, representando o percentual de aproveitamento real de cada núcleo adicionado.

---
---

# 4. Resultados Experimentais

| Nº Threads/Processos | Tempo de Execução (s) |
| -------------------- | --------------------- |
| 1                    |          34.07        |
| 2                    |          25.08        |
| 4                    |          17.38        |
| 8                    |          12.50        |
| 12                   |          10.73        |

---

# 5. Cálculo de Speedup e Eficiência

## Fórmulas Utilizadas

### Speedup

```
Speedup(p) = T(1) / T(p)
```

Onde:

* **T(1)** = tempo da execução serial
* **T(p)** = tempo com p threads/processos

### Eficiência

```
Eficiência(p) = Speedup(p) / p
```

Onde:

* **p** = número de threads ou processos

---

# 6. Tabela de Resultados

Preencha a tabela abaixo utilizando os tempos medidos.

| Threads/Processos | Tempo (s) | Speedup | Eficiência |
| ----------------- | --------- | ------- | ---------- |
| 1                 |  34.07    | 1.0     | 1.0        |
| 2                 |  25.08    | 1.35    | 0.67       |
| 4                 |  17.38    | 1.96    | 0.49       |
| 8                 |  12.50    | 2.72    | 0.34       |
| 12                |  10.73    | 3.17    | 0.26       |

---

# 7. Gráfico de Tempo de Execução

Construa um gráfico mostrando o **tempo de execução em função do número de threads/processos**.

## Orientações

* Eixo X: número de threads/processos
* Eixo Y: tempo de execução (segundos)

Inserir o gráfico abaixo:

<img width="750" height="446" alt="image" src="https://github.com/user-attachments/assets/3fa3c246-7529-46cc-a287-69ac0600736b" />


---

# 8. Gráfico de Speedup

Construa um gráfico mostrando o **speedup obtido**.

## Orientações

* Eixo X: número de threads/processos
* Eixo Y: speedup
* Incluir também a **linha de speedup ideal (linear)** para comparação

Inserir o gráfico abaixo:

<img width="750" height="446" alt="image" src="https://github.com/user-attachments/assets/bac8b6b6-a0db-403d-b07a-10dd8fc142a8" />


---

# 9. Gráfico de Eficiência

Construa um gráfico mostrando a **eficiência da paralelização**.

## Orientações

* Eixo X: número de threads/processos
* Eixo Y: eficiência
* Valores entre 0 e 1

Inserir o gráfico abaixo:

<img width="750" height="446" alt="image" src="https://github.com/user-attachments/assets/1be8ba71-23f6-4dd1-88f1-5b5cbe273e6a" />


---

# 10. Análise dos Resultados

A análise dos dados revela um comportamento clássico de sistemas distribuídos em hardware local:

Ganho de Performance Real: Diferente do cenário anterior, a paralelização foi positiva. Ao passar de 1 para 12 processos, reduzimos o tempo de execução em aproximadamente 68%.

O "Degrau" da Eficiência: A eficiência cai de 1.0 (100%) no serial para 0.26 (26%) com 12 processos. Isso demonstra um alto custo de Overhead de Comunicação. Em Python, o MPI precisa "serializar" os dados (via Pickle) para movê-los entre processos, o que consome ciclos de CPU que poderiam estar sendo usados no cálculo.

Lei de Amdahl na Prática: Observamos que o ganho de velocidade não é linear. Ao dobrar os processos de 4 para 8, o tempo não caiu pela metade (foi de 17.3s para 12.5s). Isso ocorre porque a parte "serial" do código (leitura do CSV e distribuição inicial) permanece constante, limitando o speedup máximo possível.

Gargalo de Hardware: Como você está no Windows 11, o agendador de tarefas do SO precisa gerenciar 12 processos competindo por recursos (cache L3, largura de banda de memória). Com 12 processos, a eficiência de 26% sugere que estamos atingindo o limite de núcleos físicos da máquina ou criando contenção de memória.

---

# 11. Conclusão

Valeu a pena paralelizar? Sim. Do ponto de vista de entrega de resultado, reduzir um processamento de 34 segundos para 10 segundos é um ganho produtivo excelente para qualquer pipeline de dados.

Entretanto, do ponto de vista de arquitetura, a implementação é "ineficiente". Uma eficiência de 26% indica que estamos gastando muita energia computacional para coordenar os processos. Para melhorar esse índice, a estratégia deveria mudar de MPI (processos isolados) para Multiprocessing com memória compartilhada ou processamento em GPU (CUDA), dado que a tarefa é massivamente paralela.

O experimento cumpre seu papel acadêmico ao provar que, embora o tempo total caia, o custo da coordenação em sistemas distribuídos nunca é zero.
---
