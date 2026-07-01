# Notificaciones — Mobile · NexoHub

> ## ⚠️ LEER PRIMERO — COPIAR EXACTO
> Pantalla mobile de alta fidelidad **terminada**. Reproducirla **idéntica** (colores, medidas, tipografía, copy, estados). No rediseñar.
> Es una **pantalla pusheada**: se abre al tocar la **campana** del header de Inicio (que hoy no lleva a nada). Lleva botón **← volver**, sin bottom-nav.

## Qué es
Centro de notificaciones del residente: lista agrupada por tiempo (Hoy / Ayer / Esta semana), con tipos por color, no-leídas resaltadas, tabs **Todas / Sin leer** y acción **Marcar leídas**.

## Stack del archivo
- React 18 + Babel inline. Se renderiza dentro del marco **`ios-frame.jsx`** (`<IOSDevice dark width={402} height={874}>`), igual que `Inicio Residente Mobile.html`.
- Fuente **Inter**. Pantalla full-viewport, escalada para entrar en cualquier ventana.
- El componente clave es **`NotificacionesApp`** (recibe `insetTop`, `insetBottom` del marco).

## ★ Acento VIOLETA (no azul)
Esta pantalla ya usa el acento **violeta `#7c5cff`** de la marca NexoHub (con magenta `#e85cff` de apoyo), **migrado del viejo azul `#5b7de8`** que todavía tiene `inicio-shared.jsx`. Al integrar, el acento de notificaciones es violeta.

## Design tokens
```
--bg:    #181626   fondo de pantalla
--bg2:   #211f30   superficies / estado vacío
--bg3:   #2a2840   chips/tabs inactivos
--card:  #23213a
--line:  rgba(255,255,255,0.07)   --line2: rgba(255,255,255,0.13)
--txt:   #f0eeff   --txt2: rgba(240,238,255,0.6)   --txt3: rgba(240,238,255,0.34)
--violet:  #7c5cff   ★ acento (tabs activos, franja y punto de no-leído, links)
--magenta: #e85cff   (tipo "reacción")
--green:#2db87a  --red:#e85b7a  --amber:#e8a030  --teal:#38bcd8
```
Helper `tint(hex, alphaHex)` → `hex+alpha` (ej. `tint('#7c5cff','0d')` = fondo de fila no-leída; `tint(col,'1e')`/`'33'` = fondo/borde del ícono).

## Layout (de arriba a abajo)
1. **Glow violeta** superior (radial 124,92,255) + **arco** de 3px violeta con bloom (reemplaza el arco cyan del resto de la app por coherencia de marca en esta pantalla — si preferís mantener el cyan global, dejá el arco cyan).
2. **Header** (`paddingTop:insetTop`): botón **← volver** (40×40 radio 12, `rgba(255,255,255,.06)` borde `--line2`) · título **"Notificaciones"** 19px/700 + subtítulo "N sin leer" 11.5px `--txt2` · acción derecha **"Marcar leídas"** 12px/600 violeta con ícono check.
3. **Tabs** (pills radio 20): **Todas** (con contador total) / **Sin leer** (con contador). Activa = fondo violeta, texto blanco, sombra `0 6px 16px rgba(124,92,255,.4)`, contador en pill translúcido blanco; inactiva = `--bg3`, `--txt2`.
4. **Lista** scrolleable (scrollbar oculta), agrupada. Cada grupo: header de sección 10.5px/700 UPPERCASE `--txt3` ("HOY", "AYER", "ESTA SEMANA") + filas.

## Fila de notificación `<Row>`
- Contenedor `padding:13px 16px`, radio 14, `gap:13`. **No-leída** → fondo `tint('#7c5cff','0d')` + **franja violeta** de 3px a la izquierda (absolute, top/bottom 14) + **punto violeta** 9px con glow a la derecha; título 13.5px/**700**. **Leída** → fondo transparente, título 13.5px/600, sin franja/punto.
- **Ícono** 42×42 radio 12, color por tipo: fondo `tint(col,'1e')`, borde `tint(col,'33')`, ícono 20px. Si `urgent` → puntito rojo sobre el ícono.
- **Texto**: fila título (flex) + **hora** 10.5px `--txt3` a la derecha; **body** 12px `--txt2` (1 línea, ellipsis).

### Tipos → color + ícono (`TYPE`)
```
expensa  → amber  · coins      reclamo  → amber  · wrench
comunidad→ violet · chat       reserva  → green  · cal
aviso    → red    · mega       paquete  → amber  · box
asamblea → violet · vote       balance  → teal   · trend
reaccion → magenta· heart
```

## Contenido seed (`DATA`)
- **Hoy**: "Expensa de junio disponible · $84.300 · vence 10/07" (no leída) · "Nuevo aviso: Corte de agua programado · Mañana 9–13h · Administración" (no leída, urgente) · "Lucía P. comentó tu publicación" (no leída).
- **Ayer**: "Tu reclamo cambió de estado · Pérdida de agua 4°B · ahora En curso" · "Reserva confirmada · SUM Sáb 28 14–17h" · "Tenés un paquete en portería · Andreani".
- **Esta semana**: "Convocatoria a asamblea · Jue 3/07 20h" · "A 8 vecinos les gustó tu aviso" · "Se publicó el balance de mayo".

## Estados / lógica
- `tab` (`'Todas' | 'Sin leer'`): "Sin leer" filtra `x.unread` y descarta grupos vacíos.
- `unread` = cantidad de no-leídas (alimenta subtítulo y contador del tab).
- **Marcar leídas**: setea todas a `unread:false` (en producción, persistir y limpiar badge de la campana).
- **Estado vacío** (tab "Sin leer" sin pendientes): ícono check violeta + "Estás al día" + "No tenés notificaciones sin leer."
- **Tap en una fila** → navegar a la pantalla destino según `type` (expensa→Expensas, reclamo→Reclamos, reserva→Reservas, paquete→Paquetes, comunidad/reaccion→Comunidad, aviso/asamblea→aviso en Comunidad, balance→Balance) y marcarla leída.

## Integración (a implementar)
- **Linkear la campana** del header de Inicio (`inicio-shared.jsx` → `Header`, el botón con `bell` y punto rojo) para abrir esta pantalla.
- Backend de notificaciones (push), badge de no-leídas, marcar-leído por ítem y deep-link por tipo.

## Iconos
SVG outline inline (`<Icon d size color sw>`, stroke 1.8). Claves: `back, coins, chat, mega, wrench, cal, box, trend, heart, vote, check, bellOff`. Reemplazables por Lucide/Feather.

## Archivos
- `Notificaciones Mobile.html` — pantalla completa (React + Babel inline). Fuente de verdad.
- `ios-frame.jsx` — marco de iPhone que la envuelve (dependencia para correr el archivo tal cual).
