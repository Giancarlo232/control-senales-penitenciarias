# Control Conceptual de Señales No Autorizadas en Centros Penitenciarios

Simulador web interactivo de **atenuación pasiva** de señal celular en centros
penitenciarios, desarrollado como proyecto del curso de Telecomunicaciones
(Universidad Mariano Gálvez de Guatemala). Modela, con las fórmulas de
ingeniería de RF reales, cómo el material y el espesor de un muro —o la
abertura de una malla metálica— pueden degradar una señal celular hasta
dejarla por debajo del umbral operativo, **sin emitir ninguna frecuencia**
(a diferencia de un inhibidor/jammer activo, que es ilegal en Guatemala bajo
el Decreto 94-96, Ley General de Telecomunicaciones, y las normativas de la
Superintendencia de Telecomunicaciones — SIT).

**Sitio en vivo:** https://giancarlo232.github.io/control-senales-penitenciarias/
**Repositorio:** https://github.com/Giancarlo232/control-senales-penitenciarias

## Qué hace el simulador

- Compara 5 bandas celulares: 2G (850 MHz), 3G (1900 MHz), 4G (2100 MHz),
  5G Sub-6 (3.5 GHz) y 5G mmWave (28 GHz).
- Compara 3 materiales de muro: concreto reforzado, ladrillo macizo y malla
  de acero (jaula de Faraday).
- Calcula, en tiempo real, la pérdida de espacio libre (FSPL), la atenuación
  del muro, la pérdida interior adicional, la potencia recibida, el piso de
  ruido y la SNR resultante.
- Muestra un corte transversal de la propagación, una gráfica comparativa de
  las 5 bandas, una gráfica de FSPL vs. distancia, un plano en planta con
  mapa de calor interactivo, y una tabla comparativa de materiales para la
  defensa técnica (1900 MHz vs. 3.5 GHz).

## Fórmulas utilizadas

**Pérdida de espacio libre (FSPL, SI):**

```
FSPL(dB) = 20·log10(d) + 20·log10(f) − 147.55
```
`d` en metros, `f` en Hz.

**Balance de potencias:**

```
Potencia_recibida = EIRP − FSPL_exterior − Atenuación_muro − Pérdida_interior
```

**Atenuación dieléctrica del concreto y el ladrillo (UIT-R P.2040,
materiales de baja pérdida):**

```
sigma = c · f_GHz^d
alfa(dB/m) ≈ 1636 · sigma / sqrt(eps')
```

**Efectividad de blindaje de la malla de acero (modelo de apertura —
guía de onda bajo corte + reflexiones múltiples):**

```
SE(dB) = max(0, 20·log10(lambda / (2a))) + 27.3 · (t / a)
```
`lambda = c/f`, `a` = abertura de la malla, `t` = grosor del alambre
(ambas en mm). A diferencia de un material dieléctrico, la malla protege
**mejor a baja frecuencia y peor a alta frecuencia**, y su efectividad no
depende del espesor del muro.

**Piso de ruido y SNR:**

```
Piso_de_ruido(dBm) = −174 + 10·log10(BW_Hz) + NF
SNR(dB) = Potencia_recibida − Piso_de_ruido
```
con NF = 7 dB y ancho de banda (BW) específico por tecnología.

## Tabla de coeficientes de atenuación dieléctrica

Calculados en el propio código con la fórmula de UIT-R P.2040 (no son
valores escritos a mano). Fuente: Recomendación UIT-R P.2040, "Effects of
building materials and structures on radiowave propagation above about
100 MHz" (parámetros ε′, c, d por material).

| Material | ε′ | c | d | Rango válido | 850 MHz* | 1900 MHz | 2100 MHz | 3.5 GHz | 28 GHz |
|---|---|---|---|---|---|---|---|---|---|
| Concreto reforzado | 5.24 | 0.0462 | 0.7822 | 1–100 GHz | ~29 dB/m | ~55 dB/m | ~59 dB/m | ~88 dB/m | ~447 dB/m |
| Ladrillo macizo | 3.91 | 0.0238 | 0.16 | 1–40 GHz | ~19 dB/m | ~22 dB/m | ~22 dB/m | ~24 dB/m | ~34 dB/m |

\* 850 MHz está fuera del rango de validación de la norma (que inicia en
1 GHz); el simulador lo marca como extrapolación.

Ambos coeficientes se limitan a un tope práctico de saturación (concreto
200 dB, ladrillo 160 dB), que representa el punto donde el ruido térmico
del receptor domina sobre cualquier absorción adicional del material.

La malla de acero **no** usa esta tabla: se modela por separado con el
modelo de apertura descrito arriba, con un tope práctico de 100 dB.

## Capturas

*(Agregar aquí 2-3 capturas de pantalla del simulador: el corte
transversal con los indicadores, el plano en planta con el mapa de calor,
y la tabla comparativa de materiales.)*

## Cómo ejecutarlo

Es una sola página HTML5 + CSS3 + JavaScript vanilla, sin dependencias de
build ni frameworks:

1. Clonar el repositorio o descargar `index.html`.
2. Abrirlo directamente en cualquier navegador moderno (doble clic, o
   `open index.html` / arrastrarlo a la ventana del navegador).
3. También está publicado en GitHub Pages, sin necesidad de instalar nada:
   https://giancarlo232.github.io/control-senales-penitenciarias/

## Alcance y limitaciones

Proyecto estrictamente de **control pasivo**: no construye, simula ni emite
ninguna interferencia activa. El modelo del muro es una simplificación de
ingeniería con fines pedagógicos (pérdida interior con exponente n=3
independiente de la frecuencia, EIRP configurable, distancias de línea
recta) y no reemplaza un estudio de propagación en sitio.

## Autor

Douglas Giancarlo Gutiérrez Morataya — carné 0900-22-5347
Curso de Telecomunicaciones, Universidad Mariano Gálvez de Guatemala
