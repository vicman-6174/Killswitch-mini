# Estrategias

El robot cuenta con tres estrategias seleccionables mediante DIP Switch.

---

## Estrategia 1

Búsqueda mediante sensores IR.

Cuando encuentra al oponente inicia una embestida frontal.

Si detecta la línea blanca ejecuta inmediatamente la maniobra de escape.

---

## Estrategia 2

Al comenzar el combate el robot acelera al máximo hacia el frente.

Después de detectar por primera vez el borde del dojo cambia al comportamiento normal basado en sensores.

---

## Estrategia 3

El robot realiza un movimiento en arco durante los primeros instantes del combate para aumentar la probabilidad de localizar al oponente.

Después utiliza el mismo algoritmo de búsqueda y ataque que las demás estrategias.

---

## Prioridad de decisiones

1. Línea blanca
2. Oponente
3. Búsqueda
