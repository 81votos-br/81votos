# Motor de atualização no seu domínio

Hoje o motor horário (tarefa agendada "Placar do Senado – varredura horária") publica o `data.json` na versão de teste do site, no link do Claude.

Quando o site estiver no GitHub e no Cloudflare Pages, o motor passa a fazer o mesmo trabalho no repositório:

1. Baixa o repositório `81votos`.
2. Procura notícias e edita o `data.json`, com as mesmas regras de evidência de hoje.
3. Faz o *commit* e envia o arquivo para o GitHub. O Cloudflare publica a nova versão em segundos.

## O que você precisa fazer

1. Criar o repositório no GitHub (passo 2 do `LEIAME.md`).
2. Na conversa com o Claude, dizer: **"o repositório é `seu-usuario/81votos`, troque o motor para publicar lá"**.
3. Aprovar o acesso ao repositório quando o Claude pedir. Ele precisa de permissão de escrita só nesse repositório.

O Claude ajusta a tarefa agendada e faz uma execução de teste. A versão no link do Claude pode continuar como ambiente de teste.

## Regras que o motor segue (não mudam)

- **Só a declaração do próprio senador muda a posição dele no placar.** Nota de partido, "bastidores", "deve votar" e contagem de terceiros não mudam.
- **Na dúvida, não publica:** o item vai para `META.pending` e aparece no resumo da execução.
- **Nunca apaga** declarações antigas.
- **Só mexe no `data.json`**, nunca no layout.
