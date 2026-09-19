# AutoCred — Definição do problema econômico

Registro da fase 1 do roadmap, concluída em 2026-09-13 com base nas decisões já estabelecidas em `AGENTS.md` e no planejamento fornecido pelo usuário.

## Objetivo e entregas (passos 1–5 e 13–14)

O objetivo final é produzir uma decisão de crédito economicamente defensável: decidir quais propostas aprovar e quais condições oferecer, considerando risco, volume e retorno da carteira efetivamente originada.

As entregas se dividem em modelo de risco e política de crédito, conectadas pela economia da operação:

| Bloco | Pergunta | Papel na solução |
| --- | --- | --- |
| PD / risco | Quem pode entrar em default? | Estimar a probabilidade de default de cada operação. Este é o componente a ser modelado por machine learning. |
| Economia da operação | Quanto podemos perder caso ocorra default? | Aplicar as regras de EAD e LGD e calcular a perda esperada com a PD estimada. |
| Decisão de negócio | Quais condições tornam a operação economicamente aceitável? | Definir aprovação e condições, avaliando seus efeitos sobre aceite, carteira, inadimplência, volume e ROI. |

A PD será utilizada como entrada econômica da política. O sucesso do projeto exige avaliar a decisão e a carteira resultante, além das métricas do modelo.

## Target e cadeia de decisão (passos 6–7)

O target da modelagem é `default_90_12`. Sua definição não será alterada sem decisão explícita e documentada. A conferência da codificação e da definição operacional no dicionário do desafio permanece para o entendimento dos dados.

`dados → PD → EAD → LGD → perda esperada → preço/condições → aceite → carteira → inadimplência → volume → ROI`

Essa cadeia representa o fluxo da solução. EAD e LGD são componentes calculados pelas regras do desafio e combinados com PD; as setas não significam que EAD seja calculada a partir de PD ou que LGD seja calculada a partir de EAD.

## Relações econômicas e fontes das regras (passos 8–12)

As relações fixadas são:

```text
EL = PD × EAD × LGD
EAD = fator_EAD × valor_financiado
EL = PD × fator_EAD × valor_financiado × LGD
```

PD é a probabilidade de default; EAD é a exposição no default; LGD é a fração da exposição perdida em caso de default; EL é a perda esperada. PD e LGD entram nas fórmulas como proporções. EAD, valor financiado e EL usam a mesma unidade monetária.

Conforme o planejamento e a constituição, o desafio fornece a lógica e os fatores de EAD e a lógica e as tabelas de LGD. Essas são as fontes obrigatórias para a implementação; não serão substituídas por modelos próprios sem decisão explícita.

O enunciado e suas tabelas ainda não estão disponíveis no projeto. Portanto, fica registrada a fonte prevista, sem afirmar que seus valores foram conferidos. A implementação depende dessa conferência nas etapas correspondentes.

## Qualidade da PD para uso econômico (passos 15–16)

Boa ordenação de risco sozinha não basta. Discriminação avalia a capacidade de ordenar clientes por risco; calibração avalia a correspondência entre probabilidades previstas e defaults observados.

Uma PD mal calibrada distorce a perda esperada e pode prejudicar a precificação e as condições ofertadas. A calibração será avaliada respeitando a separação temporal; a necessidade e o método de eventual recalibração serão decididos na etapa própria.

## Alavancas da política (passos 17–24)

| Alavanca | Decisão e efeito a avaliar |
| --- | --- |
| Aprovação | Determina quais propostas recebem oferta e afeta o volume e o risco da carteira. |
| Taxa | Define o preço da oferta e pode alterar aceite e composição da carteira. |
| Prazo | Altera a estrutura da operação e exige recalcular EAD conforme as regras do desafio. |
| Entrada | Altera valor financiado e LTV e pode afetar simultaneamente risco, recuperação e aceite. |

Aumentar taxa não necessariamente aumenta lucro: o aceite pode cair e pode ocorrer seleção adversa, alterando o mix dos clientes que contratam. Esses efeitos serão avaliados pela lógica do simulador do desafio, sem assumir antecipadamente sua magnitude.

A entrada é uma alavanca de risco e de estrutura financeira. Mudanças de entrada exigirão recalcular valor financiado, LTV e os componentes afetados de EAD, LGD e PD conforme as regras fornecidas, seguidos de novo cálculo de EL. Não se presume uma redução causal de PD apenas por modificar uma feature do modelo.

Os valores de taxas, entradas, prazos e cortes de aprovação não são definidos nesta fase. Sua escolha depende das regras oficiais, dos dados e da avaliação econômica nas etapas posteriores.

## Método de trabalho e conclusão (passo 25)

O ciclo adotado é `conceito → hipótese → código → resultado → interpretação → decisão`, com desenvolvimento incremental. A implementação ocorre quando necessária para testar uma hipótese já explicitada; esta fase é de definição e seu resultado é este registro documental.

Critérios de conclusão verificados:

- Objetivo, entregas e três blocos explicitados.
- Target preservado e cadeia econômica registrada.
- Três fórmulas registradas de forma consistente com a constituição.
- Fontes previstas para EAD/LGD identificadas, com a conferência documental ainda pendente explicitada.
- Uso econômico da PD, calibração e quatro alavancas explicados.
- Efeitos possíveis de taxa e entrada registrados sem fixar política ou inventar parâmetros.
- Ciclo de trabalho adotado, sem necessidade de nova decisão de negócio para concluir esta fase.

A conclusão é conceitual e documental: não representa validação de dados, modelos, tabelas financeiras ou políticas. Essas validações permanecem nas fases correspondentes do roadmap.
