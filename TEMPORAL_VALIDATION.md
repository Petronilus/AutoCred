# Fase 8 — Validação temporal

Implementada em Python conforme a referência de AGENTS.md e os passos 142–151. A decisão de período foi discutida e confirmada pelo usuário ao solicitar a fase 8.

| Partição de A | Intervalo | Contratos | Defaults | Taxa observada |
| --- | --- | ---: | ---: | ---: |
| Treino | 2022-01-01 ≤ data < 2024-01-01 | 6.670 | 587 | 8,8006% |
| Validação interna | 2024-01-01 ≤ data < 2025-01-01 | 3.330 | 239 | 7,1772% |

Usamos `data_originacao`, sem sorteio ou estratificação pelo target. Todos os 10.000 contratos foram alocados exatamente uma vez. A taxa de default é descritiva, não uma métrica do modelo nem o guardrail de política.

## Reproduzir

```text
python scripts/verify_phase8.py
python -m unittest discover -s tests
```

```python
from src.temporal import split_temporal
from src.features import build_features
from src.config import TARGET

# a é o DataFrame da Base A, lido sem transformações.
train, validation = split_temporal(a)
X_train = build_features(train)
y_train = train[TARGET].copy()
X_validation = build_features(validation)
y_validation = validation[TARGET].copy()
```

`split_temporal` rejeita datas inválidas, registros fora de 2022–2024, IDs ausentes/duplicados, target inválido e partições vazias. Retorna cópias ordenadas por data e ID, preservando os valores originais e missing. Não descarta registros inesperados silenciosamente.

O manifesto de IDs está em `outputs/temporal/split_ids.csv`, com uma linha por contrato e sua partição. Não é uma base tratada. Resumos mensais, configuração e hash da origem estão na mesma pasta. Os hashes dos dados brutos foram preservados.

## Separação de responsabilidades

Nenhum preprocessing foi ajustado nesta fase. Na fase 9, imputação, encoding e scaling devem ser ajustados somente em X_train e apenas aplicados à validação. O split por si só não impede alguém de usar a validação indevidamente em código futuro; essa proteção deverá ser verificada na implementação do Pipeline.

B não participa do split, treino ou escolha de modelos: continua reservada para aplicação posterior e avaliação externa pelo desafio. C pertence à aplicação de política. O script de split lê somente A; os arquivos brutos restantes são apenas verificados por hash para integridade.

## Plano de métricas por safra

Nas etapas de avaliação, reportar tamanho, defaults e taxa observada por ano e mês; depois, AuROC, Gini, KS e calibração, conforme disponibilidade. Métricas de discriminação não são definidas em grupos com apenas uma classe: registrar como indisponíveis, sem inventar zero. Meses pequenos devem ser interpretados com cautela.

Identificar explicitamente resultados in-sample em 2022/2023 versus resultados da validação em 2024. A avaliação de 2024 verifica ordenação e calibração em safras posteriores. Se utilizada para escolher modelos, é validação de desenvolvimento; não apresentar seu resultado como teste independente final. Calibração deverá ter protocolo próprio que respeite treino e validação, antes da fase 14.

## Limitação de maturação do target — F8-01

Este split é uma avaliação retrospectiva por safra usando a Base A com performance fechada, conforme o dicionário. Não é uma reprodução comprovada do conjunto de informações disponível em 01/01/2024: contratos originados no fim de 2023 exigem acompanhamento posterior para fechar o target de 12 meses.

Antes de alegar validação operacional em uma data histórica ou implementar backtesting que reproduza implantação, definir datas de disponibilidade dos rótulos e eventual intervalo de maturação. Isso pode exigir rever as janelas com decisão explícita; não foi introduzida mudança silenciosa na referência 2022–2023/2024. F8-01 permanece aberta para essa finalidade e deve aparecer na interpretação final.

## Verificação de conclusão

Passos 142–151 concluídos: split implementado, integridade e fronteiras verificadas, valores/missing preservados, regras de uso e plano por safra documentados. Suíte atual: 13 testes aprovados. Ajuste de preprocessing permanece na fase 9; nenhum modelo foi treinado.
