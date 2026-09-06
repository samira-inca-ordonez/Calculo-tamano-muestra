# Cálculo de Tamaño de Muestra - Guía Práctica

**Autor:** Jaira Samira Inca Ordoñez

Este repositorio contiene los ejercicios prácticos del artículo **"¿Cómo calcular el tamaño de muestra? Una guía práctica con ejemplos aplicados"**.

---

## ¿Cómo usar este repositorio?

### Para investigadores con experiencia en R

1. **Clona o descarga** este repositorio.
2. **Abre RStudio** y ejecuta los scripts de los escenarios que necesites.
3. **Sigue las preguntas** (a, b, c, d) dentro de cada escenario para reflexionar sobre el caso.
4. **Completa la tabla resumen** al final con tus propios cálculos.

### Para investigadores sin experiencia en R

1. No te preocupes, **puedes leer el README directamente**.
2. Cada escenario incluye:
   - El **caso clínico** (contexto realista).
   - Las **preguntas guía** (para que reflexiones).
   - El **código R** (ya escrito, solo cópialo y pégalo).
   - La **interpretación** (qué significa el resultado).
3. Si no quieres ejecutar el código, **lee directamente las interpretaciones** en negrita.

### Flujo de trabajo sugerido

1. **Primero:** Revisa la **Tabla Maestra de Decisión** (en el artículo principal) para saber qué escenario corresponde a tu estudio.
2. **Segundo:** Ve directamente al escenario que necesites usando el índice.
3. **Tercero:** Lee el caso clínico y trata de responder las preguntas por ti mismo antes de ver el código.
4. **Cuarto:** Ejecuta el código en R y compara tus respuestas con los resultados.
5. **Quinto:** Usa la tabla de **Convenciones de Cohen** para interpretar el tamaño del efecto.

---

## Contenido

| Escenario | Descripción |
|-----------|-------------|
| [Escenario 1](#escenario-1) | Media vs. valor fijo (colesterol LDL) |
| [Escenario 2](#escenario-2) | Proporción vs. valor fijo (lactancia materna) |
| [Escenario 3](#escenario-3) | Diferencia de medias, independientes (tiempo de recuperación) |
| [Escenario 4](#escenario-4) | Diferencia de proporciones, independientes (fracaso antibiótico) |
| [Escenario 5](#escenario-5) | Diferencia de medias, pareadas (rango de movimiento) |
| [Escenario 6](#escenario-6) | ANOVA una vía (dietas) |
| [Escenario 7](#escenario-7) | ANOVA una vía con datos piloto (gasto en salud) |
| [Escenario 8](#escenario-8) | ANOVA dos vías (consejería × actividad física) |
| [Reflexión final](#reflexión-final) | Tabla resumen con ajuste por pérdidas |

---

## Requisitos

Para ejecutar los scripts necesitas:

- **R** (versión 4.0 o superior)
- **Paquetes `pwr` y `pwr2`**

```r
install.packages("pwr")
install.packages("pwr2")
```

---

## Convenciones de Cohen por tipo de prueba

Para interpretar el tamaño del efecto, Cohen (1988) propuso los siguientes puntos de corte:

| Prueba | Símbolo | Pequeño | Mediano | Grande |
|--------|---------|---------|---------|--------|
| Diferencia de medias (t-test) | d | 0.2 | 0.5 | 0.8 |
| Diferencia de proporciones (arcoseno) | h | 0.2 | 0.5 | 0.8 |
| ANOVA | f | 0.10 | 0.25 | 0.40 |
| Correlación | r | 0.10 | 0.30 | 0.50 |

---

## Tabla resumen: ¿qué función de R usar?

| Escenario | Función de R | Familia |
|-----------|--------------|---------|
| 1. Media vs. valor fijo | `pwr.t.test(type = 'one.sample')` | Una muestra |
| 2. Proporción vs. valor fijo | `pwr.p.test()` | Una muestra |
| 3. Diferencia de medias, independientes | `pwr.t.test(type = 'two.sample')` | Dos muestras independientes |
| 4. Diferencia de proporciones, independientes | `pwr.2p.test()` | Dos muestras independientes |
| 5. Diferencia de medias, pareadas | `pwr.t.test(type = 'paired')` | Dos muestras pareadas |
| 6. ANOVA una vía (Cohen) | `pwr.anova.test()` | Más de dos muestras |
| 7. ANOVA una vía (datos piloto) | `power.anova.test()` | Más de dos muestras |
| 8. ANOVA dos vías | `pwr.2way()` / `ss.2way()` | Más de dos muestras, 2 factores |

---

## Cargar paquetes

```r
library(pwr)   # cálculos básicos de potencia usando tamaños de efectos y notaciones de Cohen
library(pwr2)  # cálculo para ANOVA de una y dos vías
```

---

## Escenario 1: Muestra para estimar incrementos de una media en una misma población

**Caso clínico:** Un nutricionista sabe, por estudios previos, que el nivel promedio de colesterol LDL en pacientes con dislipidemia no tratada es de **160 mg/dL**. Después de 8 semanas con una nueva dieta baja en grasas saturadas, espera que el promedio baje a **145 mg/dL**. Estudios previos reportan una desviación estándar de **20 mg/dL** para esta variable. El investigador quiere una prueba a dos colas, con un nivel de significancia de 0.05 y un poder de 80%.

**Resolución:**

| Parámetro | Valor |
|-----------|-------|
| Media inicial (μ₀) | 160 mg/dL |
| Media final esperada (μ₁) | 145 mg/dL |
| Diferencia esperada (d) | 15 mg/dL |
| Desviación estándar (σ) | 20 mg/dL |
| α | 0.05 |
| Poder | 80% |

Las hipótesis del estudio son:

   - Ho: μ = 160 (el promedio de LDL no cambia respecto al valor basal)
   - Ha: μ ≠ 160 (el promedio de LDL es distinto de 160 tras la dieta)

El tamaño del efecto estandarizado se calcula con la fórmula d = |μ₁ - μ₀| / σ, lo que da:
```r
# Complete el cálculo del efecto de diseño con los datos del caso
d = (160 - 145)/20
d
```
   - d = (160-145)/20 = 0.75 (efecto grande según convención de Cohen)

Con este efecto, el tamaño de muestra mínimo necesario se obtiene con la función pwr.t.test() para una muestra:
```r
# n: Calculamos el tamaño de muestra para el Escenario 1.
pwr.t.test(d=0.75, sig.level=0.05, power=0.80, type='one.sample')
```
   - n ≈ 15.98 → se redondea hacia arriba a 16 pacientes.

Si el investigador solo puede reclutar 5 pacientes, ¿qué poder de estudio obtendría?
```r
pwr.t.test(n=5, d=0.75, sig.level=0.05, type='one.sample')
```
   - Poder ≈ 0.2537 (25.4%), por debajo del 80% deseado.

Si desea trabajar con un poder de estudio de 80% y una muestra de 15 pacientes, ¿cuál sería el efecto de diseño mínimo detectable?

```r
pwr.t.test(n=15, sig.level=0.05, power=0.80, type='one.sample')
```
   - d ≈ 0.778 (ligeramente mayor al d=0.75 esperado; por eso con n=15 el poder cae un poco por debajo de 80%).

**Interpretación:** El estudio necesita al menos 16 pacientes para tener 80% de probabilidad de detectar una reducción de 15 mg/dL en el LDL. Con solo 5 pacientes, el poder cae a 25.4%, es decir, existe un riesgo mayor de no detectar el efecto aunque este exista realmente. Debido a que la diferencia es de un solo sujeto, se recomienda elevar el tamaño muestral a n=16 para preservar el rigor.

---

## Escenario 2: Muestra para estimar incrementos de una proporción en una misma población

**Caso clínico:** La prevalencia nacional de lactancia materna exclusiva en menores de 6 meses es de **35%**. Un equipo de salud pública en una región rural sospecha que, gracias a una intervención educativa y reciente, la prevalencia local podría haber aumentado a **45%**. Quieren verificar esta hipótesis con una prueba a dos colas, alfa=0.05 y poder=80%.

**Resolución:**

| Parámetro | Valor |
|-----------|-------|
| Prevalencia nacional (p₀) | 35% (0.35) |
| Prevalencia local esperada (p₁) | 45% (0.45) |
| Diferencia esperada (d) | 10% (0.10) |
| α | 0.05 |
| Poder | 80% |

Las hipótesis del estudio son:

   - Ho: p = 0.35 (la prevalencia local es igual a la nacional)
   - Ha: p ≠ 0.35 (la prevalencia local es distinta de la nacional)

El tamaño del efecto se calcula mediante la transformación arcoseno con la función ES.h():
```r
# Complete con las proporciones del caso (prevalencia local esperada, prevalencia nacional)
h <- ES.h(0.45, 0.35)
h
```
   - h ≈ 0.2045 (efecto pequeño-mediano)

Con este efecto, el tamaño de muestra necesario se calcula con pwr.p.test():
```r
# n: Calculamos el tamaño de muestra para el Escenario 2.
pwr.p.test(h=h, power=0.80, sig.level=0.05)
```
   - n ≈ 187.63 → se redondea a 188 madres.

Si finalmente encuestan a 150 madres, ¿qué poder de estudio alcanzarían?
```r
pwr.p.test(h=h, n=150, sig.level=0.05)
```
   - Poder ≈ 0.7071 (70.7%), por debajo del 80% deseado.

Si se desea mantener el poder en 80% con una muestra de 150, el efecto mínimo detectable sería:

```r
pwr.p.test(n=150, sig.level=0.05, power=0.80)
```

**Interpretación:** ¿Es viable esta encuesta con los recursos disponibles?

No es plenamente viable con 150 madres: el estudio necesitaría 188 para alcanzar el 80% de poder deseado, pero con 150 el poder baja a 70.7%. Es una caída importante (casi 10 puntos porcentuales), por lo que se recomienda ampliar la muestra o, si no es posible, reportar explícitamente esta limitación de poder en las conclusiones del estudio.

---

## Escenario 3: Muestra para estimar diferencia de medias de dos poblaciones independientes con varianzas desiguales

**Caso clínico:** Un equipo de cardiología quiere comparar el tiempo de recuperación (en días) después de una cirugía cardiaca entre pacientes operados con la técnica tradicional versus una técnica mínimamente invasiva. No cuentan con datos previos exactos, por lo que decidirán usar un tamaño de efecto convencional "mediano" según Cohen para su cálculo preliminar, con alfa=0.05 (dos colas) y poder=80%.

**Resolución:**

| Parámetro | Valor |
|-----------|-------|
| Tamaño del efecto (d) | 0.5 (mediano, Cohen) |
| α | 0.05 |
| Poder | 80% |
| Tipo de prueba  | Dos colas |


Las hipótesis del estudio son:

   - H₀: μ₁ = μ₂ (el tiempo de recuperación es igual en ambas técnicas)
   - Hₐ: μ₁ ≠ μ₂ (el tiempo de recuperación es distinto entre técnicas)

El uso del tamaño de efecto "mediano" de Cohen en este caso es debido a que no existen datos piloto ni literatura previa con las desviaciones estándar exactas de ambas técnicas; ante esa incertidumbre, usar la convención de Cohen (d=0.5) es una forma conservadora y estándar de hacer un cálculo preliminar.
```r
# Cálculo del efecto de diseño esperado
efecto_grupo2_grupo1 <- cohen.ES(test="t", size="medium")
efecto_grupo2_grupo1
```

Con este efecto, el tamaño de muestra necesario por grupo se calcula con pwr.t.test() para dos muestras independientes:
```r
# n: Calculamos el tamaño de muestra para el Escenario 3
pwr.t.test(d=0.5, sig.level=0.05, power=0.80, type="two.sample", alternative="two.sided")
```
   - n ≈ 63.77 → se redondea a 64 pacientes por grupo (128 en total)

Si finalmente solo consiguen reclutar 40 pacientes por grupo, ¿qué poder tendrían?
```r
# Poder con n=40 por grupo
pwr.t.test(d=0.5, n=40, sig.level=0.05, type="two.sample", alternative="two.sided")
```
   - Poder ≈ 0.5981 (59.8%), considerablemente bajo.

Si se desea mantener el poder en 80% con 40 por grupo, el efecto mínimo detectable sería:
```r
pwr.t.test(n=40, sig.level=0.05, power=0.80, type="two.sample", alternative="two.sided")
```

**Interpretación:** ¿Qué recomendaría al equipo de cardiología respecto al tamaño de muestra?

Con solo 40 pacientes por grupo el poder cae a 59.8%, muy por debajo del 80% recomendado — es decir, el estudio tendría más de un 40% de probabilidad de no detectar una diferencia real entre técnicas. Se recomienda al equipo de cardiología reclutar al menos 64 pacientes por grupo, o si eso no es factible, ser transparentes en el reporte sobre la limitación de poder del estudio.

---

## Escenario 4: Muestra para estimar diferencia de proporciones en dos poblaciones independientes

**Caso clínico:** Se sabe que la tasa de fracaso de un tratamiento antibiótico estándar para neumonía adquirida en la comunidad es de **20%**. Un nuevo antibiótico promete reducir esta tasa de fracaso a **8%**. Los investigadores plantean una prueba a dos colas con alfa=0.05 y poder=80%.

**Resolución:**

| Parámetro | Valor |
|-----------|-------|
| Tasa de fracaso estándar (p₁) | 20% (0.20) |
| Tasa de fracaso nuevo (p₂) | 8% (0.08) |
| Diferencia esperada (d) | 12% (0.12) |
| α | 0.05 |
| Poder | 80% |

Las hipótesis del estudio son:

   - Ho: p2 = p1 (la tasa de fracaso del nuevo antibiótico es igual a la del estándar)
   - Ha: p2 ≠ p1 (la tasa de fracaso del nuevo antibiótico es distinta a la del estándar)

El tamaño del efecto se calcula mediante la transformación arcoseno:
```r
# Cálculo del efecto de diseño esperado
efecto_p2_p1 <- ES.h(0.08, 0.20)
efecto_p2_p1
```
   - h = |-0.3538| (efecto mediano)

Con este efecto, el tamaño de muestra necesario por grupo se calcula con pwr.2p.test():
```r
# n: Calculamos el tamaño de muestra para el Escenario 4
pwr.2p.test(h=efecto_p2_p1, sig.level=0.05, power=0.80, alternative="two.sided")
```
   - n ≈ 125.42 → se redondea a 126 pacientes por grupo (252 en total).

Si solo consiguen 60 pacientes por grupo, ¿qué poder de estudio alcanzarían?
```r
pwr.2p.test(h=efecto_p2_p1, n=60, sig.level=0.05, alternative="two.sided")
```
   - Poder ≈ 0.4912 (49.1%), insuficiente para un ensayo clínico

Si se desea mantener el poder en 80% con 60 por grupo, el efecto mínimo detectable sería:
```r
pwr.2p.test(n=60, sig.level=0.05, power=0.80, alternative="two.sided")
```

**Interpretación:** ¿Es clínicamente relevante la diferencia que se busca detectar?

Aunque una reducción de 20% a 8% suena clínicamente relevante, en términos estadísticos no es un efecto tan "grande" (h=0.35, efecto mediano) y requiere una muestra considerable: 126 pacientes por grupo. Con solo 60 por grupo, el poder es de apenas 49%, insuficiente para un ensayo clínico serio — sería arriesgado concluir "no hay diferencia" si el resultado no es significativo con ese tamaño de muestra.

---

## Escenario 5: Muestra para estimar diferencia de medias de dos muestras dependientes con varianzas iguales

**Caso clínico:** Un fisioterapeuta mide el rango de movimiento (en grados) de la rodilla en los mismos pacientes con osteoartritis, antes y después de un programa de rehabilitación de 6 semanas. Se espera una mejora promedio de **8 grados**, con una desviación estándar de **8 grados** (asumida igual en ambas mediciones). Se plantea una prueba a dos colas, alfa=0.05 y poder=80%.

**Resolución:**

| Parámetro | Valor |
|-----------|-------|
| Mejora esperada | 8 grados |
| Desviación estándar de las diferencias (σ) | 8 grados |
| Tamaño del efecto estandarizado (d) | 1.0 |
| α | 0.05 |
| Poder | 80% |

Este es un diseño de muestras pareadas y no independientes debido a que la variable se mide dos veces en el mismo paciente (antes y después de la rehabilitación), no en dos grupos distintos de personas. Esto genera una correlación entre ambas mediciones que reduce la variabilidad de la diferencia y, por lo tanto, el tamaño de muestra necesario.

Las hipótesis del estudio son:
   - H₀: d = 0 (no hay mejora en el rango de movimiento)
   - Ha: d ≠ 0 (hay mejora en el rango de movimiento)

El tamaño del efecto estandarizado se calcula con la fórmula d = (μ₁ - μ₀) / σ, pero para muestras pareadas se usa la desviación estándar de las diferencias:
```r
# Cálculo del efecto de diseño (en valor absoluto)
efecto_pareado <- (0 - 8) / 8
efecto_pareado
```
   - d = 1 (efecto muy grande, se le toma como valor absoluto el d)

 Con este efecto, el tamaño de muestra necesario (número de pares) se calcula con pwr.t.test() para muestras pareadas:
 ```r
pwr.t.test(d=efecto_pareado, power=0.80, sig.level=0.05, type="paired", alternative="two.sided")
```
   - n ≈ 9.94 → se redondea a 10 pacientes.

Si solo consiguen 20 pacientes, ¿qué poder de estudio tendrían?
```r
pwr.t.test(d=efecto_pareado, n=20, sig.level=0.05, type="paired", alternative="two.sided")
```
   - Poder ≈ 0.9886 (98.9%), muy por encima de lo necesario.

Si se desea mantener el poder en 80% con 20 pares, el efecto mínimo detectable sería:
```r
pwr.t.test(n=20, power=0.80, sig.level=0.05, type="paired", alternative="two.sided")
```

**Interpretación:** Compare el n obtenido aquí con el que se hubiera necesitado si este fuera un diseño de dos muestras independientes (Escenario 3). ¿Por qué difieren?

```r
pwr.t.test(d=1, sig.level=0.05, power=0.80, type="two.sample", alternative="two.sided")
```

El diseño de medidas repetidas requiere una muestra notablemente menor (10 pares frente a los 34 participantes por grupo del diseño independiente) debido a la eficiencia estadística del emparejamiento. Al utilizar a cada paciente como su propio control, se elimina por completo la variabilidad interindividual —es decir, las diferencias naturales que existen entre distintas personas—. Esto reduce el término de error en el análisis y deja únicamente la variabilidad del cambio interno de cada sujeto, lo que aumenta la potencia estadística y permite detectar el mismo tamaño de efecto (d = 1) con menos de un tercio de los participantes.

---

## Escenario 6: Muestra para análisis de tipo ANOVA unidireccional (comparación de más de dos medias)

**Caso clínico:** Un estudio nutricional compara el nivel de glucosa postprandial (mg/dL) entre pacientes asignados a tres dietas distintas: baja en carbohidratos, mediterránea, y dieta estándar del hospital. Se espera una varianza de error de **900** (mg/dL)². No hay datos piloto disponibles, así que se usará un tamaño de efecto "mediano" convencional de Cohen para 3 grupos, con alfa=0.05 y poder=80%.

**Resolución:**

| Parámetro | Valor |
|-----------|-------|
| Número de grupos (k) | 3 |
| Tamaño del efecto (f) | 0.25 (mediano, Cohen) |
| Varianza de error | 900 (mg/dL)² |
| α | 0.05 |
| Poder | 80% |

Las hipótesis del estudio son:
   - Ho: μ1 = μ2 = μ3 (las 3 dietas producen el mismo nivel promedio de glucosa)
   - Ha: δ ≠ 0 (al menos una de las medias es diferente de las demás)

El tamaño del efecto "mediano" para ANOVA es f = 0.25:
```r
efecto_anova <- cohen.ES(test="anov", size="medium")
efecto_anova
```
   - f = 0.25

Con este efecto, el tamaño de muestra necesario por grupo se calcula con pwr.anova.test():
```r
pwr.anova.test(f=0.25, k=3, sig.level=0.05, power=0.80)
```
   - n ≈ 52.40 → se redondea a 53 pacientes por grupo (159 en total).

Si finalmente consiguen 50 pacientes por grupo, ¿qué poder tendrían?
```r
pwr.anova.test(f=0.25, k=3, n=50, sig.level=0.05)
```
   - Poder ≈ 0.779 (78.0%), muy cercano al 80% buscado.6

Si se desea mantener el poder en 80% con 50 por grupo, el efecto mínimo detectable sería:
```r
pwr.anova.test(k=3, n=50, sig.level=0.05, power=0.80)
```

**Interpretación:** ¿Cuántos participantes en total (sumando los 3 grupos) necesita el estudio?

Se necesitan aproximadamente 159 participantes en total (53 por dieta) para un poder de 80%. Con 50 por grupo (150 en total), el poder de 78% es una diferencia mínima y en la práctica muchos investigadores lo considerarían un compromiso aceptable, aunque lo ideal es completar los 3 participantes adicionales por grupo.

---

## Escenario 7: Muestra para estimar la media entre dos o más poblaciones independientes (con datos piloto)

**Caso clínico:** Un estudio piloto midió el gasto en salud mensual (soles) en tres distritos: se obtuvieron promedios de **480, 520 y 560 soles**. La varianza dentro de cada distrito (within.var) se estima en **5000**. Se desea un poder de 80% y alfa=0.05.

**Resolución:**

| Parámetro | Valor |
|-----------|-------|
| Promedios por distrito | 480, 520, 560 soles |
| Varianza dentro de cada distrito | 5000 |
| α | 0.05 |
| Poder | 80% |

Las hipótesis del estudio son:
   - Ho: μ₁ = μ₂ = μ₃ (el gasto en salud es igual en los tres distritos)
   - Ha: δ ≠ 0 (al menos un distrito tiene gasto diferente)

El tamaño de muestra necesario por grupo usando los datos del piloto se calcula con power.anova.test():
```r
grupos <- c(480, 520, 560)
p <- power.anova.test(groups=length(grupos), between.var = var(grupos), within.var=5000, power =0.80, sig.level = 0.05, n=NULL)
p
```
   - n ≈ 16.10 → se redondea a 17 por distrito (51 en total).

Para explorar la relación entre el tamaño de muestra y el poder, se puede calcular el poder para diferentes valores de n. En este caso entre 20 y 30:

b) Grafique la relación entre el tamaño de muestra y el poder de estudio para valores de n entre 20 y 30.
```r
# Exploración del poder para n entre 20 y 30
p_anova <- sapply(seq(20,30, by=1), function(n){power.anova.test(groups=3, between.var = var(grupos), within.var=5000, power = NULL, sig.level = 0.05, n=n)})
p_anova
```
```r
# Gráfico de la curva de potencia
plot(20:30, unlist(p_anova["power", ]), type = "b", pch = 19, col = "steelblue", lwd = 2, las = 1, bty = "n", xlab = "n", ylab = "Potencia", main = "Curva de Potencia")
abline(h = 0.8, linetype = 2, col = "gray")
```
   - Ver gráfico: el poder ya supera 88% desde n=20 y se acerca a 98% en n=30 (crecimiento marginal decreciente).

El tamaño del efecto observado en el estudio piloto se calcula como la diferencia de las medias entre el grupo más bajo y el grupo más alto sobre la desviación estándar común media:
```r
(560 - 480) / sqrt(5000)
```
   - Efecto = 1.13 (efecto muy grande).

**Interpretación:** ¿Cuántos distritos adicionales o participantes recomendaría añadir para asegurar un poder de al menos 85%?

El estudio piloto muestra un efecto muy grande entre distritos (1.13), lo que explica por qué se necesita una muestra pequeña (17 por distrito) para un poder de 80%. Dado que ya con n=20 se supera 88% de poder, no sería necesario agregar más distritos; con los 3 distritos actuales y 17-20 hogares encuestados por distrito, el estudio queda razonablemente bien powered.

---

## Escenario 8: Prueba de proporción de más de dos muestras (ANOVA de dos vías)

**Caso clínico:** Un estudio evalúa el efecto de dos intervenciones (factor A: tipo de consejería nutricional, con 2 niveles; factor B: nivel de actividad física recomendada, con 2 niveles) sobre la reducción de peso corporal. Cada combinación de grupo tendrá **35 participantes**. Se espera un tamaño de efecto moderado (0.25) para el factor A y grande (0.55) para el factor B, con alfa=0.05.

**Resolución:**

| Parámetro | Valor |
|-----------|-------|
| Factor A (consejería) | 2 niveles |
| Factor B (actividad física) | 2 niveles |
| Tamaño de efecto factor A (f) | 0.25 (mediano) |
| Tamaño de efecto factor B (f) | 0.55 (grande) |
| α | 0.05 |

Las hipótesis del estudio son:
   - H₀ para factor A: No hay efecto del tipo de consejería sobre la reducción de peso
   - H₀ para factor B: No hay efecto del nivel de actividad física sobre la reducción de peso
   - Hₐ: Al menos un factor tiene efecto significativo

El poder de estudio para cada factor principal con 40 participantes por combinación se calcula con pwr.2way():
```r
pwr.2way(a=2, b=2, alpha=0.05, size.A=40, size.B=40, f.A=0.25, f.B=0.55)
```
   - Poder A ≈ 0.8816 (88.2%); Poder B ≈ 0.99999996 (≈100%).

Si se desea un poder de 80% (β = 0.20) para ambos factores, el tamaño de muestra necesario por combinación se calcula con ss.2way():
```r
ss.2way(a=2, b=2, alpha=0.05, beta=0.20, f.A=0.25, f.B=0.55,
delta.A=NULL, delta.B=NULL, sigma.A=NULL, sigma.B=NULL, B=40)
```
   - n = 32 por combinación (128 en total).

**Interpretación:** ¿Es factible este estudio con los recursos típicos de un proyecto de tesis?

Con 40 participantes por combinación, el poder ya alcanza 88% para el factor A y prácticamente 100% para el factor B, superando el mínimo recomendado (n=32 por combinación para 80% de poder). Es decir, el estudio está sobre-powered para el factor B (probablemente porque su efecto esperado es grande), pero el tamaño de muestra sigue siendo razonable y factible para un proyecto de tesis.

---

## Reflexión final

Para cada uno de los 8 Escenarios anteriores, complete la siguiente tabla resumen (ajuste con 15% de pérdidas esperadas: N'' = N/(1-0.15)):

| Escenario | Escenario | n calculado | n ajustado por 15% de pérdidas esperadas |
|-----------|-----------|-------------|------------------------------------------|
| 1 | Media vs. valor fijo | 16 | 19 |
| 2 | Proporción vs. valor fijo | 188 | 222 |
| 3 | Diferencia de medias, independientes | 64 por grupo (128 total) | 76 por grupo (152 total) |
| 4 | Diferencia de proporciones, independientes | 126 por grupo (252 total) | 149 por grupo (298 total) |
| 5 | Diferencia de medias, pareadas | 10 pares | 12 pares |
| 6 | ANOVA una vía (Cohen) | 53 por grupo (159 total) | 63 por grupo (189 total) |
| 7 | ANOVA una vía (datos piloto) | 17 por grupo (51 total) | 20 por grupo (60 total) |
| 8 | ANOVA dos vías | 32 por combinación (128 total) | 38 por combinación (152 total) |

**Recuerda:** Cuando el estudio tiene múltiples fuentes de pérdida (rechazo, pérdida en seguimiento, datos incompletos, etc.), el ajuste se realiza de forma compuesta. Si las tasas de pérdida son q₁, q₂, q₃, ..., la retención total es (1 - q₁) × (1 - q₂) × (1 - q₃) × ... y la muestra ajustada se calcula como n / retención_total.

---

## Créditos

Desarrollado por **Jaira Samira Inca Ordoñez** como parte de la guía práctica de cálculo de tamaño de muestra.
