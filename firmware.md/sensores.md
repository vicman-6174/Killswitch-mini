## Sensores de línea (QTR)

El sistema emplea **dos sensores reflectivos tipo QTR-RC** conectados a los pines digitales **D2** y **D4** del controlador **XMotion V3**. La lectura de estos sensores no se realiza mediante una conversión analógica convencional, sino mediante la **medición del tiempo de descarga de un circuito RC**, técnica característica de los sensores QTR-RC.

### Principio de funcionamiento

La función encargada de realizar la lectura es:

```cpp
int LeerQTR(int pin)
```

El procedimiento de medición consiste en:

1. Configurar el pin como salida y cargar el circuito RC durante **10 μs**.
2. Cambiar el pin al modo entrada.
3. Medir el número de iteraciones durante las cuales el pin permanece en estado lógico alto.
4. Limitar la medición a un máximo de **300 conteos**.

De esta forma:

- **Mayor tiempo de descarga → mayor valor de lectura.**
- **Menor tiempo de descarga → menor valor de lectura.**

### Rango de medición

| Superficie | Valor aproximado |
|------------|-----------------:|
| Blanco     | 0 – 199 |
| Negro      | 200 – 300 |

### Umbral de detección

El algoritmo emplea el siguiente umbral para identificar la línea blanca perimetral del dojo:

```cpp
int umbral = 199;
```

La condición utilizada es:

```cpp
if (leftLine < umbral || rightLine < umbral)
```

Cuando cualquiera de los sensores registra una lectura **inferior a 199**, el sistema interpreta que se ha detectado el borde del área de combate y ejecuta automáticamente la maniobra de evasión correspondiente.
