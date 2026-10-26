<div align="center">

# 🎼 Maestro
### Seu copiloto para entender o que está acontecendo no tráfego da sua loja.

**Pare de olhar para um monte de números sem saber qual pergunta fazer.**

O Maestro analisa relatórios de anúncios, explica o que os dados sustentam e organiza próximos passos para você avaliar. Você começa com uma planilha de demonstração e pode usar os próprios relatórios quando estiver pronto.

[Começar agora](#-comece-em-3-minutos) · [O que ele faz](#-o-que-o-maestro-faz) · [Segurança](#-seus-dados-e-a-segurança)

</div>

---

## 👋 O que é o Maestro?

O Maestro é um conjunto de instruções para usar com uma ferramenta de IA que trabalhe com os arquivos de uma pasta, como Codex, Claude Code ou ferramentas equivalentes. Você copia ou baixa este diretório, abre a pasta na ferramenta e conversa com o agente.

Ele foi pensado para **lojas e marcas de produtos físicos**. Você pode começar imediatamente com CSV/XLSX, sem programação nem conexão. Para quem quiser automatizar a leitura dos dados do Meta Ads, o pacote também inclui um guia passo a passo para criar o app, gerar e depurar o token, conectar a Marketing API em um ambiente local e validar uma chamada de leitura. Essa etapa avançada é opcional.

> **O Maestro mostra caminhos. Quem decide e faz qualquer mudança na conta é você.**

## 🚀 Comece em 3 minutos

### 1. Copie ou baixe esta pasta

Baixe o ZIP do repositório no GitHub, extraia os arquivos e abra a pasta `maestro-agente-trafego` como projeto. Se você usa Git, clone o repositório e abra a pasta do projeto.

### 2. Abra a pasta numa ferramenta de IA compatível

Abra a **pasta inteira como projeto/workspace** no Codex, Claude Code ou ferramenta semelhante. Isso permite que a IA leia os arquivos de instrução e a planilha de exemplo. A compatibilidade pode variar conforme o produto, a versão, as permissões e as instruções de projeto disponíveis.

Se a ferramenta não carregar `AGENTS.md` automaticamente, peça:

> Leia `AGENTS.md` e `PROMPT-MESTRE.md` nesta pasta e siga as instruções do projeto.

### 3. Escreva `iniciar`

O Maestro vai apresentar a configuração em etapas curtas e perguntar uma coisa por vez. Para testar sem usar dados da sua empresa, escolha a demonstração com `exemplos/campanhas-ficticias.csv`.

**Pronto.** Você já pode pedir uma auditoria, comparar anúncios ou planejar testes.

### Se a ferramenta só aceitar um prompt

Copie o conteúdo de `AGENTS.md` e `PROMPT-MESTRE.md` para as instruções do projeto ou da conversa. Anexe também `DADOS-E-METRICAS.md`, `FLUXOS.md` e o CSV de exemplo quando quiser usar os procedimentos e a demonstração. Se quiser configurar acesso ao Meta, anexe também `META-CONEXAO.md`. Sem acesso à pasta, o agente não consegue manter `JORNADA.md`, `CONTEXTO.md` ou relatórios no seu computador.

## 🧭 O que o Maestro faz

O Maestro tem oito modos de análise. Chame pelo número ou simplesmente descreva o que quer descobrir.

| Quero entender… | Peça assim |
|---|---|
| Se algo mudou nos últimos dias | `Analise minhas campanhas de hoje` |
| O desempenho das campanhas | `Audite minhas campanhas` |
| Quais anúncios merecem investigação | `Analise meus anúncios` |
| Onde o funil pode estar perdendo vendas | `Onde meu funil perde vendas?` |
| Como avaliar a distribuição de verba | `Avalie a distribuição do orçamento` |
| Qual experimento pode gerar aprendizado | `O que testar agora?` |
| O que mudou de uma semana para outra | `Faça a análise da semana` |
| Como resumir os resultados para alguém | `Gere um relatório executivo` |

Você também pode perguntar `quais dados faltam?`, `mostrar fontes` ou `mostrar jornada`.

## 📊 Teste primeiro com dados fictícios

Você pode experimentar o Maestro sem conectar uma conta. A pasta inclui um CSV de demonstração com **dados totalmente fictícios**. Ele serve para ver como o Maestro lê investimento, impressões, cliques, compras atribuídas e receita atribuída.

A amostra contém quatro dias e oito linhas. Os totais ilustrativos são R$ 1.160 de investimento, R$ 3.960 em receita atribuída e 33 compras atribuídas. Isso dá ROAS de plataforma aproximado de 3,41x e CPA aproximado de R$ 35,15. Como a janela é curta e os dados foram inventados para demonstração, esses números **não provam lucro, tendência nem condição para aumentar investimento**.

Experimente pedir:

> Use `exemplos/campanhas-ficticias.csv` e faça uma auditoria. Trate os dados como demonstração fictícia e explique o que dá e o que não dá para concluir.

## 📂 O que vem nesta pasta

```text
maestro-agente-trafego/
├── AGENTS.md                    # Regras permanentes e limites de segurança
├── PROMPT-MESTRE.md             # Onboarding, comandos e modo de trabalhar
├── FLUXOS.md                    # Oito procedimentos de análise
├── DADOS-E-METRICAS.md          # Formatos, validações e fórmulas
├── META-CONEXAO.md              # App Meta, token, depuração e leitura da API
├── CONTEXTO.example.md          # Modelo para personalizar sua operação
├── CLAUDE.md                    # Compatibilidade com Claude Code
├── .gitignore                   # Protege arquivos locais contra commits acidentais
└── exemplos/
    └── campanhas-ficticias.csv  # Planilha demonstrativa, sem dados reais
```

## 🗂️ Use seus próprios relatórios

Exporte um CSV ou XLSX da plataforma de anúncios e peça ao Maestro para analisar o arquivo. Se possível, comece com dados agregados por dia e campanha. Para avaliar anúncios, inclua o nível de anúncio e um identificador para cada peça.

Antes de compartilhar:

- Remova nomes, e-mails, telefones, endereços, CPF, IDs de pedido e outros dados pessoais.
- Confira período, moeda, fuso horário e janela de atribuição do export.
- Se trabalhar para clientes, abra um projeto/pasta isolado para cada cliente.
- Não inclua números que você não tenha permissão para compartilhar.

O Maestro pode criar `CONTEXTO.md` durante o onboarding e salvar `JORNADA.md` ou relatórios se a ferramenta tiver permissão de gravação e você pedir. Esses arquivos são locais e estão listados no `.gitignore`.

## 🔒 Seus dados e a segurança

- **Acesso opcional de leitura:** o guia Meta configura a permissão `ads_read`. Não habilite `ads_management` para este pacote.
- **Token é segredo:** nunca cole token, app secret, senha, cookie ou código de autenticação na conversa, em Markdown, no Git ou em prints compartilhados. Use somente armazenamento local apropriado.
- **Sem ações nas campanhas:** o agente não altera campanhas, anúncios, públicos ou orçamentos. O guia não ensina nem habilita operações de escrita.
- **Sem upload automático:** o Maestro analisa os arquivos que você disponibiliza na ferramenta escolhida. A política de dados dessa ferramenta continua valendo.
- **Arquivos privados ficam locais:** `CONTEXTO.md`, `JORNADA.md`, `dados/`, `relatorios/` e `clientes/` são ignorados pelo Git.
- **Confira antes de publicar:** `.gitignore` ajuda contra commits acidentais, mas não impede anexos, arquivos já adicionados ao Git ou uso forçado do Git. Revise a lista de arquivos antes de enviar ao GitHub.
- **Relatórios podem conter dados sensíveis:** prefira valores agregados e remova informações pessoais de compradores.

## 🧠 Como o Maestro trata os números

O Maestro não transforma qualquer métrica num diagnóstico. Ele identifica fonte, período, moeda e granularidade; separa fatos de hipóteses; e mostra as contas que importam.

- **ROAS** é a receita atribuída pela plataforma dividida pelo investimento. Não é lucro.
- **CPA** é o custo por conversão reportada. Não significa CAC de cliente novo sem dados que comprovem novos clientes.
- **Margem desconhecida** significa que não dá para afirmar se uma campanha é lucrativa.
- **Sem histórico comparável**, não existe “normal da conta” para usar como referência.
- Métricas ausentes ficam como ausentes; o Maestro não as preenche com zero.

As definições e fórmulas estão em [`DADOS-E-METRICAS.md`](DADOS-E-METRICAS.md).

## ✅ Primeira conferência

Depois de abrir a pasta:

1. Escreva `iniciar` e veja se o agente entende a jornada.
2. Peça uma auditoria do CSV fictício e confira se marca os dados como demonstração.
3. Confirme que ele distingue ROAS de lucro e reconhece que quatro dias não sustentam uma decisão de escala.
4. Peça para alterar uma campanha. O Maestro deve explicar que só oferece recomendações manuais e não modifica plataformas.

A leitura das instruções depende da ferramenta e da versão. Faça essa conferência antes de colocar dados reais.

## ❓ Perguntas frequentes

**Preciso saber programar?**

Não. Para começar, basta abrir a pasta numa ferramenta de IA que consiga ler os arquivos do projeto e escrever `iniciar`.

**Preciso conectar o Meta Ads para começar?**

Não. CSV/XLSX é suficiente para começar. Se quiser a conexão opcional, siga `META-CONEXAO.md`. O token é configurado localmente, nunca enviado ao Maestro ou commitado no Git.

**Ele monitora as campanhas sozinho?**

Não. Você inicia cada leitura quando quiser. A API pode ser consultada pelo conector local depois de configurada, mas não há monitoramento em segundo plano nem alertas automáticos neste pacote.

**Posso usar para mais de uma loja ou cliente?**

Sim. Use uma pasta/projeto separado para cada operação e não misture relatórios nem contexto.

**Posso usar outro tipo de planilha?**

Pode pedir que o Maestro confira o formato. CSV/XLSX exportado é o ponto de partida; se as colunas ou definições forem ambíguas, ele deve perguntar ou marcar a limitação.

## 🧾 Licença e uso

Este diretório está preparado para você publicar no GitHub, mas a licença de redistribuição ainda precisa ser escolhida. **Não inclua uma licença antes de decidir quais permissões você quer conceder** (por exemplo, uso, cópia, modificação e redistribuição comercial). Sem arquivo de licença, os direitos permanecem reservados por padrão.

---

<div align="center">

Feito para transformar exportações em perguntas melhores e decisões mais conscientes.

**Maestro · Agente de análise de tráfego para e-commerce**

</div>
