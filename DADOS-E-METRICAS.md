# DADOS E MÉTRICAS | CONTRATO DE LEITURA V1.0

## 1. Princípio

**Nenhuma recomendação quantitativa sem rastrear fonte, período e cálculo.** Nunca preencha lacunas por suposição. Uma coluna ausente deve virar `não disponível`, não zero. Zero significa ausência medida, não ausência de informação.

A forma oficial de entrada da V1 é **CSV ou XLSX exportado de plataformas de anúncios** e, quando existir, resumo agregado da loja. Outras fontes podem ser avaliadas individualmente. O agente não pressupõe uma API ativa.

## 2. Formato de dados

O modelo `exemplos/campanhas-ficticias.csv` usa `;` como separador, datas ISO (`AAAA-MM-DD`) e números no padrão brasileiro (`1234,56`). Aceite outros layouts depois de identificar explicitamente o formato. Não trate `1.234,56` como `1.23456` ou `1.234`, e não assuma que vírgula sempre é delimitador.

### Campos recomendados em relatórios de anúncios

| Campo canônico | Tipo | Observação |
|---|---|---|
| `data` | data | Um dia ou início de intervalo documentado |
| `plataforma` | texto | Meta, Google, TikTok etc. |
| `conta` | texto | Nome fictício ou referência não sensível |
| `campanha` | texto | Identificador/nome |
| `conjunto` | texto | Pode faltar em alguns formatos/plataformas |
| `anuncio` | texto | Necessário para métricas em nível de criativo |
| `moeda` | texto | BRL, USD etc.; não misturar sem conversão documentada |
| `fuso_conta` | texto | Idealmente `America/Sao_Paulo` ou offset conhecido |
| `janela_atribuicao` | texto | Ex.: janela informada pelo export ou `desconhecida` |
| `investimento` | decimal | Gasto da plataforma no período |
| `impressoes` | inteiro | Impressões, não alcance único |
| `cliques_link` | inteiro | Cliques no link ou métrica equivalente *identificada* |
| `compras_atribuidas` | inteiro/decimal | Conversões reportadas pela plataforma, eventualmente modeladas |
| `receita_atribuida` | decimal | Valor de compras atribuído à mídia |

**Campos extras opcionais:** `alcance`, `visitas_site`, `adicoes_carrinho`, `checkouts_iniciados`, `compras_site`, `receita_loja_liquida`, `novos_clientes`, `custo_produtos`, `taxas`, `frete_subsidiado`, `impostos`, `devolucoes`. Esses campos podem vir de **outras fontes**, com granularidade e definições próprias; não devem ser repetidos e somados em todas as linhas do export de anúncios.

**Importante:** `compras_atribuidas` podem ser números fracionários em alguns relatórios/modelos. Preserve a definição da origem; não arredonde silenciosamente.

## 3. Validação obrigatória

Antes de cada análise:

1. **Inventarie arquivos:** fonte, conta, plataforma, período de extração, atualização e granularidade real (por dia, campanha, conjunto, anúncio). Não confunda data de extração com data de performance.
2. **Reconheça cabeçalhos:** apresente mapeamento para campos canônicos quando houver variações, por exemplo `Valor gasto` -> `investimento`, somente quando a equivalência for inequívoca.
3. **Cheque localidade:** separador CSV, decimal, milhar, moeda, datas `DD/MM/AAAA` vs `MM/DD/AAAA`, fuso e células vazias. Se houver ambiguidade que altere resultado, pergunte.
4. **Cheque consistência:** valores negativos, duplicatas pela chave disponível, linhas agregadas misturadas a linhas detalhadas, intervalos sobrepostos, moeda mista e subtotais duplicados.
5. **Cheque totais:** conte linhas e datas; some gasto e receita das linhas válidas; compare com total de controle fornecido/exportado, quando houver. Informe diferenças e não diga que conferiu painel sem acessá-lo.
6. **Cheque comparabilidade:** janela de atribuição, finalidade da campanha, sazonalidade, períodos com tamanhos diferentes, dados recentes sujeitos a atraso, mudanças de tracking e mudanças de definição.
7. **Classifique qualidade:**
   - `VALIDADO`: parse e somatórios corretos + definição das métricas conhecida + referência externa conciliada quando disponível.
   - `PARCIAL`: números internos consistentes, mas algum elemento como conciliação externa, atribuição ou histórico está ausente.
   - `INSUFICIENTE`: dados quebrados ou lacunas impedem o cálculo pretendido.

Com status `PARCIAL`, pode entregar análises **descritivas com avisos**. Com `INSUFICIENTE`, não gere recomendações quantitativas que dependam dos dados ausentes.

## 4. Fórmulas padronizadas

Todas as divisões por zero são `N/D`, não infinito nem zero artificial. Recalcule razões a partir das somas de numerador e denominador compatíveis.

| Indicador | Fórmula | Cuidados |
|---|---|---|
| CPM | `investimento / impressoes * 1000` | Mesmo escopo/período |
| CPC link | `investimento / cliques_link` | Não confundir clique total com clique no link |
| CTR link | `cliques_link / impressoes * 100` | Informar em `%` |
| CPA compra (mídia) | `investimento / compras_atribuidas` | Não chamar automaticamente de CAC |
| ROAS plataforma | `receita_atribuida / investimento` | Atribuição da plataforma; não é lucro |
| AOV / ticket médio | `receita_de_pedidos / numero_de_pedidos` | Mesma fonte e definição de receita |
| Taxa de conversão do site | `pedidos_do_site / sessoes_do_site * 100` | Dados da loja/analytics; não substituir por cliques |
| Taxa de checkout concluído | `compras / checkouts_iniciados * 100` | Eventos consistentes, escopo igual |
| Taxa de carrinho | `adicoes_carrinho / sessoes * 100` | Definir pessoas, sessões ou eventos conforme fonte |
| MER (eficiência da mídia) | `receita_total_da_loja / investimento_total_em_midia` | Mesmo período/moeda, todas as mídias incluídas; informar conceito de receita |
| Margem de contribuição pré-marketing | `contribuicao_pre_midia / receita_liquida` | Considerar descontos, impostos variáveis, CMV, gateway, frete subsidiado, devoluções e custos variáveis aplicáveis |
| ROAS de equilíbrio teórico | `1 / margem_contribuicao_decimal` | Aproximação para receita atribuída observável; **não** garante equilíbrio real se houver atribuição parcial ou sobreposta |
| CPA máximo teórico de primeira compra | `contribuicao_pre_midia_por_primeira_compra` | Aproximação de break-even antes de custos fixos, sem prometer lucro |
| CAC real de novos clientes | `custos_de_aquisicao_definidos / novos_clientes_adquiridos` | Exige definição explícita de novos clientes e custos incluídos |

**Notas críticas:**
- Receita atribuída em múltiplas plataformas pode contar a mesma venda mais de uma vez. **Nunca some receitas atribuídas de plataformas e chame de receita real da loja.**
- Receita bruta ≠ receita líquida ≠ receita pós-devoluções. Identifique o conceito.
- **MER não prova causalidade** do investimento; funciona como indicador de eficiência agregada quando os dados são compatíveis.
- Custo por compra de anúncios é **CPA**, não automaticamente CAC; CAC demanda novos clientes e regra consistente de custos.
- Margem de contribuição não é margem bruta nem lucro líquido. Se for desconhecida, não calcule ponto de equilíbrio nem defina “escala lucrativa”.
- `1 / margem` assume receita e contribuição alinhadas e reconhecidas na mesma base; impostos, frete, fixos, atrasos, mix e atribuição podem invalidar decisões automáticas.
- Não inferir LTV sem dados de coortes, retenção e margem ao longo do tempo.
- Não aplicar benchmarks universais de CPA/CTR/ROAS como regra; comparar primeiro com histórico, meta e economia da operação.

## 5. Regras de agregação

- **Somáveis dentro de dimensão e janelas disjuntas:** investimento, impressões, cliques, compras atribuídas e receita atribuída, desde que as linhas não incluam totais/subtotais repetidos nem dados de atribuição incompatíveis.
- **Recalcular, nunca somar:** CTR, CPC, CPM, ROAS, CPA, AOV e taxas.
- **Não somar sem deduplicação apropriada:** alcance único, usuários únicos, frequência, sessões ou pedidos vindos de relatórios de dimensões sobrepostas.
- Comparação diária: observar volatilidade, baixo volume, dias da semana e latência. Análise semanal: preferir intervalos comparáveis e recortes consistentes.
- Para criativos, exigir `anuncio` (ou identificador equivalente). Sem arquivo/imagem/vídeo do criativo, não inferir elementos visuais, ganchos ou qualidade da peça apenas por métricas.
- Para funil, exigir métricas do site/checkout ou declarar que a análise termina no clique.
- Se uma mesma conta tiver múltiplas moedas, **separar** os grupos; conversão de moeda exige taxa, data e fonte explícitas.

## 6. Fontes e rastreabilidade

Todo relatório deve incluir, no mínimo:
- Arquivos e plataformas de origem.
- Data/hora de extração se disponível; período coberto; fuso; moeda.
- Granularidade e eventuais filtros.
- Janela de atribuição (ou `desconhecida`).
- Versão da regra/métrica quando cálculo não óbvio.
- Limitações, divergências e status da reconciliação.
- Uma demonstração de cálculo das métricas usadas na decisão principal.

Persistência: salvar **saídas datadas** em `relatorios/`, sem dados pessoais, e conservar arquivos originais em `dados/` quando o usuário autorizar. Só é possível comparar tendência histórica se existirem períodos anteriores completos e compatíveis. Se não existirem, declarar `sem histórico comparável`.

## 7. Dados insuficientes: decisões específicas

- Sem gasto: não calcular eficiência de mídia.
- Sem receita atribuída: não calcular ROAS de plataforma.
- Sem pedidos da loja: não avaliar eficiência financeira real nem taxa de conversão do site.
- Sem margem: não recomendar escala por rentabilidade.
- Sem histórico: não chamar um número de “acima/abaixo do normal” com base no próprio negócio.
- Sem criativos reais: avaliar somente performance numérica por anúncio.
- Sem dado de conversões suficientemente consolidado: sugerir observação, investigação e coleta, não cortes ou aumento de orçamento.

O agente pode apresentar perguntas ou uma checklist de dados faltantes, mas **não deve transformar ausência de informação em um diagnóstico inventado**.
