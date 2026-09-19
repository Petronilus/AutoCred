# Fase 14 — Protocolo proposto para calibração

Data: 2026-09-15. Status: P14-01 aprovado pelo usuário ("Sim, vamos testar") e executado. Sigmoid e isotonic não melhoraram Brier e log-loss em 2023H2; o protocolo selecionou o controle sem recalibração. F14-01 resolvida por D008: fase encerrada como calibração testada e não adotada, mantendo o benchmark original como candidato, sem comprovar aptidão econômica. [Resultados](outputs/calibration/CALIBRATION_REPORT.md).

## Por que há uma decisão antes do ajuste

O CatBoost da fase 12 foi treinado em todo 2022–2023. Usar suas previsões nesses mesmos contratos para ensinar um calibrador introduziria otimismo: os rótulos já participaram do treinamento do classificador. 2024 está reservado à avaliação. A documentação do scikit-learn exige separar os dados que ajustam o classificador dos que ajustam o calibrador.

## P14-01 — divisão temporal simples e reproduzível

| Uso | Período de A | Contratos | Defaults |
| --- | --- | ---: | ---: |
| Ajustar preprocessing e CatBoost | 2022 | 3.380 | 293 |
| Ajustar calibradores candidatos | janeiro–junho/2023 | 1.683 | 161 |
| Comparar métodos antes de olhar 2024 | julho–dezembro/2023 | 1.607 | 133 |
| Avaliar o procedimento escolhido | 2024 | 3.330 | 239 |

1. Ajustar uma nova instância do mesmo CatBoost e preprocessing apenas em 2022, mantendo a configuração registrada e as decisões D001–D003.
2. Com esse classificador congelado, gerar previsões para os dois semestres de 2023. Ajustar Platt/sigmoid e isotonic apenas no primeiro semestre. Manter a opção sem calibração como controle.
3. Comparar as três opções no segundo semestre: log-loss como critério principal, Brier como verificação adicional, discriminação e curva como diagnósticos. Uma melhora apenas visual não será aceita. Se Brier e log-loss apontarem direções conflitantes, registrar e discutir o trade-off antes de escolher. Se nenhum calibrador melhorar ambos contra o controle, manter sem recalibração como resultado possível.
4. Após escolher o método em 2023, reajustar somente o calibrador escolhido usando todo 2023 (3.290 contratos, 294 defaults), ainda com o classificador treinado em 2022 congelado. Se o controle vencer, não ajustar calibrador.
5. Avaliar em 2024, comparando antes/depois sobre exatamente o mesmo classificador de 2022. Comparar também com o benchmark antigo, mas distinguir a mudança de período de treino do efeito da calibração. Não escolher outro método por resultado de 2024.

## Trade-off que exige confirmação

O classificador passa a aprender com 3.380 contratos, em vez dos 6.670 do benchmark. Isso preserva uma separação clara, mas pode alterar discriminação e estabilidade. Portanto, o AuROC anterior de 0,7464 não é garantido no novo procedimento. O benchmark antigo permanece como referência; não será sobrescrito.

Não ajustar novamente o classificador em todo 2022–2023 e reutilizar automaticamente esse calibrador: isso mudaria a distribuição das previsões que ele aprendeu a corrigir. Alternativas com previsões temporais fora do fit em múltiplas janelas existem, mas exigem protocolo mais complexo; não serão introduzidas silenciosamente.

O método isotônico é mais flexível e pode produzir empates, alterando AuROC. O número de defaults importa, além do total de contratos; 161 defaults para ajuste inicial não garante qualidade. A avaliação deve reportar contagens por faixa e limites de generalização.

2024 continua sendo uma validação de desenvolvimento já consultada, não um teste final independente. A análise continua retrospectiva por safra, com a limitação de maturação do target registrada em F8-01. B/C não participam do protocolo.

## Critérios de encerramento

Não marcar melhoria de calibração ou aptidão econômica como demonstradas apenas porque o código executou. Se o procedimento não melhorar de forma útil, registrar o resultado e decidir o próximo passo; não forçar a conclusão dos passos 221–223.

Fonte: [scikit-learn — calibração de probabilidades](https://scikit-learn.org/1.8/modules/calibration.html), particularmente a separação entre ajuste do classificador e do calibrador.
