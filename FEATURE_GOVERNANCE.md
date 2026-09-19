# Fase 7 — Governança de features

**Atualização D019:** adaptador src/proposal_features.py implementado, sem alteração das 15 features. Condições desejadas + juros de referência definidos explicitamente; renda ausente não reconstruída; preprocessing congelado. F6-02 resolvida operacionalmente para a referência, sem afirmar validade causal/calibração em C. Evidência: outputs/c_integration/C_INTEGRATION_REPORT.md.

**Atualização D018:** F6-02 será resolvida na nova fase 21, antes da avaliação do benchmark em C. O contrato de 15 features permanece congelado. Ver C_POLICY_PROTOCOL.md para condições, comprometimento/renda ausente, ordem de cálculo e limites causais. Este planejamento não autoriza imputação ou criação silenciosa de condições.

## Contrato definido

Target fixo: `default_90_12`. As cinco colunas de leakage da constituição estão em `src/config.py` e são proibidas em X, assim como o próprio target.

`FEATURE_COLS` contém as 15 candidatas históricas cujo papel é Preditora e cuja disponibilidade na concessão é Sim no dicionário A/B. A correspondência foi verificada diretamente no XLSX por `scripts/verify_phase7.py`. É uma lista de elegibilidade inicial baseada na documentação, não seleção por desempenho ou resultado de tuning. Um subconjunto, como bureau sozinho, é permitido para os baselines.

Identificadores, datas e campos de apoio não entram automaticamente. Datas permanecem disponíveis fora de X para split e análise de safras; valores de apoio continuam disponíveis para a economia da operação. Nenhuma escolha de feature foi orientada por B/C.

## Uso em Python

```python
from src.data import load_bases
from src.features import build_features, validate_feature_columns
from src.config import TARGET

a = load_bases()['A']
X = build_features(a)
y = a[TARGET].copy()
validate_feature_columns(X.columns)
```

Este exemplo não treina modelo. O split temporal e os pipelines serão implementados nas próximas fases. `build_features` apenas seleciona e copia colunas; não imputa, recalcula idade ou reconstrói renda (D001/D002 preservadas).

`validate_feature_columns` rejeita target, leakage, colunas não autorizadas, nomes duplicados e lista vazia. `build_features` rejeita features ausentes e colunas de origem duplicadas. A presença de realizados na base bruta é permitida; sua entrada em X é proibida. Novas colunas da base nunca entram por seleção automática de “tudo exceto target”.

## Verificação e limites

Executar `python -m unittest discover -s tests` e `python scripts/verify_phase7.py`. Os testes inserem deliberadamente cada coluna proibida para comprovar rejeição; verificam também novas colunas, ausências, duplicatas e preservação dos valores. A verificação nas bases reais fica em `outputs/governance/phase7_checks.json`.

A/B produzem X com 15 candidatas e sem colunas proibidas. C não produz X contratual completo: campos desejados não são renomeados automaticamente para contratados e comprometimento não é inventado. A aplicação final depende de F6-02, a resolver antes de aplicar o pipeline a C. O diagnóstico desta falha não modifica a lista com base em C.

Essa proteção detecta nomes não autorizados. Não prova a origem de valores renomeados ou derivados; toda nova transformação deverá ser revisada quanto à disponibilidade temporal. Na implementação do modelo, a validação deverá ser chamada na fronteira de treino e aplicação; ainda não existe modelo integrado nesta fase.

## Conclusão e dependências

Passos 126–141 concluídos com contrato, implementação e testes. Regra permanente: nenhuma informação pós-concessão pode entrar nas features. Fases 8–9 devem preservar o contrato ao separar temporalmente e aprender preprocessing somente no treino. F6-02 permanece aberta para aplicação em C; nenhuma decisão de negócio foi tomada nesta fase.
