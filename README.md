# RoverAI — Sistema Inteligente de Decisão Autônoma para Exploração Planetária

Projeto desenvolvido para a **Global Solution 2026.1** da disciplina *Inteligência Artificial e Generative AI* (FIAP). O trabalho propõe um protótipo de Inteligência Artificial para apoiar a tomada de decisão de um rover em uma missão de exploração planetária, a partir de dados simulados de sensores e telemetria.

[Abrir notebook no Google Colab](https://colab.research.google.com/drive/1uAbLkDp5EJcKd7oxr4oHttG7jxqjEdU4?usp=sharing)

---

## 1. Contexto e justificativa

Em uma missão de exploração planetária, o rover opera em ambiente desconhecido, sujeito a obstáculos, variações de terreno, risco de superaquecimento e instabilidade na comunicação com a Terra. Como o atraso e a perda de sinal inviabilizam o controle remoto contínuo, o veículo precisa decidir de forma autônoma.

Este projeto modela esse problema como uma tarefa de **classificação supervisionada**: a partir das leituras dos sensores, o sistema deve selecionar a ação operacional mais adequada dentre sete categorias possíveis: `seguir_em_frente`, `reduzir_velocidade`, `desviar_direita`, `desviar_esquerda`, `parar`, `retornar_base` e `modo_seguro`.

## 2. Objetivos

O objetivo geral é desenvolver, em formato de notebook, um protótipo que aplique técnicas de IA à decisão autônoma do rover. Como objetivos específicos, o trabalho propõe-se a:

- comparar o desempenho e a interpretabilidade de uma **Árvore de Decisão** e de uma **Rede Neural Artificial (MLP)** na classificação da ação;
- estimar o consumo de energia do veículo por meio de **Regressão Linear**, como variável auxiliar de planejamento;
- demonstrar, em caráter complementar, a aplicação de **Visão Computacional** (detecção de bordas) à percepção do terreno.

## 3. Base de dados

A base foi gerada sinteticamente em Python, conforme previsto no enunciado do desafio, e não deriva de fonte externa. Cada registro representa uma situação operacional da missão, com leituras de sensores geradas por faixas de valores coerentes com o domínio físico de cada ação (por exemplo, obstáculo muito próximo tende a resultar em `parar`; bateria muito baixa, em `retornar_base`). O conjunto possui **300 registros**, distribuídos entre as sete classes, superando o mínimo de 100 exigido.

### 3.1. Variáveis

| Variável | Descrição | Unidade |
|---|---|---|
| `distancia_obstaculo_m` | Distância até o obstáculo mais próximo | metros |
| `inclinacao_terreno_graus` | Inclinação do terreno | graus |
| `bateria_percentual` | Nível de bateria do rover | % |
| `temperatura_sistema_c` | Temperatura do sistema/motor | °C |
| `vibracao_ms2` | Vibração percebida pelo rover | m/s² |
| `sinal_comunicacao_percentual` | Qualidade do sinal de comunicação | % |
| `luminosidade_lux` | Intensidade de luz do ambiente | lux |
| `velocidade_atual_ms` | Velocidade atual do rover | m/s |
| `poeira_percentual` | Estimativa de poeira no ambiente | % |
| `tipo_terreno` | Tipo de terreno (plano, rochoso, arenoso, cratera) | categórica |
| `consumo_energia_wh` | Consumo estimado de energia | Wh |
| `acao_rover` | Variável-alvo da classificação | — |

A variável `tipo_terreno` é categórica e submetida a *One-Hot Encoding* antes do treinamento; a variável-alvo `acao_rover` é codificada com `LabelEncoder`. Para aproximar o cenário de falhas reais de sensores, foram inseridos valores ausentes de forma controlada, posteriormente tratados pela média da respectiva coluna.

## 4. Metodologia

O tratamento dos dados contempla a verificação e o preenchimento de valores ausentes, a codificação das variáveis categórica e alvo, e a divisão estratificada entre treino (75%) e teste (25%). Os modelos de classificação foram avaliados sobre o mesmo conjunto de teste, garantindo comparabilidade.

### 4.1. Árvore de Decisão

Modelo de classificação baseado em regras hierárquicas (critério de Gini, profundidade máxima 8). Sua principal vantagem no contexto é a **interpretabilidade**: as regras podem ser visualizadas e auditadas, permitindo justificar cada decisão — atributo crítico em sistemas autônomos.

### 4.2. Rede Neural Artificial (MLP)

Modelo capaz de capturar relações não lineares entre as variáveis. Por ser sensível à escala, os dados foram padronizados com `StandardScaler` antes do treinamento. Em contrapartida à Árvore, apresenta menor transparência, uma vez que a decisão decorre de pesos distribuídos entre as camadas.

### 4.3. Regressão Linear complementar

Aplicada à estimativa do consumo de energia (Wh), variável relevante para o planejamento de deslocamento e a gestão da autonomia da bateria.

### 4.4. Visão Computacional (etapa opcional)

Geração de uma imagem sintética de terreno planetário e aplicação do detector de bordas **Canny** (OpenCV), ilustrando como o processamento de imagens poderia complementar os sensores na percepção de obstáculos.

## 5. Resultados

| Modelo | Acurácia (teste) | Interpretabilidade |
|---|---|---|
| Árvore de Decisão | 98,67% | Alta — regras visíveis e auditáveis |
| Rede Neural MLP | 89,33% | Baixa — pesos internos |

A Regressão Linear obteve MAE de 1,1495 Wh, RMSE de 1,4506 Wh e R² de 0,9692, indicando boa capacidade de estimativa sobre a base simulada.

A validação com cenários inéditos (obstáculo próximo, bateria crítica, terreno inclinado, superaquecimento, perda de comunicação, entre outros) evidenciou que ambos os modelos falharam em situações críticas — notadamente na perda parcial de comunicação, em que divergiram da ação esperada. Esse achado reforça a principal conclusão metodológica do trabalho: a IA não deve constituir a única camada de decisão.

## 6. Conclusão

A Árvore de Decisão destacou-se pela conjugação de desempenho e interpretabilidade, atributo essencial em sistemas críticos. A MLP demonstrou capacidade de aprendizado de padrões mais complexos, porém com menor transparência, exigindo validação mais rigorosa para aplicação real.

Os resultados indicam que, em uma missão real, os modelos de IA devem ser combinados com **regras fixas de segurança**, validações redundantes e monitoramento contínuo, de modo a assegurar que condições extremas — bateria crítica, superaquecimento ou ausência de comunicação — resultem obrigatoriamente em ações seguras, independentemente da predição.

## 7. Estrutura do repositório

```
RoverAI/
├── RoverAI_Projeto_GS_IA.ipynb   # Notebook principal
├── RoverAI_base_simulada.csv     # Base de dados gerada pelo notebook
└── README.md
```

## 8. Execução

1. Abrir `RoverAI_Projeto_GS_IA.ipynb` no Google Colab ou Jupyter Notebook.
2. Executar todas as células em ordem.
3. Conferir os outputs, gráficos e tabelas gerados.

## 9. Autores

| Nome | RM |
|---|---|
| Ana Julia Vecchi | 563661 |
| Gabriel Yuji| 563581 |
| Marcella Fernandes | 570768 |
