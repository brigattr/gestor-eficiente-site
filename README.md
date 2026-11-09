# Site da ChiefDesk (Gestor Eficiente)

Site comercial (landing page) do Gestor Eficiente, uma plataforma ChiefDesk (solução BP.CONN).
Uma única página estática (`index.html`) + imagens (`img/`). Não tem servidor, banco nem build.

## Arquivos

- `index.html` — o site (é este que vai para o ar).
- `img/` — telas do sistema (dados fictícios de exemplo), ícones e a imagem de compartilhamento (`og-chiefdesk.jpg`, 1200×630).
- `robots.txt` e `sitemap.xml` — orientam o Google (endereço oficial: https://chiefdesk.com.br/).

## SEO (revisado em 05/11/2026)

- Lighthouse: SEO 100, acessibilidade 100, boas práticas 100, desempenho 99 (celular).
- Título e descrição com palavras-chave, canonical, Open Graph/Twitter (prévia no WhatsApp/LinkedIn),
  dados estruturados (Organization, SoftwareApplication e FAQPage gerado das perguntas frequentes).
- Ao mudar as perguntas frequentes, atualize também o bloco `application/ld+json` no `<head>`.
- Depois de publicar: cadastrar o domínio no Google Search Console e enviar `https://chiefdesk.com.br/sitemap.xml`.

## Segurança

- `_headers` (lido pelo Cloudflare Pages): HTTPS obrigatório (HSTS), política de conteúdo (CSP) que só deixa rodar o
  script do próprio site e as fontes do Google, proteção contra o site ser embutido em outro (clickjacking) e outras.
- Se mudar o `<script>` do fim do `index.html`, o hash em `script-src` precisa ser recalculado
  (sha256 em base64 do conteúdo do script), senão o WhatsApp e o menu do celular param de funcionar.

## O que ajustar antes de publicar

No fim do `index.html`, dentro do `<script>`:
- `WHATSAPP` — número de vendas.
- `APP_URL` — endereço do sistema (`https://app.chiefdesk.com.br`).

## Publicar (Cloudflare Pages)

1. Cloudflare → Workers e Pages → Criar → Pages → conectar o repositório `brigattr/gestor-eficiente-site`.
   Sem comando de build; pasta de saída `/`.
2. Domínios: o site fica em `chiefdesk.com.br` e `www.chiefdesk.com.br`; o sistema passa para `app.chiefdesk.com.br`
   (trocar o domínio personalizado do projeto `chiefdeck` para `app.chiefdesk.com.br`).

## Publicar depois numa VPS

Por ser estático, basta copiar a pasta para o servidor e servir com nginx (ou Caddy):

```nginx
server {
  server_name chiefdesk.com.br www.chiefdesk.com.br;
  root /var/www/chiefdesk-site;
  index index.html;
  location /img/ { expires 30d; add_header Cache-Control "public"; }
}
```

HTTPS com Let's Encrypt (`certbot --nginx`) ou Caddy (automático).
