# CLAUDE.md — Tradición Manabita

Menú digital de **Tradición Manabita**, restaurante de cocina manabita en Ecuador.

## Tecnología

HTML/CSS/JS vanilla, sin build, sobre el motor compartido de craft-catalog.

```
index.html          # UI
config.json         # config tienda (tema, whatsapp, mapa, category_order) — sync solo pisa `categories`
productos.json      # generado por catalogsync (no editar a mano)
assets/app.js       # lógica
assets/style.css    # estilos
media/logo.webp     # logo (fondo transparente, 1:1 ~800×800) — lo sube el cliente
media/favicon.png   # favicon — lo sube el cliente
```

Imágenes de productos: las mapea catalogsync en el push (no se versionan a mano).

## Fuente de verdad de los productos

**craft-crm** (nodo core, producción). Los productos viven en la DB por `business_id`.
`catalogsync` regenera `productos.json` + la clave `categories` de `config.json` desde la DB y hace push a este repo → Cloudflare Pages despliega.

Todo lo demás de `config.json` (tema, tipografías, WhatsApp, ubicación, redes) lo controla este repo y **se preserva** en cada sync. No editar `productos.json` a mano.

## Datos del cliente

- **WhatsApp pedidos**: +593 99 804 8706
- **País**: Ecuador
- **Cloudflare Pages**: `tradicion-manabita.pages.dev`

## Branding

- **Paleta**: azul marino `#123D91`, celeste `#27B9E8` y naranja del logo como acento puntual.
- **Tipografías**: Fraunces (títulos) + DM Sans (cuerpo).

## Productos e imágenes

Los platos e imágenes se administran desde craft-crm. Cada imagen vive en `media` del producto y
catalogsync la descarga a `producto/<slug>/images/`, por lo que persiste en futuros resyncs.

## Deploy

```bash
git add . && git commit -m "descripción" && git push
```

Cloudflare Pages despliega automáticamente en push a `main` (workflow en `.github/workflows/deploy.yml`).
