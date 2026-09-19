# Integração e avaliação da política em C — plano D018

**Atualização D024:** em 2026-09-18, usuário decidiu não adotar tetos D023 nesta etapa e autorizou preparar a fase 26. Fase 25 encerrada; benchmark preservado. PHASE26_PROTOCOL.md contém proposta de resposta de aceite/risco para a busca de preço, ainda não adotada.

**Atualização D023:** fase 25 comparou tetos 48/36 meses sob protocolo autorizado em PHASE25_PROTOCOL.md. Resultados em outputs/term_experiments/TERM_EXPERIMENT_REPORT.md. Passos 301–305 verificados; 306 aguarda decisão, sem adoção de teto ou alteração do benchmark. Reação de aceite por 12 meses encurtados é cenário, não elasticidade identificada. Fase 26 não iniciada.

**Atualização D022:** fase 24 avaliou entrada/preço com aceite heterogêneo, PD sob oferta e controle de PD original, sem mudar score de referência. Cenários em PHASE24_PROTOCOL.md e resultados em outputs/entry_experiments/ENTRY_EXPERIMENT_REPORT.md. Aceite/risco reais não identificados; futuras buscas precisam de função explícita de resposta no domínio estudado. Benchmark v1 preservado para comparação, não fixado como solução final.

**Atualização D021:** fase 23 executada sob protocolo aprovado em PHASE23_PROTOCOL.md. Aceite uniforme e risco temporal são cenários, não comportamento estimado. Resultados em outputs/benchmark_finance/BENCHMARK_FINANCE_REPORT.md. Para melhorias, explicitar reação às condições e reavaliar o benchmark sob a mesma versão; não inferir causalidade ou validade oficial.

**Atualização D020:** fase 22 concluiu os guardrails executáveis. O benchmark em C passa numericamente aprovação e CET, mas permanece `AVALIAÇÃO INCOMPLETA` até que a fase 23 forneça aceite, inadimplência e volume originado por cenários. Abaixo, o protocolo D018/D019 continua como base metodológica.

**Atualização D019 — fase 21 concluída:** usuário adotou referência fixa nas condições desejadas com juros/CET de 1,6% a.m. sob hipótese sem custos adicionais; renda/comprometimento ausentes tratados exclusivamente pelo preprocessing congelado, com sensibilidade aos quartis do treino. F6-02 resolvida para esse protocolo; F21-01/F21-02 permanecem limitações. [Resultados e uso](outputs/c_integration/C_INTEGRATION_REPORT.md). O plano abaixo registra as questões que motivaram a decisão; alternativas não escolhidas não são regras vigentes.

## Papéis e estado atual

A é desenvolvimento histórico: treino 2022–2023 e validação de desenvolvimento 2024, já consultada. B recebe o pipeline congelado para avaliação oficial de AuROC; não é laboratório de escolha de política. C é o público de propostas para política e avaliação oficial de ROI. O benchmark v1 foi adotado como referência, não validado economicamente. Os percentuais de aprovação calculados em A são verificações mecânicas num histórico selecionado, sem extrapolação automática para C.

Não refazer modelo, cortes, EAD/LGD ou EL pela confusão de comunicação. Não alterar as 15 features, preprocessing ou PDs de B. Novo aprendizado demandaria decisão específica e outro experimento.

## Fase 21: resolver F6-02 antes de aplicar em C

Inspeção de `src/config.py`, `src/features.py`, `src/preprocessing.py` e `src/policy.py`: o classificador usa valor financiado, LTV, prazo e comprometimento de renda. Não usa taxa diretamente. Comprometimento depende da parcela e renda, logo pode depender indiretamente de taxa e prazo. A função de política recebe uma PD pronta e ainda não executa essa integração.

Antes de implementar, definir e registrar:

1. Condições de referência disponíveis para todos os proponentes, incluindo os que serão recusados. Preservar a distinção entre desejo do cliente e oferta do banco.
2. Como calcular parcela/comprometimento: Price confere no histórico, mas CET não equivale automaticamente a juros. Documentar convenção de custos/amortização e qualquer hipótese de conversão antes de calcular.
3. Renda ausente: não reconstruir renda, nem estimar comprometimento usando renda imputada silenciosamente. D002 preserva informação histórica, não autoriza criar comprometimento em C. O preprocessing congelado possui imputação, mas isso não comprova validade de introduzir um padrão de ausência diferente; avaliar alcance e sensibilidade antes de adotar tratamento.
4. Mapeamento explícito das colunas e identificadores de proposta, preservando ordem e cardinalidade. Campos proibidos existentes em C permanecem excluídos. Guardas para dados inválidos devem sinalizar problemas sem removê-los do denominador da aprovação silenciosamente.
5. Ordem entre PD, score e oferta. Documentar PD de referência para segmentação e eventual PD predita sob oferta em campos separados. Não fazer laço até convergir sem regra prévia, teste de convergência e tratamento de oscilação. Uma alternativa a discutir é segmentação por referência fixa e avaliação posterior da oferta; outra é avaliar uma grade finita de condições. Nenhuma dessas alternativas foi adotada por este documento.
6. Separar previsão do modelo sob condições da hipótese de risco efetivo no cenário. O modelo observacional em aprovados não identifica o efeito causal de mudar entrada ou taxa. Evitar contar seleção adversa ou efeito de entrada duas vezes no modelo e no cenário.
7. Drift/extrapolação, cobertura e ausências em C: reaproveitar diagnósticos existentes, conferir o que mudou com as features construídas. Não usar gabaritos nem modificar o modelo para melhorar resultados em C.

Critério de conclusão: protocolo das premissas aprovado quando necessário, adaptador reproduzível/testado, todas as propostas contabilizadas, nenhuma feature proibida, artefatos de inferência preservados. Apenas prever não comprova calibração na nova população.

## Fases 22–23: controles e benchmark antes de melhorias

Definir o contrato das métricas e implementar guardrails antes de aceitar qualquer resultado de política. Aprovação é número de decisões favoráveis do banco dividido por total de propostas; aceite é posterior. CET máximo deve considerar todas as ofertas aprovadas. Valores aprovados não são volume originado. Não tratar métricas ausentes como conformidade.

Antes da primeira execução financeira, registrar cenários, hipóteses de aceite, risco, amortização, juros recebidos, horizonte, perda e prazo médio. PD 90/12 e EL correspondem ao horizonte do target; não multiplicar por anos ou reutilizar como perda de toda a vida do contrato sem uma hipótese explícita que compatibilize o ROI do desafio. Preservar fatores EAD e LGD oficiais e suas limitações temporais.

Entregar por cenário: aprovação, CET máximo, volume originado estimado, inadimplência estimada, perda e ROI estimado, além do estado individual dos quatro guardrails. Distinguir aproximações por valores esperados de carteiras realizadas simuladas: razão de esperanças não é automaticamente esperança da razão. Declarar ponderações, carteira vazia e números de contratos aceitos insuficientes.

O benchmark deve ser avaliado primeiro em C e preservado com configuração, cenário, versões, seeds quando aplicáveis e hashes. Nenhum simulador oficial é requisito de entrega. Documentar resultados oficiais separadamente quando recebidos.

## Comparações e limites estatísticos

As fases 24 em diante comparam entrada, prazo e preço ao benchmark sob os mesmos cenários. Se houver sorteio, usar comparações pareadas e sementes controladas para reduzir ruído; não escolher a melhor política por uma única realização favorável. Separar variação Monte Carlo da incerteza estrutural nas hipóteses de comportamento e da incerteza da PD. Intervalos simulados não garantem cobertura do mundo real.

Registrar cenário central e estresses antes da busca; não escolher premissas para fazer uma candidata passar. Reavaliar todas as candidatas, inclusive benchmark, quando o protocolo mudar, com nova versão. C é população de decisão; otimizar condições nela sob cenários não a transforma em validação independente. Preservar 2024 como validação de desenvolvimento, não holdout virgem. B permanece fora dessa busca.

Escolha final deve observar os quatro guardrails, resultados entre cenários, explicabilidade e estabilidade. Não relaxar limites nem declarar resultado oficial antes dos gabaritos. A meta de ROI de 15% a.a. e os parâmetros atuais não ganham novas interpretações por este planejamento.
