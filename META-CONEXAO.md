# Conexão opcional: Meta Ads em modo de leitura

Este guia ajuda você a preparar um app Meta e validar um token de usuário para **consultar dados de anúncios**. A V1 recomenda manter o uso por CSV/XLSX como caminho inicial; a conexão só deve ser feita se você realmente quiser automatizar a leitura.

> **Atenção:** token e App Secret são credenciais. Não cole em conversas com IA, tickets, documentos, prints ou repositórios. Digite-os somente em ferramentas oficiais do Meta ou em um conector local confiável que você controla.

> **Validade:** tokens de usuário de curta duração podem ser trocados por tokens de longa duração, normalmente com validade de até 60 dias. A validade real e a opção de extensão dependem do tipo de token, permissões, configuração do app e políticas vigentes. Confira sempre o resultado no Access Token Debugger. “Longa duração” não significa permanente.

As telas e nomes de menus do Meta mudam. Se diferirem do que está descrito, pare e consulte a documentação oficial atual antes de conceder permissões.

## O que você vai preparar

- Uma conta Meta com acesso à conta de anúncios que deseja consultar.
- Acesso ao Meta for Developers para criar um app.
- O produto/caso de uso da Marketing API habilitado no app.
- Permissão mínima de leitura, em geral `ads_read`, aprovada para a sua situação.
- Um token de usuário, conferido no debugger.
- Um conector local de leitura para fazer chamadas à Marketing API. **Este diretório não inclui conector nem coleta o token por você.**

## 1. Crie o app no Meta for Developers

1. Acesse [Meta for Developers — My Apps](https://developers.facebook.com/apps/) e entre na sua conta.
2. Se ainda não tiver acesso de desenvolvedor, conclua o cadastro solicitado pelo Meta.
3. Escolha **Create App / Criar app**. Os tipos e casos de uso oferecidos dependem das opções atuais do seu painel.
4. Selecione o caso de uso que habilita a Marketing API para leitura de insights/anúncios. O texto da interface pode mudar; confirme na [documentação da Marketing API](https://developers.facebook.com/docs/marketing-apis/).
5. Dê um nome que identifique o seu uso pessoal e vincule o app ao Business correto, se o fluxo pedir.
6. Conclua os requisitos do painel. Para uso próprio em modo de desenvolvimento, a conta que autoriza pode precisar ter um papel no app e acesso à conta de anúncios. Acesso por outras pessoas/empresas pode exigir análise e aprovação do Meta.

Não crie um app para contornar revisões ou acessar ativos sem autorização. Não solicite permissões de escrita para este agente.

## 2. Configure o acesso mínimo

Para leitura de dados de anúncios, comece apenas com `ads_read`, se esse escopo estiver disponível para seu caso. Não selecione `ads_management`: ele permite operações de gerenciamento e não é necessário para os fluxos de análise deste pacote.

A permissão `business_management` não deve ser adicionada por padrão. Só considere qualquer escopo adicional se uma chamada documentada realmente precisar dele, você entender o acesso concedido e o Meta solicitar/aprovar esse uso. Não habilite permissões de Página, Instagram ou leads para esta finalidade sem um caso de uso separado.

## 3. Gere um token de usuário

1. Abra o [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
2. Escolha o app que você acabou de criar no seletor de app.
3. Selecione um **User Access Token / Token de usuário**.
4. Adicione somente `ads_read` (ou o conjunto mínimo documentado para a operação específica).
5. Gere o token e aprove a autorização na janela oficial do Meta. Confira com qual usuário e ativos você está autorizando.
6. Copie o valor apenas para uma área segura/local onde você o utilizará. Não envie o token ao assistente. Nunca o coloque em `CONTEXTO.md`, no projeto do Maestro ou no GitHub.

Para uma configuração local, use o cofre de segredos do sistema, gerenciador de credenciais da ferramenta ou configuração local fora do diretório versionado. Não crie arquivos de texto simples com tokens dentro do projeto.

## 4. Depure e confira a validade

1. Abra o [Access Token Debugger](https://developers.facebook.com/tools/debug/accesstoken/), operado pelo Meta.
2. Cole o token diretamente no campo do debugger e escolha **Debug**. Não cole o token neste chat nem compartilhe a página enquanto o token estiver visível.
3. Confira os metadados exibidos:
   - app associado é o app que você criou;
   - tipo do token é o esperado (token de usuário);
   - usuário/escopos correspondem ao que você autorizou;
   - `ads_read` aparece concedido;
   - conta de anúncios está acessível ao usuário;
   - validade/`Expires` e `Data Access Expires`, quando mostradas.
4. Se o token for de curta duração e a página oferecer **Extend Access Token**, use essa opção dentro do fluxo oficial e reabra o debugger para conferir a nova validade. A troca geralmente gera um token de usuário de longa duração, com validade de até cerca de 60 dias.
5. Se a opção não aparecer, o token continuar curto, ou o debugger mostrar erro/permissões faltando, não tente contornar. Revise o tipo do token, app, caso de uso, papel do usuário e permissões seguindo a documentação do Meta.
6. Registre para você a data de expiração e programe uma renovação manual antes dela. Este pacote não renova tokens automaticamente.

**Não confunda tokens diferentes.** Um token de usuário, um token de sistema e um token de Página têm regras de emissão e expiração distintas. Este guia descreve o caminho comum de token de usuário de longa duração para uma conexão individual de leitura. A validade depende do Meta e pode ser revogada antes da data exibida (por exemplo, mudança de senha, remoção de acesso ou revogação de permissões).

## 5. Teste uma chamada de leitura antes de usar o agente

Faça primeiro um teste simples com a ferramenta oficial/documentada ou com um conector local auditado. A documentação deve indicar o endpoint e os campos adequados à versão da API habilitada no seu app. Confirme que:

- a resposta corresponde à conta de anúncios esperada;
- o período e fuso estão corretos;
- os campos retornados podem ser conciliados com o Ads Manager;
- não houve nenhuma operação de escrita.

Não copie uma chamada antiga de blog sem conferir versão, endpoint, campos e permissões na [documentação oficial da Marketing API](https://developers.facebook.com/docs/marketing-apis/). O token concede acesso de acordo com o usuário e os escopos; mantenha o escopo mínimo.

## 6. Configure um conector local com cuidado

Este pacote contém instruções, não um programa de conexão. Para usar a API a partir de uma ferramenta de IA, você precisa de um conector local separado que:

- faça apenas requisições `GET` de leitura para endpoints documentados;
- não tenha ferramenta de criação, edição, pausa, exclusão ou mudança de orçamento;
- leia o token de um cofre local/variável de ambiente, nunca do prompt ou de arquivo commitado;
- não registre tokens em logs, mensagens de erro ou histórico;
- permita limitar conta, campos e período;
- mostre a origem e o momento da consulta e permita comparar totais com o Ads Manager.

Peça revisão técnica antes de instalar ou autorizar um conector de terceiros. Até que o conector tenha sido instalado e testado por você, continue usando exports CSV/XLSX. O Maestro não deve afirmar que a conexão está ativa só porque você configurou um token.

## Erros comuns

| Sintoma | O que conferir |
|---|---|
| `Invalid OAuth access token` / sessão expirada | Validade no debugger, revogação, senha e usuário que autorizou. Gere/estenda um novo token no fluxo oficial. |
| `Permission denied` | Se `ads_read` foi concedida, se o app/usuário tem acesso à conta e se o app está no modo e estado adequados. |
| Conta não aparece | ID da conta, papel/permissão do usuário no Business e vínculo ao app; não adivinhe IDs. |
| Funciona no Explorer, mas não no conector | App selecionado, token realmente configurado no cofre local, versão da API, endpoint e logs sem vazamento de segredo. |
| Token expira antes do esperado | Confira `Expires` e `Data Access Expires` no debugger. A validade pode mudar ou ser revogada; não trate 60 dias como garantia. |

## Referências oficiais

- [Meta for Developers — Apps](https://developers.facebook.com/apps/)
- [Meta Marketing API](https://developers.facebook.com/docs/marketing-apis/)
- [Marketing API — Authentication](https://developers.facebook.com/docs/marketing-apis/overview/authentication/)
- [Graph API Explorer](https://developers.facebook.com/tools/explorer/)
- [Access Token Debugger](https://developers.facebook.com/tools/debug/accesstoken/)
- [Meta Permissions Reference](https://developers.facebook.com/docs/permissions/)
