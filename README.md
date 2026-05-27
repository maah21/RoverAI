# RoverAI — Sistema Inteligente de Decisão Autônoma para Exploração Planetária

Projeto desenvolvido para a GS 2026.1 da disciplina **Inteligência Artificial e Generative AI**.

O objetivo do projeto é simular um sistema de Inteligência Artificial capaz de apoiar a tomada de decisão de um rover planetário a partir de dados simulados de sensores e telemetria.

## Contexto

Em uma missão de exploração planetária, um rover precisa operar em ambientes desconhecidos, com comunicação limitada e diferentes condições de risco. Para isso, o sistema deve interpretar informações como distância de obstáculos, inclinação do terreno, bateria, temperatura, vibração, luminosidade, poeira e qualidade do sinal de comunicação.

Com base nesses dados, o rover deve decidir automaticamente uma ação adequada, como seguir em frente, reduzir velocidade, desviar, parar, retornar à base ou entrar em modo seguro.

## Objetivo

Desenvolver um protótipo em Python, no formato de notebook, que utilize técnicas de Inteligência Artificial para classificar a ação mais adequada do rover durante uma missão de exploração planetária.

O projeto compara dois modelos de classificação:

- **Árvore de Decisão**
- **Rede Neural Artificial MLP**

Além disso, inclui uma etapa complementar de **Regressão Linear** para prever o consumo de energia do rover e uma etapa opcional de **Visão Computacional** para simular a detecção de bordas em um terreno planetário.

## Base de dados

A base de dados foi criada de forma simulada com Python, seguindo as regras do desafio.

A base possui **300 registros**, em que cada linha representa uma situação operacional enfrentada pelo rover durante a missão.

### Variáveis utilizadas

| Variável | Descrição |
|---|---|
| `distancia_obstaculo_m` | Distância entre o rover e o obstáculo mais próximo, em metros |
| `inclinacao_terreno_graus` | Inclinação do terreno em graus |
| `nivel_bateria_pct` | Nível de bateria do rover em porcentagem |
| `temperatura_sistema_c` | Temperatura do sistema/motor em graus Celsius |
| `vibracao_mm_s` | Nível de vibração do rover |
| `sinal_comunicacao_pct` | Qualidade do sinal de comunicação com a base |
| `luminosidade_lux` | Nível de luminosidade do ambiente |
| `poeira_pct` | Intensidade de poeira no ambiente |
| `velocidade_atual_ms` | Velocidade atual do rover em metros por segundo |
| `terreno_codificado` | Representação numérica do tipo de terreno |
| `consumo_energia_wh` | Consumo estimado de energia em Wh |
| `acao_rover` | Ação esperada do rover, usada como variável-alvo dos modelos de classificação |

### Ações possíveis do rover

- `seguir_em_frente`
- `reduzir_velocidade`
- `desviar_direita`
- `desviar_esquerda`
- `parar`
- `retornar_base`
- `modo_seguro`

A coluna `acao_rover` é a variável-alvo do projeto, ou seja, é aquilo que os modelos de IA tentam prever.

## Modelos utilizados

### 1. Árvore de Decisão

A Árvore de Decisão foi utilizada para classificar a ação do rover com base nos dados dos sensores.

Esse modelo é importante para o contexto do projeto porque suas regras podem ser visualizadas e auditadas, o que facilita a interpretação da decisão em sistemas críticos.

**Resultado obtido:**

- Acurácia no teste: **98,67%**

### 2. Rede Neural Artificial MLP

A MLP foi usada para resolver o mesmo problema de classificação. Esse modelo utiliza camadas de neurônios artificiais, pesos, bias e funções de ativação para aprender padrões nos dados.

Antes do treinamento, as variáveis numéricas foram padronizadas com `StandardScaler`, pois redes neurais são sensíveis à escala dos dados.

**Resultado obtido:**

- Acurácia no teste: **89,33%**

## Comparação entre os modelos

A Árvore de Decisão apresentou melhor desempenho no conjunto de teste e maior facilidade de interpretação. A MLP também apresentou bom resultado, mas é menos transparente, pois suas decisões dependem de pesos internos distribuídos entre neurônios e camadas.

No contexto de um rover autônomo, a interpretabilidade é importante porque permite entender por que uma decisão foi tomada, especialmente em situações críticas como obstáculo próximo, bateria baixa, superaquecimento ou perda de comunicação.

## Regressão Linear complementar

A etapa de Regressão Linear foi utilizada para prever o consumo de energia do rover, uma variável numérica relevante para o planejamento da missão.

**Resultados obtidos:**

- MAE: **1,1495 Wh**
- RMSE: **1,4506 Wh**
- R²: **0,9692**

Esses resultados indicam que o modelo conseguiu estimar o consumo de energia com baixo erro para a base simulada.

## Testes com novos cenários

Foram criados novos cenários para avaliar o comportamento dos modelos em situações diferentes, como:

- obstáculo muito próximo;
- bateria baixa;
- terreno inclinado;
- superaquecimento;
- perda parcial de comunicação;
- operação normal.

A análise mostrou que os modelos apresentaram bom comportamento geral, mas também falharam em algumas situações críticas. No cenário de perda parcial de comunicação, por exemplo, ambos os modelos divergiram da regra esperada, o que reforça a necessidade de combinar IA com regras fixas de segurança em aplicações reais.

## Visão Computacional

Como etapa opcional, foi criada uma imagem sintética simulando um terreno planetário com obstáculos e linhas de relevo. Em seguida, foi aplicado o detector de bordas **Canny**, estudado na aula de Visão Computacional.

Essa etapa mostra como imagens do ambiente poderiam complementar os sensores do rover, auxiliando na percepção de obstáculos e irregularidades do terreno.

## Tecnologias utilizadas

- Python
- NumPy
- Pandas
- Matplotlib
- Scikit-learn
- OpenCV
- Google Colab / Jupyter Notebook

## Estrutura do repositório

```text
RoverAI/
├── RoverAI_Projeto_GS_IA.ipynb
├── roverai_base_simulada_300registros.csv
└── README.md
```

## Como executar

1. Abra o arquivo `RoverAI_Projeto_GS_IA.ipynb` no Google Colab ou Jupyter Notebook.
2. Execute todas as células em ordem.
3. No Google Colab, recomenda-se usar:
   - `Runtime` → `Restart and run all`
4. Verifique se todas as células foram executadas e se os outputs, gráficos e tabelas aparecem corretamente.

## Conclusão

O projeto RoverAI demonstrou como técnicas de Inteligência Artificial podem apoiar a tomada de decisão de um rover planetário. A Árvore de Decisão se destacou pela interpretabilidade e pelo desempenho, enquanto a MLP mostrou capacidade de aprender padrões da base simulada, mas com menor transparência.

A análise dos cenários críticos mostrou que, em uma missão real, a IA não deveria ser a única camada de decisão. O sistema deveria ser combinado com regras fixas de segurança, validações redundantes e monitoramento constante para garantir que situações extremas resultem em ações seguras, como `retornar_base` ou `modo_seguro`.
