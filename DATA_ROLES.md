# AutoCred — Papel das bases e limites de generalização

Registro da fase 4 (passos 78–88), em 2026-09-13. As definições abaixo formalizam o planejamento fornecido e as seções 5–8 de `AGENTS.md`. Não são conclusões obtidas por auditoria dos arquivos.

## Uso das bases (passos 78–80)

| Base e arquivo em data/raw/ | Papel estabelecido | Uso previsto |
| --- | --- | --- |
| A — `base_A_autocred_base_desenvolvimento.csv` | Base principal de desenvolvimento de PD | Treino, comparação e validação temporal na própria A, com target `default_90_12`. Referência inicial: treino em 2022–2023 e validação em 2024. Transformações aprendidas somente no treino. |
| B — `base_B_autocred_base_teste_modelo.csv` | Base out-of-time, sem target disponível para desenvolvimento | Aplicação posterior do pipeline já decidido e congelado, com geração das PDs. Diagnóstico de drift conforme as restrições abaixo. |
| C — `base_C_autocred_base_politica.csv` | População de propostas para aplicação da política | Aplicação da PD e avaliação das condições ofertadas, aceite e carteira resultante pelo simulador, nas etapas correspondentes. |

O termo “teste” no nome do arquivo B não autoriza utilizá-lo na escolha do modelo. O papel de C é avaliar a política sobre propostas; não constitui, por si só, evidência de desempenho preditivo com target observado.

## Seleção histórica e população (passos 81–85)

Conforme a definição do projeto, A e B contêm apenas clientes aprovados pela política anterior. Ambas têm viés de seleção: os dados históricos observados estão condicionados à aprovação antiga e não representam automaticamente todas as propostas possíveis.

Em A, a avaliação temporal mede desempenho dentro da população histórica aprovada. Em B, a mudança de período não elimina a seleção da política antiga; além disso, sem target disponível não se calculam métricas observadas de discriminação ou calibração.

C representa uma população de propostas diferente em seu processo de seleção da população histórica aprovada. Não se presume que suas distribuições sejam iguais às de A ou B. A magnitude e a localização de diferenças estatísticas ainda não foram medidas; são objetos da auditoria e da análise de drift.

A política anterior não será tomada como regra ótima nem a aprovação histórica será usada como substituto do target de default. Não há, nesta fase, decisão de introduzir método de correção de viés, inferência sobre recusados ou nova fonte de dados.

## Proteção da Base B (passos 86–87)

A Base B não pode ser usada para tuning, escolha de hiperparâmetros, escolha entre modelos, seleção de features ou decisões destinadas a melhorar artificialmente o modelo. Nenhum preprocessing ou calibração pode ser ajustado em B.

As inspeções de B nas fases 5–6 têm finalidade diagnóstica: descrever qualidade, compatibilidade e mudança de população. Seus resultados não podem orientar a seleção ou adaptação do modelo. Uma incompatibilidade que impeça aplicação deve ser registrada e discutida antes de qualquer mudança; não autoriza refazer silenciosamente o pipeline com base em B.

O pipeline final será aplicado a B somente depois de fixados modelo, preprocessing e calibração na fase 15. Não há desenvolvimento supervisionado nem validação observada de PD em B sem target disponível.

## Extrapolação A → C (passo 88)

Desempenho histórico em A não garante o mesmo desempenho em C. Propostas de C podem ocupar regiões com pouca ou nenhuma representação no histórico aprovado, tornando incerta a qualidade das PDs nessas regiões.

As análises previstas devem comparar distribuições e identificar regiões pouco representadas, registrando limitações de cobertura. Diferença de distribuição é um diagnóstico de mudança de população; isoladamente não quantifica perda de desempenho nem demonstra que o modelo falhou.

Não se define aqui corte de rejeição, correção de PD ou tratamento especial para essas propostas. Caso a análise indique necessidade de ação, a decisão deverá ser explícita, com justificativa e avaliação econômica. A política deve continuar respeitando os guardrails.

## Pendências abertas

| ID | Conferência ou decisão pendente | Quando resolver |
| --- | --- | --- |
| F4-01 | Conferir datas efetivas, disponibilidade e codificação do target e correspondência dos arquivos com seus papéis no dicionário e nos dados. | Auditoria da fase 5, antes de implementar o split. Divergências devem ser sinalizadas, sem redefinição silenciosa. |
| F4-02 | Conferir no material oficial o escopo de propostas de C e os critérios de seleção histórica de A/B; registrar detalhes não fornecidos. | Entendimento e auditoria dos dados. As definições acima permanecem fundamentadas no planejamento, não em verificação empírica. |
| F4-03 | Medir drift e cobertura A → B e A → C; definir e justificar critérios de diagnóstico quando necessários. | Fase 6. Não há limiares de drift ou extrapolação escolhidos nesta fase. |
| F4-04 | Decidir eventual tratamento de propostas fora da cobertura histórica, se esse problema for identificado. | Após evidência de extrapolação e antes da decisão de política afetada; solicitar input do usuário para decisões de negócio. |

## Evidência de conclusão

Os três papéis foram definidos, a seleção histórica e suas limitações foram registradas para A e B, a distinção de população de C foi explicitada sem inventar resultados de drift, e as proibições de uso de B foram confrontadas com a constituição. Os passos 78–88 estão concluídos no escopo de definição da fase 4. Auditoria, modelagem e análise de drift permanecem pendentes.
