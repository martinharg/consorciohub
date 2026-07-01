# Landing page — NexoHub

> ## ⚠️ LEER PRIMERO
> Landing de marketing **terminada**, un solo archivo HTML+CSS (sin JS, sin frameworks). Es **estática**: se puede publicar tal cual (Vercel/Netlify/GitHub Pages) o reimplementar en el stack que usen (Next, Astro, etc.) respetando el diseño 1:1.
> Dirección elegida: **"D · Comunidad"** — dark, estilo *blueprint*, con la paleta del logo (violeta + magenta), hero a dos columnas con la screen real de la app.

## Archivos
- `Landing NexoHub.html` — la landing completa. **Único** asset referenciado: `app-inicio.jpeg`.
- `app-inicio.jpeg` — captura real de la pantalla de Inicio, va en el teléfono del hero. Debe quedar **junto** al HTML (misma carpeta).
- Para publicar: renombrar el HTML a `index.html` y subir ambos archivos.

## Fuente
**Inter** (Google Fonts, 400–900), ya enlazada por CDN en el `<head>`. Única tipografía. Acentos técnicos en monoespaciada del sistema (`ui-monospace`) para badges y anotaciones.

## Paleta (de `:root` + inline)
```
--bg:    #0a0613   fondo (violeta casi negro)
--card:  #121738   tarjetas
--line:  rgba(255,255,255,.08)   --line2: rgba(255,255,255,.14)
--txt:   #eef1ff   --txt2: rgba(238,241,255,.64)   --txt3: rgba(238,241,255,.36)
--green: #33d98a   (checks del pricing / microcopy)
Acentos de marca (del logo NexoHub):
  violeta   #9b4dff   (base)     violeta claro #b98cff
  magenta   #e85cff   (acento)   glow profundo #7a2bff
  Degradé primario: linear-gradient(120deg,#9b4dff,#e85cff)
```
El botón primario, el emblema del logo, el % destacado y "residencial/edificio" usan el **degradé violeta→magenta**. "Hub" y las anotaciones van en magenta.

## Estructura (orden de secciones)
1. **Nav** — marca (emblema degradé + "Nexo**Hub**" junto) · links (El problema / Funciones / Precios) · botón "Solicitar demo".
2. **Hero (split, 2 columnas)**:
   - Izq: badge mono "● [AR] Piloto disponible en Argentina" · **H1 "Conectá con tu _edificio_"** (68px/900, "edificio" con degradé violeta→magenta + glow) · lead con "mercado vecinal" subrayado en magenta · **form de email** (input + botón "¡Quiero el piloto gratis! 🚀") · microcopy "✓ Sin tarjeta · Piloto 2 meses gratis · Setup en 24hs".
   - Der: **teléfono** (`.device`, borde magenta + bloom) con `app-inicio.jpeg`, y 3 **anotaciones** mono flotantes (`// eventos del edificio`, `// mercado vecinal`, `// reservá el SUM`) con punto+línea cyan/magenta.
   - Fondo: grilla técnica con máscara radial + 2 glows (violeta arriba-der, magenta abajo-izq).
3. **El problema** — kicker "[ El problema ]" · H2 "Un edificio lleno de gente, y nadie se conoce" · **4 tarjetas** (No conocés a tus vecinos / WhatsApp infinito / Expensas en PDF / Reservar el SUM es un quilombo), borde violeta translúcido, ícono en rojo-rosado.
4. **Funciones** — kicker + H2 "Primero la comunidad. Después, todo lo demás." · **6 tarjetas**; la 1ª (**Comunidad y chat**) va destacada con etiqueta "El corazón". Luego Eventos del edificio, Mercado vecinal, Expensas digitales, Reservas de amenities, Cámaras en vivo. Hover: sube + borde cyan/magenta.
5. **Precios** — 3 planes: **Esencial $390**, **Completo $690** (destacado, "Más elegido"), **Administraciones A medida**. Cada uno con lista de checks verdes + botón.
6. **Footer** — marca + "Tu edificio en un solo lugar · Buenos Aires, Argentina" + © 2026.

## Layout / técnica
- Contenedor central `.wrap` (max-width 1180px, padding 40px). Hero usa su propio `.inner` con `grid-template-columns:1.04fr .96fr`.
- Grids: problema `repeat(4,1fr)`, funciones/precios `repeat(3,1fr)`, gap 18–20px.
- **Responsive**: hay un breakpoint `@media (max-width:920px)` que apila el hero a 1 columna, oculta el teléfono y baja el H1 a 54px. Las grillas de 3/4 columnas **no** tienen aún breakpoints intermedios — si se necesita mobile fino, agregar media queries para pasar problema/funciones/precios a 1–2 columnas por debajo de ~760px.
- Sin JS: el form no envía (conectar a tu backend/Mailchimp/Tally al integrar). `scroll-behavior:smooth` para los anclas del nav.
- Íconos: SVG outline inline (ya incrustados). El emblema del logo en el nav/footer es un SVG de "edificio"; si preferís, reemplazalo por `assets/nexohub-logo-cut.png`.

## Copy (es-AR, voseo) — usar literal
Título "Conectá con tu edificio" · lead sobre comunidad/mercado/expensas/reservas/cámaras · CTAs "Solicitar demo" / "¡Quiero el piloto gratis! 🚀" / "¡Quiero el piloto!" / "Empezar gratis" / "Hablar con ventas". Precios y textos de planes son **tentativos** (ajustar con negocio).

## Publicar / actualizar (Vercel)
1. Renombrar `Landing NexoHub.html` → `index.html`, dejar `app-inicio.jpeg` al lado.
2. **GitHub:** subir ambos al repo → Vercel importa (framework **Other**, sin build command) → deploy. Para actualizar: reemplazar archivos + `git push` (Vercel redeploya solo).
3. **CLI:** `npm i -g vercel` → `vercel --prod` en la carpeta. Para actualizar: reemplazar archivos + `vercel --prod` de nuevo.

## Notas
- Es **una** de 4 direcciones que se exploraron (A Cobalto / B Split / C Light / D Blueprint). Esta es la D con enfoque comunidad-first y la paleta del logo. Las otras quedaron en el proyecto por si se quieren comparar.
