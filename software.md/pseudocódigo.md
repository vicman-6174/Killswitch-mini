# Arquitectura del software

El firmware fue diseñado utilizando una máquina de estados sencilla.

Todas las estrategias comparten el mismo comportamiento base.

La máxima prioridad siempre corresponde a la detección del borde mediante los sensores QTR.

El pseudocódigo completo se presenta a continuación.

Prioridad de oro: la línea blanca (QTRs) SIEMPRE manda sobre los IR. Si se ve línea, se ejecuta la maniobra de borde y se ignora todo lo demás.

1. Definición de pines
// ---- QTR (línea blanca = borde del dojo) ----
QTR_IZQ        = A1
QTR_DER        = A2

// ---- IR (oponente: 1 = lo ve, 0 = nada) ----
IR_EXT_IZQ     = A4       // extremo izquierdo
IR_45_IZQ      = A5       // 45° izquierdo
IR_CENTRAL     = D4       // central (al frente)
IR_45_DER      = D0       // 45° derecho (D0 = RX del UART, ojo con el serie)
IR_EXT_DER     = D1       // extremo derecho (D1 = TX del UART)

// ---- Motores ----
MOTOR_DER_PWM  = D11
MOTOR_DER_DIR  = D13
MOTOR_IZQ_PWM  = D10
MOTOR_IZQ_DIR  = D12

// ---- Dip switch (selección de estrategia) ----
DIP1 = D5     // estrategia 1
DIP2 = D6     // estrategia 2
DIP3 = D7     // estrategia 3

// ---- Arrancador (pin A0 usado como entrada DIGITAL) ----
ARRANCADOR = A0

// ---- Pantalla OLED (I2C) ----
OLED_SDA = D2    
OLED_SCL = D3

2. Parámetros a calibrar (valores de ejemplo)
UMBRAL_BLANCO      = ???    // lectura QTR para distinguir blanco de negro

VEL_BUSQUEDA = 120          // avance lento mientras busca
VEL_ATAQUE   = 255          // full (embestida / recto)
VEL_GIRO     = 180          // velocidad al girar

RETRO_MS = 200             // cuánto retrocede al ver línea
GIRO_MS  = 250             // cuánto gira al ver línea
ARCO_MS  = 700             // duración del arco inicial (estrategia 3)
// Valor de DIR para ir al frente 
ADELANTE = HIGH
ATRAS    = LOW

3. Funciones auxiliares
funcion leerArrancador():
    // A0 se lee como entrada DIGITAL (el arrancador entrega un 1/0 limpio)
    retornar digitalRead(ARRANCADOR)

// --- Control de motores ---
funcion motores(vel_izq, dir_izq, vel_der, dir_der):
    digitalWrite(MOTOR_IZQ_DIR, dir_izq); analogWrite(MOTOR_IZQ_PWM, vel_izq)
    digitalWrite(MOTOR_DER_DIR, dir_der); analogWrite(MOTOR_DER_PWM, vel_der)

funcion adelante(v):        motores(v, ADELANTE, v, ADELANTE)
funcion retroceder(v):      motores(v, ATRAS,    v, ATRAS)
funcion girarIzquierda(v):  motores(v, ATRAS,    v, ADELANTE)   // gira sobre su eje
funcion girarDerecha(v):    motores(v, ADELANTE, v, ATRAS)
funcion parar():            motores(0, ADELANTE, 0, ADELANTE)

// --- Lectura de la línea blanca ---
funcion verLinea():
    blanco_izq = (analogRead(QTR_IZQ) indica BLANCO según UMBRAL_BLANCO)
    blanco_der = (analogRead(QTR_DER) indica BLANCO según UMBRAL_BLANCO)
    si blanco_izq Y blanco_der:  retornar AMBAS
    si blanco_izq:               retornar IZQUIERDA
    si blanco_der:               retornar DERECHA
    retornar NINGUNA

4. Reacción a la línea blanca (prioridad máxima)
funcion reaccionarLinea(lado):
    si lado == DERECHA:                        // QTR derecho vio blanco
        retroceder(VEL_ATAQUE);   esperar(RETRO_MS)
        girarIzquierda(VEL_GIRO); esperar(GIRO_MS)

    si lado == IZQUIERDA:                       // QTR izquierdo vio blanco
        retroceder(VEL_ATAQUE);   esperar(RETRO_MS)
        girarDerecha(VEL_GIRO);   esperar(GIRO_MS)

    si lado == AMBAS:                           // borde de frente -> atrás y giro izq.
        retroceder(VEL_ATAQUE);   esperar(RETRO_MS)   // retroceso considerable
        girarIzquierda(VEL_GIRO); esperar(GIRO_MS)

    // al salir, el loop retoma el avance hacia adelante

5. Comportamiento por sensores (compartido por las 3 estrategias)
funcion comportamientoPorSensores():

    // (1) La línea SIEMPRE tiene prioridad
    lado = verLinea()
    si lado != NINGUNA:
        reaccionarLinea(lado)
        retornar

    // (2) Sin línea -> buscar / atacar con los IR
    c   = digitalRead(IR_CENTRAL)
    ei  = digitalRead(IR_EXT_IZQ)
    i45 = digitalRead(IR_45_IZQ)
    ed  = digitalRead(IR_EXT_DER)
    d45 = digitalRead(IR_45_DER)

    si c == 1:
        adelante(VEL_ATAQUE)              // oponente al frente -> embestir
    sino si (ei == 1) O (i45 == 1):
        girarIzquierda(VEL_GIRO)          // oponente a la izquierda
    sino si (ed == 1) O (d45 == 1):
        girarDerecha(VEL_GIRO)            // oponente a la derecha
    sino:
        adelante(VEL_BUSQUEDA)            // nadie a la vista -> recto lento
Nota: el orden de prioridad (centro → izquierda → derecha) es ajustable. Aquí prioriza embestir de frente; si prefieres que primero corrija el ángulo, cambia el orden de los si.

6. Programa principal
// Variables globales de estado
arrancador_prev = 0
fase_inicial    = verdadero
t_inicio        = 0

funcion setup():
    configurar como SALIDA: PWM y DIR de ambos motores
    configurar como ENTRADA: los 5 IR y los 3 DIP
    iniciar OLED (I2C en OLED_SDA / OLED_SCL)
    parar()

funcion loop():
    arr = leerArrancador()

    // --- Arrancador en 0: match no iniciado o detenido ---
    si arr == 0:
        parar()
        arrancador_prev = 0
        retornar

    // --- Flanco 0 -> 1: comienza un nuevo match ---
    si arrancador_prev == 0:
        fase_inicial = verdadero
        t_inicio     = millis()
    arrancador_prev = 1

    // --- Selección de estrategia (solo UNA debe estar en 1) ---
    n = leer(DIP1) + leer(DIP2) + leer(DIP3)

    si n != 1:
        OLED: "ERROR: selecciona una sola estrategia"
        parar()
        retornar

    si leer(DIP1) == 1:
        OLED: "Estrategia 1"
        estrategia1()
    sino si leer(DIP2) == 1:
        OLED: "Estrategia 2"
        estrategia2()
    sino si leer(DIP3) == 1:
        OLED: "Estrategia 3"
        estrategia3()

7. Estrategias
// ===== ESTRATEGIA 1: puro sensores desde el inicio =====
// Si no ve a nadie, avanza recto lento hasta detectar algo.
funcion estrategia1():
    comportamientoPorSensores()


// ===== ESTRATEGIA 2: full adelante hasta la 1ª línea, luego sensores =====
funcion estrategia2():
    si fase_inicial:
        lado = verLinea()
        si lado == NINGUNA:
            adelante(VEL_ATAQUE)          // motores a fondo al frente
            retornar
        sino:
            reaccionarLinea(lado)         // topó la línea -> maniobra de borde
            fase_inicial = falso          // y a partir de aquí, por sensores
            retornar
    comportamientoPorSensores()


// ===== ESTRATEGIA 3: arco de reconocimiento al inicio, luego sensores =====
funcion estrategia3():
    si fase_inicial:
        // termina el arco por tiempo o si aparece la línea
        si (millis() - t_inicio < ARCO_MS) Y (verLinea() == NINGUNA):
            motores(VEL_GIRO, ADELANTE, VEL_ATAQUE, ADELANTE)  // arco (ajusta ruedas)
            retornar
        sino:
            fase_inicial = falso
    comportamientoPorSensores()

Resumen de la lógica
Todo arranca al encender la electrónica, pero nada se mueve hasta que el arrancador (A0) marque 1.
Al pasar de 0 → 1 se reinicia la "fase inicial" del match.
Se lee el dip switch: debe haber exactamente una estrategia activa; si no, la OLED avisa y el robot se detiene.
Cada estrategia hace su arranque especial (2 = full recto, 3 = arco) y después todas caen en el mismo control por sensores.
En el control por sensores, la línea blanca siempre gana: retrocede, gira al lado contrario y sigue de frente.
En cuanto el arrancador vuelve a 0 (desactivado), el robot se detiene por completo.
