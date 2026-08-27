# Sofa Clean Wicklow — Documentação de Operações

Referência técnica de tudo que foi configurado para o site e a presença digital
do negócio. Atualizado conforme mudanças são feitas.

## Domínio e hospedagem

- **Domínio**: `sofacleanwicklow.ie` — registrado via LetsHost (renovação anual ~€46.99/ano)
- **Hospedagem**: GitHub Pages, repositório `adrianobmelo/landing-page-sofacleanwicklow`, branch `main`, raiz do repo
- **HTTPS**: certificado automático do GitHub Pages, "Enforce HTTPS" ativado
- **DNS**: gerenciado via **Cloudflare** (plano Free), porque o LetsHost não tem editor de zona DNS
  - Nameservers apontando para Cloudflare (`elly.ns.cloudflare.com` / `fattouche.ns.cloudflare.com`)
  - Registros DNS (todos em modo **"DNS only"**, sem proxy laranja — proxy ativo quebraria o certificado automático do GitHub Pages):
    - 4x registro `A` (raiz) → `185.199.108.153`, `.109.153`, `.110.153`, `.111.153`
    - 1x `CNAME` `www` → `adrianobmelo.github.io`
  - Arquivo `CNAME` no repositório contém `sofacleanwicklow.ie`

## Rastreamento e analytics

- **Google Tag Manager**: `GTM-K9DDF7WP`
- **Google Analytics 4**: `G-QWDFE527HV`
  - Eventos customizados: `whatsapp_click` (com `service`, `source`, `agent`) e `phone_click` (com `source`, `agent`)
  - `agent` distingue clique no chatbot do WhatsApp (`bot`) de ligação direta pro Adriano (`human`)
- **Meta Pixel**: `1039487698849495`
  - Instalado direto no `<head>` do `index.html` (não depende do GTM)
  - Eventos: `PageView` (automático), `Lead` (clique em WhatsApp), `Contact` (clique em telefone)
  - Testado e confirmado funcionando via extensão "Meta Ads Data Advisor"

## Google Business Profile

- Perfil criado e verificado
- Site atualizado para `https://sofacleanwicklow.ie/`
- Conectado ao Windsor.ai para leitura de dados (reviews, localização) sem expor API keys no site
- Place ID: `ChIJRdu648F8si0Rh6LCCC4TJXs`
- Maps CID: `8873519729239827079`
- Link direto para deixar review: `https://search.google.com/local/writereview?placeid=ChIJRdu648F8si0Rh6LCCC4TJXs`

## Google Search Console

- Propriedade `https://sofacleanwicklow.ie` verificada (via tag do GTM já instalada)
- Sitemap enviado: `https://sofacleanwicklow.ie/sitemap.xml`
- Indexação solicitada manualmente para a home

## Monitoramento

- **UptimeRobot** (plano free): monitor HTTP(S) em `https://sofacleanwicklow.ie`, checagem a cada 5 min, alerta por e-mail em caso de queda

## SEO técnico

- `robots.txt` e `sitemap.xml` publicados na raiz
- Schema.org (JSON-LD) no `<head>`:
  - `LocalBusiness` com endereço, `geo` (`GeoCoordinates`, 52.9808/-6.0446), área de atendimento, `aggregateRating`, `founder`, `makesOffer` (catálogo de serviços com `Service` detalhado por item)
  - `areaServed`: Wicklow Town, Ashford, Rathnew, Greystones, Newtownmountkennedy
  - `Review` estruturado dos 2 reviews reais do Google (Thainara Cunha, A BM)
  - `FAQPage` com as perguntas frequentes do site
- Meta tags de geolocalização no `<head>`: `geo.region` (`IE-WW`), `geo.placename`, `geo.position` e `ICBM`, todas apontando para Wicklow Town (52.9808, -6.0446)
- Open Graph / Twitter Card configurados, imagem `og-image.jpg` (1200×630)
- Imagens em WebP com fallback JPG (`<picture>`), `loading="lazy"` exceto hero (`fetchpriority="high"` + preload)
- Todas as tags `<img>` com `width`/`height` (ou `aspect-ratio` + `height: auto`) para CLS = 0
- Fonte do Google Fonts carregada de forma assíncrona (não bloqueia renderização)
- Vídeos de processo com `preload="none"` (só carregam ao clicar em play)

## Redes sociais

- Instagram (`@sofacleanwicklow`) com link na bio apontando para `sofacleanwicklow.ie`

## Canais de contato no site

- **WhatsApp** (chatbot): `wa.me/353899629764` — usado em todos os CTAs principais, com `data-service`/`data-source`/`data-agent="bot"` para rastreamento
- **Telefone direto** (Adriano): `+353 87 004 7288` — `data-agent="human"`

## Preços e ofertas atuais

- Ver tabela de preços completa na seção "Pricing" do `index.html`
- **Oferta de lançamento**: 10% de desconto, com teto de €10 (nunca ultrapassa €10, mesmo em jobs grandes) — condicionado a autorizar uso de foto antes/depois no Instagram/Facebook + deixar review no Google

## Pendências / próximos passos sugeridos

- [ ] Ativar 2FA (autenticação em duas etapas) nas contas GitHub, Cloudflare, Google e Meta Business Manager
- [ ] Escrever 3–5 artigos/páginas de SEO local (ex: "limpeza de sofá Wicklow", "preço limpeza de colchão Irlanda")
- [ ] Testar carregamento do site em rede 5G real (não só simulação do PageSpeed)
- [ ] Revisar relatório de "Cobertura/Páginas" no Search Console em ~1 semana, quando o Google já tiver rastreado mais o domínio novo
- [ ] Definir processo de pedido de review pós-serviço via WhatsApp (automação de negócio, fora do código do site)
- [ ] Avaliar WhatsApp Business API (respostas automáticas fora do horário, catálogo de serviços) — fora do escopo do site estático

## Notas para futuras alterações

- Todo o site é um único arquivo `index.html` (sem build step) — editar direto e commitar
- Sempre validar após editar: os 2 blocos JSON-LD devem continuar sendo JSON válido, e a contagem de `<div>` aberta/fechada deve bater
- Ao adicionar `width`/`height` numa `<img>` que só tem `width: 100%` no CSS (sem `height` nem `aspect-ratio`), sempre incluir também `height: auto` no `style` — senão o atributo `height` vira um valor fixo em pixels e distorce a imagem (bug já corrigido duas vezes nesta sessão)
