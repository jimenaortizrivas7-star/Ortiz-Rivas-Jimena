# Ortiz-Rivas-Jimena
Calculo Actuarial 2
# Resumen: Cálculo Actuarial II - Unidad I

**Preliminares probabilísticos y entorno computacional reproducible**

---

## 1. Propósito y Enfoque del Curso

- El cálculo actuarial modela obligaciones inciertas combinando **modelo matemático**, **implementación en código (Python)** y **datos reales**.
- El flujo de trabajo busca la **reproducibilidad**: cada cálculo debe poder entenderse, ejecutarse y verificarse a partir de código, datos e hipótesis.
- **Resultados de aprendizaje:** distinguir resultado, evento y variable aleatoria; usar condicional, Bayes e independencia; calcular momentos y cuantiles; reconocer las distribuciones básicas; simular con Python; mantener un repositorio en GitHub; leer una estadística oficial sin confundir conteo, tasa bruta y probabilidad individual.

---

## 2. El Problema Actuarial

- **Obligación** = evento incierto + monto + momento de pago.
- **Valor actuarial** = E[valor presente aleatorio].
- Costo aleatorio: X = B · I, con E[X] = B · q.
- **Ejemplo:** B = 500 000 y q = 0.004 dan E[X] = 2 000 por póliza.
- El valor esperado **no es una prima comercial**: faltan gastos, margen de riesgo, capital, reaseguro, etc.

---

## 3. Entorno Computacional y Git

- **Herramientas base:** Python 3, Git, GitHub (con autenticación de dos factores) y Visual Studio Code con las extensiones Python y Jupyter.
- **Trazabilidad:** fuente de datos → transformación → modelo → resultado → reporte.
- **Entorno virtual:** `.venv` y `requirements.txt` (generado con `pip freeze`); seleccionar el intérprete de `.venv` en VS Code.
- **Estructura del repositorio:** `data/` (con `raw/` y `processed/`), `notebooks/`, `src/`, `tests/`, `figures/`, `reports/` y `.github/workflows/`.
- **Ciclo diario de Git:** `git status`, `git add`, `git commit`, `git pull` y `git push`, con commits coherentes (`feat:`, `data:`, `test:`, `notes:`).
- **Buenas prácticas:**
  - Los datos crudos de `raw/` **jamás se editan manualmente**.
  - Nunca subir contraseñas, tokens, llaves ni datos personales o sensibles.
  - Ignorar con `.gitignore`: `.venv/`, `__pycache__/`, `.env` y datos grandes.
- **Automatización:** pruebas con `pytest` y **GitHub Actions** (Python 3.12) que corren en cada push.
- **README:** debe explicar el problema, los datos, la versión de Python, la instalación, la ejecución, los resultados y las hipótesis.

---

## 4. Python Mínimo para Modelar Riesgo

- **NumPy:** cálculo vectorizado (`q * benefit`).
- **Pandas:** tablas de datos, por ejemplo una cartera con columna `expected_cost`.
- **Matplotlib:** la gráfica como herramienta de diagnóstico.
- **SciPy:** distribuciones (`binom.pmf`, `binom.cdf`, `poisson.cdf`).
- **Semillas:** `default_rng(31415)` permite reproducir la simulación; no la hace "más aleatoria".

---

## 5. Lenguaje de Probabilidad

- **Espacio de probabilidad:** terna (Ω, ℱ, ℙ), con ℙ(Ω) = 1, ℙ ≥ 0 y σ-aditividad.
- **σ-álgebra:** contiene a Ω, al complemento y a las uniones numerables.
- **Operaciones:** ℙ(Aᶜ) = 1 − ℙ(A) y ℙ(A ∪ B) = ℙ(A) + ℙ(B) − ℙ(A ∩ B) (ej.: 0.08 + 0.05 − 0.015 = 0.115).
- **Probabilidad condicional:** ℙ(A | B) = ℙ(A ∩ B) / ℙ(B). Condicionar cambia el universo de referencia.
- **Probabilidad total:** segmentos 60 % / 40 % con frecuencias 0.02 y 0.06 dan ℙ(C) = 0.036.
- **Teorema de Bayes:** ℙ(B | C) = 2/3. El segmento B es 40 % de la cartera pero origina dos terceras partes de las reclamaciones. Informativo no significa causal.
- **Independencia:** ℙ(A ∩ B) = ℙ(A) · ℙ(B). Es un **supuesto**, no una consecuencia. Eventos disjuntos con probabilidad positiva **no** son independientes. Falla ante epidemias, catástrofes o inflación médica.

---

## 6. Variables Aleatorias

- Una variable aleatoria es una función medible X: Ω → ℝ (la regla es fija; lo incierto es el resultado).
- **Discretas:** función de masa p(x) = ℙ(X = x). **Continuas:** densidad f(x), y ℙ(X = x) = 0.
- **CDF:** F(x) = ℙ(X ≤ x); no decreciente, continua por la derecha, y f = F′.
- **Indicadores:** E[1_A] = ℙ(A). Si N = I₁ + … + Iₙ, entonces E[N] = ℙ(I₁ = 1) + … + ℙ(Iₙ = 1), sin requerir independencia.

---

## 7. Esperanza, Varianza y Dependencia

- **Esperanza:** definida para discretas y continuas; **LOTUS** para E[g(X)].
- **Linealidad:** E[aX + b] = a·E[X] + b y la esperanza de una suma es la suma de esperanzas, **sin requerir independencia**.
- **Varianza:** Var(X) = E[X²] − E[X]² y Var(aX + b) = a² · Var(X).
- **Covarianza:** Cov(X, Y) = E[XY] − E[X]·E[Y], y Var(X + Y) = Var(X) + Var(Y) + 2·Cov(X, Y). La dependencia común infla el riesgo agregado.
- **Ley iterada:** E[X] = E[E[X | Y]] y Var(X) = E[Var(X | Y)] + Var(E[X | Y]) (variabilidad dentro de grupos + entre grupos).

---

## 8. Cuantiles y Transformaciones de Pérdidas

- **Cuantil:** qα = inf{x : F(x) ≥ α}. La media no describe la cola.
- **Deducible:** pago (X − d)₊ = máx{X − d, 0}, con E[(X − d)₊] igual a la integral de ℙ(X > x) para x desde d hasta ∞.
- **Límite:** pago mín{X, u}. Con deducible y límite, primero se define la función de pago y después se programa.

---

## 9. Distribuciones Discretas

| Distribución | Esperanza | Varianza | Notas |
|---|---|---|---|
| Bernoulli(p) | p | p(1 − p) | Ocurre / no ocurre |
| Binomial(n, p) | np | np(1 − p) | Suma de Bernoulli independientes |
| Poisson(λ) | λ | λ | Revisar sobredispersión |
| Geométrica(p) | 1/p | (1 − p)/p² | Sin memoria |

- **Ejemplo binomial:** n = 50, q = 0.04 da E = 2, Var = 1.92 y ℙ(N = 2) ≈ 0.2762.
- Con probabilidades distintas pⱼ aparece la **Poisson-binomial**.
- **Poisson:** límite de la binomial (n grande, p pequeño). Si Var ≫ E hay sobredispersión y conviene una binomial negativa o una mezcla.

---

## 10. Distribuciones Continuas

| Distribución | Esperanza | Varianza | Notas |
|---|---|---|---|
| Uniforme(a, b) | (a + b)/2 | (b − a)²/12 | Base de la simulación (X = F⁻¹(U)) |
| Exponencial(λ) | 1/λ | 1/λ² | Sin memoria, intensidad μ(t) = λ |
| Gamma(α, θ) | αθ | αθ² | Severidades sesgadas; α = 1 da la exponencial |
| Normal(μ, σ²) | μ | σ² | Admite negativos; útil en sumas (TCL) |

- La exponencial es demasiado restrictiva para mortalidad humana en muchas edades.
- La normal no siempre sirve para costos individuales positivos con cola pesada.
- **No elegir una distribución por costumbre:** revisar soporte, cola, media-varianza, mecanismo generador y ajuste.

---

## 11. Riesgo Agregado

- **Suma de pérdidas:** S = X₁ + … + Xₙ con E[S] = E[X₁] + … + E[Xₙ]; con independencia, Var(S) = Var(X₁) + … + Var(Xₙ).
- **Caso iid:** E[S] = nμ, Var(S) = nσ² y coeficiente de variación (1/√n)·(σ/μ) (diversificación bajo independencia).
- **Modelo colectivo:** S = X₁ + … + X_N, con N aleatorio. Entonces E[S] = μ·E[N] y Var(S) = E[N]·σ² + Var(N)·μ².
- **Compound Poisson:** E[S] = λμ y Var(S) = λ(σ² + μ²).

---

## 12. Ley de los Grandes Números y Simulación

- El promedio muestral converge a μ; la frecuencia empírica fluctúa y converge sin ser monótona (simulación Bernoulli con p = 0.08).
- Una cartera grande **no elimina el riesgo sistemático** (choques comunes).

---

## 13. Datos de México: INEGI (EDR)

- **EDR 2024:** 819 672 defunciones registradas, 74 variables (archivo DEFUN24), 4 930 fuentes informantes y tasa bruta nacional de **630 por 100 mil**.

| Año | Defunciones registradas | Tasa por 100 mil |
|---|---|---|
| 2015 | 655 688 | 536 |
| 2016 | 685 766 | 555 |
| 2017 | 703 047 | 563 |
| 2018 | 722 611 | 574 |
| 2019 | 747 784 | 588 |
| 2020 | 1 086 743 | 860 |
| 2021 | 1 122 249 | 879 |
| 2022 | 847 716 | 659 |
| 2023 | 799 869 | 619 |
| 2024 | 819 672 | 630 |

- **Conteo ≠ tasa bruta ≠ probabilidad qₓ:** 630 por 100 mil equivale a 0.0063, pero agrega edades y sexos; qₓ está condicionada a la edad.
- **Ejemplo:** 10 000 exposiciones con esa tasa dan 63, solo un benchmark agregado, no una tarifa.
- **Ocurrencia vs. residencia:** CDMX tuvo la tasa más alta (863) y Quintana Roo la menor (490) por entidad de ocurrencia; es un caso de sesgo de selección.
- **Leer el diccionario antes del CSV:** `EDAD` mezcla minutos, horas, días, meses y años. Flujo: `raw` → script → `processed` → comparar con cifras oficiales.

---

## 14. Ejemplos Resueltos

- **Al menos una reclamación en 5 años** (p = 0.03): 1 − 0.97⁵ ≈ 0.1413.
- **Binomial(100, 0.02):** ℙ(N = 3) con `stats.binom.pmf`.
- **Poisson(4.5):** ℙ(N > 7) = 1 − cdf(7).
- **Exponencial(0.25):** ℙ(T > 3) = e^(−0.75) ≈ 0.4724 y E[T] = 4.
- **Deducible exponencial:** E[(X − d)₊] = e^(−λd)/λ ≈ 6 065.31 (con λ = 1/10 000 y d = 5 000).
- **Compound Poisson(120)** con media 8 000 y desviación estándar 12 000: E[S] = 960 000, Var(S) = 24 960 000 000 y SD ≈ 157 987.

---

## 15. Errores Conceptuales a Evitar

1. Confundir densidad con probabilidad.
2. Confundir una tasa agregada con qₓ.
3. Usar independencia por comodidad.
4. Elegir Poisson solo porque la variable es un conteo.
5. Interpretar la esperanza como una predicción.
6. Redondear demasiado pronto.
7. Editar datos crudos.
8. Subir información sensible a GitHub.
9. Guardar solamente el notebook.
10. Concluir "Python dio este número" sin explicación matemática.

---

## 16. Práctica Guiada

- **Parte A (repositorio):** cuenta con 2FA, `.venv`, `requirements.txt`, estructura de carpetas, al menos 4 commits, `pytest -q` sin errores y Actions en verde.
- **Parte B (notebook):** `01_probabilidad_actuarial.ipynb` con Bernoulli, binomial, Poisson, exponencial, grandes números, lectura de datos y sección final "Conclusiones actuariales".
- **Parte C (datos de México):** DataFrame EDR 2015-2024, dos gráficas, cambio porcentual 2023-2024, máximo de la serie, explicar por qué 630 no es qₓ y citar la fuente.
- **Parte D (entrega reproducible):** otra persona debe poder clonar, crear un entorno limpio, instalar dependencias y ejecutar todo en orden.

---

## 17. Ejercicios y Lista de Verificación

- **31 ejercicios:** A fundamentos (1-7), B momentos (8-12), C distribuciones (13-18), D agregados (19-22), E México (23-27), F GitHub (28-31).
- **Lista de verificación:** distinguir Ω, evento y variable aleatoria; usar condicional y Bayes; saber cuándo la independencia es un supuesto; calcular momentos; simular con semilla; manejar Git y `.venv`; documentar fuentes; no confundir tasa bruta con qₓ.

---

## Referencias

- INEGI: Estadísticas de Defunciones Registradas (EDR) 2024 y archivo DEFUN24.
- GitHub Docs, VS Code, Python (entornos virtuales) y Pro Git.
- Dickson, Hardy y Waters: *Actuarial Mathematics for Life Contingent Risks*.
- Klugman, Panjer y Willmot: *Loss Models: From Data to Decisions*.

