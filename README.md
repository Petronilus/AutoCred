# AutoCred

**Estado vigente (D024):** usuário encerrou a fase 25 sem adoção de novos tetos em 2026-09-18. Item 306 encerrado por decisão explícita; benchmark e candidatas preservados. Fase 26 em preparação: [proposta de experimento de preço](PHASE26_PROTOCOL.md). Notas abaixo são históricas.

**Estado vigente (D023):** fase 25 executada nos passos 301–305: tetos de 48/36 meses, 510 avaliações esperadas, 204.000 carteiras simuladas e 86 testes. Ambos reduzem resultado acumulado em reais; ganho de ROI depende do horizonte. Nenhum teto adotado; passo 306 aguarda decisão, fase 26 não iniciada. [Relatório](outputs/term_experiments/TERM_EXPERIMENT_REPORT.md) e [protocolo](PHASE25_PROTOCOL.md). Registros seguintes preservam histórico.

**Estado vigente (D022):** fase 24 concluída: comparação de entrada e preço, 750 avaliações esperadas, 300.000 carteiras simuladas, 81 testes. Maior ROI pode coexistir com menor resultado em reais; nenhuma candidata substituiu o benchmark. [Relatório](outputs/entry_experiments/ENTRY_EXPERIMENT_REPORT.md). Próxima fase: 25, prazo; busca de taxa necessária (296) transferida pendente à fase 26. Condições continuam abertas à otimização.

**Estado vigente (D021):** fase 23 concluída: avaliação financeira do benchmark em C sob 30 cenários e 60.000 carteiras simuladas, 76 testes. ROI esperado de referência 7,07% a.a., sem validação oficial. [Resultados e limitações](outputs/benchmark_finance/BENCHMARK_FINANCE_REPORT.md). Próxima fase: 24, entrada como alavanca; definir reação do cliente antes de comparar alterações. Registros anteriores abaixo são históricos.

**Estado vigente (D020):** fase 22 concluída com guardrails executáveis; benchmark em C tem aprovação 47,38% e CET máximo 1,6%, mas a avaliação permanece incompleta sem aceite, inadimplência e volume originado. [Relatório](outputs/guardrails/GUARDRAILS_REPORT.md). Próxima: fase 23, avaliação financeira do benchmark por cenários.

**Estado vigente (D019):** fase 21 concluída: 5.000 propostas de C pontuadas; benchmark aprova 2.369 (47,38%) sob hipóteses explícitas. 63 testes aprovados. [Relatório](outputs/c_integration/C_INTEGRATION_REPORT.md). Próxima: fase 22, guardrails executáveis. Volume originado/default/ROI ainda dependem da avaliação por cenários da fase 23. Os estados anteriores abaixo são históricos.

**Estado vigente (D018):** fases 1–20 encerradas nos escopos registrados. Próxima: **21 — integrar condições e PD em C**, seguida de 22 — guardrails e 23 — avaliar benchmark em C; depois entrada, prazo e preço (24–26). [Roadmap atual](PROJECT_ROADMAP.md) e [protocolo de integração/avaliação](C_POLICY_PROTOCOL.md). Os estados e referências numéricas abaixo são históricos. Modelo e benchmark não foram alterados.

**Atualização vigente (D017):** fase 20 concluída com benchmark simples v1 adotado; 59 testes aprovados. [Tabela, justificativas e reprodução](outputs/policy/BENCHMARK_REPORT.md). Próxima fase: 21, entrada como alavanca. Avaliação integral por cenários ainda pendente. As notas abaixo preservam estados anteriores.


**Atualização vigente (D016):** fases 18 e 19 concluídas; fase 20 em andamento, motor de ofertas configurável implementado. Tabela candidata ainda pendente de decisão. Entrega completa antes dos gabaritos; cenários locais não representam avaliação oficial. [Definições atuais](POLICY_CONSTRAINTS.md) e [fase 20](outputs/policy/PHASE20_REPORT.md). Os estados abaixo são registros anteriores.

**Estado atual:** fase 18 concluída; EAD e LGD oficiais implementadas e 49 testes aprovados. [Relatório e exemplos de LGD](outputs/lgd/LGD_REPORT.md). Reprodução: `python scripts/run_lgd.py`. Próxima execução: fase 19, perda esperada e consolidação econômica do score, sem retorno à fase 16. Notas anteriores abaixo são registros históricos.

**Sequência vigente (D013):** fases 16 (score inicial) e 17 (EAD) encerradas; próxima fase 18 (LGD), seguida da fase 19 (perda esperada e consolidação econômica do score) e fase 20 (política). Os itens econômicos pendentes foram transferidos para a fase 19; não há retorno à fase 16. As notas de execução anteriores abaixo preservam o histórico do planejamento.

Fase 17 concluída: EAD tabelada por prazo/LTV, com limites superiores inclusivos (D012), 20 fatores oficiais verificados e 44 testes aprovados. [Relatório e exemplos Python](outputs/ead/EAD_REPORT.md). Reprodução: `python scripts/run_ead.py`. Próxima fase: LGD (18); a avaliação econômica do score aguarda LGD e EL.

Fase 16, parte estatística concluída: dez faixas provisórias de score, com cortes aprendidos somente no treino e aplicados a 2024/B. [Relatório e pendências](outputs/scoring/SCORING_REPORT.md), notebook `07_score_bands.ipynb`. Reprodução: `python scripts/run_score_bands.py` e `python scripts/document_phase16.py`. A conclusão econômica aguarda fases 17–19; próxima execução: fase 17.

Fase 15 concluída: pipeline CatBoost congelado em `models/pd_model.pkl` e PDs de 3.000 contratos em `outputs/pd_base_b.csv`. [Relatório e guia de uso Python](outputs/final_model/FINAL_MODEL_REPORT.md). Reprodução: `python scripts/freeze_pd_model.py`. Modelo sem recalibração adicional, conforme D008/D009; B usada apenas para aplicação.

Fase 14 encerrada por D008: calibração testada e não adotada; CatBoost original preservado como candidato, com limitações explícitas. [Relatório e resultados](outputs/calibration/CALIBRATION_REPORT.md). Guia Python: `notebooks/06_calibration.ipynb`; reprodução: `python scripts/run_calibration.py` e `python scripts/document_phase14.py`.

Projeto de decisão de crédito que conecta dados, PD, EAD, LGD, perda esperada, política, aceite, carteira, inadimplência, volume e ROI.

## Documentação

- [Discriminação e estabilidade — fase 13](outputs/validation/VALIDATION_REPORT.md)

- [Boosted e Random Forest — fase 12](outputs/ensembles/ENSEMBLES_REPORT.md)

- [Regressão logística — fase 11](outputs/logistic/LOGISTIC_REPORT.md)

- [Baseline de bureau — fase 10](outputs/baseline/BASELINE_REPORT.md)

- [Preprocessing — fase 9](PREPROCESSING.md)

- [Validação temporal — fase 8](TEMPORAL_VALIDATION.md)

- [Governança de features — fase 7](FEATURE_GOVERNANCE.md)

- [Registro de decisões](DECISIONS.md)
- [Diagnóstico de população — fase 6](outputs/drift/DRIFT_REPORT.md)

- [Constituição do projeto](AGENTS.md)
- [Roadmap e acompanhamento](PROJECT_ROADMAP.md)
- [Definição do problema econômico](PROJECT_SCOPE.md)
- [Restrições e pendências da política](POLICY_CONSTRAINTS.md)
- [Papel das bases e limites de generalização](DATA_ROLES.md)

## Estrutura

```text
AutoCred/
  data/
    raw/          # Bases originais A, B e C; imutáveis
    processed/    # Dados tratados gerados posteriormente
  notebooks/      # Nove notebooks de análise previstos no roadmap
  src/            # Módulos de lógica reutilizável
  models/         # Artefatos de modelos
  outputs/        # Tabelas e submissão geradas
  tests/          # Testes de leakage, política e regras financeiras
  requirements.txt
```

O notebook 01 contém o roteiro da auditoria, executada pelo módulo src.audit; os demais notebooks permanecem iniciais. src/data.py e src/audit.py implementam leitura e auditoria; tests/test_audit.py verifica o diagnóstico. Os demais módulos e testes permanecem estruturas iniciais. A criação desses arquivos atende à fase 3; não comprova conclusão das etapas de análise e implementação.

## Dados brutos

Colocar as bases A, B e C em `data/raw/`, preservando nomes, formatos e conteúdo originais. Nunca sobrescrever dados brutos. Recepção conferida em 2026-09-13: os três CSVs estão presentes e não vazios. A identificação abaixo segue os nomes dos arquivos; a auditoria de conteúdo pertence à fase 5.

- Base A: `base_A_autocred_base_desenvolvimento.csv`.
- Base B: `base_B_autocred_base_teste_modelo.csv`.
- Base C: `base_C_autocred_base_politica.csv`.
- Documentos recebidos: `AutoCred_Dicionario_de_Dados.xlsx` e `AutoCred_parametros_ead_lgd.xlsx`. Conteúdo ainda não conferido.

## Caminhos reservados

| Caminho | Artefato futuro |
| --- | --- |
| `models/pd_model.pkl` | Pipeline final de PD |
| `outputs/pd_base_b.csv` | PDs da Base B |
| `outputs/score_table.csv` | Tabela consolidada de score |
| `outputs/credit_policy.csv` | Política de crédito |
| `outputs/submission.csv` | Submissão do desafio |

Esses caminhos estão reservados documentalmente. Os arquivos serão gerados quando houver resultados válidos, evitando arquivos vazios que aparentem ser modelos ou entregas concluídas. Esquemas e requisitos oficiais de submissão ainda precisam ser conferidos.

## Ambiente e execução

Toda implementação e análise será em **Python**, conforme preferência do usuário. Ambiente da auditoria: Python 3.12.14; dependências verificadas em requirements.txt. Para reproduzir em um ambiente Python próprio:

```text
python -m pip install -r requirements.txt
python -m src.audit
python -m src.drift
python -m unittest discover -s tests
```

O notebook 01 é um guia Python; requer ambiente Jupyter separado para execução interativa. Jupyter não está instalado no runtime usado nesta auditoria. O comando do módulo gera as tabelas sem Jupyter. Veja [relatório e pendências](outputs/audit/AUDIT_REPORT.md).

## Pendências da fase 3

| ID | Status | Pendência |
| --- | --- | --- |
| F3-01 | Resolvida em 2026-09-13 | Bases A, B e C recebidas em data/raw/, identificadas pelos nomes e verificadas como arquivos não vazios. |
| F3-02 | Ambiente de auditoria validado; notebook interativo pendente | Python 3.12.14 e dependências registradas; Jupyter ainda não configurado. |
| F3-03 | Aberta; resolver antes da geração dos artefatos | Conferir formatos e esquemas oficiais de entrega no enunciado. |

As demais pendências metodológicas e econômicas estão registradas no roadmap, em `PROJECT_SCOPE.md` e em `POLICY_CONSTRAINTS.md`. Questões abertas devem permanecer documentadas até sua resolução explícita.



Ambiente atual de execução: `.venv/Scripts/python.exe`. Fase 9 implementada em src/preprocessing.py; reproduzir com `scripts/verify_phase9.py`. Dependências em requirements.txt. Notebook de apoio: notebooks/02b_preprocessing.ipynb.
