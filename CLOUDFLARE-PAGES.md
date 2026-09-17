# Deploy no Cloudflare Pages

Painel: **Workers & Pages -> Create -> aba Pages -> Connect to Git**.

| Campo | Valor |
|---|---|
| Production branch | `main` |
| Framework preset | None |
| Root directory | _(vazio)_ |
| Build command | `npm run build` |
| Build output directory | `dist` |

## Nada a converter

O `netlify.toml` so tinha o build e o fallback de SPA — nenhum header, nenhum
redirect de conteudo. Por isso nao ha `_headers` nem `_redirects` aqui.

## Fallback de SPA

A regra `/* /index.html 200` **nao** deve ser recriada em `_redirects`. No
Cloudflare Pages um redirect vence qualquer arquivo estatico ("redirects are
always followed, regardless of whether or not an asset matches the incoming
request"), entao o catch-all `/*` alcancaria tambem `/assets/*.js`.

Ela tambem nao e necessaria: como o build nao gera um `404.html` na raiz, o
Cloudflare ja assume que o projeto e uma SPA e devolve `index.html` para
qualquer rota desconhecida — que e o que o React Router precisa.

**Nao adicione um `404.html` na raiz do build** sem antes reativar o roteamento
por outro meio: a presenca dele desliga esse fallback automatico.

O `netlify.toml` foi mantido de proposito. O Cloudflare nao le esse arquivo, e
ele mantem o site no ar no Netlify ate a troca de DNS.
