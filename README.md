# ⚽ Porra Mundial 2026

Web de clasificación en tiempo real para tu porra. Datos gestionados desde Google Sheets, publicada gratis con GitHub Pages.

---

## 🚀 Puesta en marcha (30 minutos, sin código)

### Paso 1 — Crea tu Google Sheet

Crea una hoja de cálculo en Google Sheets con **dos pestañas**.

---

#### Pestaña 1: `Partidos`

| id | fase | grupo | local | visitante | gL | gV | fecha |
|----|------|-------|-------|-----------|----|----|-------|
| 1 | Fase de grupos | A | España | Marruecos | 3 | 1 | 12 Jun |
| 2 | Fase de grupos | A | Brasil | Argentina | | | 13 Jun |
| 3 | Dieciseisavos | | España | Brasil | | | 30 Jun |
| ... | | | | | | | |
| 64 | Final | | — | — | | | 13 Jul |

**Columnas:**
- `id` → número único del partido (1, 2, 3...)
- `fase` → uno de: `Fase de grupos`, `Dieciseisavos`, `Octavos`, `Cuartos`, `Semifinal`, `3er puesto`, `Final`
- `grupo` → A, B, C... (solo fase de grupos, el resto vacío)
- `local` / `visitante` → nombre del equipo
- `gL` / `gV` → goles local / visitante (déjalo vacío si aún no se ha jugado)
- `fecha` → texto libre ("12 Jun", "20:00h", etc.)

---

#### Pestaña 2: `Pronosticos`

Cada participante tiene una fila por partido + una fila de predicciones finales + una fila de premios.

| jugador | partido_id | gL | gV | campeon | subcampeon | tercero | bota_oro | bota_plata | bota_bronce | balon_oro | balon_plata | balon_bronce |
|---------|------------|----|----|---------|------------|---------|----------|------------|-------------|-----------|-------------|--------------|
| Fran | 1 | 3 | 1 | | | | | | | | | |
| Fran | 2 | 2 | 0 | | | | | | | | | |
| Fran | predicciones | | | España | Francia | Brasil | | | | | | |
| Fran | premios | | | | | | Mbappé | Vinicius | Lewandowski | Mbappé | Pedri | Bellingham |
| Dani | 1 | 1 | 0 | | | | | | | | | |
| ... | | | | | | | | | | | | |
| _resultados | predicciones | | | | | | | | | | | |
| _resultados | premios | | | | | | | | | | | |

**Filas especiales:**
- `partido_id = predicciones` → pronóstico de campeón, subcampeón y 3.º puesto
- `partido_id = premios` → pronóstico de Bota y Balón de Oro/Plata/Bronce
- `jugador = _resultados` → **esta fila la rellenas tú** cuando se conozcan los ganadores reales. La web la usa para calcular puntos automáticamente.

---

### Paso 2 — Publica el Sheet como CSV

Para **cada pestaña** (Partidos y Pronosticos):

1. Ve a **Archivo → Compartir → Publicar en la web**
2. En el primer desplegable selecciona la pestaña (`Partidos` o `Pronosticos`)
3. En el segundo desplegable selecciona **Valores separados por comas (.csv)**
4. Haz clic en **Publicar** y copia la URL

Obtendrás dos URLs como:
```
https://docs.google.com/spreadsheets/d/XXXXX/pub?gid=0&single=true&output=csv
https://docs.google.com/spreadsheets/d/XXXXX/pub?gid=123456&single=true&output=csv
```

---

### Paso 3 — Configura el `index.html`

Abre `index.html` y busca la sección `CONFIG`:

```javascript
const CONFIG = {
  SHEET_PARTIDOS_URL: 'TU_URL_HOJA_PARTIDOS_CSV_AQUI',      // ← pega aquí
  SHEET_PRONOSTICOS_URL: 'TU_URL_HOJA_PRONOSTICOS_CSV_AQUI', // ← y aquí
  ...
};
```

---

### Paso 4 — Sube a GitHub Pages

1. Crea cuenta en [github.com](https://github.com)
2. Nuevo repositorio → nombre `porra-mundial` → **Public**
3. Sube `index.html` (arrástralo o "Add file")
4. **Settings → Pages** → Source: `main` / `root` → Guardar
5. Tu URL: `https://TU_USUARIO.github.io/porra-mundial`

---

## 📋 Uso diario

| Acción | Qué hacer |
|--------|-----------|
| Añadir pronóstico | Añade filas en `Pronosticos` |
| Actualizar resultado | Rellena `gL` y `gV` en `Partidos` |
| Declarar campeón / subcampeón / 3.º | Rellena la fila `_resultados` / `predicciones` |
| Declarar Bota y Balón de Oro | Rellena la fila `_resultados` / `premios` |
| La web se actualiza | Al instante al recargar la página |

---

## 🎯 Sistema de puntuación

### Partidos — puntos acumulativos por fase

La puntuación es **acumulativa**: si aciertas el resultado exacto, sumas signo + diferencia + exacto.

| | Fase de grupos | Dieciseisavos | Octavos | Cuartos | Semifinal | 3er puesto | Final |
|---|---|---|---|---|---|---|---|
| Signo (1/X/2) | 2 | 3 | 3 | 4 | 5 | 4 | 6 |
| + Diferencia de goles | 1 | 2 | 2 | 2 | 2 | 2 | 3 |
| + Resultado exacto | 2 | 3 | 3 | 4 | 5 | 4 | 6 |
| **Máximo por partido** | **5** | **8** | **8** | **10** | **12** | **10** | **15** |

**Ejemplo:** pronóstico 2-0, real 3-0 → signo ✓ (+2) + diferencia ✗ = **2 pts**
**Ejemplo:** pronóstico 2-0, real 2-0 → signo ✓ (+2) + diferencia ✓ (+1) + exacto ✓ (+2) = **5 pts**

### Clasificados a siguiente ronda (se añaden en la pestaña Partidos)

| | Fase de grupos | Dieciseisavos / Octavos | Cuartos | Semifinal | 3er/Final |
|---|---|---|---|---|---|
| Posición exacta en grupo | 3 | — | — | — | — |
| Equipo clasificado | 4 | 5 | 6 | 6 / 8 | — |

### Predicciones finales

| Premio | Puntos |
|--------|--------|
| Campeón del mundo | 12 |
| Subcampeón | 6 |
| 3.er puesto | 4 |

### Premios individuales

| Premio | Puntos |
|--------|--------|
| Bota de Oro (máximo goleador) | 6 |
| Bota de Plata | 4 |
| Bota de Bronce | 2 |
| Balón de Oro (mejor jugador) | 6 |
| Balón de Plata | 4 |
| Balón de Bronce | 2 |

---

## ❓ Preguntas frecuentes

**¿Los datos se actualizan solos?**
Sí, cada recarga de página lee el Google Sheet en tiempo real.

**¿Mis amigos pueden editar el Sheet?**
No les des acceso de edición. El CSV publicado es de solo lectura.

**¿Qué pasa si alguien no tiene pronóstico para un partido?**
Se muestra `?–?` y no suma puntos.

**¿Puedo añadir participantes a mitad del torneo?**
Sí, solo añade sus filas en `Pronosticos`.
