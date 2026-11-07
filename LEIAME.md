# 81 Votos (81votos.com.br): como colocar no ar

Site estático. Não precisa de servidor, banco de dados nem programação. Custo fixo estimado: só o domínio (cerca de R$ 40/ano) e, se você escolher o Plausible, a estatística.

## O que tem nesta pasta

| Arquivo | Para que serve |
|---|---|
| `index.html` | O site inteiro. A configuração fica no topo, no bloco `CONFIGURAÇÃO ÚNICA DO SITE`. |
| `data.json` | Os dados do placar. **É o único arquivo que o motor de atualização mexe.** |
| `og.png` | Imagem que aparece quando o link é compartilhado no X, WhatsApp etc. |
| `favicon.svg` | Ícone da aba do navegador. |
| `privacidade.html` | Página de privacidade (LGPD). |
| `robots.txt`, `sitemap.xml` | Para o Google encontrar o site. |
| `_headers` | Regras do Cloudflare: o `data.json` nunca fica em cache, e há cabeçalhos de segurança. |
| `MOTOR-GITHUB.md` | Como o motor horário passa a publicar no seu domínio. |

## Passo a passo (cerca de 1 hora)

### 1. Domínio
O domínio `81votos.com.br` já está registrado e todos os arquivos já apontam para ele. Não há nada a trocar. Você só vai mexer no registro.br no passo 3, para apontar os servidores DNS para o Cloudflare.

### 2. Crie o repositório no GitHub
1. Em [github.com/new](https://github.com/new), crie um repositório chamado `81votos` (pode ser privado).
2. Clique em **uploading an existing file** e arraste **todos os arquivos desta pasta**.
3. Clique em **Commit changes**.

O repositório é o que permite ao motor atualizar o site sozinho: ele altera o `data.json` ali, e o Cloudflare publica em segundos.

### 3. Publique no Cloudflare Pages (gratuito)
1. Crie uma conta em [dash.cloudflare.com](https://dash.cloudflare.com).
2. Vá em **Workers & Pages → Create → Pages → Connect to Git** e escolha o repositório `81votos`.
3. Em *Build settings*: **Framework preset: None**, **Build command: vazio**, **Build output directory: `/`**.
4. Clique em **Save and Deploy**. Em um minuto o site fica no ar em `81votos.pages.dev`.
5. Em **Custom domains**, adicione `81votos.com.br`. O Cloudflare mostra dois *nameservers*: copie-os para o seu domínio no registro.br (opção "Alterar servidores DNS"). A propagação leva de minutos a algumas horas.

### 4. Crie o e-mail de correções (gratuito)
No Cloudflare, em **Email → Email Routing**, crie `contato@81votos.com.br` encaminhando para o seu e-mail pessoal. O botão "Enviar correção" do site já aponta para esse endereço. Se usar outro, troque `contato:` no bloco de configuração do `index.html`.

### 5. Ligue a estatística (sem cookies)
Escolha **uma** opção e edite o bloco de configuração no topo do `index.html`:

**Opção A: Plausible** (pago, a partir de cerca de US$ 9/mês, o mais simples)
1. Crie a conta em [plausible.io](https://plausible.io) e adicione o site `81votos.com.br`.
2. No `index.html`: `provider: 'plausible'`.
3. No Plausible, em *Site settings → Goals → Add goal → Custom event*, cadastre os eventos listados abaixo, com os nomes exatos.
4. Em *Site settings → Custom properties*, cadastre: `senador`, `origem`, `veiculo`, `onde`, `resultado`, `celula`, `uf`, `tipo`, `valor`, `intervalo`.

**Opção B: Umami Cloud** (gratuito para começar)
1. Crie a conta em [cloud.umami.is](https://cloud.umami.is) e adicione o site.
2. Copie o **Website ID**.
3. No `index.html`: `provider: 'umami'` e `umamiId: 'cole-o-id-aqui'`. Os eventos aparecem sozinhos, sem cadastro.

Depois de editar, suba o `index.html` de novo no GitHub (arraste o arquivo, depois **Commit changes**).

**Para testar:** ponha `debug: true` no bloco de configuração, abra o site, aperte F12 e veja os eventos `[track]` aparecendo no Console. Depois volte para `false`.

## Eventos medidos e o que cada um responde

| Evento | Propriedades | Pergunta que responde |
|---|---|---|
| `Abrir senador` | `senador`, `origem` (`link` ou `clique`) | Quem chega por link de card (`origem=link`) mostra se os cards estão circulando. |
| `Copiar link` | `senador` | Quais senadores as pessoas querem compartilhar. |
| `Clique em fonte` | `veiculo`, `onde` | A prova é usada ou é só enfeite? |
| `Verificar selo` | `resultado` | O selo desperta interesse? |
| `Pedir correção` | `senador` | Quantos pedidos de correção (cruze com a caixa de e-mail). |
| `Disse x Fez` | `celula` | O cruzamento chama atenção? |
| `Mapa estado` | `uf` | De onde vem o interesse regional. |
| `Filtro` | `tipo`, `valor` | Como as pessoas exploram a lista. |
| `Visita de retorno` | `intervalo` | As pessoas voltam? Sem cookie: o navegador guarda só a data da última visita. |

Visitas e origem do tráfego (X, WhatsApp, Google) a ferramenta já mede sozinha.

## Métricas de sucesso: teste de 10 dias

Siga sem culpa se, em 10 dias, aparecer **pelo menos um** destes sinais:
- **1 citação na imprensa.** Para saber, crie um [Google Alerts](https://www.google.com/alerts) para `"81 Votos"` e `"81votos"` e para o domínio, e busque o domínio no X todo dia.
- **1 pedido de correção** de senador ou assessoria (chega pelo e-mail de contato).
- **Cerca de 5 mil visitantes únicos**, com parte relevante em `Visita de retorno`.

Se nenhum aparecer, encerre sem culpa.

## Antes de divulgar (checklist)
- [ ] O site abre no domínio, com o cadeado (https).
- [ ] O topo mostra "Varredura há X min" com um horário recente, ou seja, o motor está publicando.
- [ ] Ao colar o link em [opengraph.xyz](https://www.opengraph.xyz), aparece a imagem `og.png`.
- [ ] O botão "Enviar correção" abre o e-mail com o assunto já preenchido.
- [ ] A estatística registra uma visita sua.
- [ ] Mandou o link para os 10 a 15 jornalistas de Congresso no mesmo dia do lançamento.
