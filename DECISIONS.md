# Registro de decisões

## D024 — Encerramento da fase 25 sem adoção de teto

- Em 2026-09-18, usuário concordou com a recomendação após leitura dos resultados D023 e autorizou seguir ao próximo passo.
- Encerrar o item 306 por decisão explícita de não incorporar os tetos 48/36 nesta etapa. Não declarar implementação/adopção inexistente: benchmark v1 permanece vigente como referência, com prazo desejado até 60 meses. Candidatas D023 preservadas para combinações futuras.
- Fase 25 encerrada no escopo comparativo e decisório; fase 26 passa à preparação do experimento de preço. Autorização para prosseguir não estima elasticidade, seleção adversa ou objetivo de otimização ausentes.
- Fase 26 deve apresentar busca no domínio de taxas permitido, ROI de 15% como meta diagnóstica (não quinto guardrail), resultado em reais, volume e risco. Novas funções de reação de aceite/risco serão explicitadas antes da execução dependente, conforme D022/F23-02.

## D023 — Experimento de prazo e reação de aceite

- Em 2026-09-17, após explicação dos cenários, o usuário autorizou prosseguir com tetos 48/36 meses no grupo aprovado score 4–6/LTV desejado >80%; aceites 50/70/90% e quedas relativas 0/5/15/30% por 12 meses efetivamente encurtados, compostas. Protocolo PHASE25_PROTOCOL.md; configuração configs/simulation/term_experiments_v1.json. Hipóteses de sensibilidade, não elasticidades observadas.
- Aprovação, score, taxa, entrada e financiado preservados; EAD, parcela, comprometimento, PD sob oferta e EL recalculados. Controle com PD original e horizontes/estresses D021 mantidos. Tetos afetam 121/316 operações; nenhum fit novo ou multiplicador causal adicional.
- 510 avaliações esperadas e 102 cenários pareados de 2.000 carteiras (204.000), seed 42; 30 referências D021 reproduzidas. 86 testes aprovados. Hashes confirmam preservação dos dados/modelo/benchmark/outputs anteriores.
- Nenhuma das 480 comparações melhora resultado acumulado em reais. Na referência de 70%/queda 15%/risco persistente/PD reestimada, ROI 7,0742% do benchmark passa a 7,0741% (48) ou 7,2054% (36); resultado cai R$ 366.113 ou R$ 963.921. O ganho de ROI do teto 36 não persiste na hipótese de defaults só no primeiro ano. Guardrails de 12 meses passam em 216/240 casos por candidata; falhas de volume preservadas; horizonte total continua sensível a F23-01.
- Passos 301–305 concluídos no escopo experimental. 306 permanece pendente de decisão de adoção/encerramento; criar candidatas não significa incorporá-las à política vigente. Recomendação técnica: manter benchmark como referência, sem adoção automática dos tetos. Não houve avanço para fase 26 nem alteração do objetivo econômico.
- Relatório: outputs/term_experiments/TERM_EXPERIMENT_REPORT.md. F25-01 resolvida para esta grade, sem identificar reação real de aceite; demais limitações F21/F23 preservadas.

## D022 — Experimento de entrada e comparação pontual de preço

- Em 2026-09-16, usuário aprovou o desenho de PHASE24_PROTOCOL.md: grupo aprovado score 4–6/LTV desejado >80%; entradas +5/+10 p.p. versus taxa 1,8%; reestimar PD e manter controle com PD original; resposta de aceite relativa 5/15/30%, composta para +10 p.p.; preço com e sem agravamento adicional de PD×1,10.
- Score/aprovação/prazo fixos, 542 propostas afetadas; dados originais e benchmark preservados. Aceite por contrato implementado com ponderações corretas. PD reestimada não é efeito causal; não forçar redução de PD/LGD em todas as operações.
- 750 avaliações esperadas e 150 cenários pareados de 2.000 carteiras, subset computacional predefinido, seed 42. Grade mantém ambos horizontes e os cinco estresses D021. Benchmark anterior reproduzido nos 30 cenários. 81 testes aprovados.
- Na referência (70% aceite, queda adicional 15%, risco persistente), +5 p.p. gera ROI 7,20% mas reduz resultado em R$508 mil; +10 p.p. gera ROI 7,28% mas reduz resultado em R$990 mil. Preço 1,8% tem efeito monetário dependente da resposta/risco. Não há vencedora adotada; ROI e resultado absoluto permanecem objetivos distintos para discussão futura.
- Fase 24 concluída no escopo comparativo. Passo 296 transferido ainda pendente à fase 26 para busca de taxa necessária sob cenário/objetivo explícitos; passo 297 permanece concluído pela comparação pontual de 1,8%, sem alegar que essa taxa seja ótima ou necessária.
- F23-02 resolvida apenas como especificação de cenários locais desta comparação; resposta real não identificada. Antes da busca ampla de preço, definir funções em todo o domínio. F21/F23 demais limitações permanecem.
- Relatório: outputs/entry_experiments/ENTRY_EXPERIMENT_REPORT.md. Configuração: configs/simulation/entry_experiments_v1.json. Próxima fase 25; benchmark não é política final fixa.

## D021 — Avaliação experimental do benchmark em C

- Em 2026-09-16, usuário aprovou o protocolo recapitulado: aceite 50/70/90%, Price sem custos adicionais, referência CET=juros D019; risco mensal constante compatível com PD12, estendido ao prazo e comparado a defaults restritos ao primeiro ano; juros interrompidos antes da parcela do evento; perda EAD×LGD uma vez; prazo médio ponderado pelo financiado; guardrail de default mostrado em 12 meses e prazo total.
- PHASE23_PROTOCOL.md e configs/simulation/benchmark_v1.json registram hipóteses e estresses PD×1,10, LGD+5 p.p., aceite×0,90. 30 cenários, 2.000 carteiras por cenário, seed 42 e pareamento. Não calibrar parâmetros em B/C ou otimizar política nesta etapa.
- Fase 23 concluída: ROI esperado de referência 7,07% a.a. versus 9,60% na hipótese de defaults apenas no primeiro ano. Meta de 15% não atingida. Aceite 70% implica volume esperado R$62,36 milhões, default12 4,95%, default no prazo 16,23%. Conforme guardrails esperados em 12 meses; reprovada no horizonte total sob risco persistente. Não é aprovação oficial.
- Incerteza estrutural separada de sorteios. F23-01 horizonte/convenções oficiais não confirmados; F23-02 reação/seleção adversa antes das melhorias; F23-03 extrapolação da PD12 e EAD tardia. Risco e aceite futuros exigem protocolo explícito, sem dupla contagem de efeitos do modelo.
- 76 testes aprovados; contratos e cenários persistidos com hashes; nenhuma alteração de modelo, PD de B, cortes, bases ou benchmark. Relatório: outputs/benchmark_finance/BENCHMARK_FINANCE_REPORT.md. Próxima fase 24.

## D020 — Guardrails executáveis e avaliação incompleta

- Data: 2026-09-15. Fase 22 executada após a integração D019. Implementar comparações exatas para aprovação ≥35%, CET mensal ≤3,5%, inadimplência ≤8% dos contratos e volume originado ≥R$40 milhões.
- Aprovação usa todas as propostas de C como denominador e decisão do banco; aceite e originação são posteriores. CET é exigido em todas as ofertas aprovadas. Duplicatas, IDs ausentes, métricas não finitas ou métricas ausentes não passam silenciosamente.
- Benchmark em C: aprovação 47,38% e CET máximo 1,6% passam; default e volume originado indisponíveis, status `AVALIAÇÃO INCOMPLETA`, sem alegação de política válida. Não inventar cenário financeiro.
- 69 testes aprovados, incluindo precisão e menores violações. Fase 22 concluída; fase 23 deve definir cenários financeiros e calcular métricas antes de comparações. Relatório: outputs/guardrails/GUARDRAILS_REPORT.md.

## D019 — Referência fixa para inferência em C

- Usuário aprovou explicitamente em 2026-09-15: condições desejadas + juros de referência 1,6% a.m.; hipótese provisória sem custos adicionais, CET = juros; score de referência fixo, sem iteração entre score/oferta.
- Renda ausente: preservar NaN de renda/comprometimento e aplicar apenas preprocessing congelado. Avaliar sensibilidade do comprometimento aos quartis do treino. Não reconstruir renda, escolher cenário favorável ou declarar essa ausência validada.
- Benchmark v1 mantém condições de referência para aprovados, de modo que PD predita sob oferta coincide nesta versão. Distinção entre PD de referência, predita sob oferta e risco causal permanece obrigatória para futuras variantes.
- 5.000 propostas pontuadas, 2.369 aprovações do banco (47,38%); nenhum aceite/ROI inferido. 63 testes. Modelo, cortes, PDs de B e raw preservados. F6-02 resolvida operacionalmente neste protocolo; F21-01 (ausência de comprometimento/sensibilidade) e F21-02 (extrapolação/calibração em C) permanecem limitações.
- Protocolo e resultados em outputs/c_integration/C_INTEGRATION_REPORT.md. Hipóteses financeiras futuras devem respeitar esta versão ou registrar substituição explícita; igualdade CET/juros não é descoberta factual.

## D018 — Integração de C antes da avaliação e melhoria da política

- Autorização do usuário em 2026-09-15 para adequar planejamento à realidade e ao rigor estatístico. Correção de fluxo e interpretação, sem refazer modelos ou alterar fases concluídas.
- A: desenvolvimento/diagnóstico histórico; B: avaliação oficial de PD; C: aplicação e avaliação da política. Testes do benchmark em A não demonstram resultados financeiros em C. Fase 20 concluiu motor e tabela, não avaliação integral.
- Nova fase 21 explicita F6-02 (condições/features/PD em C); fase 22 antecipa guardrails; fase 23 avalia benchmark em C; fases 24–26 avaliam entrada/prazo/preço; futuras fases 27–36 consolidam alternativas/robustez/entrega. IDs dos passos e status existentes preservados; referências históricas não foram reescritas.
- Inspeção confirmou dependência da PD de financiado/LTV/prazo/comprometimento, com taxa apenas indiretamente via parcela. A ordem PD→score→oferta precisa de protocolo explícito. Nenhum mecanismo causal, tratamento novo de renda ausente ou conversão CET/juros foi decidido nesta revisão.
- Registrar horizonte da perda versus ROI, denominadores, cenários e hipóteses antes dos cálculos. Comparações sob mesmos cenários; distinguir ruído de simulação de incerteza estrutural. 2024 já consultada não é teste final virgem; não usar B na busca de políticas.
- Protocolo: C_POLICY_PROTOCOL.md. Evidência de preservação: outputs/planning/D018_checks.json. Esta decisão supera somente o fluxo futuro de D016/D017, preservando benchmark e resultados.

## D017 — Benchmark simples de política

- Em 2026-09-15, usuário adotou explicitamente o benchmark proposto: scores 4–10 aprovados, CET uniforme de 1,6% a.m., sem entrada adicional, prazo desejado até 60 meses; scores 1–3 recusados.
- Referência inicial reversível, sem alegação de ótimo ou conformidade integral. Preço ancorado apenas descritivamente na mediana de juros de treino (1,587%); CET não equiparado a juros. Corte não confunde PD individual com guardrail de inadimplência da carteira.
- Configuração versionada em configs/policy/benchmark_simples_v1.json; manter v1 para comparar futuras melhorias. Rascunho original preservado e não executável pelo carregador.
- Verificação mecânica em A: 70% de aprovação no treino e 68,8889% na validação, CET 1,6%. Não são resultados causais ou de C; inadimplência/volume originado/ROI não calculados sem cenários. B/C não usadas para definição ou ajuste.
- F20-01 resolvida; fase 20 concluída com tabela candidata e 59 testes. F6-02 e protocolo de cenários continuam pendentes. Relatório: outputs/policy/BENCHMARK_REPORT.md.


## D016 — Entrega anterior à avaliação oficial e distinção das decisões

- Usuário aprovou em 2026-09-15: aprovação é decisão do banco; aceite é decisão do cliente; originação decorre da contratação. Denominador da aprovação: todas as propostas, sem filtragem silenciosa.
- Projeto completo antes dos gabaritos. Simulador oficial presumido reservado ao professor; entrega não depende de acesso a ele. Avaliações locais usarão cenários com premissas explícitas, sem alegação de reproduzir o mecanismo oficial.
- Quatro guardrails preservados. Aprovação/CET conferidos nas ofertas; volume, inadimplência e ROI condicionados aos cenários. Nenhum resultado estimado será apresentado como garantia realizada.
- Fases futuras ajustadas; nenhuma fase histórica reaberta. Fase 20 inicia com motor configurável sem parâmetros padrão. Tabela candidata depende de discussão de seus números (F20-01); integração da PD nas condições de C permanece F6-02.
- CET mensal não é taxa de juros implícita. Documentação vigente em POLICY_CONSTRAINTS.md e outputs/policy/PHASE20_REPORT.md.

## D015 — Perda esperada e consolidação econômica

- Data: 2026-09-15. Execução da fase 19 autorizada pelo usuário. Fórmula EL = PD × EAD × LGD por operação; agregar por soma, preservando PD congelada e tabelas oficiais.
- Comparar EL média em reais e EL/financiado por score, separadamente em treino e validação. Critérios explicitados antes da execução. Ambas ordenadas; manter cortes existentes, sem seleção em B e sem mudança metodológica.
- Escada de perda estimada consolidada, sem alegar monotonicidade observada ou calibração comprovada. F16-01/F16-02 e limitações D008/D012 permanecem.
- Agregação por configuração implementada e testada; somente condições históricas aplicadas. Candidatas reais ficam na fase 20. Não define aprovação, taxa ou lucro.
- Evidências e reprodução: outputs/expected_loss/EXPECTED_LOSS_REPORT.md; tabela em outputs/score_table.csv.


## D014 — LGD oficial e identificação descritiva das operações

- Data: 2026-09-15. Fase 18 solicitada pelo usuário; aplicar os 20 valores oficiais por idade/LTV e ajuste aditivo de −0,061 com avalista, sem estimar novos parâmetros.
- Idade fornecida preservada (D001); faixas inteiras 0–2, 3–5, 6–8 e 9+; limites superiores de LTV inclusivos (D012). Idade negativa/fracionária, LTV inválido ou avalista ausente/desconhecido geram erro, sem imputação ou arredondamento implícito.
- Preservar pequenas irregularidades da tabela, sem impor monotonicidade, clipping ou substituição por LGD realizada. Ajuste de avalista equivale a 6,1 pontos percentuais, não redução relativa de 6,1%.
- Passos 261–262: identificar operações nas faixas oficiais 9+ anos e LTV>90% e ordená-las por LGD. Não estabelecer limiar arbitrário de “LGD elevada” ou regra de recusa. Médias reportadas são descritivas, não ponderadas; a perda esperada será calculada por operação na fase 19 e depois somada.
- Relatório em outputs/lgd/LGD_REPORT.md. EAD anterior, modelo congelado e PDs de B preservados. Nenhuma mudança de política foi definida.

## D013 — Fluxo linear do roadmap, preservando o trabalho concluído

- Data: 2026-09-15. Usuário determinou adequar o roadmap ao fluxo real, sem retorno a fases anteriores e sem alterar itens já concluídos.
- Substitui o plano de retorno de D010: a fase 16 fica encerrada no escopo do score inicial já realizado; itens 241–243 e 245–247, ainda pendentes, passam para um bloco de consolidação econômica ao final da fase 19.
- Fluxo: score inicial (16) → EAD (17) → LGD (18) → perda esperada e score econômico (19) → política (20). IDs são preservados para rastreabilidade; a posição de execução é dada pelos blocos do roadmap.
- Nenhum item pendente foi marcado como concluído e nenhuma implementação/resultados anteriores foram modificados. O texto dos itens concluídos permanece preservado. Relatórios anteriores registram o plano vigente na época; suas referências ao retorno à fase 16 estão superadas por esta decisão.

## D012 — Fronteiras de LTV para tabelas econômicas

- Data: 2026-09-15. Usuário autorizou escolher a convenção após apresentação das alternativas. Limites superiores inclusivos adotados por coerência com “até 60%” e “acima de 90%”: ≤60%, (60%,70%], (70%,80%], (80%,90%], >90%.
- A estatística não identifica qual lado de uma fronteira o professor pretendia; trata-se de convenção explícita, não conclusão empírica. F5-06 resolvida quanto ao tratamento das faixas de LTV; registrar revisão se houver orientação oficial diferente. Mesma convenção prevista para a LGD na fase 18.
- Fatores oficiais preservados, inclusive acima de 1 e diferenças não monotônicas. Sem clipping, interpolação entre prazos ou ajuste das tabelas aos dados. Somente prazos tabelados são aceitos.
- Em A/B usar LTV e valor financiado publicados; conferir a diferença bem−entrada e diagnosticar efeito da precisão do LTV. Novas condições recalculam financiado, LTV e EAD, sem reutilizar LTV antigo. Nenhuma mudança nas features do pipeline de PD.
- Sem arredondamento monetário adicional nesta fase; F2-05 permanece aberta para métricas finais. A fórmula aceita financiado zero (EAD zero), sem implicar elegibilidade. Lookup de LTV acima de 1 usa a última coluna; a construção de condições rejeita entrada negativa ou acima do valor do bem.
- F5-05, relativa a EAD realizado, permanece separada. As tabelas oficiais incluem safras 2024 e não fornecem evidência de backtest econômico prospectivo naquele ano. Detalhes em EAD_PROTOCOL.md e outputs/ead/EAD_REPORT.md.

## D011 — Faixas iniciais de score

- Data: 2026-09-15. Implementação da parte inicial da fase 16 solicitada pelo usuário, seguindo D010. Convenções técnicas explicitadas antes da execução, sem escolha de regra de negócio.
- Nove cortes por quantis lineares das PDs do treino 2022–2023 do pipeline congelado; dez faixas provisórias, score 10 menor risco. Limites fixos em treino/2024/B; nenhum target ou dado de B/validação define os cortes.
- Limite superior incluso, inferior exclusivo exceto zero; PD igual recebe score igual, sem desempate por ID. Quantis repetidos exigem revisão. PD original preservada.
- F16-01: inversões de default observado entre 8→7 e 7→6 em 2024; não fundir ou ajustar faixas automaticamente usando a validação. F16-02: cortes aprendidos com PDs in-sample; limitação declarada, sem alegação de estabilidade futura ou adequação econômica.
- Resultados em outputs/scoring/SCORING_REPORT.md. Passos 232–240 e 244 concluídos; 241–243 e 245–247 aguardam fases 17–19. Nenhuma política de aprovação ou precificação foi escolhida.

## D010 — Dependências do score e referências à PD

- Data: 2026-09-15. Usuário aprovou os ajustes sugeridos na revisão da milestone da fase 15.
- Executar a parte inicial da fase 16 (232–240 e 244), implementar as fases 17–19 e retornar à avaliação econômica do score (241–243 e 245–247) antes da fase 20. Não declarar a fase 16 integralmente concluída durante esse intervalo.
- Atualizar o passo 263 e a cadeia final para PD do pipeline congelado sem recalibração adicional, conforme D008/D009. Isso não altera probabilidades, target, modelo ou fórmulas.
- A discussão sobre possíveis usos de Bernoulli/Kelly/MPT não define objetivo de otimização, alocações, parâmetros ou mudança no classificador. As extensões E28 continuam experimentos futuros sujeitos às premissas registradas.

## D009 — Congelamento do pipeline de PD

- Data: 2026-09-15. Execução da fase 15 solicitada pelo usuário, dando continuidade à seleção D006 e ao encaminhamento aprovado D008.
- Modelo final desta versão: CatBoost original treinado em A/2022–2023, com seu preprocessing já ajustado, 15 features permitidas e sem recalibração adicional. Nenhum novo fit foi realizado.
- Artefato `models/pd_model.pkl` é cópia byte a byte do benchmark selecionado; formato joblib. Parâmetros, dependências, hashes e contrato de entrada registrados em `outputs/final_model/manifest.json` antes da leitura de B.
- B recebeu somente inferência posterior. Não alterar este pipeline em função de resultados de B ou da política. Qualquer revisão futura exige justificativa metodológica, versão distinta e avaliação temporal documentadas.
- Mantidas as limitações D008, F8-01 e de seleção da população. Congelar uma versão não comprova calibração econômica ou generalização para C.

## D008 — Encerrar calibração como testada e não adotada

- Data: 2026-09-15. Usuário concordou explicitamente com o encaminhamento: "Sim, concordo."
- Encerrar a fase 14 com resultado negativo de P14-01; preservar o CatBoost original treinado em 2022–2023 como candidato, sem recalibração adicional.
- Não substituir o benchmark pelo classificador de 2022 do experimento. Não inferir que o benchmark dispensa calibração em geral ou que suas PDs são economicamente validadas.
- F14-01 resolvida. Os passos condicionais à adoção de calibrador foram dispensados ou reformulados explicitamente no roadmap, sem marcar melhoria inexistente como realizada.
- Congelamento do modelo/preprocessing e aplicação à Base B permanecem na fase 15. Preservar limitações de calibração, validação de desenvolvimento e população nas etapas econômicas e de sensibilidade.

## D007 — Experimento temporal de calibração P14-01

- Data: 2026-09-15. Usuário aprovou explicitamente o protocolo com "Sim, vamos testar".
- Preprocessing/classificador em 2022; calibradores sigmoid/isotonic em 2023H1; escolha com controle em 2023H2; avaliação posterior em 2024. B/C fora do desenvolvimento.
- Resultado: nenhum calibrador melhorou Brier e log-loss; mantido controle sem recalibração conforme regra aprovada. Não houve reajuste de calibrador em 2023 completo.
- Benchmark original preservado. A versão treinada apenas em 2022 não o substitui automaticamente. Não confundir o efeito da redução do treino com o da calibração.
- F13-02 resolvida quanto ao protocolo deste experimento. F14-01 resolvida posteriormente por D008, sem declarar melhoria nem aptidão econômica comprovadas. Evidências em outputs/calibration/CALIBRATION_REPORT.md.

## D001 — Preservar idade do veículo fornecida

- Data: 2026-09-14.
- Fonte: orientação do professor, comunicada pelo usuário nesta conversa.
- Decisão: considerar os dados como fornecidos; não recalcular nem substituir `idade_veiculo_anos` a partir da data e do ano/modelo para corrigir F5-01.
- Justificativa informada: não há tempo para todos os participantes atualizarem as bases antes do desafio.
- Aplicação: manter o campo fornecido no desenvolvimento e na aplicação da solução, inclusive quando utilizado pelas regras de LGD. Não introduzir uma idade alternativa como correção indireta sem nova decisão.
- Limitação aceita: em A, a idade corresponde a `2022 - ano_modelo`, divergindo do ano da originação em 6.620 contratos. O diagnóstico permanece válido; a decisão resolve o tratamento, não comprova a correção factual do campo.
- Implicação para análises futuras: ao interpretar drift de idade e estabilidade temporal, explicitar que a convenção observada em A difere de B/C. Não atribuir automaticamente toda diferença a mudança real de população.
- Escopo: resolve F5-01. Não resolve as dúvidas de comprometimento de renda, convenção de EAD realizado ou fronteiras de LTV.

## Colaboração e evolução do roadmap

O usuário autorizou a identificação de lacunas e a sugestão de melhorias no roadmap. Registrar propostas e justificativas; decisões relevantes de metodologia ou negócio continuam exigindo explicitação e input quando necessário. Manter a implementação em Python e a execução incremental.

## D002 — Preservar comprometimento informado quando renda está ausente

- Data: 2026-09-14; decisão aprovada explicitamente pelo usuário.
- Preservar comprometimento_renda informado, inclusive nos 770 casos de A e 258 de B sem renda declarada; não reconstruir ou preencher renda por inversão da fórmula.
- Justificativa: o dicionário declara disponibilidade do campo na concessão; onde renda está presente, a fórmula confere.
- Limitação aceita: a origem dos valores de comprometimento nos casos sem renda não foi confirmada. A decisão resolve o tratamento F5-03, sem provar sua origem.
- C não fornece comprometimento contratado: sua disponibilidade e cálculo para aplicação da política devem ser definidos na etapa de features/condições. Esta decisão não autoriza inventar condições contratadas para C.
- F5-05 (convenção do EAD realizado) e F5-06 (fronteiras LTV) continuam abertas antes da implementação financeira.

## D003 — Preprocessing inicial

- Data: 2026-09-14. Usuário aprovou mediana do treino mais indicadores de ausência após discutir significado, rigor e limitações.
- Aplicar as mesmas medianas ao treino e à validação; nunca aprender da validação. One-hot de categorias e scaling numérico para logística ajustados somente no treino; scaling opcional para outros modelos.
- Não declarar esse método universalmente melhor. É o benchmark inicial, reversível; alternativas futuras exigem avaliação temporal adequada.
- D001/D002 preservadas. Detalhes e evidências em PREPROCESSING.md.

## D004 — Experimentos futuros solicitados

- Data: 2026-09-14. Solicitação do usuário: incluir Random Forest além dos boosted e explorar Bernoulli/crescimento geométrico/Kelly e Teoria Moderna de Portfólios como possíveis diferenciais.
- Planejamento registrado em E12-01 e E28-01 a E28-04 no roadmap. Execução permanece na sequência dessas fases.
- Autorização para explorar e comparar não define novas premissas de negócio, nem aprova substituir ROI, regras financeiras ou guardrails. Premissas ausentes serão discutidas antes da implementação dependente.

## D005 — AuROC treino/validação e gap

Solicitação do usuário: tratar comparação de AuROC treino e validação como diagnóstico importante em todos os modelos. Registrar o gap com sinal, investigar overfitting/underfitting sem diagnóstico automático, preservar a validação temporal como evidência principal. Implementado no baseline e planejado em E13-01 para os próximos modelos.

## D006 — Candidato provisório para calibração

- Data: 2026-09-15. Seleção técnica realizada no escopo do passo 209 solicitado pelo usuário; anunciada antes do registro. Não é escolha de regra de negócio nem congelamento final.
- CatBoost da fase 12 selecionado para calibração pelo conjunto de discriminação, Brier/log-loss, gap relativo às árvores e resultados trimestrais. Logística e demais benchmarks preservados.
- Trade-offs: não lidera KS nem tem a menor oscilação trimestral ou menor viés médio de PD. Gap continua relevante; diferenças não têm significância demonstrada.
- A escolha é reversível. Configuração não foi otimizada e nenhuma calibração foi ajustada. Protocolo do calibrador depende de F13-02. Evidência e premissas em outputs/validation/VALIDATION_REPORT.md.
