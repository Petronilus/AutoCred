# Fase 9 — Preparação dos dados

Status: concluída em 2026-09-14. Estratégia aprovada pelo usuário (D003), implementada e verificada com fit somente no treino.

## Diagnóstico após o split temporal

| Variável | Ausentes no treino | Ausentes na validação | Mediana do treino |
| --- | ---: | ---: | ---: |
| renda_mensal_declarada | 509 | 261 | 5208.07 |
| tempo_emprego_meses | 780 | 427 | 42.0 |
| score_bureau | 228 | 99 | 631.0 |

Valores reproduzidos em `outputs/preprocessing/missing_before.csv`. Estatísticas destinadas à imputação foram calculadas somente no treino; a validação foi usada apenas para contar ausências.

## Estratégia P9-01 — aprovada (D003)

- Numéricas: imputar valores ausentes com a mediana do treino e adicionar indicadores de ausência para renda, tempo de emprego e bureau. Os indicadores permitem diferenciar um valor observado de um valor preenchido.
- Preservar o comprometimento informado, inclusive quando renda está ausente (D002), sem recalculá-lo após imputar renda. Preservar idade fornecida (D001).
- Categóricas: one-hot encoding ajustado no treino. Ausências futuras representadas por uma categoria explícita; categorias novas tratadas sem reajustar o encoder, com ocorrência registrada. Não existem ausências categóricas nas bases auditadas.
- Padronização de variáveis numéricas para o benchmark logístico, com parâmetros ajustados no treino. Deixar configurável para modelos que não a exijam.
- Usar Pipeline/ColumnTransformer do scikit-learn, preservando split e contrato de features. Não usar B/C para decidir ou ajustar transformações.

A mediana e os indicadores são uma proposta inicial simples e reversível; não foram escolhidos por desempenho na validação. A mediana não reconstrói o valor real ausente e não resolve a incerteza de renda.

## Dependências

- P9-01: resolvida por aprovação explícita do usuário.
- P9-02: resolvida. Dependências instaladas em .venv; versões verificadas registradas em requirements.txt. O bootstrap de pip do Python disponível falhou; a instalação foi concluída direcionando o pip do runtime ao Python da .venv, sem modificar dependências compartilhadas.
- F6-02 permanece aberta para construção das condições de C; esta fase não preencherá campos contratuais inexistentes.

## Execução e resultados

```text
.venv\Scripts\python.exe scripts/verify_phase9.py
.venv\Scripts\python.exe -m unittest discover -s tests
```

Em outro computador com Python 3.12 instalado normalmente: criar `.venv` com `python -m venv .venv`, instalar requirements.txt com o Python desse ambiente e executar os comandos acima.

O Pipeline contém validação de features seguida de ColumnTransformer. A saída tem 33 colunas: 11 numéricas, 3 indicadores 0/1 e 19 categorias one-hot (incluindo uma categoria de ausência reservada para cada um dos quatro campos categóricos). O treino tem 6.670 linhas e a validação 3.330; todas as saídas são finitas. Nenhuma categoria nova foi encontrada na validação atual.

As numéricas recebem mediana e, com scale=True, StandardScaler. Os indicadores não são padronizados. Categóricas ausentes recebem uma categoria própria; categorias inéditas produzem zeros no bloco correspondente, sendo contabilizadas por unknown_categories. Nenhum desses casos reajusta categorias na validação.

O ponto de entrada recomendado é fit_temporal_preprocessor(base_a), que separa temporalmente antes de fit_transform no treino e chama somente transform na validação. A fábrica make_preprocessor existe para composição com modelos; não deve ser ajustada à base completa. A proteção por código depende de utilizar a entrada correta: não impede mau uso deliberado das APIs de fit.

Evidências: outputs/preprocessing/learned_parameters.csv, transformed_features.csv e phase9_checks.json. Nenhum modelo de PD foi treinado. Não houve escolha baseada em B/C, recálculo de idade, reconstrução de renda ou alteração de comprometimento.

Os testes incluem uma validação artificial com valores extremos e categorias inéditas, verificando que ela não altera medianas, médias ou categorias aprendidas. Também verificam indicadores, categoria de ausência, rejeição de target, preservação da origem e split antes do ajuste. Suíte completa: 17 testes aprovados.

Um notebook adicional de apoio, notebooks/02b_preprocessing.ipynb, torna esta fase reproduzível sem misturá-la ao baseline da fase 10. Ele contém código Python e explicações, ainda sem execução em kernel Jupyter; o mesmo fluxo foi executado pelos scripts. Jupyter permanece uma dependência opcional não instalada.
