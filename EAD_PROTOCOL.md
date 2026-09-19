# Fase 17 — Protocolo de implementação da EAD

Status: P17-01 adotada por D012 após autorização do usuário para escolher a convenção. Fase 17 implementada e verificada em 2026-09-15; 20 fatores oficiais conferidos e 44 testes aprovados. F5-06 resolvida quanto ao tratamento de LTV; não representa confirmação externa do professor.

## Regras já definidas

Fonte: `data/raw/AutoCred_parametros_ead_lgd.xlsx`, aba Fator_EAD e Leia-me, com conteúdo extraído em `outputs/audit/source_documents.json`. A integridade dos arquivos brutos foi conferida contra o manifesto da fase 15.

`valor_financiado = valor_bem - valor_entrada`

`EAD = fator_EAD(prazo_meses, LTV) × valor_financiado`

| Prazo | até 60% | 60% a 70% | 70% a 80% | 80% a 90% | acima de 90% |
| --- | ---: | ---: | ---: | ---: | ---: |
| 24 | 0,980 | 1,003 | 0,995 | 1,013 | 1,002 |
| 36 | 1,021 | 1,015 | 1,020 | 1,015 | 1,027 |
| 48 | 1,024 | 1,027 | 1,032 | 1,040 | 1,038 |
| 60 | 1,040 | 1,037 | 1,041 | 1,040 | 1,042 |

Preservar os 20 valores oficiais, inclusive fatores acima de 1 e pequenas reduções entre faixas. Não impor monotonicidade artificial nem reestimar parâmetros. As tabelas foram fornecidas pelo desafio; sua origem inclui safras 2022–2024, portanto a análise econômica que as utiliza não constitui backtest estritamente prospectivo em 2024. Isso não muda o split do modelo de PD.

F5-05 trata da reprodução de EAD realizado contrato a contrato a partir de saldo e mês do default. Essa reprodução não é requisito desta fase: a fórmula prospectiva do desafio é a multiplicação pelo fator tabelado.

## Convenção P17-01 adotada — fronteiras superiores inclusivas

- LTV ≤ 60%: primeira coluna.
- 60% < LTV ≤ 70%: segunda coluna.
- 70% < LTV ≤ 80%: terceira coluna.
- 80% < LTV ≤ 90%: quarta coluna.
- LTV > 90%: quinta coluna.

“Até 60%” e “acima de 90%” esclarecem as extremidades. Os títulos internos não esclarecem inequivocamente se 70% e 80% pertencem à faixa que termina ou à que começa nesses valores. P17-01 propõe uma convenção consistente; não é apresentada como confirmação do professor.

Exemplo: financiamento de R$ 50.000 em 24 meses com LTV exatamente 70%. P17-01 usa fator 1,003 e EAD de R$ 50.150. Atribuir à coluna seguinte usaria 0,995 e EAD de R$ 49.750. A decisão afeta valores monetários e precisa ser explícita.

## Verificações previstas após decisão

Confrontar todos os 20 fatores com a fonte; testar cada fronteira exata e seus valores imediatamente abaixo/acima; rejeitar prazo não tabelado e entradas inválidas; verificar recálculo de valor financiado, LTV e EAD após alteração de entrada/prazo. Não arredondar valores apenas para forçar enquadramento. Aplicações históricas devem explicitar a precisão do LTV fornecido; cenários de novas condições devem usar o LTV calculado dessas condições e não manter o anterior.

Passos 248–254 concluídos. Implementação em src/ead_lgd.py; reprodução por scripts/run_ead.py; resultados em outputs/ead/EAD_REPORT.md. Nenhuma regra de aprovação ou precificação foi definida. LGD permanece na fase 18, seguida de perda esperada e retomada econômica da fase 16.
