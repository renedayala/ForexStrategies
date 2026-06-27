# 5 Estrategias de Trading Forex — Temporalidad M15 (Set 2, ajustado)

**Reglas de diseño:** máximo 2 indicadores por estrategia, lógica simétrica compra/venta. Ajustadas desde la versión M10 conservando la mayoría de periodos (son medidas normalizadas, no dependen del TF) y agregando comisión/slippage configurables + filtro de sesión horaria en el código para validación más realista.

---

## Estrategia A — Ruptura de Canal Donchian + Filtro ADX
**Indicadores:** Canal Donchian + ADX

| Parámetro | Estándar (M15) | Ventana equivalente |
|---|---|---|
| Donchian | 20 | 14 |
| ADX | 14 | 10 |
| Umbral ADX | 25 | 25 |

- **Compra:** cierre rompe por encima del máximo de Donchian de la vela anterior **y** ADX > umbral.
- **Venta:** cierre rompe por debajo del mínimo de Donchian de la vela anterior **y** ADX > umbral.
- **Salida:** trailing en la línea media del canal.

---

## Estrategia B — Parabolic SAR + CCI
**Indicadores:** Parabolic SAR + CCI

| Parámetro | Estándar (M15) | Ventana equivalente |
|---|---|---|
| SAR (start/inc/max) | 0.02 / 0.02 / 0.2 | sin cambio (no depende del TF) |
| CCI | 20 | 14 |

- **Compra:** el precio cruza por encima del SAR (flip alcista) **y** CCI > 0.
- **Venta:** el precio cruza por debajo del SAR (flip bajista) **y** CCI < 0.
- **Salida:** trailing en el propio SAR.

---

## Estrategia C — Canales de Keltner + Awesome Oscillator
**Indicadores:** Keltner Channels + Awesome Oscillator

| Parámetro | Estándar (M15) | Ventana equivalente |
|---|---|---|
| EMA base Keltner | 20 | 14 |
| ATR Keltner | 10 | 7 |
| Multiplicador ATR | 2 | 2 |
| Awesome Oscillator | 5/34 | sin cambio (valores clásicos universales) |

- **Compra:** cierre rompe la banda superior de Keltner **y** AO > 0.
- **Venta:** cierre rompe la banda inferior de Keltner **y** AO < 0.
- **Salida:** TP en la media, SL en la banda contraria.

---

## Estrategia D — Williams %R + Filtro de Volatilidad ATR
**Indicadores:** Williams %R + ATR

| Parámetro | Estándar (M15) | Ventana equivalente |
|---|---|---|
| Williams %R | 14 | 9 |
| ATR (filtro) | 14 | 9 |
| Base SMA del ATR | 50 | 33 |

- **Compra:** %R sale de sobreventa (-80) **y** ATR actual > su promedio (hay suficiente volatilidad).
- **Venta:** %R sale de sobrecompra (-20) **y** ATR actual > su promedio.
- **Salida:** TP/SL en múltiplos de ATR fijados al momento de la entrada (no recalculados cada vela).

---

## Estrategia E — Hull Moving Average + Rate of Change (ROC)
**Indicadores:** HMA + ROC

| Parámetro | Estándar (M15) | Ventana equivalente |
|---|---|---|
| HMA | 20 | 14 |
| ROC | 10 | 7 |

- **Compra:** la HMA gira hacia arriba **y** el ROC cruza por encima de 0, en la misma vela.
- **Venta:** la HMA gira hacia abajo **y** el ROC cruza por debajo de 0, en la misma vela.
- **Salida:** cuando la HMA vuelve a girar en contra.

---

## Qué se agregó en el código Pine respecto a la versión M10

1. **Selector "Modo de periodos"** (`input.string`): alterna entre los valores Estándar y Ventana equivalente de la tabla de cada estrategia, sin tener que editar cada input por separado.
2. **Comisión y slippage configurables** directamente en la declaración `strategy()` (grupo "Backtest"), para que el primer test ya incluya costos realistas en vez de spread/comisión en cero.
3. **Filtro de sesión horaria** (`input.session`): por defecto cubre todo el día (`0000-2400`), pero puedes restringirlo (ej. `0800-1700` para Londres-NY) sin tocar el resto del código.

## Recordatorio de validación
Sigue aplicando lo mismo de antes: revisa número de operaciones (100+), Profit Factor y Max Drawdown (no solo % de aciertos), y prueba en más de un par/periodo antes de confiar en el resultado de cualquiera de las 5.
