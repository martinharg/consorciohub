# Balance — Admin web · ConsorcioHub

Estado de resultados mensual del consorcio. Rediseño de la pantalla vieja (fondo oscuro con emojis y todo en `$0`) al sistema visual del **admin web** de escritorio.

Archivo: `Balance Admin.html` (React 18 + Babel inline, single-file, fuente Inter).

---

## Layout
Estructura idéntica al resto del admin web (`Gastos.html`, `Reclamos.html`, etc.):
- **Sidebar 182px** — `#0a0812`, rail cyan con bloom a la izquierda, logo ConsorcioHub, grupos `PRINCIPAL` / `GESTIÓN`, ítem activo **Balance** (`--active-bg #211e36` + texto blanco + ícono violeta), footer de usuario (Martín H. · Administrador).
- **Topbar 54px** — botón back circular a la izquierda; centrado, ícono `trending` + título "Balance".
- **Contenido** — scroll vertical, padding `18px 22px 40px`.

## Bloques (de arriba hacia abajo)
1. **Selectores de período** — dropdown Mes (ancho flexible) + Año (120px). Mismo componente `Drop` que Gastos.
2. **3 KPIs** (fila):
   - Ingresos del mes — verde `#2fbd80` — `$ 4.562.700`
   - Egresos del mes — rojo `#ef5d7d` — `$ 4.029.400`
   - Resultado del período — violeta `#7c5cff` (rojo si déficit) — `+ $ 533.300`, sub "Superávit/Déficit del mes"
3. **Flujo de saldo** (2/3) + **Fondo de reserva** (1/3):
   - Saldo anterior → + Ingresos → − Egresos → = Saldo final, con flechas entre cada paso.
   - Fondo de reserva: card con ícono `shield` teal — `$ 1.842.500`.
4. **Desglose en dos columnas**:
   - **Ingresos**: Expensas cobradas (39/48 unidades), Intereses por mora, Uso del SUM/amenities.
   - **Egresos por rubro**: Remuneraciones y cargas sociales, Suministros/servicios/seguros, Mantenimiento y reparaciones, Gastos administrativos.
   - Cada fila: ícono tinteado + label + subtítulo + monto (ingresos con `+` verde, egresos con `−`).
5. **Gráfico Ingresos vs. Egresos** — barras agrupadas, últimos 6 meses (Ene–Jun), leyenda verde/rojo, mes actual (Jun) resaltado. Animación `growBar` al entrar.
6. **Acciones** (derecha): `Exportar balance` (secundario) · `Publicar a vecinos` (violeta primario).

## Paleta
Hereda las variables del admin web:
```
--bg #0d0b16   --bg2 #15131f   --card #15131f   --sidebar #0a0812
--line rgba(255,255,255,.07)   --line2 rgba(255,255,255,.13)
--txt #f0eeff  --txt2 60%  --txt3 32%
--violet #7c5cff (acento)   --green #2fbd80   --red #ef5d7d   --amber #eaa53a   --teal #38bcd8
```
Sin gradientes de color en cards, sin emojis, íconos outline SVG, esquinas 9–14px.

## Datos (hoy mockeados en el archivo)
- `SALDO_ANT` (saldo anterior del período)
- `INGRESOS[]` — { label, sub, amt, icon, color }
- `EGRESOS[]` — { label, sub, amt, icon, color } (= rubros de Gastos)
- `TREND[]` — { m, in, eg } para el gráfico de 6 meses
- Derivados: `totalIn`, `totalEg`, `resultado = in − eg`, `saldoFinal = saldoAnt + resultado`

**Producción**: ingresos vienen de Expensas cobradas del período; egresos de los rubros cargados en Gastos. El balance es de solo lectura (se calcula), salvo las acciones Exportar / Publicar.

## Estados / interacción
- Dropdowns de mes/año (cambian el período; en el mock los datos son fijos).
- Acción **Publicar a vecinos** → debería dejar el balance visible en la sección Comprobantes/Transparencia del residente.
- Tweaks: panel informativo (de dónde sale el balance).

## Pendientes
- Conectar selección de mes/año con datos reales.
- Estado vacío (mes sin movimientos) — heredar el patrón "Sin egresos cargados en…".
- Export PDF real del estado de resultados.
