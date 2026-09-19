# AutoCred — Restrições da política

**Atualização D020:** guardrails executáveis implementados na fase 22. Nas ofertas atuais de C, aprovação 47,38% e CET 1,6% passam; inadimplência e volume originado permanecem indisponíveis até cenários da fase 23, mantendo `AVALIAÇÃO INCOMPLETA`. Não equiparar diagnóstico em A a conformidade em C; não substituir perdas de 12 meses por perdas de toda a vida do contrato sem hipótese explícita. Ver outputs/guardrails/GUARDRAILS_REPORT.md e C_POLICY_PROTOCOL.md.

## Atualização vigente — D016 (2026-09-15)

Decisão aprovada pelo usuário: o projeto completo será entregue antes dos gabaritos. Trabalhar com a hipótese documentada de simulador oficial reservado à avaliação posterior; sua disponibilização não bloqueia a entrega. A mensagem original está em data/raw/documentacao/mentoria_autocred_2026-09-12.txt; análise em outputs/ENUNCIADO_REVIEW.md.

- Aprovação = propostas aprovadas pelo banco / total de propostas recebidas. Aceite é decisão posterior do cliente; não entra nesse numerador. Erros de dados e duplicatas devem ser sinalizados antes da avaliação, sem exclusão silenciosa para melhorar a taxa.
- Aceite entre aprovados = ofertas aceitas / ofertas aprovadas, quando definido. Aprovação, aceite e conversão total são métricas distintas. Zero aprovados não permite calcular aceite condicional.
- Originação depende das ofertas aceitas; valor aprovado não é volume originado.
- Teto de 0,035 refere-se ao CET mensal. Conferir todas as ofertas aprovadas antes da entrega; não equiparar CET a taxa de juros sem hipótese explícita de custos. Não truncar violações silenciosamente.
- Inadimplência de 8% refere-se a contratos, não a ponderação monetária. Antes da entrega, volume, inadimplência e ROI serão avaliados sob cenários identificados; conformidade nesses cenários não comprova resultado oficial.
- Manter os quatro guardrails em todos os cenários. Identificar cada violação, sem relaxar limites nem escolher cenários apenas para fazê-los passar.
- ROI anual informado: [(juros recebidos − perda realizada) / volume financiado] / prazo médio em anos. Hipóteses sobre juros, perdas e ponderação devem ser explicitadas antes da implementação. A meta de 15% ao ano não é automaticamente um quinto guardrail.
- F2-01 esclarecida quanto ao denominador; F2-02 quanto à periodicidade/CET; F2-03 quanto à contagem de contratos; F2-04 quanto à dependência do aceite. Tratamentos de dados, convenções operacionais e F2-05 permanecem a definir no protocolo de cenários. Não esperar gabaritos para resolvê-los como hipóteses de trabalho.

Os registros abaixo preservam o estado documental da fase 2; esta atualização complementa e supera a antiga ausência de enunciado.

Registro da fase 2 (passos 26–35), em 2026-09-13. Fonte dos limites: `AGENTS.md`, seção 18, e roadmap fornecido pelo usuário.

## Limites obrigatórios

| Passo | Restrição | Comparação obrigatória |
| --- | --- | --- |
| 26 | Aprovação mínima de 35% | `approval_rate >= 0.35` |
| 27 | Taxa máxima de 3,5% | `max_rate <= 0.035` |
| 28 | Inadimplência máxima de 8% | `default_rate <= 0.08` |
| 29 | Volume originado mínimo de R$ 40 milhões | `originated_volume >= 40_000_000` |

Os quatro limites são restrições duras e devem ser satisfeitos simultaneamente (passo 30). A igualdade com o limite é permitida. ROI elevado não compensa o descumprimento de qualquer restrição. Nenhum limite pode ser removido, flexibilizado ou reinterpretado sem decisão explícita.

As taxas nas comparações são proporções, e o volume é expresso em reais. A periodicidade da taxa de juros e as definições operacionais das métricas ainda precisam ser conferidas no enunciado; não são presumidas neste documento.

## Plano das validações automáticas (passos 31–34)

Os testes abaixo serão implementados em `tests/test_policy.py` na etapa correspondente. São casos sintéticos para verificar as comparações, sem representar resultados do projeto. Em cada teste isolado, manter as outras três métricas dentro dos limites.

| Validação | Caso que passa | Fronteira que passa | Caso que falha |
| --- | --- | --- | --- |
| Aprovação mínima | `0.36` | `0.35` | `0.3499` |
| Taxa máxima | `0.034` | `0.035` | `0.03501` |
| Inadimplência máxima | `0.07` | `0.08` | `0.08001` |
| Volume mínimo | `41_000_000` | `40_000_000` | `39_999_999.99` |

Planejar também testes conjuntos: as quatro métricas exatamente no limite devem passar; a violação isolada de cada limite deve invalidar a política; múltiplas violações devem ser identificadas. Os testes devem impedir que arredondamento apenas para apresentação transforme uma violação em aprovação.

Dados ausentes, não finitos ou métricas não calculáveis não comprovam conformidade. A validação deverá impedir que esses casos recebam indicação de política válida e informar o problema de cálculo. A definição de sua representação técnica fica para a implementação.

## Resultado da avaliação (passo 35)

Uma política que viole qualquer limite será marcada como `POLÍTICA INVÁLIDA`. A avaliação deverá registrar a métrica, o valor calculado, o limite e as violações encontradas, permitindo explicar a rejeição. Somente uma política com as quatro verificações satisfeitas poderá ser considerada válida quanto a estes guardrails; isso não dispensa as demais avaliações do projeto.

As validações deverão ocorrer após cada simulação na etapa de integração. Nesta fase, os testes estão planejados; não foram implementados ou executados.

## Pendências abertas

Todas as pendências abaixo estão abertas e devem ser resolvidas com o enunciado ou com decisão explícita do usuário caso o enunciado seja omisso. Nenhuma altera os limites já fixados.

| ID | Definição a confirmar | Momento em que bloqueia o avanço |
| --- | --- | --- |
| F2-01 | Denominador da aprovação e tratamento de propostas duplicadas, inelegíveis ou sem oferta. | Antes de implementar o cálculo de `approval_rate`. |
| F2-02 | Periodicidade e convenção da taxa de 3,5%; universo sobre o qual calcular a taxa máxima (ofertas, aprovados ou contratos originados). | Antes de implementar precificação e `max_rate`. |
| F2-03 | Definição da inadimplência no simulador: horizonte, população, ponderação e uso de resultado simulado ou probabilidade esperada. Não equiparar silenciosamente essa métrica à média de PD. | Antes de implementar `default_rate`. |
| F2-04 | Definição e horizonte do volume originado, incluindo como o aceite determina os valores considerados. | Antes de implementar `originated_volume`. |
| F2-05 | Convenções oficiais de precisão e arredondamento no cálculo das métricas. Não adotar tolerância que relaxe os guardrails. | Antes de implementar a avaliação final dos limites. |

O enunciado ainda não está disponível no projeto. Essas pendências impedem uma validação operacional de política, mas não o registro dos limites e o planejamento dos testes exigidos pela fase 2.

## Evidência de conclusão

Os quatro valores e operadores foram confrontados com a constituição; os quatro testes têm casos de sucesso, fronteira e violação; a regra de invalidade simultânea foi explicitada; as definições ainda ausentes foram registradas. Assim, os passos 26–35 estão concluídos no escopo documental e de planejamento da fase 2. Nenhuma política foi simulada ou aprovada.
