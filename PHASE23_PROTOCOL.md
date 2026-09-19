# Fase 23 — Proposta de avaliação financeira do benchmark

Status: adotado explicitamente pelo usuário em 2026-09-16 (D021), após recapitulação das hipóteses. Modelo e benchmark preservados. Parâmetros abaixo são hipóteses, não estimativas do comportamento do professor. Reprodução em scripts/run_benchmark_finance.py; configuração versionada em configs/simulation/benchmark_v1.json.

## Escopo inicial

Avaliar apenas benchmark v1 em C, com condições e PDs da fase 21. Não otimizar preço, entrada, prazo ou corte nesta etapa. Aceite uniforme não serve para escolher condições futuras: antes dessas comparações será necessário modelar explicitamente reação e seleção adversa por cenário.

## Proposta de cenários e convenções

1. Aceite uniforme de 50%, 70% e 90% entre aprovados, independente do risco no cenário-base. São três hipóteses ilustrativas sem probabilidade atribuída; 70% é referência de apresentação, não previsão. Fixar estes percentuais antes de ver ROI/volume para evitar seleção oportunista de cenário.
2. Fluxo Price mensal postecipado, CET = juros sem custos adicionais conforme D019. Principal amortizado não é receita de juros. Não incluir captação/custos operacionais na fórmula do desafio, que os omite; não chamar o resultado de lucro líquido bancário.
3. Conversão da PD de 12 meses em risco mensal constante: h = 1 − (1 − PD12)^(1/12). Isso reproduz PD12 no horizonte de 12 meses. Estender o mesmo h por todo o prazo é hipótese de cenário, não conclusão do modelo. Comparar obrigatoriamente com cenário alternativo em que há defaults somente nos primeiros 12 meses, deixando explícito que o risco posterior zero é uma hipótese otimista.
4. Reconhecimento do evento antes do pagamento do mês; a partir desse evento não há mais juros recebidos. Essa convenção simplifica a cronologia dos 90 dias de atraso: não representa observação do mês de primeiro não pagamento. Meses sem evento recebem os juros do saldo contratual Price programado. Pré-pagamento, cura e recuperação posterior de juros não são modelados.
5. Perda por evento = EAD tabelada × LGD tabelada, aplicada uma única vez. Não substituir EAD oficial pelo saldo Price. Usar a tabela também em eventos após 12 meses é extrapolação econômica explícita: pode superestimar perdas tardias, especialmente perto do vencimento. O cenário restrito aos primeiros 12 meses mostra a dependência do resultado dessa extrapolação, sem calibrar tabela em C.
6. Prazo médio contratual em anos ponderado pelo valor financiado dos aceites. Não encurtar denominador quando há default. Reportar também o prazo médio simples para evidenciar a convenção. ROI = [(juros recebidos − perda) / volume financiado] / prazo médio em anos, como no enunciado.
7. Inadimplência do guardrail definida provisoriamente em 12 meses, por número de contratos, alinhada ao target 90/12. Reportar separadamente default acumulado no prazo total e se ele violaria 8%, pois o horizonte oficial do guardrail não foi explicitado. Não esconder a sensibilidade à interpretação.
8. Resumo determinístico por valores esperados (razões de somas esperadas identificadas, não esperança exata de ROI) e simulação de carteiras para observar distribuição do ROI, inadimplência e volume. Seed fixa, mesmos sorteios entre cenários comparáveis, defaults/aceites independentes entre clientes condicionalmente às PDs. Intervalos de simulação não representam incerteza estrutural nem calibração comprovada.
9. Estresses explícitos da constituição: PD12 × 1,10, LGD + 5 pontos percentuais e aceite × 0,90; individuais e combinado. Saturar probabilidades em 1 somente com registro da quantidade afetada, por ser domínio matemático, sem flexibilizar guardrail. Manter cenários desfavoráveis no relatório, sem excluir políticas reprovadas da evidência.

## Saídas propostas

Por cenário/horizonte: aprovação do banco, CET máximo, aceites, volume originado, default12, default no prazo, juros, perda, resultado financeiro, prazo médio e ROI anualizado. Quatro guardrails no resumo esperado e em cada carteira simulada. Registrar violações sem interromper a coleta dos demais cenários; impedir que um resultado inválido seja aceito como política válida.

Separar resultado esperado, distribuição sob sorteios e hipóteses não identificadas. Não declarar conformidade oficial. Versão do simulador, configurações, seeds, inputs e resultados persistidos. Execução: 2.000 carteiras por cenário, seed 42, 30 cenários (dois horizontes × três aceites × cinco condições de estresse). Essa quantidade é convenção computacional; não atesta precisão da hipótese econômica.
