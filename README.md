# Recomendação de Planos Megaline com Machine Learning

Projeto da Sprint 8 em que desenvolvo um modelo de classificação para recomendar o plano mais adequado aos clientes da operadora Megaline, com base no comportamento de uso de cada um.

## Objetivo

A Megaline quer migrar os clientes de planos antigos para os planos atuais, **Smart** e **Ultra**. O objetivo é criar um modelo que analise o uso mensal de cada cliente e recomende um dos dois planos, com **acurácia mínima de 75%**.

## Dados

| Arquivo | Conteúdo |
|---|---|
| `users_behavior.csv` | 3.214 registros mensais de uso por cliente |

| Coluna | Descrição |
|---|---|
| `calls` | Número de chamadas |
| `minutes` | Duração total das chamadas, em minutos |
| `messages` | Número de mensagens de texto |
| `mb_used` | Tráfego de internet, em MB |
| `is_ultra` | Plano do mês: 1 para Ultra e 0 para Smart (variável alvo) |

## Etapas

1. **Exploração dos dados:** verificação de tipos e de valores ausentes (não havia nenhum)
2. **Divisão dos dados:** 60% treino (1.928), 20% validação (643) e 20% teste (643)
3. **Treinamento de modelos:** Árvore de Decisão, Floresta Aleatória e Regressão Logística
4. **Ajuste de hiperparâmetros:** `RandomizedSearchCV` com validação cruzada de 5 partes
5. **Avaliação final:** teste do melhor modelo no conjunto de teste e comparação com a validação para verificar sobreajuste

## Resultados

### Comparação dos modelos (acurácia na validação)

| Modelo | Padrão | Após ajuste |
|---|---|---|
| Árvore de Decisão | 0,73 | 0,78 |
| **Floresta Aleatória** | **0,79** | **0,80** |
| Regressão Logística | 0,72 | 0,74 |

### Modelo final

A **Floresta Aleatória** teve o melhor desempenho e foi escolhida como modelo final.

- **Acurácia no teste:** 81%, acima da meta de 75%
- **Acurácia na validação:** 81%. A diferença para o teste é de apenas 0,01, o que indica que o modelo não sofreu sobreajuste.

| Plano | Precisão | Recall | F1-score |
|---|---|---|---|
| Smart (0) | 0,82 | 0,95 | 0,88 |
| Ultra (1) | 0,79 | 0,49 | 0,61 |

O modelo acerta muito bem os clientes do Smart, mas identifica apenas cerca de metade dos clientes do Ultra. Isso acontece porque há menos exemplos do Ultra nos dados. Técnicas de balanceamento de classes poderiam melhorar esse resultado.

## Tecnologias

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn

## Como executar

1. Clone o repositório
2. Instale as dependências:
   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn jupyter
   ```
3. Abra o notebook:
   ```bash
   jupyter notebook projetosprint8.ipynb
   ```

> Observação: o notebook lê o arquivo em `/datasets/users_behavior.csv`. Ajuste o caminho se o CSV estiver em outra pasta.
