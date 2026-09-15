# play.cubicase.net

Página de convite do Cubicase (link bonito `play.cubicase.net/<slug>` no lugar
do IP da malha Tailscale ou do código `CF-XXXXXX` cru). Recurso pago (Cubicase
Plus) — ver a aba Assinatura nas configurações do app.

Site estático publicado via GitHub Pages, num repositório **separado** do
repositório principal do Cubicase — cada site do GitHub Pages só aceita um
domínio customizado, e este é um subdomínio (`play.`) diferente do domínio
raiz (`cubicase.net`, que já é o GitHub Pages do repositório principal).

## Como funciona

1. Alguém abre `play.cubicase.net/nome-do-mundo`.
2. Como não existe nenhum arquivo com esse nome, o GitHub Pages devolve
   `404.html` — que aqui é uma cópia de `index.html` (mesmo conteúdo, veja
   abaixo). O JavaScript lê o slug direto de `location.pathname`.
3. A página chama `GET /api/v1/servers/by-slug/<slug>` na API Central
   (Cloudflare Worker, `cubeforge-api`, repositório principal) para resolver
   o slug em um `shortCode`.
4. Redireciona para `cubicase://join/<shortCode>` — o app Cubicase já escuta
   esse esquema (ver `src/lib/joinDeepLink.ts` no repositório principal).
5. Se o app não abrir em ~1,5s (não instalado), mostra um link pra baixar.

`index.html` e `404.html` precisam ficar **idênticos** — se editar um, copie
pro outro.

## Publicar/atualizar

```bash
git add -A
git commit -m "Atualiza página de convite"
git push
```

O GitHub Pages já fica configurado (branch + domínio customizado) — nenhum
passo extra depois do primeiro deploy (ver instruções de configuração inicial
no repositório principal).
