# AutoCred — relatório técnico e defesa da política

Data da análise: 18 de setembro de 2026. Fontes: dois arquivos MD da raiz, dicionário e parâmetros em `bases/`, três bases CSV e exemplos em `entregaveis/`. Os arquivos originais foram preservados. Os hashes das bases constam em `diagnosticos/metricas.json`.

## 1. Decisão e resultados

A proposta aprova scores de 6 a 10, equivalentes a PD de referência inferior a 7,5%. As taxas variam de 2,25% a 2,60% ao mês. A entrada mínima é de 10% a 20%; o prazo máximo é de 60 meses, reduzido a 48 no score 6. Há 2.213 aprovações e 2.787 recusas.

O modelo selecionado é HistGradientBoosting calibrado por regressão logística. No teste temporal de julho a dezembro de 2024, obteve AuROC **0,7417**, KS **39,85%** e Brier **0,05720**. O intervalo bootstrap de 95% da AuROC é de 0,6893 a 0,7931. O teste tem 1.613 contratos e 105 defaults; a incerteza não é desprezível.

| Indicador | Favorável | Central | Adverso | Limite/meta |
|---|---:|---:|---:|---:|
| Aprovação | 44,26% | 44,26% | 44,26% | ≥35% |
| Contratos esperados | 1.849 | 1.561 | 1.175 | Sem mínimo de contagem |
| Aceite entre aprovados | 83,53% | 70,53% | 53,09% | Não informado |
| Volume contratado | R$ 67,21 mi | R$ 56,72 mi | R$ 42,65 mi | ≥R$ 40 mi |
| Default dos contratados | 4,75% | 5,89% | 7,72% | ≤8% |
| Taxa média mensal | 2,41% | 2,41% | 2,41% | Teto individual de 3,50% |
| ROI anualizado | 15,93% | 15,55% | 14,95% | >15% |

**A meta de ROI falha no cenário adverso.** Volume e default ficam próximos dos limites nesse cenário, sem garantia de cumprimento realizado. A aprovação e a taxa máxima são verificáveis diretamente nos CSVs; volume, inadimplência e ROI dependem de respostas e desfechos não fornecidos. Os cenários são razões entre valores esperados, não a esperança exata do ROI aleatório nem intervalos de confiança.

## 2. Auditoria dos dados e da pré-análise

| Base | Linhas | Colunas | Uso |
|---|---:|---:|---|
| A | 10.000 | 28 | Desenvolvimento e teste temporal interno |
| B | 3.000 | 23 | Escoragem para avaliação externa, sem alvo |
| C | 5.000 | 21 | Propostas para a política |

Os IDs são únicos. Em A, faltam 770 rendas, 1.207 tempos de emprego e 327 scores de bureau. Em B, são 258, 356 e 92; em C, 395, 580 e 175. Os realizados de A ficam ausentes nos adimplentes por definição e não são imputados para modelagem.

A pré-análise do Gemini está correta quanto à separação das bases, aos limites e às seis colunas indisponíveis na concessão. Ajustes importantes:

- A métrica oficial do modelo no enunciado é **AuROC**, com KS secundário. Gini = 2 × AuROC − 1 é equivalente para ordenação, mas não substitui o nome da métrica oficial.
- Não é possível calcular o desempenho real de B sem seu alvo. O teste interno usa apenas o final de A.
- `possui_avalista` contém **“Sim” e “Não”**, não números 0/1. O código aplica o ajuste de LGD a “Sim”.
- C contém `prazo_desejado_meses`; a política define o prazo máximo. Não há taxa contratada em C.
- O score não precisa ser decil. Foram adotados limites fixos de PD para estabilidade e interpretação.
- A condição de não disponibilidade está escrita “NÃO” no dicionário. A exclusão foi feita explicitamente, sem depender de comparação sensível a essa grafia.

Taxa de default por ano: 8,67% em 2022 (3.380 contratos), 8,94% em 2023 (3.290) e 7,18% em 2024 (3.330). A média geral é 8,26%. Os números não mostram aumento contínuo de inadimplência ao longo das safras disponíveis, ainda que o caso narre desempenho pior que o plano.

## 3. Variáveis e prevenção de vazamento

Entradas: valor financiado, LTV, prazo, idade do veículo, idade do cliente, renda, tempo de emprego, score de bureau, restrições ativas, consultas ao bureau, canal, ocupação, residência e avalista. São 14 campos originais e uma variável derivada: comprometimento de renda sob taxa de referência de **1,60% ao mês**.

O comprometimento de referência é calculado para **A, B e C da mesma forma**: parcela Price à taxa fixa dividida pela renda declarada ou pela mediana aprendida no treino. Essa taxa é uma convenção analítica próxima da taxa média histórica, não uma oferta de crédito nem uma estimativa de taxa de mercado em 2025. A transformação usa `log1p` para renda, valor financiado e tempo de emprego.

A taxa histórica de concessão, a parcela e o comprometimento originais não entram no modelo. A opção facilita transporte para C, que não tem taxa, e reduz dependência da política de preço antiga. Valor do bem, entrada e ano-modelo são redundantes com outras entradas. IDs e datas não são preditores; data define os cortes de validação. Idade do veículo usa diretamente o campo do dicionário, sem recalcular por ano-modelo.

Exclusões obrigatórias: `qtd_parcelas_em_atraso_12m`, `default_90_12`, `mes_default`, `ead_realizado`, `lgd_realizado` e `perda_financeira`. As colunas pós-concessão não são sequer selecionadas pelo transformador. Um teste alterou arbitrariamente todas essas colunas e confirmou previsões idênticas.

O Pipeline ajusta mediana, indicadores de ausência, escala e codificação categórica apenas no treino de cada janela. Categorias desconhecidas usam `handle_unknown='ignore'`. Não há balanceamento ou oversampling, preservando a prevalência para PD. A escala é desnecessária para árvores, mas permite compartilhar o pré-processamento entre candidatos.

## 4. Seleção e validação temporal

Foram comparadas sete configurações, sem busca extensa:

| Família | Configuração | AuROC média nas duas janelas |
|---|---|---:|
| Logística linear | C = 0,1 | 0,6775 |
| Logística linear | C = 1 | 0,6762 |
| Logística linear | C = 10 | 0,6760 |
| Logística com splines | C = 0,1 | 0,6959 |
| Logística com splines | C = 1 | 0,6950 |
| HistGradientBoosting | 7 folhas | **0,7309** |
| HistGradientBoosting | 15 folhas | 0,7287 |

Janelas de escolha: treino até junho de 2023 e validação em julho–dezembro de 2023; treino até dezembro de 2023 e validação em janeiro–junho de 2024. Escolha pela maior AuROC média, com Brier como desempate. O conjunto julho–dezembro de 2024 permaneceu reservado até a escolha final; não houve ajuste de hiperparâmetro após seu resultado.

Configuração vencedora: 200 iterações, taxa de aprendizado 0,05, máximo de 7 folhas, mínimo de 70 observações por folha, regularização L2 de 10 e semente 2026. `early_stopping=False` evita uma divisão aleatória interna para parada.

A calibração logística usa o logit das previsões fora do tempo em três semestres: 2023 H1, 2023 H2 e 2024 H1, sempre treinando o modelo-base em períodos anteriores. Intercepto do calibrador: 0,08786; inclinação: 1,05577. Como os registros de calibração também participaram da escolha da família, suas métricas não são consideradas teste independente.

| Avaliação | AuROC | KS | PD média | Default observado |
|---|---:|---:|---:|---:|
| Treino até junho de 2024, aparente | 0,8213 | 48,40% | 8,35% | 8,60% |
| Previsões temporais usadas na calibração | 0,7214 | 35,33% | 8,55% | 8,55% |
| Teste reservado 2024 H2 | **0,7417** | **39,85%** | **7,74%** | **6,51%** |

No teste reservado, a PD média excede o observado em 1,23 ponto percentual. A calibração melhora discretamente o Brier de 0,057208 para 0,057198. Não se recalibrou usando esse teste. O intervalo bootstrap usa 1.000 reamostragens de contratos, semente 2026; não cobre toda a incerteza de deriva futura ou dependência entre safras.

Após registrar os resultados, o modelo-base foi reajustado em toda A; o calibrador temporal foi mantido. Esse artefato final gera as PDs de B e C. Seu desempenho posterior ao refit não foi medido em B. Importância por permutação no teste, apenas como diagnóstico posterior, destacou idade do cliente, prazo, score de bureau e restrições. A importância não foi usada para nova seleção de variáveis.

O gráfico em `diagnosticos/modelo.png` reúne ROC, calibração por decil e distribuição de PD em C. Os CSVs preservam as previsões individuais que sustentam cada análise.

## 5. Regras da política e contrato do CSV

| Score | PD de referência | Decisão | Taxa mensal | Prazo máximo | Entrada mínima |
|---:|---|---|---:|---:|---:|
| 10 | 0% a <2% | Aprovar | 2,25% | 60 | 10% |
| 9 | 2% a <3,5% | Aprovar | 2,30% | 60 | 10% |
| 8 | 3,5% a <5% | Aprovar | 2,40% | 60 | 15% |
| 7 | 5% a <6% | Aprovar | 2,50% | 60 | 15% |
| 6 | 6% a <7,5% | Aprovar | 2,60% | 48 | 20% |
| 5 | 7,5% a <10% | Negar | — | — | — |
| 4 | 10% a <15% | Negar | — | — | — |
| 3 | 15% a <22% | Negar | — | — | — |
| 2 | 22% a <35% | Negar | — | — | — |
| 1 | 35% a 100% | Negar | — | — | — |

Limites inferiores inclusivos e superiores exclusivos, exceto 100%, incluído no score 1. Há 36, 651, 784, 382 e 360 propostas nos scores aprovados 10 a 6. O preço não redefine o score após a oferta, evitando decisão circular. Não foram aplicados cortes adicionais ocultos por renda, canal ou veículo.

`pd` no CSV é a probabilidade do modelo sob o pedido e a taxa de referência, **sem o multiplicador de estresse**. `prazo_meses` é interpretado como **prazo máximo da faixa**, conforme a tabela do template e o enunciado. No cálculo interno, prazo efetivo = mínimo entre prazo desejado e máximo; entrada efetiva = máximo entre entrada desejada e mínima. O valor financiado é recalculado como valor do bem × (1 − entrada efetiva). Campos de condições ficam vazios nas recusas, exatamente como no exemplo. Decisões são `APROVAR` e `NEGAR`; taxas e entradas são decimais.

O documento separado “Regras da Competição” é citado no enunciado, mas não foi recebido. É necessário confrontar com ele a interpretação de prazo máximo e quaisquer requisitos adicionais antes da submissão externa. Não se presume conhecer seu conteúdo.

## 6. Projeção financeira reproduzível

O simulador oficial, elasticidades e sorteios não estão disponíveis. Não foi feita tentativa de reproduzir ou descobrir coeficientes ocultos. A política é uma proposta de negócio com testes de sensibilidade ilustrativos. A Base C já estava disponível: a projeção é anterior aos desfechos, não anterior ao acesso às propostas.

Para cada aprovado, a prestação Price é `V × r / (1 − (1+r)^(-n))`. O cenário aplica probabilidade de aceite `a` e PD de cenário `p`. As variáveis das hipóteses são:

- `u = max((taxa_ofertada − 0,016) × 100, 0)`, prêmio de taxa em **pontos percentuais mensais**.
- `e = entrada_efetiva − entrada_desejada`, aumento de entrada em fração.
- `h = (prazo_desejado − prazo_efetivo) / prazo_desejado`, fração de encurtamento.
- `c = prestação / renda`, imputando mediana histórica se necessário.
- `o = 1` se bureau <460, restrições >2 ou LTV desejado >95%; caso contrário, zero. Esses limites são o suporte observado de A, não limites oficiais do desafio.

Fórmulas dos cenários:

```text
aceite = a0 × exp(− b_preco × u − b_entrada × e − b_prazo × h)
         × exp(− max(c − 0,45, 0))
logit(PD_cenario) = logit(PD_referencia) + ln(fator_mar_aberto)
                   + k_preco × u + o × ln(fator_fora_suporte)
```

| Hipótese | Favorável | Central | Adverso |
|---|---:|---:|---:|
| Aceite-base `a0` | 0,95 | 0,88 | 0,75 |
| Elasticidade ao preço | 0,12 | 0,22 | 0,35 |
| Elasticidade à entrada | 1,20 | 2,00 | 3,50 |
| Elasticidade ao encurtamento | 0,15 | 0,30 | 0,50 |
| Multiplicador de odds por mar aberto | 1,00 | 1,15 | 1,35 |
| Coeficiente do prêmio de taxa no logit | 0,10 | 0,20 | 0,35 |
| Multiplicador de odds fora do suporte | 1,10 | 1,25 | 1,50 |

**Todos esses números são hipóteses de sensibilidade não estimadas.** O efeito do comprometimento acima de 45% também é hipótese. O limiar não é um corte de aprovação e não certifica capacidade de pagamento.

Exemplo de interpretação central: aumento de 1 ponto percentual na taxa multiplica o aceite por exp(−0,22) = 0,803 e as odds de default por exp(0,20) = 1,221. Aumento de 10 pontos percentuais na entrada multiplica aceite por exp(−0,20) = 0,819. Os multiplicadores são explícitos para que o grupo possa discordar e refazer os cálculos.

Não se atribui benefício causal de PD à entrada adicional nem se presume que diminuir prazo reduza PD: a intensidade de ambos é desconhecida e a parcela pode aumentar. A entrada altera valor, LTV, EAD e LGD diretamente. Esse tratamento é prudencial, mas não elimina erro de modelo.

O EAD é fator da tabela × valor financiado efetivo. As faixas de LTV são ≤60%, (60%,70%], (70%,80%], (80%,90%] e >90%. As faixas de veículo são ≤2, 3–5, 6–8 e ≥9 anos. LGD = tabela − 0,061 se avalista “Sim”, limitada entre 0 e 1.

Para adimplentes, juros totais = prestação × prazo − principal. Para inadimplentes, calcula-se apenas o juro acumulado até cada mês de default de 1 a 12 e pondera-se pela distribuição fornecida, normalizada por sua soma. Receita esperada = `(1−p) × juros_adimplente + p × juros_default`. Principal recebido não é classificado como juro. Não se adicionam juros futuros dos inadimplentes.

```text
Volume = soma(aceite × valor_financiado)
Perda = soma(aceite × PD_cenario × EAD × LGD)
Juros = soma(aceite × receita_esperada)
Prazo_medio = soma(aceite × prazo_efetivo) / soma(aceite)
ROI_anual = ((Juros − Perda) / Volume) / (Prazo_medio / 12)
Default_carteira = soma(aceite × PD_cenario) / soma(aceite)
```

Prazo médio e default são ponderados por número esperado de contratos, não pelo volume. O ROI segue a definição simples do enunciado: sem TIR, desconto a valor presente ou custos de funding, capital, tributos e operação. O modelo considera default 90/12; não projeta eventos adicionais após 12 meses para sobreviventes, simplificação coerente com o exercício e inadequada como previsão completa de vida do crédito.

No cenário central, há R$ 33,05 milhões de juros esperados e R$ 2,265 milhões de perda esperada sobre R$ 56,724 milhões de volume. O prazo médio é 41,88 meses. O volume ofertado antes do aceite é R$ 80,985 milhões; se o aceite fosse uniforme, cerca de **49,39%** desse volume precisaria se converter para atingir R$ 40 milhões. Aceite seletivo por ticket pode mudar esse limiar de contagem.

## 7. Reconciliação dos parâmetros recebidos

Nos 826 defaults de A, o fator EAD realizado médio é 1,02881 e a LGD média é 69,58%. A multiplicação de EAD e LGD publicados aproxima a perda observada; o maior desvio absoluto é R$ 4,10, compatível com os números arredondados disponibilizados.

Há uma nuance na fórmula do EAD: usar saldo Price no próprio mês de default mais três prestações gera diferença absoluta mediana de R$ 1.286,17 contra a base. Usar o saldo no último pagamento, `max(mês_default − 3, 0)`, mais três prestações reconcilia com diferença mediana inferior a R$ 0,01 e máxima de R$ 0,034. Isso indica que os realizados preservam saldo antes das três parcelas em atraso. A projeção utiliza **as tabelas oficiais entregues**, evitando substituir a convenção por uma interpretação diferente da fórmula textual.

## 8. Riscos que a defesa precisa reconhecer

A média de bureau cai de 645,52 em A para 549,48 em C; a de restrições sobe de 0,625 para 1,695. A contém bureau mínimo 460 e no máximo duas restrições, enquanto C chega a bureau zero e cinco restrições. Árvores não extrapolam de forma causal o risco além do suporte. O ajuste por cenário apenas explicita prudência e não resolve inferência de rejeitados; não há rótulos dos antigos recusados para validar uma correção.

Entre os aprovados, 177 estão fora desse suporte, 182 não informaram renda e 220 têm comprometimento ofertado acima de 45% usando renda declarada ou imputada. A política entregue não contém exceções individuais além da tabela. Uma implantação real precisaria de verificação de renda, validação independente, avaliação de uso de idade e ocupação e monitoramento antes de expandir essa população. Estas são recomendações para implantação futura, não regras adicionais escondidas no CSV do desafio.

O default histórico caiu em 2024, mas não foi presumida continuidade dessa queda. A melhora pode resultar de composição e seleção, além de variação amostral. A estabilidade de A para B e C é limitada. O teste interno é pequeno e a calibração superestima a taxa média observada do último semestre.

O preço selecionado entrega pequena folga sobre 15%; seleção adversa mais forte, aceite pior ou defaults realizados acima da média podem eliminar a folga e acionar penalidades. Os três cenários não são uma distribuição de probabilidade sobre o resultado oficial.

## 9. Reprodutibilidade e validação

Os scripts estão em `src/`. `train.py` registra comparação, validação, calibrador, versões e hashes; `policy.py` produz condições e memória de cálculo de cada aprovado; `validate.py` verifica os arquivos; `build_document.py` preenche uma cópia do template sem modificar as demais partes OOXML.

Os testes passaram para: formato idêntico aos exemplos, 3.000 e 5.000 registros, IDs únicos na ordem de origem, probabilidades finitas em [0,1], score coerente com faixas, decisão e preço coerentes com a tabela, condições vazias nas recusas, teto de juros, reprodução do modelo salvo, invariância a campos de vazamento, limites exatos das faixas, amortização Price por cálculo mensal independente e reconciliação dos três cenários.

O relatório `diagnosticos/validacao_entregas.json` registra a verificação local. Nenhuma métrica da Base B ou ROI oficial da Base C foi inventado. Não houve submissão externa. A identificação do grupo e seus três responsáveis continuam a definir.

## 10. Roteiro curto para a defesa

1. Mostrar que o teste é posterior ao treino e que parcelas em atraso foram excluídas, apesar de seu poder preditivo aparente.
2. Explicar por que a AuROC 0,742 indica ordenação útil, mas não garante calibração ou retorno em mar aberto.
3. Ligar PD, EAD e LGD à perda e mostrar que juros de contratos inadimplentes param no default.
4. Defender corte de 7,5%, taxas até 2,60%, entrada moderada e preservação do prazo desejado dentro do máximo.
5. Diferenciar 2.213 aprovações de aproximadamente 1.561 contratações no cenário central.
6. Encerrar com a sensibilidade: 15,55% de ROI central e 14,95% adverso, sem prometer conhecer o simulador.
