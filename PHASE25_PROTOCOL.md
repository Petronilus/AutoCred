# Fase 25 — Preparação e pendências

Status em 2026-09-17: desenho abaixo autorizado pelo usuário após explicação dos cenários de aceite (D023). A fase 24 está concluída no commit `4c8fc61`. Autorização para comparar não adota política nova.

## Diagnóstico inicial

O grupo de referência da fase 24 contém 542 propostas aprovadas, com score 4–6 e LTV desejado acima de 80%: 102 com prazo de 24 meses, 124 com 36, 195 com 48 e 121 com 60.

Mantendo Price, CET = juros de 1,6% a.m., ausência de custos adicionais e a fórmula de ROI do avaliador, mesmo receber todas as parcelas sem qualquer perda produz os seguintes ROIs anualizados:

| Prazo | ROI sem perdas |
| --- | --- |
| 24 meses | 10,6070% |
| 36 meses | 10,7753% |
| 48 meses | 11,0069% |
| 60 meses | 11,2609% |

Cálculo reproduzível: `interest_schedule(10000, 0.016, prazo).sum() / 10000 / (prazo / 12)`, usando `src.simulator.interest_schedule`. O principal escolhido apenas normaliza a operação. Não se trata de TIR ou de retorno composto. Sob essas hipóteses, alterar apenas o prazo entre essas opções não permite alcançar ROI de 15%, mesmo sem perdas. Isso não demonstra inviabilidade da meta sob outros preços ou sob o simulador oficial.

## Desenho autorizado

Comparar o benchmark com limites de 48 e 36 meses no mesmo grupo de referência. Manter entrada, taxa, score e decisão de aprovação; recalcular parcela, comprometimento, PD sob oferta, EAD e perda esperada. Preservar também o controle com PD original e os horizontes e estresses D021. A mudança de PD predita não identifica efeito causal.

Um limite de 48 meses alteraria 121 operações; um limite de 36 alteraria 316. Os demais contratos do grupo conservariam seus prazos.

**F25-01 — Resolvida para o experimento:** aceite de referência de 50%, 70% ou 90%; queda relativa de 0%, 5%, 15% ou 30% por 12 meses efetivamente encurtados. Fórmula por operação: `q = q_ref × (1 − queda) ** ((prazo_original − prazo_ofertado) / 12)`. Aplicar depois o estresse de aceite D021. Operações sem redução preservam o aceite de referência. São sensibilidades hipotéticas, não estimativas observadas. A autorização veio nesta conversa após a explicação com exemplos; não foi inferida de D022.

## Execução e critérios, definidos antes dos resultados

Configuração: configs/simulation/term_experiments_v1.json. Tetos candidatos de 48/36 meses para scores 4–6 e LTV desejado >80%; demais condições e decisões preservadas. Prazo tabelado mínimo de 24 meses; não estender prazo nem interpolar tabelas. As duas candidatas são alternativas separadas ao benchmark, não combinadas com a entrada da fase 24.

Reutilizar o avaliador D021/D022 e a construção de features sob oferta. PD reestimada e controle de PD de referência; nenhum multiplicador causal adicional por prazo. Recomputar EAD/LGD, parcela, comprometimento e EL12. LGD deve permanecer igual porque seus determinantes não mudam. Preservar NaN de renda/comprometimento. Registrar aumentos e quedas de PD, inclusive resultados desfavoráveis.

Grade esperada: 30 referências D021 mais 480 comparações (2 tetos × 2 tratamentos de PD × 4 reações × 2 horizontes × 3 aceites × 5 estresses). Conferir cada referência contra o output anterior.

Simulações pareadas: seed 42, 2.000 carteiras por cenário; ambos horizontes, todas as alternativas/PDs/reações, aceite 70% nos estresses base e combinado e aceite 50% no combinado. São 102 cenários incluindo 6 referências. Subconjunto computacional segue o desenho D022, definido antes de resultados; os demais cenários têm apenas valores esperados. Mesmo ordenamento de IDs e mesmos sorteios em cada comparação.

Comparar ROI anualizado, resultado acumulado em reais, juros, perdas, volume e defaults nos dois horizontes. O ROI utiliza a fórmula vigente com prazo médio ponderado pelo financiado aceito; não é TIR e não supõe reinvestimento após prazo menor. Os quatro guardrails continuam obrigatórios; apresentar separadamente a ambiguidade de horizonte F23-01. Nenhuma probabilidade real de sucesso pode ser inferida da contagem de cenários que passam.

Passos 301–305 dependem de execução e verificação. O passo 306 depende da interpretação e decisão de adoção: arquivos de candidatas não significam incorporação à política vigente. Não avançar automaticamente para fase 26. Preservar F21/F23 e dados/modelo/benchmark/outputs anteriores por hashes.
