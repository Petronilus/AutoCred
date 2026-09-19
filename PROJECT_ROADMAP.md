# AutoCred — Roadmap do projeto

**Estado vigente (D024): fase 25 encerrada; fase 26 em preparação.** Em 2026-09-18, usuário decidiu não adotar novos tetos nesta etapa (306 encerrado sem incorporação). Benchmark e candidatas preservados. Próxima execução: experimento de preço, após definição explícita de resposta de aceite/risco ao longo das taxas. [Proposta](PHASE26_PROTOCOL.md). Notas de próxima execução abaixo são históricas.

**Próxima execução (D022): fase 25 — prazo como variável econômica.** Fase 24 comparou entrada/preço em cenários aprovados, sem substituir o benchmark. Passo 296 foi transferido pendente à fase 26; nenhum item histórico concluído foi reaberto. Taxa, prazo, entrada e corte seguem sujeitos a otimização e discussão de ROI versus resultado em reais.

**Próxima execução (D021): fase 24 — entrada como alavanca.** Fase 23 concluída sob cenários aprovados; ROI de referência 7,07% a.a. (70% de aceite, risco persistente), sem validação oficial. F23-01 a F23-03 documentadas; reação do cliente deve ser definida antes de comparar novas condições. Notas anteriores abaixo preservam histórico.

**Próxima execução (D020): fase 23 — avaliar o benchmark em C por cenários.** Fase 22 concluiu os guardrails executáveis; aprovação e CET do benchmark passam, enquanto inadimplência e volume originado permanecem indisponíveis. Fase 21 foi concluída sob protocolo experimental explícito; resultados não são avaliação oficial. Nenhuma escolha causal ou validação financeira integral foi inferida.

## Estado vigente — D018

Fases 1–20 concluídas nos seus escopos; fase 20 concluiu motor/tabela candidata, não avaliação financeira em C. Próxima execução: **fase 21, integração de condições e PD em C**. Fluxo futuro: 21 integração → 22 guardrails → 23 benchmark em C → 24 entrada → 25 prazo → 26 preço → 27–36 comparação, robustez e entrega. Ver [protocolo](C_POLICY_PROTOCOL.md).

Somente fases futuras foram renumeradas/reordenadas. Correspondência: antiga 25→22, 24→23, 21→24, 22→25, 23→26 e antigas 26–35→27–36. Os 504 IDs numéricos e extensões existentes permanecem únicos e com o mesmo status. E28 continua identificador permanente da extensão, agora localizada na fase 29. Seis novos itens E21-C documentam F6-02. Notas de execução anteriores abaixo e relatórios históricos conservam a numeração vigente na época; este bloco prevalece para a próxima execução.

A/B/C: A desenvolve o modelo e permite diagnósticos históricos; B avalia oficialmente a PD; C recebe a política e sua avaliação financeira. Percentuais históricos do benchmark em A não são evidência de resultado financeiro em C.

## Acompanhamento e execução

Recuperado em 2026-09-15 após sobrescrita acidental. Os 504 passos e a ordem das fases foram recuperados do anexo original; seis extensões foram extraídas do histórico desta tarefa. Marcações e notas de status foram reconstruídas a partir das decisões e relatórios preservados. Não é uma restauração byte a byte das notas anteriores. Proveniência e verificações em [registro de recuperação](backups/roadmap_recovery_2026-09-15/RECOVERY_REPORT.md).

Fases 1–18 encerradas no escopo atual; próxima execução: fase 19 (perda esperada e consolidação econômica do score). Por D013, os seis itens econômicos que estavam pendentes na fase 16 foram transferidos para o final da fase 19, após a perda esperada. Não há retorno à fase 16. Nenhum item pendente foi marcado como concluído por essa reorganização. A fase 14 teve resultado negativo e foi encerrada por D008, sem declarar melhoria inexistente. E12-01 e E13-01 concluídos; E28-01 a E28-04 pendentes. Trabalhar incrementalmente em Python e seguir AGENTS.md. Não marcar uma etapa somente porque o código foi escrito.

**Fluxo vigente:** 16 — score inicial → 17 — EAD → 18 — LGD → 19 — perda esperada e consolidação econômica do score → 20 — política. Os números dos itens são identificadores permanentes, não posições de execução: seguir a ordem das fases e dos blocos abaixo. Registros anteriores que previam retornar à fase 16 documentam o plano antigo, substituído por D013; resultados e itens concluídos permanecem preservados.

## Decisões e pendências preservadas

- [DECISIONS.md](DECISIONS.md): D001 preserva idade fornecida; D002 preserva comprometimento informado quando renda está ausente; D003 preprocessing aprendido somente no treino; D004 extensões financeiras/Random Forest; D005 AuROC treino/validação e gap; D006 seleção provisória; D007 protocolo de calibração; D008 não adoção; D009 congelamento final.
- F2-01 a F2-05: definições operacionais dos guardrails, periodicidade, população, horizonte e arredondamento em [POLICY_CONSTRAINTS.md](POLICY_CONSTRAINTS.md). Resolver antes dos cálculos dependentes.
- F3-03: conferir esquema oficial de submissão; CSV de B é output interno. Execução interativa dos notebooks requer ambiente Jupyter; scripts e testes foram executados.
- F5-01/F5-03 resolvidas por D001/D002. F5-06 resolvida quanto ao tratamento por D012: limites superiores de LTV inclusivos. F5-05 (convenção de EAD realizado) permanece aberta apenas para reproduzir o realizado; não bloqueia a EAD tabelada implementada. [Auditoria histórica](outputs/audit/AUDIT_REPORT.md) e [protocolo atual](EAD_PROTOCOL.md).
- F8-01: limitação retrospectiva de maturação do target; 2024 é validação de desenvolvimento já consultada. [Validação temporal](TEMPORAL_VALIDATION.md).
- F13-02 e F14-01 resolvidas para o experimento e seu encaminhamento. Calibração econômica e generalização para C não comprovadas; manter essas limitações nas análises de política e sensibilidade.
- E28-P01: definir com o usuário objetivo econômico, capital, horizonte, reinvestimento e dependência das perdas antes dos experimentos Bernoulli/Kelly/MPT. Não substituir regras oficiais ou inventar covariâncias. Fronteira inadimplência–ROI não equivale automaticamente à fronteira média–variância.

## Sequência original e acompanhamento

Abaixo está a **ordem de produção que eu seguiria do começo ao fim**, reorganizando a conversa inteira. Não preservei a ordem em que as ideias apareceram: trouxe para antes tudo o que precisa ser decidido antes de uma etapa posterior. Também tratei como **não definitivos** todos os números de política que o texto explicitamente chama de ilustrativos.

## Fase 1 — Fixar o problema econômico antes de modelar

**Status: fase 1 concluída.** Evidências e limitações: [registro da fase](PROJECT_SCOPE.md).

A conversa deixa claro que o objetivo não é apenas produzir um bom classificador: o modelo de PD alimenta uma decisão de crédito e, depois, uma política economicamente defensável. 

- [x] 1. Definir que o objetivo final é uma **decisão de crédito**, não apenas um modelo.
- [x] 2. Separar mentalmente a entrega em **modelo de risco** e **política de crédito**.
- [x] 3. Definir o primeiro bloco como o problema de **PD**.
- [x] 4. Definir o segundo bloco como o problema de **economia da operação**.
- [x] 5. Definir o terceiro bloco como o problema de **decisão de negócio**.
- [x] 6. Fixar `default_90_12` como target da modelagem.
- [x] 7. Escrever a cadeia `dados → PD → EAD → LGD → perda esperada → preço/condições → aceite → inadimplência → ROI`.
- [x] 8. Fixar a fórmula `EL = PD × EAD × LGD`.
- [x] 9. Fixar a fórmula `EAD = fator_EAD × valor_financiado`.
- [x] 10. Combinar as fórmulas em `EL = PD × fator_EAD × valor_financiado × LGD`.
- [x] 11. Registrar que o desafio fornece a lógica/fatores necessários para EAD.
- [x] 12. Registrar que o desafio fornece a lógica/tabelas necessárias para LGD.
- [x] 13. Registrar que o componente que será modelado por ML é a **PD**.
- [x] 14. Registrar que a PD será usada economicamente depois.
- [x] 15. Registrar que boa ordenação de risco sozinha não basta.
- [x] 16. Registrar que probabilidades mal calibradas podem prejudicar a precificação.
- [x] 17. Identificar **aprovação** como uma alavanca da política.
- [x] 18. Identificar **taxa** como uma alavanca da política.
- [x] 19. Identificar **prazo** como uma alavanca da política.
- [x] 20. Identificar **entrada** como uma alavanca da política.
- [x] 21. Registrar que aumentar taxa não necessariamente aumenta lucro.
- [x] 22. Registrar que taxas maiores podem alterar aceite.
- [x] 23. Registrar que taxas maiores podem causar seleção adversa.
- [x] 24. Registrar que entrada pode alterar simultaneamente risco e estrutura financeira da operação.
- [x] 25. Adotar como ciclo de trabalho `conceito → hipótese → código → resultado → interpretação → decisão`. 

## Fase 2 — Fixar as restrições que a solução final precisa respeitar

**Status: fase 2 concluída.** Evidências e limitações: [registro da fase](POLICY_CONSTRAINTS.md).

Os quatro guardrails são tratados no texto como regras inegociáveis e devem virar validações de código, não apenas argumentos de apresentação. 

- [x] 26. Registrar aprovação mínima de **35%**.
- [x] 27. Registrar taxa máxima de **3,5%**.
- [x] 28. Registrar inadimplência máxima de **8%**.
- [x] 29. Registrar volume originado mínimo de **R$ 40 milhões**.
- [x] 30. Tratar esses quatro limites como restrições durVoas da política.
- [x] 31. Planejar um teste automático para aprovação mínima.
- [x] 32. Planejar um teste automático para taxa máxima.
- [x] 33. Planejar um teste automático para inadimplência máxima.
- [x] 34. Planejar um teste automático para volume mínimo.
- [x] 35. Definir que uma política que viole qualquer restrição será marcada como inválida.

## Fase 3 — Organizar o projeto antes de espalhar código

**Status: fase 3 concluída.** Evidências e limitações: [registro da fase](README.md).

A estrutura sugerida separa dados, notebooks, módulos reutilizáveis, modelos, outputs e testes. 

- [x] 36. Criar a pasta raiz `autocred/`.
- [x] 37. Criar `data/`.
- [x] 38. Criar `data/raw/`.
- [x] 39. Colocar a Base A em `data/raw/`.
- [x] 40. Colocar a Base B em `data/raw/`.
- [x] 41. Colocar a Base C em `data/raw/`.
- [x] 42. Criar `data/processed/`.
- [x] 43. Criar `notebooks/`.
- [x] 44. Criar `src/`.
- [x] 45. Criar `models/`.
- [x] 46. Criar `outputs/`.
- [x] 47. Criar `tests/`.
- [x] 48. Criar `01_data_audit.ipynb`.
- [x] 49. Criar `02_eda.ipynb`.
- [x] 50. Criar `03_baseline.ipynb`.
- [x] 51. Criar `04_model_selection.ipynb`.
- [x] 52. Criar `05_model_validation.ipynb`.
- [x] 53. Criar `06_calibration.ipynb`.
- [x] 54. Criar `07_score_bands.ipynb`.
- [x] 55. Criar `08_credit_policy.ipynb`.
- [x] 56. Criar `09_policy_optimization.ipynb`.
- [x] 57. Criar `src/config.py`.
- [x] 58. Criar `src/data.py`.
- [x] 59. Criar `src/features.py`.
- [x] 60. Criar `src/model.py`.
- [x] 61. Criar `src/calibration.py`.
- [x] 62. Criar `src/scoring.py`.
- [x] 63. Criar `src/ead_lgd.py`.
- [x] 64. Criar `src/pricing.py`.
- [x] 65. Criar `src/policy.py`.
- [x] 66. Criar `src/simulator.py`.
- [x] 67. Criar `src/metrics.py`.
- [x] 68. Reservar `models/pd_model.pkl` para o modelo final.
- [x] 69. Reservar `outputs/pd_base_b.csv`.
- [x] 70. Reservar `outputs/score_table.csv`.
- [x] 71. Reservar `outputs/credit_policy.csv`.
- [x] 72. Reservar `outputs/submission.csv`.
- [x] 73. Criar `tests/test_leakage.py`.
- [x] 74. Criar `tests/test_policy.py`.
- [x] 75. Criar `tests/test_financial_rules.py`.
- [x] 76. Criar `requirements.txt`.
- [x] 77. Criar `README.md`.

## Fase 4 — Entender formalmente o papel das três bases

**Status: fase 4 concluída.** Evidências e limitações: [registro da fase](DATA_ROLES.md).

A Base A é de desenvolvimento; B é OOT sem target; C representa propostas em “mar aberto”. A e B contêm apenas aprovados da política anterior, criando viés de seleção. 

- [x] 78. Definir a Base A como base principal de desenvolvimento.
- [x] 79. Definir a Base B como base out-of-time sem target.
- [x] 80. Definir a Base C como população de propostas para aplicação da política.
- [x] 81. Registrar que a Base A contém apenas clientes aprovados pela política antiga.
- [x] 82. Registrar que a Base B contém apenas clientes aprovados pela política antiga.
- [x] 83. Registrar a existência de viés de seleção em A.
- [x] 84. Registrar a existência de viés de seleção em B.
- [x] 85. Registrar que C inclui uma população diferente da população histórica aprovada.
- [x] 86. Proibir o uso da Base B para escolher hiperparâmetros.
- [x] 87. Proibir o uso da Base B para escolher entre modelos.
- [x] 88. Tratar diferenças entre A e C como possível risco de extrapolação.

## Fase 5 — Auditar os dados antes de treinar qualquer coisa

**Status: fase 5 concluída.** Evidências e limitações: [registro da fase](outputs/audit/AUDIT_REPORT.md).

A auditoria indicada inclui estrutura, missing, duplicidade, distribuições, default, safras, categorias, outliers, fórmulas e drift. 

- [x] 89. Carregar a Base A.
- [x] 90. Carregar a Base B.
- [x] 91. Carregar a Base C.
- [x] 92. Verificar o `shape` da Base A.
- [x] 93. Verificar o `shape` da Base B.
- [x] 94. Verificar o `shape` da Base C.
- [x] 95. Inspecionar os tipos das colunas da Base A.
- [x] 96. Inspecionar os tipos das colunas da Base B.
- [x] 97. Inspecionar os tipos das colunas da Base C.
- [x] 98. Medir dados ausentes da Base A.
- [x] 99. Medir dados ausentes da Base B.
- [x] 100. Medir dados ausentes da Base C.
- [x] 101. Verificar duplicados na Base A.
- [x] 102. Verificar duplicados na Base B.
- [x] 103. Verificar duplicados na Base C.
- [x] 104. Inspecionar distribuições na Base A.
- [x] 105. Inspecionar distribuições na Base B.
- [x] 106. Inspecionar distribuições na Base C.
- [x] 107. Calcular a taxa de default disponível na Base A.
- [x] 108. Analisar as safras da Base A.
- [x] 109. Inspecionar as categorias das variáveis categóricas.
- [x] 110. Procurar outliers.
- [x] 111. Verificar a consistência das fórmulas fornecidas pelo desafio.
- [x] 112. Documentar anomalias encontradas.
- [x] 113. Resolver inconsistências necessárias antes da modelagem.

## Fase 6 — Medir mudança de população antes de confiar na generalização

**Status: fase 6 concluída.** Evidências e limitações: [registro da fase](outputs/drift/DRIFT_REPORT.md).

A análise de drift A→B→C aparece mais tarde na conversa, mas operacionalmente deve vir cedo, porque ajuda a entender onde o modelo será aplicado. 

- [x] 114. Comparar a distribuição de A com B.
- [x] 115. Comparar a distribuição de A com C.
- [x] 116. Comparar `score_bureau` entre A, B e C.
- [x] 117. Comparar `renda` entre A, B e C.
- [x] 118. Comparar `LTV` entre A, B e C.
- [x] 119. Comparar `idade_veiculo` entre A, B e C.
- [x] 120. Comparar variáveis de restrições entre A, B e C.
- [x] 121. Comparar `canal` entre A, B e C.
- [x] 122. Comparar `ocupacao` entre A, B e C.
- [x] 123. Identificar quais variáveis apresentam mudança relevante de população.
- [x] 124. Identificar se C parece ocupar regiões pouco representadas em A.
- [x] 125. Registrar explicitamente eventual extrapolação além do domínio histórico de aprovação.

## Fase 7 — Eliminar leakage antes de definir qualquer feature set

**Status: fase 7 concluída.** Evidências e limitações: [registro da fase](FEATURE_GOVERNANCE.md).

A conversa trata leakage como provavelmente o erro técnico mais grave do desafio. 

- [x] 126. Definir `TARGET = "default_90_12"`.
- [x] 127. Criar uma lista explícita de variáveis de leakage.
- [x] 128. Inserir `qtd_parcelas_em_atraso_12m` na lista de leakage.
- [x] 129. Inserir `mes_default` na lista de leakage.
- [x] 130. Inserir `ead_realizado` na lista de leakage.
- [x] 131. Inserir `lgd_realizado` na lista de leakage.
- [x] 132. Inserir `perda_financeira` na lista de leakage.
- [x] 133. Excluir `qtd_parcelas_em_atraso_12m` das features.
- [x] 134. Excluir `mes_default` das features.
- [x] 135. Excluir `ead_realizado` das features.
- [x] 136. Excluir `lgd_realizado` das features.
- [x] 137. Excluir `perda_financeira` das features.
- [x] 138. Definir o conjunto de features apenas com informações disponíveis na concessão.
- [x] 139. Criar teste automático que detecte colunas de leakage.
- [x] 140. Fazer o teste falhar caso alguma coluna proibida entre no modelo.
- [x] 141. Registrar essa regra como decisão de governança do modelo.

## Fase 8 — Criar o split temporal antes de aprender qualquer transformação

**Status: fase 8 concluída.** Evidências e limitações: [registro da fase](TEMPORAL_VALIDATION.md).

O texto recomenda treino em 2022–2023 e validação em 2024 como exemplo de divisão temporal, e rejeita a divisão aleatória como validação principal. 

- [x] 142. Usar `data_originacao` para separar passado e futuro.
- [x] 143. Definir 2022–2023 como período de treino.
- [x] 144. Definir 2024 como período de validação.
- [x] 145. Manter a ordem temporal da avaliação.
- [x] 146. Não usar `train_test_split(random_state=42)` como validação principal.
- [x] 147. Não misturar registros futuros no treino.
- [x] 148. Não aprender preprocessing usando registros da validação.
- [x] 149. Reservar a Base B para aplicação OOT posterior.
- [x] 150. Planejar métricas separadas por safra.
- [x] 151. Tratar a validação como teste de capacidade de ordenar risco no futuro.

## Fase 9 — Preparar o preprocessing sem contaminar a validação

**Status: fase 9 concluída.** Evidências e limitações: [registro da fase](PREPROCESSING.md).

O texto destaca missing em renda, tempo de emprego e bureau e exige que o tratamento seja aprendido apenas no treino, dentro de `Pipeline`. 

- [x] 152. Identificar missing em renda.
- [x] 153. Identificar missing em tempo de emprego.
- [x] 154. Identificar missing em score de bureau.
- [x] 155. Definir a estratégia de imputação.
- [x] 156. Ajustar a imputação apenas no treino.
- [x] 157. Aplicar a imputação aprendida à validação.
- [x] 158. Configurar one-hot encoding para variáveis categóricas quando necessário.
- [x] 159. Ajustar o encoding apenas no treino.
- [x] 160. Aplicar o encoding aprendido à validação.
- [x] 161. Configurar padronização quando o modelo exigir.
- [x] 162. Ajustar a padronização apenas no treino.
- [x] 163. Aplicar a padronização aprendida à validação.
- [x] 164. Encapsular imputação e transformação dentro de `Pipeline`.
- [x] 165. Impedir imputação global antes do split.
- [x] 166. Impedir qualquer transformação aprendida usando treino e validação juntos.

## Fase 10 — Criar o baseline mais simples possível

**Status: fase 10 concluída.** Evidências e limitações: [registro da fase](outputs/baseline/BASELINE_REPORT.md).

A recomendação é começar com um baseline ingênuo, inclusive para descobrir o valor incremental do ML. 

- [x] 167. Criar o Modelo 0.
- [x] 168. Usar `score_bureau` sozinho no Modelo 0.
- [x] 169. Gerar o ranking de risco produzido pelo baseline.
- [x] 170. Medir o AuROC do baseline.
- [x] 171. Medir o Gini do baseline.
- [x] 172. Medir o KS do baseline.
- [x] 173. Guardar essas métricas como referência.
- [x] 174. Usar esse resultado para medir quanto os modelos posteriores realmente acrescentam.

## Fase 11 — Criar a regressão logística de benchmark

**Status: fase 11 concluída.** Evidências e limitações: [registro da fase](outputs/logistic/LOGISTIC_REPORT.md).

A logística entra como benchmark interpretável antes dos modelos boosted. 

- [x] 175. Criar o Modelo 1 com regressão logística.
- [x] 176. Conectar o preprocessing à regressão via `Pipeline`.
- [x] 177. Treinar a regressão somente no período de treino.
- [x] 178. Gerar PDs para o período de validação.
- [x] 179. Calcular AuROC da regressão.
- [x] 180. Calcular Gini da regressão.
- [x] 181. Calcular KS da regressão.
- [x] 182. Avaliar a calibração da regressão.
- [x] 183. Guardar os resultados da regressão como benchmark interpretável.

## Fase 12 — Comparar modelos boosted e Random Forest

**Status: fase 12 concluída.** Evidências e limitações: [registro da fase](outputs/ensembles/ENSEMBLES_REPORT.md).

A conversa sugere testar LightGBM, XGBoost, CatBoost e HistGradientBoosting, priorizando LightGBM/CatBoost com logística como benchmark. 

- [x] 184. Criar um experimento com LightGBM.
- [x] 185. Treinar LightGBM apenas no treino.
- [x] 186. Avaliar LightGBM na validação temporal.
- [x] 187. Criar um experimento com XGBoost.
- [x] 188. Treinar XGBoost apenas no treino.
- [x] 189. Avaliar XGBoost na validação temporal.
- [x] 190. Criar um experimento com CatBoost.
- [x] 191. Treinar CatBoost apenas no treino.
- [x] 192. Avaliar CatBoost na validação temporal.
- [x] 193. Criar um experimento com HistGradientBoosting.
- [x] 194. Treinar HistGradientBoosting apenas no treino.
- [x] 195. Avaliar HistGradientBoosting na validação temporal.
- [x] 196. Manter a regressão logística na tabela comparativa.
- [x] 197. Manter o baseline de bureau na tabela comparativa.
- [x] 198. Não usar a Base B para decidir qual modelo venceu.


### Extensões solicitadas

- [x] E12-01. Treinar e avaliar Random Forest como candidato adicional, após os baselines, com preprocessing apropriado e a mesma validação temporal. Comparar discriminação, calibração e estabilidade; registrar seed e configuração. Não usar B/C para tuning ou seleção. Random Forest é um experimento adicional, não substitui os modelos boosted previstos.

## Fase 13 — Comparar discriminação e estabilidade

**Status: fase 13 concluída.** Evidências e limitações: [registro da fase](outputs/validation/VALIDATION_REPORT.md).

O texto recomenda AuROC, Gini, KS, calibração e estabilidade temporal, inclusive Gini separado por ano.  

- [x] 199. Calcular AuROC de cada modelo.
- [x] 200. Calcular Gini de cada modelo.
- [x] 201. Calcular KS de cada modelo.
- [x] 202. Comparar os modelos pela capacidade de discriminação.
- [x] 203. Calcular Gini para 2022.
- [x] 204. Calcular Gini para 2023.
- [x] 205. Calcular Gini para 2024.
- [x] 206. Comparar a estabilidade do Gini ao longo do tempo.
- [x] 207. Investigar modelos que ganham performance agregada mas perdem estabilidade temporal.
- [x] 208. Considerar estabilidade temporal na escolha do modelo.
- [x] 209. Selecionar o candidato de PD para a etapa de calibração.


### Extensões solicitadas

- [x] E13-01. Para cada modelo, apresentar AuROC de treino (in-sample), AuROC de validação temporal e gap = treino − validação, desde a fase 11, incluindo boosted e Random Forest. Investigar possível overfitting/underfitting junto com desempenho absoluto, complexidade, drift e variabilidade; não inferir diagnóstico por gap isolado nem criar limiar universal. Referência do bureau já disponível em outputs/baseline/generalization.csv.

## Fase 14 — Verificar e corrigir a calibração da PD

**Status: fase 14 encerrada como calibração testada e não adotada (D008).** Sigmoid e isotonic não melhoraram Brier/log-loss em 2023H2; nenhuma melhoria ou aptidão econômica foi demonstrada. F14-01 resolvida. [Relatório](outputs/calibration/CALIBRATION_REPORT.md).

Esse é um dos principais diferenciais sugeridos na conversa: AuROC mede ordenação, enquanto a política precisa de probabilidades úteis economicamente. 

- [x] 210. Gerar uma curva de calibração do modelo candidato.
- [x] 211. Comparar PD prevista com default observado.
- [x] 212. Verificar se o modelo subestima risco.
- [x] 213. Verificar se o modelo superestima risco.
- [x] 214. Não assumir que AuROC alto implica PD correta.
- [x] 215. Decidir se recalibração é necessária.
- [x] 216. Testar isotonic scaling se necessário.
- [x] 217. Testar Platt scaling se necessário.
- [x] 218. Escolher o método de calibração adequado.
- 219. **Dispensado por D008:** gerar PD com calibrador adotado; nenhum foi adotado. Previsões dos candidatos preservadas no experimento.
- [x] 220. Refazer a curva de calibração.
- 221. **Não demonstrado; encerrado por D008:** melhoria útil da calibração. O resultado negativo motivou não adotá-la.
- [x] 222. Revisar a discriminação para o encaminhamento. — D008 preserva o candidato original avaliado na fase 13; sem limiar universal ou garantia de adequação econômica.
- [x] 223. Definir o encaminhamento da PD para a etapa econômica (reformulado por D008). — PD do CatBoost original sem recalibração, com limitações explícitas; congelamento concluído na fase 15.

## Fase 15 — Congelar o modelo antes de partir para a política

**Status: fase 15 concluída em 2026-09-15 (D009).** CatBoost original de 2022–2023 e preprocessing congelados sem refit e sem recalibração. Artefato models/pd_model.pkl; PDs dos 3.000 contratos de B em outputs/pd_base_b.csv. Previsões históricas reproduzidas e 33 testes aprovados. B não possui target e não participou de ajuste ou seleção. [Relatório](outputs/final_model/FINAL_MODEL_REPORT.md).

- [x] 224. Definir qual modelo produzirá a PD final.
- [x] 225. Fixar o preprocessing associado ao modelo.
- [x] 226. Fixar a calibração associada ao modelo. — Sem recalibração adicional, conforme D008/D009.
- [x] 227. Salvar o artefato em `models/pd_model.pkl`.
- [x] 228. Evitar continuar alterando o modelo em função dos resultados da política sem uma justificativa explícita. — Regra permanente D009; script recusa sobrescrever outro modelo final.
- [x] 229. Aplicar o modelo à Base B.
- [x] 230. Gerar PD para cada registro da Base B.
- [x] 231. Salvar as PDs da Base B em `outputs/pd_base_b.csv`.

## Fase 16 — Transformar a PD em uma escada de risco

**Status: score inicial concluído em 2026-09-15 (D011); fase encerrada neste escopo por D013.** Nove cortes calculados somente nas PDs de treino 2022–2023; dez scores aplicados a treino, 2024 e B, preservando a PD. Duas inversões de default observado em 2024 (8→7 e 7→6), registradas em F16-01 sem ajuste automático. Limitação dos decis in-sample em F16-02. 39 testes aprovados; relatório, gráfico, notebook 07 e tabela provisória em [outputs/scoring/SCORING_REPORT.md](outputs/scoring/SCORING_REPORT.md). Sem conclusão econômica das faixas ou política definida.

**Escopo vigente:** passos 232–240 e 244, todos já concluídos. Os passos 241–243 e 245–247 foram transferidos, ainda pendentes, para a consolidação econômica ao final da fase 19. F16-01/F16-02 seguem documentadas e serão consideradas nessa consolidação. Os itens concluídos e seus resultados não foram alterados.

O texto propõe score de 1 a 10, inicialmente via quantis, mas recomenda não encerrar a construção simplesmente em decis. 

- [x] 232. Ordenar clientes pela PD.
- [x] 233. Definir score 10 como menor risco.
- [x] 234. Definir score 1 como maior risco.
- [x] 235. Criar uma primeira versão das dez faixas usando quantis.
- [x] 236. Contar clientes em cada faixa.
- [x] 237. Calcular PD média prevista em cada faixa.
- [x] 238. Calcular default observado em cada faixa onde houver target.
- [x] 239. Verificar monotonicidade de risco entre as faixas. — Verificado; duas inversões observadas em 2024, sem alteração automática de cortes.
- [x] 240. Calcular LTV médio por faixa.
- [x] 244. Comparar PD prevista e default observado por faixa.

## Fase 17 — Implementar EAD

**Status: fase 17 concluída em 2026-09-15 (D012).** Limites superiores de LTV inclusivos; 20 fatores conferidos com a planilha oficial, aplicação aos 13.000 contratos de A/B e recálculo de condições verificados. 44 testes aprovados. [Relatório e guia Python](outputs/ead/EAD_REPORT.md); [protocolo](EAD_PROTOCOL.md). F5-06 resolvida por convenção explícita. F5-05 permanece separada e não bloqueia a EAD tabelada. A fase 16 continua parcialmente aberta até LGD/EL e revisão econômica.

A EAD deve reagir às condições da operação, especialmente prazo e LTV. 

- [x] 248. Implementar a tabela/regra de fator de EAD fornecida pelo desafio.
- [x] 249. Fazer o fator de EAD receber o prazo da operação.
- [x] 250. Fazer o fator de EAD receber o LTV da operação.
- [x] 251. Calcular o valor financiado.
- [x] 252. Calcular `EAD = fator_EAD × valor_financiado`.
- [x] 253. Garantir que mudanças de prazo recalculam o EAD.
- [x] 254. Garantir que mudanças de LTV recalculam o fator de EAD quando aplicável.

## Fase 18 — Implementar LGD

**Status: fase 18 concluída em 2026-09-15 (D014).** 20 valores oficiais e ajuste aditivo por avalista conferidos; LGD calculada para A/B, identificação por faixas oficiais e recálculo por LTV executados. Idade fornecida preservada por D001 e fronteiras D012. 49 testes aprovados. [Relatório e guia Python](outputs/lgd/LGD_REPORT.md). Pequenas irregularidades da tabela preservadas; nenhuma regra de recusa ou precificação definida.

A LGD depende de idade do veículo, LTV e avalista; com avalista, o texto informa ajuste de 0,061. 

- [x] 255. Implementar a tabela/regra de LGD fornecida pelo desafio.
- [x] 256. Fazer a LGD receber a idade do veículo.
- [x] 257. Fazer a LGD receber o LTV.
- [x] 258. Fazer a LGD receber a informação de avalista.
- [x] 259. Aplicar o ajuste de `-0,061` quando houver avalista.
- [x] 260. Recalcular LGD sempre que o LTV mudar.
- [x] 261. Identificar operações com veículo antigo e LGD elevada. — Faixa oficial 9+ anos, ordenada por LGD; sem limiar de aprovação inventado.
- [x] 262. Identificar operações com LTV alto e LGD elevada. — Faixa oficial >90%, ordenada por LGD; sem regra de recusa.

## Fase 19 — Implementar a perda esperada e consolidar economicamente o score

Dependências: EAD da fase 17, LGD da fase 18 e score inicial da fase 16. Executar primeiro a perda esperada e depois a consolidação abaixo; a fase 20 recebe as faixas economicamente avaliadas. Status: fase 19 concluída em 2026-09-15 (D015). Perda por contrato, agregação e tabela econômica verificadas. Cortes preservados; escada de perda estimada, sem garantia de monotonicidade observada. Relatório: outputs/expected_loss/EXPECTED_LOSS_REPORT.md.

### Perda esperada por operação

- [x] 263. Receber a PD do pipeline congelado da operação, sem recalibração adicional conforme D008/D009, preservando as limitações de calibração registradas.
- [x] 264. Receber o EAD da operação.
- [x] 265. Receber a LGD da operação.
- [x] 266. Calcular `EL = PD × EAD × LGD`.
- [x] 267. Guardar a perda esperada no nível da proposta.
- [x] 268. Agregar perda esperada por score.
- [x] 269. Agregar perda esperada por configuração de política. — Função e separação testadas; aplicação às condições históricas. Candidatas reais dependem da fase 20.
- [x] 270. Usar perda esperada como ligação entre modelo e decisão financeira.

### Consolidação econômica do score — itens transferidos por D013

Estes itens estavam pendentes na fase 16 e mantêm seus identificadores. O passo 268 fornece a agregação de EL por score usada no passo 243; não são dois cálculos independentes. Considerar F16-01/F16-02 e definir critérios antes de rever faixas, preservando a PD congelada.

- [x] 241. Calcular EAD por faixa.
- [x] 242. Calcular LGD por faixa.
- [x] 243. Calcular perda esperada por faixa. — Consolidar a agregação do passo 268 na avaliação das faixas.
- [x] 245. Ajustar as faixas caso a divisão puramente quantitativa seja pouco útil. — Definir critérios antes de ajustar, sem usar B para seleção. — Avaliação realizada; cortes mantidos, sem necessidade de ajuste identificada.
- [x] 246. Transformar as faixas em uma verdadeira escada de risco. — Consolidar após a avaliação econômica. — Escada de perda estimada verificada; F16-01/F16-02 preservadas.
- [x] 247. Salvar a tabela consolidada em `outputs/score_table.csv`.

## Fase 20 — Implementar a função básica de política

Dependência atendida: fase 19 concluída. D016: entregar política completa antes dos gabaritos; avaliação oficial posterior não bloqueia o projeto.

**Status: fase 20 concluída em 2026-09-15 (D017).** Motor e benchmark simples v1 adotados: scores 4–10, CET de 1,6% a.m., entrada desejada sem piso adicional, prazo desejado até 60 meses. 59 testes aprovados; aplicação mecânica em A verificada, sem alegar conformidade em C ou resultados de aceite/ROI. F20-01 resolvida; integração da PD em C permanece F6-02. Relatório: outputs/policy/BENCHMARK_REPORT.md.

A função sugerida recebe risco e características da proposta e devolve decisão e condições. 

- [x] 271. Criar `politica_credito(cliente, pd)`.
- [x] 272. Passar a PD para a função.
- [x] 273. Passar `valor_bem` para a função.
- [x] 274. Passar `entrada_desejada` para a função.
- [x] 275. Passar LTV para a função.
- [x] 276. Passar `prazo_desejado` para a função.
- [x] 277. Passar `idade_veiculo` para a função.
- [x] 278. Passar a informação de avalista para a função.
- [x] 279. Fazer a função retornar decisão de aprovação.
- [x] 280. Fazer a função retornar taxa.
- [x] 281. Fazer a função retornar prazo.
- [x] 282. Fazer a função retornar entrada mínima.
- [x] 283. Fazer a função retornar valor financiado.
- [x] 284. Associar regras de política às faixas de score. — Mapeamento explícito validado; valores de negócio dependem do passo 285.
- [x] 285. Criar uma primeira tabela de política candidata. — Benchmark simples v1 explicitamente adotado (D017); configuração e justificativas salvas. Referência experimental, sem garantia nos quatro guardrails.
- [x] 286. Não tratar os números ilustrativos fornecidos na conversa como política final.

## Fase 21 — Integrar condições e PD na Base C

Etapa D018 concluída em 2026-09-15 por D019; F6-02 resolvida operacionalmente sob hipóteses aprovadas. 5.000 propostas pontuadas, sem exclusão; 63 testes. F21-01/F21-02 permanecem limitações. Relatório: outputs/c_integration/C_INTEGRATION_REPORT.md. Protocolo: [C_POLICY_PROTOCOL.md](C_POLICY_PROTOCOL.md). Não executar hipóteses relevantes sem explicitação e decisão; não esperar gabaritos para entregar o projeto.

- [x] E21-C01. Definir as condições de referência de todos os proponentes e separar campos desejados de ofertados.
- [x] E21-C02. Definir parcela, juros versus CET e comprometimento de renda, incluindo renda ausente, sem mudar o pipeline congelado.
- [x] E21-C03. Especificar a ordem entre PD de referência, score e oferta, e distinguir reavaliação preditiva de efeitos causais assumidos; evitar circularidade ou dupla contagem.
- [x] E21-C04. Implementar o adaptador de C para as 15 features, com IDs preservados, exclusão de leakage e validação de disponibilidade na decisão.
- [x] E21-C05. Conferir drift, extrapolação, ausências e cobertura das propostas após construir as features; não excluir casos silenciosamente.
- [x] E21-C06. Testar a integração e preservar hashes do modelo, cortes e dados; registrar premissas e limitações antes da avaliação do benchmark.

## Fase 22 — Fazer os guardrails rodarem automaticamente

Dependência D018: integração da fase 21 e definição do contrato das métricas. Implementar controles para serem chamados durante a avaliação da fase 23; esta etapa não declara política válida sem resultados.

**Status: fase 22 concluída em 2026-09-15 (D020).** Guardrails implementados, testados e executados sobre as ofertas atuais de C. Aprovação 47,38% e CET máximo 1,6% passam; inadimplência e volume originado estão indisponíveis, portanto o status geral é `AVALIAÇÃO INCOMPLETA`. 69 testes aprovados. Relatório: outputs/guardrails/GUARDRAILS_REPORT.md. A fase 23 permanece pendente.

- [x] 328. Implementar `assert approval_rate >= 0.35`.
- [x] 329. Implementar `assert max_rate <= 0.035`.
- [x] 330. Implementar `assert default_rate <= 0.08`.
- [x] 331. Implementar `assert originated_volume >= 40_000_000`.
- [x] 332. Executar os asserts após cada simulação.
- [x] 333. Interromper a aceitação de políticas que falhem em algum guardrail.
- [x] 334. Marcar políticas reprovadas como `POLÍTICA INVÁLIDA`.
- [x] 335. Colocar esses testes em `test_policy.py`.
- [x] 336. Colocar regras financeiras relevantes em `test_financial_rules.py`.
- [x] 337. Garantir que os guardrails não existam apenas na apresentação.

## Fase 23 — Avaliar o benchmark em C por cenários antes da entrega

**Status: concluída em 2026-09-16 (D021).** Protocolo aprovado, 30 cenários, 2.000 carteiras por cenário e 76 testes. Modelo/benchmark preservados. Guardrail de 8% reportado nos dois horizontes; limites verificados em cada simulação. Relatório: outputs/benchmark_finance/BENCHMARK_FINANCE_REPORT.md. F23-01 (horizonte/convenções), F23-02 (aceite/seleção adversa nas melhorias) e F23-03 (extrapolação temporal/econômica) permanecem limitações explícitas, sem esperar gabaritos para a entrega.

Dependências D018: fases 21–22. Primeira execução deve ser do benchmark v1; registrar hipóteses financeiras e de comportamento antes de calcular, incluindo compatibilidade PD 12 meses versus horizonte do ROI. Guardar referência reproduzível para todas as melhorias. Não há duplicação de integração ou nova escolha de modelo.

D016: nas etapas de avaliação e melhoria, “simulador” significa avaliação local sob hipóteses documentadas, salvo disponibilidade oficial confirmada. Antes da execução, definir contrato de ofertas, cenários de aceite/PD, juros, perdas, horizonte, ponderação e arredondamento. Simulador/gabaritos do professor ficam para confronto posterior à entrega, sem ajuste retroativo dos resultados entregues. Resultados de volume, inadimplência e ROI são condicionais aos cenários, não validação oficial. Aprovação = decisão do banco / todas as propostas; aceite e originação são posteriores.

- [x] 316. Aplicar a política proposta à Base C. — Reutiliza integração D019.
- [x] 317. Determinar quais propostas são aprovadas.
- [x] 318. Determinar quais condições são ofertadas.
- [x] 319. Simular aceite das ofertas.
- [x] 320. Identificar a carteira originada em cada cenário, distinguindo resultado simulado de contratação oficial.
- [x] 321. Calcular taxa de aprovação.
- [x] 322. Calcular volume originado.
- [x] 323. Calcular inadimplência da carteira resultante. — 12 meses e prazo total separados.
- [x] 324. Calcular retorno financeiro.
- [x] 325. Calcular ROI.
- [x] 326. Calcular demais resultados exigidos pelo exercício. — Juros, perdas e prazos registrados; formato oficial de submissão continua F3-03, sem inventar requisitos ausentes.
- [x] 327. Comparar esses resultados com os guardrails.

## Fase 24 — Usar entrada antes de recorrer automaticamente a taxa maior

**Status: concluída em 2026-09-16 (D022), no escopo comparativo.** Grupo de 542 propostas; +5/+10 p.p. de entrada e comparação com CET 1,8%. 750 avaliações esperadas, 150 cenários pareados de 2.000 carteiras, 81 testes. Benefícios de ROI não implicam melhora do resultado absoluto; nenhuma candidata adotada. Passo 296 transferido pendente à fase 26, pois comparar 1,8% não calcula taxa necessária. Relatório: outputs/entry_experiments/ENTRY_EXPERIMENT_REPORT.md.

Dependência D018: benchmark avaliado em C na fase 23. Comparar alternativas sob os mesmos cenários e controles; não substituir o benchmark v1.

F23-02: antes dos experimentos, definir reação de aceite e risco às condições, sem escolher coeficientes para favorecer uma candidata. Aceite uniforme de D021 serve apenas à referência fixa e não identifica resposta à entrada/taxa. Se o protocolo mudar, reavaliar também o benchmark com a mesma versão.

D016: EAD/LGD e financiado seguem regras disponíveis. Efeitos de entrada/taxa sobre PD e aceite serão hipóteses explícitas de cenários locais, não fórmulas oficiais inventadas. Definir hipóteses antes de executar itens dependentes; variar premissas para avaliar robustez.

Esse é um dos principais diferenciais sugeridos: otimizar a estrutura da operação antes de simplesmente aumentar o preço. 

- [x] 287. Identificar propostas com PD alta. — Scores 4–6 entre aprovados, grupo experimental explícito.
- [x] 288. Identificar propostas com LTV alto. — LTV desejado >80% no grupo, sem nova recusa automática.
- [x] 289. Testar aumento de entrada nessas propostas.
- [x] 290. Recalcular o valor financiado após a entrada adicional.
- [x] 291. Recalcular o LTV após a entrada adicional.
- [x] 292. Recalcular a PD conforme a lógica prevista pelo desafio. — Predição sob oferta e controle de PD fixa; sem fórmula causal oficial presumida.
- [x] 293. Recalcular a LGD após a mudança de LTV.
- [x] 294. Recalcular a EAD quando aplicável.
- [x] 295. Recalcular a perda esperada.
- [x] 297. Comparar a nova estrutura com a alternativa de apenas aumentar a taxa. — CET 1,8% comparador, não taxa ótima ou necessária.
- [x] 298. Medir o impacto da nova entrada sobre o aceite. — Cenários explícitos, não comportamento real identificado.
- [x] 299. Tratar entrada como alavanca dupla de risco quando ela reduz PD e LGD. — Reduções agregadas verificadas; aumentos individuais preservados e relatados.
- [x] 300. Preferir estruturas economicamente melhores a aumentos automáticos de taxa. — Comparação multidimensional realizada; nenhuma superioridade robusta simultânea em ROI/resultado/volume, sem adoção automática.

## Fase 25 — Testar prazo como variável econômica

**Status: concluída no escopo comparativo e decisório em 2026-09-18 (D024).** Experimento D023: tetos 48/36 afetam 121/316 propostas do grupo de 542; 510 avaliações esperadas, 102 cenários de 2.000 carteiras e 86 testes. Usuário decidiu não adotar tetos nesta etapa. [Protocolo experimental](PHASE25_PROTOCOL.md) e [resultados](outputs/term_experiments/TERM_EXPERIMENT_REPORT.md). Referências anteriores preservadas.

D018: usar integração, cenários e guardrails já implementados; comparar ao benchmark de C, sem recriar o avaliador ou mudar premissas entre candidatas.

- [x] 301. Criar alternativas de prazo para operações elegíveis.
- [x] 302. Recalcular o fator de EAD para cada prazo.
- [x] 303. Recalcular EAD para cada prazo.
- [x] 304. Recalcular perda esperada para cada prazo.
- [x] 305. Avaliar se reduzir prazo melhora economicamente operações de maior risco. — Nenhum ganho de resultado acumulado; ganho de ROI do teto 36 depende do horizonte.
- [x] 306. Decidir sobre incorporação de prazo máximo à política por faixa de score. — D024: usuário decidiu não incorporar novos tetos nesta etapa. Encerrado por não adoção explícita, sem afirmar que tetos foram implementados na política vigente; candidatas preservadas.

## Fase 26 — Testar preço sem cair na armadilha “taxa maior = lucro maior”

**Status: preparação iniciada em 2026-09-18 (D024).** Desenho proposto em [PHASE26_PROTOCOL.md](PHASE26_PROTOCOL.md); novas funções de aceite e seleção adversa ainda não adotadas. Nenhuma execução de preço ampla ou escolha de taxa realizada.

Transferência D022: calcular taxa necessária aqui, com objetivo e hipóteses definidos; o comparador de 1,8% da fase 24 não resolve esse problema. Calcular fronteira viável em todo o intervalo permitido; não presumir monotonicidade de ROI, PD ou aceite. Se meta for inviável, registrar em vez de inventar taxa.

- [ ] 296. Recalcular a taxa necessária para a operação. — Transferido da fase 24, ainda pendente; preservar ID.

D018: usar integração, cenários e guardrails já implementados; comparar ao benchmark de C, sem recriar o avaliador ou mudar premissas entre candidatas.

O próprio texto alerta que o simulador penaliza preço excessivo por aceite e seleção adversa. 

- [ ] 307. Criar taxas candidatas para cada segmento.
- [ ] 308. Passar as taxas pelo simulador.
- [ ] 309. Medir aceite para cada taxa.
- [ ] 310. Medir a composição da carteira aceita.
- [ ] 311. Observar eventual seleção adversa.
- [ ] 312. Recalcular inadimplência da carteira resultante.
- [ ] 313. Recalcular retorno da carteira resultante.
- [ ] 314. Evitar escolher taxa olhando apenas margem nominal.
- [ ] 315. Escolher taxa considerando comportamento de aceite.

## Fase 27 — Criar versões candidatas da política

- [ ] 338. Criar uma primeira política conservadora.
- [ ] 339. Medir seus resultados.
- [ ] 340. Criar uma política um pouco mais permissiva.
- [ ] 341. Medir seus resultados.
- [ ] 342. Alterar o score mínimo de aprovação.
- [ ] 343. Medir o efeito na aprovação.
- [ ] 344. Medir o efeito no volume.
- [ ] 345. Medir o efeito na inadimplência.
- [ ] 346. Medir o efeito no ROI.
- [ ] 347. Alterar entradas mínimas.
- [ ] 348. Medir novamente os resultados.
- [ ] 349. Alterar prazos máximos.
- [ ] 350. Medir novamente os resultados.
- [ ] 351. Alterar taxas.
- [ ] 352. Medir novamente os resultados.
- [ ] 353. Guardar apenas configurações que respeitem as restrições obrigatórias.

## Fase 28 — Fazer análise marginal por score

A conversa recomenda mostrar explicitamente onde adicionar segmentos começa a deteriorar a carteira. 

- [ ] 354. Avaliar uma política que aprove apenas scores 10–8.
- [ ] 355. Calcular ROI dessa política.
- [ ] 356. Calcular aprovação dessa política.
- [ ] 357. Calcular inadimplência dessa política.
- [ ] 358. Calcular volume dessa política.
- [ ] 359. Avaliar uma política que inclua score 7.
- [ ] 360. Recalcular ROI.
- [ ] 361. Recalcular aprovação.
- [ ] 362. Recalcular inadimplência.
- [ ] 363. Recalcular volume.
- [ ] 364. Avaliar uma política que inclua score 6.
- [ ] 365. Repetir as métricas.
- [ ] 366. Avaliar uma política que inclua score 5.
- [ ] 367. Repetir as métricas.
- [ ] 368. Identificar a faixa em que o retorno marginal começa a piorar.
- [ ] 369. Usar essa análise para justificar o cutoff de risco.

## Fase 29 — Gerar uma fronteira risco-retorno

A proposta é avaliar muitas políticas, remover as dominadas e escolher dentro da fronteira eficiente. 

- [ ] 370. Gerar muitas combinações de política.
- [ ] 371. Simular cada combinação.
- [ ] 372. Guardar inadimplência de cada política.
- [ ] 373. Guardar ROI de cada política.
- [ ] 374. Guardar aprovação de cada política.
- [ ] 375. Guardar volume de cada política.
- [ ] 376. Eliminar políticas que violem guardrails.
- [ ] 377. Plotar inadimplência no eixo X.
- [ ] 378. Plotar ROI no eixo Y.
- [ ] 379. Representar cada política como um ponto.
- [ ] 380. Identificar políticas dominadas.
- [ ] 381. Remover as políticas dominadas da consideração final.
- [ ] 382. Identificar a fronteira eficiente.
- [ ] 383. Comparar as políticas restantes em crescimento.
- [ ] 384. Comparar as políticas restantes em risco.
- [ ] 385. Comparar as políticas restantes em retorno.
- [ ] 386. Escolher uma política eficiente compatível com o mandato.


### Extensões solicitadas

- [ ] E28-01. Estudar e documentar a ligação entre utilidade logarítmica de Bernoulli, crescimento geométrico e critério de Kelly, distinguindo seus objetivos e hipóteses. Definir com o usuário horizonte, capital, reinvestimento e distribuição dos retornos/perdas antes de simular.
- [ ] E28-02. Implementar em Python um experimento de crescimento geométrico/Kelly compatível com crédito, comparando com a política-base. Se útil, avaliar Kelly fracionário; não aplicar automaticamente uma fórmula de aposta binária a financiamentos. Não alterar silenciosamente a definição de ROI ou os guardrails.
- [ ] E28-03. Avaliar uma análise de Teoria Moderna de Portfólios (Markowitz), definindo unidades de alocação, retornos, risco e dependência entre operações/segmentos. Estimar covariâncias apenas se os dados permitirem; caso contrário, propor cenários explicitamente hipotéticos, sujeitos a decisão do usuário, sem fabricar estimativas empíricas.
- [ ] E28-04. Comparar resultados e limitações das extensões com a fronteira-base e submetê-las aos cenários de sensibilidade da fase 31. Só incorporá-las à recomendação final se trouxerem interpretação ou ganho verificável. Se alguma abordagem for inadequada ao desafio ou não identificável pelos dados, documentar a conclusão e a justificativa.

## Fase 30 — Não sacrificar explicabilidade por otimização excessiva

O texto alerta explicitamente contra milhares de parâmetros que ninguém consegue defender. 

- [ ] 387. Verificar quantos parâmetros a política final possui.
- [ ] 388. Verificar se cada regra possui justificativa financeira.
- [ ] 389. Remover complexidade que não produza ganho relevante.
- [ ] 390. Preferir uma política explicável quando a vantagem de uma alternativa complexa for pequena.
- [ ] 391. Garantir que cada faixa de score tenha lógica compreensível.
- [ ] 392. Garantir que cada exigência de entrada tenha racional econômico.
- [ ] 393. Garantir que cada limite de prazo tenha racional econômico.
- [ ] 394. Garantir que cada taxa tenha racional econômico.
- [ ] 395. Garantir que cada negativa tenha racional econômico.

## Fase 31 — Fazer análise de sensibilidade antes de declarar a política vencedora

A conversa sugere três choques explícitos para mostrar robustez. 

- [ ] 396. Salvar os resultados da política em cenário-base.
- [ ] 397. Criar cenário com `PD real = PD estimada × 1,10`.
- [ ] 398. Recalcular a carteira nesse cenário.
- [ ] 399. Recalcular inadimplência.
- [ ] 400. Recalcular ROI.
- [ ] 401. Reavaliar os guardrails.
- [ ] 402. Criar cenário com `LGD = LGD + 5 p.p.`.
- [ ] 403. Recalcular a carteira nesse cenário.
- [ ] 404. Recalcular ROI.
- [ ] 405. Reavaliar os guardrails.
- [ ] 406. Criar cenário com aceite 10% menor.
- [ ] 407. Recalcular volume.
- [ ] 408. Recalcular retorno.
- [ ] 409. Reavaliar os guardrails.
- [ ] 410. Comparar os três cenários adversos com o cenário-base.
- [ ] 411. Verificar se a política continua economicamente saudável.
- [ ] 412. Preferir uma política robusta a uma política extremamente otimizada para um único cenário.

## Fase 32 — Fazer uma revisão técnica dos erros críticos antes da entrega

A conversa lista nove erros especialmente importantes. 

- [ ] 413. Confirmar que nenhuma variável pós-concessão entrou no modelo.
- [ ] 414. Confirmar que a validação principal não foi aleatória.
- [ ] 415. Confirmar que a imputação não ocorreu antes do split.
- [ ] 416. Confirmar que o preprocessing foi ajustado apenas no treino.
- [ ] 417. Confirmar que a Base B não foi usada para tuning.
- [ ] 418. Confirmar que o modelo não foi escolhido apenas por AuROC.
- [ ] 419. Confirmar que a calibração foi analisada.
- [ ] 420. Confirmar que a política antiga não foi tomada como verdade definitiva.
- [ ] 421. Confirmar que o viés de seleção de A e B foi reconhecido.
- [ ] 422. Confirmar que a mudança de população em C foi discutida.
- [ ] 423. Confirmar que taxa alta não foi tratada automaticamente como maior lucro.
- [ ] 424. Confirmar que o efeito da taxa no aceite foi considerado.
- [ ] 425. Confirmar que o efeito da taxa na seleção adversa foi considerado.
- [ ] 426. Confirmar que a política não maximiza ROI simplesmente negando quase todos.
- [ ] 427. Confirmar aprovação de pelo menos 35%.
- [ ] 428. Confirmar volume de pelo menos R$ 40 milhões.
- [ ] 429. Confirmar inadimplência de no máximo 8%.
- [ ] 430. Confirmar taxa máxima de 3,5%.
- [ ] 431. Confirmar que a política final pode ser explicada.

## Fase 33 — Consolidar os artefatos finais

- [ ] 432. Salvar o modelo de PD.
- [ ] 433. Salvar as PDs da Base B.
- [ ] 434. Salvar a tabela de score.
- [ ] 435. Salvar a tabela da política de crédito.
- [ ] 436. Salvar o arquivo de submissão.
- [ ] 437. Registrar as métricas do modelo.
- [ ] 438. Registrar os resultados temporais.
- [ ] 439. Registrar a análise de calibração.
- [ ] 440. Registrar os resultados de drift.
- [ ] 441. Registrar os resultados da política.
- [ ] 442. Registrar os guardrails.
- [ ] 443. Registrar a análise marginal.
- [ ] 444. Registrar a fronteira risco-retorno.
- [ ] 445. Registrar os cenários de sensibilidade.
- [ ] 446. Documentar a lógica final no `README.md`.

## Fase 34 — Montar a narrativa da apresentação na ordem de decisão

A sequência sugerida para o conselho vai do problema ao ROI. 

- [ ] 447. Abrir com o problema de negócio.
- [ ] 448. Explicar o target.
- [ ] 449. Explicar quais dados estavam disponíveis na concessão.
- [ ] 450. Explicar quais variáveis foram proibidas por leakage.
- [ ] 451. Mostrar o split temporal.
- [ ] 452. Mostrar o modelo escolhido.
- [ ] 453. Mostrar o benchmark.
- [ ] 454. Mostrar AuROC.
- [ ] 455. Mostrar Gini.
- [ ] 456. Mostrar KS quando útil.
- [ ] 457. Mostrar calibração.
- [ ] 458. Mostrar estabilidade temporal.
- [ ] 459. Mostrar Gini por safra.
- [ ] 460. Mostrar o score de 1 a 10.
- [ ] 461. Mostrar PD por faixa.
- [ ] 462. Mostrar default observado por faixa.
- [ ] 463. Mostrar EAD.
- [ ] 464. Mostrar LGD.
- [ ] 465. Mostrar perda esperada.
- [ ] 466. Apresentar a política de crédito.
- [ ] 467. Mostrar decisão de aprovação por faixa.
- [ ] 468. Mostrar taxa por faixa.
- [ ] 469. Mostrar prazo por faixa.
- [ ] 470. Mostrar entrada mínima por faixa.
- [ ] 471. Explicar por que entrada foi usada como alavanca de risco.
- [ ] 472. Mostrar o efeito da política sobre aceite.
- [ ] 473. Mostrar a carteira resultante.
- [ ] 474. Mostrar a inadimplência resultante.
- [ ] 475. Mostrar o volume resultante.
- [ ] 476. Mostrar o ROI resultante.

## Fase 35 — Inserir os diferenciais que elevam a defesa acima de uma solução básica

Esses são os seis diferenciais explicitamente destacados no fim da conversa. 

- [ ] 477. Destacar que discriminação não é calibração.
- [ ] 478. Mostrar por que a calibração importa para precificação.
- [ ] 479. Mostrar performance separada por período.
- [ ] 480. Mostrar drift A→B.
- [ ] 481. Mostrar drift A→C.
- [ ] 482. Explicar eventual extrapolação em C.
- [ ] 483. Mostrar análise marginal por cutoff de score.
- [ ] 484. Mostrar em que ponto adicionar risco destrói retorno.
- [ ] 485. Mostrar a fronteira risco-retorno.
- [ ] 486. Destacar a política escolhida na fronteira eficiente.
- [ ] 487. Explicar por que essa política foi escolhida.
- [ ] 488. Mostrar o choque de +10% na PD.
- [ ] 489. Mostrar o choque de +5 p.p. na LGD.
- [ ] 490. Mostrar o choque de -10% no aceite.
- [ ] 491. Mostrar se os guardrails sobrevivem aos choques.
- [ ] 492. Defender a solução como **política robusta**, não apenas como modelo de boa performance.

## Fase 36 — Fechar a defesa deixando clara a arquitetura mental do projeto

- [ ] 493. Relembrar que o Modelo 1 responde **quem pode dar default**.
- [ ] 494. Relembrar que o bloco econômico responde **quanto se perde se houver default**.
- [ ] 495. Relembrar que o bloco de decisão responde **quais condições tornam o cliente economicamente aceitável**.
- [ ] 496. Explicar que apenas o primeiro bloco é propriamente machine learning.
- [ ] 497. Explicar que o segundo bloco é matemática financeira/economia de crédito.
- [ ] 498. Explicar que o terceiro bloco é decisão de negócio.
- [ ] 499. Demonstrar como a PD alimenta EAD e LGD para chegar à perda esperada.
- [ ] 500. Demonstrar como a perda esperada influencia preço e condições.
- [ ] 501. Demonstrar como preço e condições influenciam aceite.
- [ ] 502. Demonstrar como aceite determina a carteira efetivamente originada.
- [ ] 503. Demonstrar como a carteira resultante determina inadimplência, volume e ROI.
- [ ] 504. Encerrar mostrando que a política satisfaz simultaneamente risco, crescimento e retorno.

### Resultado final esperado

Se você seguir essa ordem, o projeto deixa de ser simplesmente **“treinar um modelo → obter AuROC → inventar uma tabela de taxas”** e passa a ter uma cadeia completa:

**dados confiáveis → features válidas → validação temporal → PD do pipeline congelado, com calibração avaliada e limitações registradas → score inicial → EAD/LGD → perda esperada → revisão econômica do score → estrutura da operação → política → simulação → otimização → robustez → defesa econômica.**

Essa é, em essência, a reorganização de toda a conversa em uma **ordem de produção**, incorporando no meio do processo as recomendações que originalmente aparecem só no final.
