# AutoCred — Project Constitution

Este arquivo contém regras permanentes para qualquer agente ou pessoa que modifique este repositório.

Estas regras têm precedência sobre conveniência, velocidade de implementação ou ganho aparente de performance.

## 1. Objetivo do projeto

O objetivo não é apenas construir um classificador.

A solução completa deve conectar:

`dados → PD → EAD → LGD → perda esperada → política → aceite → carteira → inadimplência → volume → ROI`

O projeto possui três blocos:

1. **PD / risco:** quem pode entrar em default?
2. **Economia:** quanto podemos perder caso ocorra default?
3. **Decisão:** quais condições tornam a operação economicamente aceitável?

Somente o primeiro bloco é fundamentalmente um problema de machine learning.

---

## 2. Princípio de implementação

Trabalhar incrementalmente.

Fluxo preferencial:

`conceito → hipótese → código → resultado → interpretação → decisão`

Não implementar grandes etapas futuras antes de as premissas necessárias estarem definidas.

Não tomar silenciosamente decisões metodológicas ou de negócio.

Quando existir uma decisão relevante ainda não resolvida:

* sinalizá-la;
* manter a implementação reversível;
* evitar transformá-la implicitamente em regra permanente.

---

## 3. Target

O target de PD é:

`default_90_12`

Não redefinir o target sem decisão explícita e documentada.

---

## 4. Leakage é proibido

Nunca utilizar informações indisponíveis no momento da concessão como features.

Colunas explicitamente proibidas:

```python
LEAKAGE_COLS = [
    "qtd_parcelas_em_atraso_12m",
    "mes_default",
    "ead_realizado",
    "lgd_realizado",
    "perda_financeira",
]
```

Essas colunas devem permanecer fora do conjunto de features.

Sempre que possível, proteger essa regra com testes automatizados.

Performance obtida por leakage é inválida independentemente das métricas alcançadas.

---

## 5. Validação temporal

A validação principal deve respeitar o tempo.

Referência inicial:

* treino: 2022–2023;
* validação: 2024.

Não utilizar split aleatório como principal evidência de generalização.

Nenhuma informação da validação pode ser utilizada para aprender preprocessing, imputação, transformação ou parâmetros do modelo.

---

## 6. Base B

A Base B é out-of-time e não possui target disponível para desenvolvimento.

É proibido utilizar a Base B para:

* tuning;
* escolha de hiperparâmetros;
* escolha de modelo;
* seleção de features;
* decisões destinadas a melhorar artificialmente o modelo.

A Base B deve permanecer uma aplicação posterior do pipeline já decidido.

---

## 7. Viés de seleção e população

As bases históricas A e B representam clientes aprovados pela política anterior.

Isso implica viés de seleção.

A Base C pode conter população diferente daquela observada nos dados históricos aprovados.

Nunca assumir automaticamente que performance histórica implica igual performance em C.

Análises de drift e extrapolação fazem parte da avaliação de risco do modelo.

---

## 8. Preprocessing

Todo preprocessing que aprende parâmetros dos dados deve ser ajustado somente no treino.

Isso inclui, quando aplicável:

* imputação;
* encoding;
* scaling;
* seleção orientada pelos dados;
* calibração;
* demais transformações aprendidas.

Preferir `Pipeline` ou estrutura equivalente que torne essa separação explícita e reproduzível.

Nunca imputar ou normalizar o conjunto completo antes do split.

---

## 9. Baselines antes de complexidade

Manter uma escada de comparação.

Referência:

1. `score_bureau` como baseline simples;
2. regressão logística como benchmark interpretável;
3. modelos boosted somente depois.

Um modelo mais complexo deve demonstrar ganho real e justificável sobre benchmarks mais simples.

---

## 10. Métricas

Não escolher modelo exclusivamente pelo maior AuROC.

Avaliar, conforme aplicável:

* AuROC;
* Gini;
* KS;
* calibração;
* estabilidade temporal;
* comportamento por safra.

Discriminação e calibração são propriedades diferentes.

Como a PD alimenta decisões econômicas, calibração é obrigatoriamente considerada.

---

## 11. PD e calibração

A saída relevante para a política é uma probabilidade de default economicamente utilizável.

Quando necessário, testar calibração apropriada, como:

* Platt scaling;
* isotonic calibration.

Não aplicar calibração apenas porque melhora visualmente uma curva.

Toda calibração deve respeitar a separação temporal e evitar leakage.

---

## 12. Score de risco

O score final utilizará referência de 1 a 10:

* 10 = menor risco;
* 1 = maior risco.

Quantis podem ser utilizados como ponto de partida, não como obrigação da solução final.

As faixas devem ser avaliadas por risco e utilidade econômica.

---

## 13. EAD e LGD

EAD e LGD devem seguir as regras/tabelas fornecidas pelo desafio.

Não substituir essas regras por modelos próprios sem decisão explícita.

EAD deve reagir às características previstas pelo desafio, incluindo prazo e LTV quando aplicável.

LGD deve reagir às características previstas pelo desafio, incluindo idade do veículo, LTV e avalista quando aplicável.

Recalcular esses componentes sempre que uma decisão de política alterar seus determinantes.

---

## 14. Perda esperada

A relação central é:

`EL = PD × EAD × LGD`

e:

`EAD = fator_EAD × valor_financiado`

A perda esperada deve ser calculável no nível da operação.

Mudanças de política que alterem PD, EAD ou LGD devem provocar novo cálculo da EL.

---

## 15. Política de crédito

A política poderá decidir:

* aprovação;
* taxa;
* prazo;
* entrada mínima;
* valor financiado.

Cada regra importante deve possuir justificativa econômica.

Não otimizar uma variável isoladamente ignorando seu efeito sobre as demais.

---

## 16. Entrada é uma alavanca de risco

Não assumir:

`risco maior → taxa maior`

como regra geral.

Sempre considerar se mudança de entrada pode melhorar a estrutura da operação através de redução de LTV e seus efeitos sobre risco e recuperação.

Quando o desafio determinar que entrada altera PD ou LGD, recalcular esses componentes corretamente.

---

## 17. Taxa não é margem isolada

Nunca assumir:

`taxa maior = lucro maior`

Taxa pode afetar:

* aceite;
* mix de clientes;
* seleção adversa;
* composição da carteira;
* retorno final.

Avaliar taxa dentro do simulador e da política completa.

---

## 18. Guardrails obrigatórios

Toda política candidata deve satisfazer simultaneamente:

```python
approval_rate >= 0.35
max_rate <= 0.035
default_rate <= 0.08
originated_volume >= 40_000_000
```

Essas regras devem existir como validações automáticas.

Uma política que viole qualquer uma delas é inválida.

Não remover, flexibilizar ou reinterpretar esses limites sem decisão explícita.

---

## 19. Não otimizar recusando todos

Uma política com excelente ROI mas originação insuficiente não resolve o problema.

Risco, crescimento e retorno devem ser avaliados conjuntamente.

Sempre verificar os guardrails ao comparar políticas.

---

## 20. Explicabilidade da política

Evitar políticas com centenas ou milhares de parâmetros sem necessidade material.

Uma política ligeiramente menos otimizada, mas estável e defensável, pode ser preferível a uma política opaca.

Cada regra relevante deve poder ser explicada em linguagem de negócio.

---

## 21. Robustez

A solução final deverá ser avaliada além do cenário-base.

Análises previstas incluem:

* performance temporal;
* drift A → B;
* drift A → C;
* análise marginal por score;
* fronteira risco-retorno;
* políticas dominadas;
* sensibilidade.

Cenários adversos previstos incluem:

* `PD_real = PD_estimada × 1.10`;
* `LGD = LGD + 5 p.p.`;
* `aceite = aceite × 0.90`.

Não confundir otimização no cenário-base com robustez.

---

## 22. Dados brutos

Arquivos em `data/raw/` são imutáveis.

Nunca sobrescrever dados brutos.

Qualquer transformação persistida deve ir para `data/processed/` ou local explicitamente destinado a outputs.

---

## 23. Notebooks e src

Notebooks servem principalmente para:

* exploração;
* análise;
* visualização;
* interpretação;
* narrativa experimental.

Lógica reutilizável ou crítica deve, sempre que fizer sentido, viver em `src/`.

Evitar copiar grandes blocos de lógica entre notebooks.

Também evitar criar abstrações sem uso real apenas para preencher a arquitetura.

---

## 24. Testes

Priorizar testes para erros que invalidariam silenciosamente o projeto.

Especialmente:

* leakage;
* separação temporal;
* fórmulas financeiras;
* regras de EAD/LGD;
* guardrails;
* comportamento da política.

Código econômico crítico deve ser verificável.

---

## 25. Reprodutibilidade

Resultados importantes devem poder ser reproduzidos.

Quando aplicável:

* controlar seeds;
* registrar configurações;
* preservar parâmetros;
* salvar artefatos;
* separar código de configuração;
* evitar estados ocultos de notebook.

---

## 26. Mudanças importantes

Antes de realizar uma alteração que:

* mude metodologia;
* mude fórmula;
* mude target;
* mude definição de população;
* mude regra de negócio;
* mude guardrail;
* introduza nova fonte de dados;

explicitar a mudança e sua justificativa.

Não fazer esse tipo de alteração incidentalmente durante refatorações.

---

## 27. ROADMAP

O arquivo `PROJECT_ROADMAP.md` contém a ordem operacional de construção do projeto.

Quando o roadmap estiver preenchido:

* utilizá-lo como sequência principal de execução;
* respeitar dependências entre etapas;
* não pular passos silenciosamente;
* marcar claramente o que foi concluído;
* não considerar uma etapa concluída apenas porque código foi escrito;
* verificar resultados e critérios de conclusão antes de avançar.

O roadmap pode evoluir.

Esta constituição define princípios mais permanentes do que o roadmap.

Em caso de conflito, não escolher silenciosamente: sinalizar o conflito antes de continuar.

---

## 28. Papel do agente

O agente é parceiro de implementação, análise e revisão.

Ele deve:

* implementar decisões já estabelecidas;
* detectar inconsistências;
* propor alternativas quando relevante;
* criar testes;
* revisar código;
* executar validações;
* reportar resultados de maneira clara.

Ele não deve:

* decidir sozinho questões relevantes de negócio;
* esconder premissas;
* fabricar resultados;
* inventar informação ausente;
* usar dados futuros para melhorar resultados;
* otimizar métricas violando metodologia;
* executar grandes partes futuras do roadmap sem necessidade.

O objetivo não é apenas produzir código que rode.

O objetivo é produzir uma solução correta, reproduzível, explicável e economicamente defensável.
