# Versionamento local e estados do modelo

O Git registra código, notebooks, documentação, testes, configurações e relatórios/manifestos textuais de outputs. Bases, modelos binários, CSVs, gráficos, backups e ambiente virtual permanecem no disco, fora do Git, e precisam de backup separado. Hashes permitem conferir integridade, mas não recuperam arquivos perdidos.

Nenhum repositório remoto é necessário. Um commit é uma versão explicitamente registrada; salvar um arquivo no editor não cria um commit. Revisar `git status` e `git diff --staged` antes de registrar alterações. Usar tags para identificar milestones verificadas. O primeiro registro representa o estado atual após a fase 15, não reconstrói commits das fases anteriores.

## Três estados de governança

1. **Candidato:** procedimento em comparação. Configuração e resultados são registrados; pode ser substituído conforme critérios de seleção definidos.
2. **Congelado para experimentos:** versão escolhida e preservada para alimentar score e política. Modelo, preprocessing e decisão sobre calibração ficam fixos; uma revisão exige justificativa explícita e nova versão. Este é o estado atual do CatBoost (D009, fase 15).
3. **Aprovado para entrega:** versão integrante de uma solução verificada contra os requisitos do desafio, com política, guardrails, arquivos de entrega e limitações revisados. Ainda não atingido. Não significa aprovação para produção bancária real.

O nome `models/pd_model.pkl` indica o modelo final da etapa de modelagem desta versão; não implica que toda a solução esteja aprovada para entrega. A passagem do segundo ao terceiro estado não exige necessariamente retreinamento: exige concluir e verificar as etapas econômicas e de entrega.

## Rotina sugerida

- Registrar commits após mudanças coerentes e verificadas, incluindo documentação das decisões.
- Identificar milestones com tags; não mover uma tag existente para ocultar revisões.
- Preservar manifestos com dados, código, ambiente e configuração dos experimentos.
- Manter backup externo dos arquivos excluídos e do próprio histórico Git.
- Antes de restaurar uma versão, conferir quais alterações locais serão substituídas.
