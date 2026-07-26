# Chasis · Robot Minisumo

**Documentación de diseño** 

---

## Descripción general

El chasis del robot se diseñó como una sola pieza, lo que reduce ensambles, tornillería y puntos débiles estructurales. Integra en un mismo cuerpo los alojamientos de todos los subsistemas: placa estructural de latón, sensores de línea, sensores de detección del oponente, tracción, electrónica de control y batería.

## Características de diseño

- **Cuerpo monopieza:** pensado para fabricarse como una sola pieza (impresión 3D).
- **Alojamiento para la placa de latón** de 70 × 48 × 6.3 mm, que actúa como elemento estructural y de peso.
- **Dos cavidades frontales** para los sensores de línea QTR.
- **Cinco alojamientos posteriores** para los sensores IR de detección del oponente, distribuidos como: uno central, dos a 30° de giro y dos a 90°, buscando cobertura total del frente y los costados.
- **Amplio compartimento** para la electrónica y la batería, ubicado detrás de los sensores IR.
- **Alojamientos laterales** para los motores.
- **Espacio protegido para la PCB XMotion**, contemplando el espacio de sus borneras, con una canasta que la cubre.
- **«Ojos de Venom»** en el frontal como elemento estético para dar un aspecto más intimidante.

## Especificaciones de diseño

| Parámetro | Valor |
|---|---|
| Construcción | Chasis monopieza, manufactura por impresión 3D (PLA) |
| Alojamiento placa de latón | 70 × 48 × 6.3 mm (cavidad central) |
| Sensores de línea (QTR) | 2 cavidades frontales |
| Sensores IR de oponente | 5 alojamientos posteriores: 1 central, 2 a 30° y 2 a 90° |
| Tracción | 2 motores + 2 rines de aluminio (alojamientos laterales) |
| Electrónica / batería | Compartimento amplio detrás de los sensores IR |
| Controladora | Alojamiento protegido para PCB XMotion con espacio para borneras + canasta de cubierta |
| Estética | «Ojos de Venom» integrados al frontal |

## 4 · Renders del modelo CAD

<img width="461" height="428" alt="image" src="https://github.com/user-attachments/assets/b0e4dd90-7a49-449e-b8ce-1fe20d924a9c" />

*Figura 1. Vista inferior — cavidad de la placa de latón, ventanas de sensores QTR (frontal inferior) y rines de aluminio.*

<p align="center">
  <img src="https://github.com/user-attachments/assets/5db721bd-d1c4-49b4-a9c5-20c46fb1627b"
       alt="image"
       width="400">
</p>

*Figura 2. Vista isométrica — compartimento de electrónica/batería, alojamiento de motores y rin de aluminio en el eje.*

## Lista de materiales — costos (BOM)


| # | Componente | Modelo | Prov. | Cant. | P. unit. | Subtotal (MXN) | Notas |
|--:|---|---|---|:--:|--:|--:|---|
| 1 | Motor CC Core 6V 750 rpm | JS16661 | JSumo | 2 | 16 USD | 558.40 | Agotado en JSumo al 16-jul-2026 |
| 2 | Par de ruedas aluminio-silicón | JS2622 | JSumo | 1 par | 12.95 USD | 225.98 | El renglón es un par (2 ruedas) |
| 3 | Sensor IR digital 40 cm | JS40F | JSumo | 5 | 12.50 USD | 1,090.63 | Detección de oponente |
| 4 | Sensor de contraste/borde QTR-1A | JS15435 | JSumo | 2 | 2.40 USD | 83.76 | Detección de línea blanca |
| 5 | Controladora XMotion All-In-One V3 | XMotion V3 | JSumo | 1 | 44.95 USD | 784.38 | PCB principal |
| 6 | Placa de latón (estructura) `*` | — | Local | 1 | 200 MXN | 200.00 | Estimado — según medida/calibre |

## Estado de adquisición

| Material | Cantidad | Estado |
|---|:--:|:--:|
| QTR | 2 | ✅ Disponible |
| Sensores IR | 4 | ✅ Disponible |
| Motores N20 400 RPM | 2 | ✅ Disponible |
| Navaja | 1 | ⏳ En camino |
| Rines de aluminio | 2 | ✅ Disponible |
| Arrancador | 1 | ✅ Disponible |
| PCB XMotion V3 | 1 | ✅ Disponible |
| Batería LiPo 3S | 1 | ✅ Disponible |
| Placa de latón | 1 | ✅ Disponible |
