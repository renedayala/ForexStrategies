# 5 Estrategias de Trading Forex — Temporalidad M5

**Reglas de diseño:** máximo 2 indicadores por estrategia, lógica simétrica para compras y ventas.

> ⚠️ Esto es un framework estructural para backtesting, no una señal de trading. Antes de operar en real, valida cada estrategia con backtest/forward-test y define tu propio money management (SL, TP, tamaño de posición, máximo riesgo por operación).

---

## Estrategia 1 — Cruce de Medias + Filtro RSI
**Indicadores:** EMA 9 / EMA 21 + RSI (14)

- **Lógica:** estrategia de tendencia. La EMA rápida cruzando la lenta da la dirección; el RSI evita entrar en zonas de sobrecompra/sobreventa extremas.
- **Compra:** EMA 9 cruza por encima de EMA 21 **y** RSI > 50 (pero < 70, para evitar entrar tarde).
- **Venta:** EMA 9 cruza por debajo de EMA 21 **y** RSI < 50 (pero > 30).
- **Salida sugerida:** cierre parcial en 1:1, resto con trailing stop o cruce contrario de EMAs.
- **Mejor contexto:** mercados con tendencia clara (evitar rangos planos, da muchas señales falsas en M5).

---

## Estrategia 2 — Reversión en Bandas de Bollinger + RSI
**Indicadores:** Bandas de Bollinger (20, 2) + RSI (14)

- **Lógica:** mean reversion. Busca rebotes en los extremos de volatilidad confirmados por sobrecompra/sobreventa.
- **Compra:** precio toca o perfora la banda inferior **y** RSI < 30, esperando vela de rechazo (mecha inferior) para confirmar entrada.
- **Venta:** precio toca o perfora la banda superior **y** RSI > 70, con vela de rechazo (mecha superior).
- **Salida sugerida:** TP en la banda media (media móvil de 20), SL un poco más allá del extremo tocado.
- **Mejor contexto:** mercados laterales/rangos; evitar en tendencias fuertes (las bandas se "caminan").

---

## Estrategia 3 — Doble Confirmación de Momentum (MACD + Estocástico)
**Indicadores:** MACD (12, 26, 9) + Estocástico (14, 3, 3)

- **Lógica:** confirmación cruzada de momentum para filtrar señales falsas.
- **Compra:** MACD cruza al alza (línea MACD sobre la señal) **y** Estocástico sale de zona de sobreventa (<20) cruzando hacia arriba.
- **Venta:** MACD cruza a la baja **y** Estocástico sale de sobrecompra (>80) cruzando hacia abajo.
- **Salida sugerida:** TP en el siguiente nivel de resistencia/soporte reciente, SL bajo el último swing.
- **Mejor contexto:** funciona bien en sesiones de mayor volumen (Londres/NY) donde el momentum es más confiable en M5.

---

## Estrategia 4 — Reversión a la Media con VWAP
**Indicadores:** VWAP + RSI (14)

- **Lógica:** el precio tiende a regresar al VWAP intradía tras desviaciones extremas; el RSI confirma agotamiento.
- **Compra:** precio se aleja significativamente por debajo del VWAP **y** RSI < 30, buscando retorno hacia el VWAP.
- **Venta:** precio se aleja significativamente por encima del VWAP **y** RSI > 70.
- **Salida sugerida:** TP en el propio VWAP, SL en el último máximo/mínimo local.
- **Mejor contexto:** ideal para trading intradía dentro de una sola sesión (el VWAP se reinicia cada día); no usar de un día para otro.

---

## Estrategia 5 — Tendencia con Filtro de Fuerza (Supertrend + ADX)
**Indicadores:** Supertrend (10, 3) + ADX (14)

- **Lógica:** sigue tendencia solo cuando hay fuerza direccional real, evitando operar en mercados débiles/planos.
- **Compra:** Supertrend cambia a señal alcista (línea verde, precio cierra por encima) **y** ADX > 25 (tendencia con fuerza).
- **Venta:** Supertrend cambia a señal bajista (línea roja) **y** ADX > 25.
- **Filtro adicional:** si ADX < 20, ignorar la señal (mercado sin tendencia clara).
- **Salida sugerida:** trailing stop siguiendo la línea del Supertrend.
- **Mejor contexto:** tendencias sostenidas; es la estrategia más "limpia" de señales pero genera menos operaciones por sesión.

---

## Resumen comparativo

| # | Estrategia | Tipo | Indicadores | Frecuencia de señales |
|---|-----------|------|--------------|------------------------|
| 1 | Cruce EMA + RSI | Tendencia | EMA 9/21, RSI | Media-Alta |
| 2 | Bollinger + RSI | Reversión | BB(20,2), RSI | Media |
| 3 | MACD + Estocástico | Momentum | MACD, Stoch | Media |
| 4 | VWAP + RSI | Reversión intradía | VWAP, RSI | Media-Baja |
| 5 | Supertrend + ADX | Tendencia filtrada | Supertrend, ADX | Baja |

---

### Notas generales
- En M5 el ruido es alto: cualquiera de estas estrategias se beneficia de un filtro de sesión horaria (ej. solo operar en solape Londres-NY).
- Ninguna estrategia aquí define tamaño de posición ni gestión de riesgo — eso debe definirse aparte y aplicarse de forma consistente.
- Todas son simétricas (compra/venta usan la misma lógica espejada), lo que las hace fáciles de codificar como reglas para un sistema automatizado o semi-automatizado.
