# Fase 24 — Proposta de experimento com entrada

Status: desenho experimental aprovado explicitamente pelo usuário em 2026-09-16 (D022). Executar comparações, sem adoção automática de vencedora. Configuração em configs/simulation/entry_experiments_v1.json; referência D021 preservada.

## Benchmark e finalidade

Benchmark v1 é referência preservada, não restrição à política final. CET, prazo, entrada e corte permanecem variáveis de decisão futuras. Comparar ROI anualizado do desafio, resultado em reais, volume e inadimplência, respeitando guardrails. Não equiparar maximização de ROI a maximização de lucro absoluto. Nenhum resultado oficial será prometido.

## Desenho proposto antes dos resultados

- Grupo inicial: aprovados com scores de referência 4–6 e LTV desejado acima de 80%. Scores 4–6 são as três faixas de maior risco entre os aprovados do benchmark; 80% é fronteira das tabelas oficiais. É recorte experimental, não limiar ótimo comprovado.
- Comparar benchmark; entrada acrescida em 5 pontos percentuais do valor do bem; entrada acrescida em 10 pontos; e alternativa somente de preço, CET de 1,8% a.m. nesse mesmo grupo. Preservar prazo e decisões de aprovação para isolar mecanismos. Aumentar entrada em 5 p.p. significa, por exemplo, passar de 15% a 20% do bem, não aumentar R$15 mil em 5%.
- Recalcular financiado, LTV, EAD, LGD, parcela, comprometimento e PD predita sob oferta com o pipeline congelado. Manter score de referência e não permitir ciclo score/oferta. Registrar PD que subir ou LGD não monotônica; não forçar melhora, clipping ou edição de tabelas.
- A resposta do modelo às condições não prova efeito causal. Avaliar tanto PD reestimada quanto controle com PD de referência mantida. Não acrescentar outro desconto causal de PD por entrada.

## Hipóteses comportamentais aprovadas

Os mesmos aceites de referência 50%, 70% e 90% serão mantidos. A queda relativa adicional de aceite será de 5%, 15% ou 30% a cada 5 p.p. adicionais de entrada. Para 10 p.p., aplicar duas vezes o mesmo fator. Na alternativa de preço, usar quedas relativas de 5%, 15% ou 30% para os 0,2 p.p. adicionais de taxa mensal. Esses cenários não afirmam equivalência real entre mudanças de entrada e taxa.

Na alternativa de preço, testar separadamente ausência de agravamento adicional e acréscimo de 10% à PD reestimada como sensibilidade à seleção adversa. Não afirmar identificação desse efeito, nem acrescentá-lo ocultamente ao modelo. Registrar saturações em 1. Interações podem duplicar sinais já capturados pelo modelo; controle sem multiplicador permanece obrigatório.

Quedas de aceite são hipóteses de resposta de clientes, não parâmetros inferidos do treino ou do professor. Reavaliar benchmark e alternativas no mesmo avaliador, nos dois horizontes de D021, preservando cenários desfavoráveis e pareamento dos sorteios por ID. Extender avaliador a aceites diferentes por contrato exige testes de agregação ponderada pelo aceite; a média simples de PD deixa de representar a carteira esperada.

## Critérios e limites

Apresentar ganho/perda incremental de ROI, resultado em reais, volume e defaults; quatro guardrails para cada cenário. Nenhuma vencedora será escolhida só pelo melhor cenário. Entrada menor financiada pode reduzir perda e receita simultaneamente, enquanto o aceite pode reduzir originação.

Passos 296–297: distinguir comparação de preço (1,8% versus 1,6%) de cálculo de taxa necessária para um objetivo financeiro. Não derivar taxa ótima de EL isolada. Uma busca por taxa que atinja ROI 15% deverá ter cenário/objetivo explícitos e verificar todo o intervalo permitido, pois o modelo pode ser não monotônico. Caso o preço seja estudado integralmente na fase 26, registrar a transferência desses itens antes de marcar a fase 24 concluída; sem inventar taxa necessária.

O passo 296 (taxa necessária para objetivo financeiro) será executado na fase 26, com busca e reação a preço explícitas. O passo 297 permanece nesta fase como comparação ao preço de 1,8%, sem chamá-lo de taxa necessária ou ótima.

Avaliação esperada: grade completa nos dois horizontes, três aceites, três respostas, dois tratamentos de PD e cinco estresses D021; seleção adversa adicional somente no grupo da alternativa de preço. Simulações pareadas: 2.000 carteiras/seed 42, todos os tratamentos, ambos horizontes, para aceite de referência 70% nos estresses base/combinado e 50% no combinado. Subconjunto computacional definido antes dos resultados; não se apresentam intervalos simulados para os cenários avaliados apenas por valores esperados.

Registrar limitações F21/F23 já existentes e manter benchmark v1 intacto. Preservação da referência não impede otimização nas fases seguintes; as candidatas são novos arquivos/versionamentos.
