# Entrega AutoCred

Os arquivos de submissão seguem os cabeçalhos e o tratamento de recusas dos exemplos recebidos. Nenhum arquivo foi enviado à organização do desafio.

| Arquivo | Conteúdo |
|---|---|
| `submissao_modelo.csv` | PD dos 3.000 contratos da Base B, na ordem original |
| `submissao_politica.csv` | PD, score, decisão e condições das 5.000 propostas C |
| `documento_politica.docx` | Documento executivo preenchido a partir do template, com três páginas |
| `tabela_politica.csv` | Dez faixas, regras, contagem e projeções por faixa |
| `relatorio_tecnico.md` | Método, desempenho, hipóteses financeiras, riscos e reprodução |
| `modelo/modelo_pd.joblib` | Pipeline treinado e calibrador, para carregar com o módulo `src/autocred_model.py` |
| `diagnosticos/` | Métricas, comparações, deriva, gráficos, memória de cálculo e validação |

**Resultado observado do teste interno:** AuROC 0,7417 e KS 39,85% em julho a dezembro de 2024. A métrica da Base B depende da apuração oficial.

**Projeção central da política:** 44,26% aprovados; R$ 56,72 milhões contratados; inadimplência de 5,89%; ROI anual de 15,55%. O ROI adverso é 14,95%. São cenários internos com elasticidades assumidas, não resultados do simulador.

O documento deixa a identificação e os responsáveis como “a definir”, pois não foram informados. O arquivo separado “Regras da Competição”, citado pelo enunciado, não estava na pasta. Os exemplos e o enunciado foram usados como contrato de entrega.

## Reproduzir

Na raiz do desafio, com Python 3.12 e as versões indicadas em `src/requirements.txt`:

```powershell
python -m venv .venv
.\.venv\Scripts\python -m pip install -r src\requirements.txt
.\.venv\Scripts\python src\train.py
.\.venv\Scripts\python src\policy.py
.\.venv\Scripts\python src\validate.py
.\.venv\Scripts\python src\build_document.py --distill
.\.venv\Scripts\python src\build_document.py
```

Os scripts também reconhecem `.runtime_deps`, usada nesta execução. Os caminhos dos dados são relativos à localização dos scripts, não ao diretório de onde foram chamados. A semente é 2026. A execução sobrescreve somente os resultados em `outputs/autocred` e preserva bases e exemplos. Os hiperparâmetros do modelo, as faixas e os cenários estão explícitos no código.

Para apenas aplicar o modelo salvo, acrescente `src` ao `PYTHONPATH`, importe `autocred_model`, carregue o arquivo com `joblib.load` e use `autocred_model.predict(bundle, dataframe)`. Para C, use antes `proposals_to_features`. As dependências precisam corresponder às versões de treinamento.

O documento foi conferido visualmente por exportação do Word. `src/render_document.ps1` repete essa etapa em Windows com Word instalado. `qa/` contém apenas material de conferência, não faz parte da submissão.
