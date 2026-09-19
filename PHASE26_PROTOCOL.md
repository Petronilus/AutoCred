# Fase 26 — Proposta de experimento de preço

Status em 2026-09-18: preparação autorizada por D024; desenho abaixo proposto, sem execução financeira ou aprovação das novas funções de reação. D022 autorizou apenas comparação pontual de 1,8% no grupo original. Não tratar esse resultado como elasticidade estimada em toda a faixa de taxas.

## Objetivo e escopo propostos

Investigar quais preços atingem ROI anual de 15% sob hipóteses explícitas e passam nos quatro guardrails; comparar também resultado acumulado em reais, volume, risco e robustez. Meta de 15% é diagnóstica, não quinto guardrail nem autorização para maximizar exclusivamente ROI. Não adotar preço vencedor automaticamente. Se uma meta não for atingida na grade, relatar esse limite sem inventar solução contínua.

Preservar benchmark v1, scores e decisões de aprovação (2.369 propostas aprovadas), entrada, financiado e prazo. Não combinar melhorias de entrada/prazo nem alterar corte nesta fase. Essas combinações permanecem fase 27.

Proposta de duas famílias: (a) taxa uniforme para todos os aprovados; (b) mudar a taxa de uma faixa de score de cada vez, scores 4 a 10, mantendo 1,6% nos demais. Sem recorte por LTV na família por score; é desenho diferente do grupo D022/D023 e deve ser acordado. A família uniforme testa alcance da meta de carteira; a família isolada identifica contribuição e comportamento por faixa sem buscar combinações de sete preços simultaneamente.

Grade técnica inicial: 0% a 3,5% a.m., inclusive, em passos de 0,1 ponto percentual (36 taxas, incluindo 1,6% e 1,8%). Zero é controle de limite inferior, não recomendação comercial. Usar inteiros para representar unidades de taxa e evitar erro de comparação com o teto. A grade cobre o intervalo mas não prova o ótimo contínuo: não presumir monotonicidade ou buscar raiz única. Reportar todas as regiões viáveis na grade e a menor taxa testada que atende à meta, se houver. Eventual refinamento deve ser registrado, sem prometer precisão não testada.

## Resposta de aceite proposta — F26-01

Definir `x = (taxa_ofertada − 0,016) / 0,002`. Para cada aceite de referência `q0` de 50%, 70% e 90%, testar queda relativa `d` de 0%, 5%, 15% e 30% por aumento de 0,2 p.p. mensal:

`q = min(1, q0 × (1 − d) ** x)`.

Taxa maior reduz aceite, taxa menor aumenta aceite até 100%; d=0 é controle sem resposta. Aplicar os estresses D021 após essa resposta, registrando saturações antes do estresse. Propostas cuja taxa não muda mantêm q0. A mesma função vale nas duas famílias; não escolher elasticidade para favorecer uma candidata.

Exemplo com q0=70% e d=15%: 1,6% preserva 70%; 1,8% gera 59,5%; 2,0% gera 50,575%. A resposta a descontos e a composição ao longo de toda a faixa são hipóteses novas, não decorrências empíricas de D022. Registrar aceite, volume e mix por score para cada cenário.

## Risco e seleção adversa propostos — F26-02

Recalcular parcela, comprometimento e PD com pipeline congelado; renda ausente continua ausente. Manter controle de PD original. EAD/LGD não mudam porque seus determinantes permanecem iguais; verificar essa invariância e recalcular EL com a PD do cenário.

Em cada tratamento de PD, comparar ausência de agravamento adicional com sensibilidade:

`multiplicador_adverso = 1,10 ** max(x, 0)`.

Aplicar apenas nas operações com taxa maior que 1,6%; não atribuir benefício causal de PD ao desconto. Impor domínio de probabilidade em 1 com contagem explícita de saturações, antes e depois do estresse PD×1,10. Essa função generaliza a hipótese pontual de D022 e exige concordância nova. É um estresse de risco dos aceites, não identificação de seleção adversa individual; pode duplicar sinais do modelo, por isso controle sem multiplicador é obrigatório.

## Avaliação e controles

Preservar Price, CET=juros sem custos adicionais, ambos horizontes D021, estresses PD×1,10/LGD+5 p.p./aceite×0,90 e combinado, fórmula vigente de ROI e guardrails em 12 meses e no prazo total. Reproduzir as 30 referências D021 no mesmo avaliador. Quaisquer mudanças técnicas de implementação devem conferir equivalência antes de comparar preços.

Começar com valores esperados para toda a grade, salvando métricas de carteira, por score e condições por operação. Excluir do ranking de políticas válidas toda violação; manter tais resultados no relatório para mostrar fronteira e inviabilidade. Não interpretar contagem de cenários como probabilidade de sucesso real.

Só depois da leitura da grade, especificar o conjunto de candidatas a verificar em simulações pareadas de 2.000 carteiras/seed 42, com critérios de seleção e distinção entre busca e verificação registrados. Não apresentar intervalo simulado para toda a grade se apenas subconjunto for simulado, nem tratar a confirmação sob as mesmas hipóteses como validação fora da amostra. Nenhuma simulação adicional foi autorizada com parâmetros ainda ausentes.

Passos 296 e 307–314 exigem concordância com F26-01/F26-02 e execução verificada. Passo 315 exige decisão após os resultados. Nenhum item foi marcado concluído durante a preparação. Modelo, raw, PDs de B, benchmark e resultados anteriores permanecem preservados. F21/F23 e viés de seleção continuam explícitos.
