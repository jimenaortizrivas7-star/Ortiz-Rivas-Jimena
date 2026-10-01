# Ortiz-Rivas-Jimena
Calculo Actuarial 2
# Resumen: CÃ¡lculo Actuarial II - Unidad I

**Preliminares probabilÃ­sticos y entorno computacional reproducible**

---

## 1. PropÃ³sito y Enfoque del Curso

- El cÃ¡lculo actuarial modela obligaciones inciertas combinando **modelo matemÃ¡tico**, **implementaciÃ³n en cÃ³digo (Python)** y **datos reales**.
- El flujo de trabajo busca la **reproducibilidad**: cada cÃ¡lculo debe poder entenderse, ejecutarse y verificarse a partir de cÃ³digo, datos e hipÃ³tesis.
- **Resultados de aprendizaje:** distinguir resultado, evento y variable aleatoria; usar condicional, Bayes e independencia; calcular momentos y cuantiles; reconocer las distribuciones bÃ¡sicas; simular con Python; mantener un repositorio en GitHub; leer una estadÃ­stica oficial sin confundir conteo, tasa bruta y probabilidad individual.

---

## 2. El Problema Actuarial

- **ObligaciÃ³n** = evento incierto + monto + momento de pago.
- **Valor actuarial** = E[valor presente aleatorio].
- Costo aleatorio: X = B Â· I, con E[X] = B Â· q.
- **Ejemplo:** B = 500 000 y q = 0.004 dan E[X] = 2 000 por pÃ³liza.
- El valor esperado **no es una prima comercial**: faltan gastos, margen de riesgo, capital, reaseguro, etc.

---

## 3. Entorno Computacional y Git

- **Herramientas base:** Python 3, Git, GitHub (con autenticaciÃ³n de dos factores) y Visual Studio Code con las extensiones Python y Jupyter.
- **Trazabilidad:** fuente de datos â†’ transformaciÃ³n â†’ modelo â†’ resultado â†’ reporte.
- **Entorno virtual:** `.venv` y `requirements.txt` (generado con `pip freeze`); seleccionar el intÃ©rprete de `.venv` en VS Code.
- **Estructura del repositorio:** `data/` (con `raw/` y `processed/`), `notebooks/`, `src/`, `tests/`, `figures/`, `reports/` y `.github/workflows/`.
- **Ciclo diario de Git:** `git status`, `git add`, `git commit`, `git pull` y `git push`, con commits coherentes (`feat:`, `data:`, `test:`, `notes:`).
- **Buenas prÃ¡cticas:**
  - Los datos crudos de `raw/` **jamÃ¡s se editan manualmente**.
  - Nunca subir contraseÃ±as, tokens, llaves ni datos personales o sensibles.
  - Ignorar con `.gitignore`: `.venv/`, `__pycache__/`, `.env` y datos grandes.
- **AutomatizaciÃ³n:** pruebas con `pytest` y **GitHub Actions** (Python 3.12) que corren en cada push.
- **README:** debe explicar el problema, los datos, la versiÃ³n de Python, la instalaciÃ³n, la ejecuciÃ³n, los resultados y las hipÃ³tesis.

---

## 4. Python MÃ­nimo para Modelar Riesgo

- **NumPy:** cÃ¡lculo vectorizado (`q * benefit`).
- **Pandas:** tablas de datos, por ejemplo una cartera con columna `expected_cost`.
- **Matplotlib:** la grÃ¡fica como herramienta de diagnÃ³stico.
- **SciPy:** distribuciones (`binom.pmf`, `binom.cdf`, `poisson.cdf`).
- **Semillas:** `default_rng(31415)` permite reproducir la simulaciÃ³n; no la hace "mÃ¡s aleatoria".

---

## 5. Lenguaje de Probabilidad

- **Espacio de probabilidad:** terna (Î©, â„±, â„™), con â„™(Î©) = 1, â„™ â‰¥ 0 y Ïƒ-aditividad.
- **Ïƒ-Ã¡lgebra:** contiene a Î©, al complemento y a las uniones numerables.
- **Operaciones:** â„™(Aá¶œ) = 1 âˆ’ â„™(A) y â„™(A âˆª B) = â„™(A) + â„™(B) âˆ’ â„™(A âˆ© B) (ej.: 0.08 + 0.05 âˆ’ 0.015 = 0.115).
- **Probabilidad condicional:** â„™(A | B) = â„™(A âˆ© B) / â„™(B). Condicionar cambia el universo de referencia.
- **Probabilidad total:** segmentos 60 % / 40 % con frecuencias 0.02 y 0.06 dan â„™(C) = 0.036.
- **Teorema de Bayes:** â„™(B | C) = 2/3. El segmento B es 40 % de la cartera pero origina dos terceras partes de las reclamaciones. Informativo no significa causal.
- **Independencia:** â„™(A âˆ© B) = â„™(A) Â· â„™(B). Es un **supuesto**, no una consecuencia. Eventos disjuntos con probabilidad positiva **no** son independientes. Falla ante epidemias, catÃ¡strofes o inflaciÃ³n mÃ©dica.

---

## 6. Variables Aleatorias

- Una variable aleatoria es una funciÃ³n medible X: Î© â†’ â„ (la regla es fija; lo incierto es el resultado).
- **Discretas:** funciÃ³n de masa p(x) = â„™(X = x). **Continuas:** densidad f(x), y â„™(X = x) = 0.
- **CDF:** F(x) = â„™(X â‰¤ x); no decreciente, continua por la derecha, y f = Fâ€².
- **Indicadores:** E[1_A] = â„™(A). Si N = Iâ‚ + â€¦ + Iâ‚™, entonces E[N] = â„™(Iâ‚ = 1) + â€¦ + â„™(Iâ‚™ = 1), sin requerir independencia.

---

## 7. Esperanza, Varianza y Dependencia

- **Esperanza:** definida para discretas y continuas; **LOTUS** para E[g(X)].
- **Linealidad:** E[aX + b] = aÂ·E[X] + b y la esperanza de una suma es la suma de esperanzas, **sin requerir independencia**.
- **Varianza:** Var(X) = E[XÂ²] âˆ’ E[X]Â² y Var(aX + b) = aÂ² Â· Var(X).
- **Covarianza:** Cov(X, Y) = E[XY] âˆ’ E[X]Â·E[Y], y Var(X + Y) = Var(X) + Var(Y) + 2Â·Cov(X, Y). La dependencia comÃºn infla el riesgo agregado.
- **Ley iterada:** E[X] = E[E[X | Y]] y Var(X) = E[Var(X | Y)] + Var(E[X | Y]) (variabilidad dentro de grupos + entre grupos).

---

## 8. Cuantiles y Transformaciones de PÃ©rdidas

- **Cuantil:** qÎ± = inf{x : F(x) â‰¥ Î±}. La media no describe la cola.
- **Deducible:** pago (X âˆ’ d)â‚Š = mÃ¡x{X âˆ’ d, 0}, con E[(X âˆ’ d)â‚Š] igual a la integral de â„™(X > x) para x desde d hasta âˆž.
- **LÃ­mite:** pago mÃ­n{X, u}. Con deducible y lÃ­mite, primero se define la funciÃ³n de pago y despuÃ©s se programa.

---

## 9. Distribuciones Discretas

| DistribuciÃ³n | Esperanza | Varianza | Notas |
|---|---|---|---|
| Bernoulli(p) | p | p(1 âˆ’ p) | Ocurre / no ocurre |
| Binomial(n, p) | np | np(1 âˆ’ p) | Suma de Bernoulli independientes |
| Poisson(Î») | Î» | Î» | Revisar sobredispersiÃ³n |
| GeomÃ©trica(p) | 1/p | (1 âˆ’ p)/pÂ² | Sin memoria |

- **Ejemplo binomial:** n = 50, q = 0.04 da E = 2, Var = 1.92 y â„™(N = 2) â‰ˆ 0.2762.
- Con probabilidades distintas pâ±¼ aparece la **Poisson-binomial**.
- **Poisson:** lÃ­mite de la binomial (n grande, p pequeÃ±o). Si Var â‰« E hay sobredispersiÃ³n y conviene una binomial negativa o una mezcla.

---

## 10. Distribuciones Continuas

| DistribuciÃ³n | Esperanza | Varianza | Notas |
|---|---|---|---|
| Uniforme(a, b) | (a + b)/2 | (b âˆ’ a)Â²/12 | Base de la simulaciÃ³n (X = Fâ»Â¹(U)) |
| Exponencial(Î») | 1/Î» | 1/Î»Â² | Sin memoria, intensidad Î¼(t) = Î» |
| Gamma(Î±, Î¸) | Î±Î¸ | Î±Î¸Â² | Severidades sesgadas; Î± = 1 da la exponencial |
| Normal(Î¼, ÏƒÂ²) | Î¼ | ÏƒÂ² | Admite negativos; Ãºtil en sumas (TCL) |

- La exponencial es demasiado restrictiva para mortalidad humana en muchas edades.
- La normal no siempre sirve para costos individuales positivos con cola pesada.
- **No elegir una distribuciÃ³n por costumbre:** revisar soporte, cola, media-varianza, mecanismo generador y ajuste.

---

## 11. Riesgo Agregado

- **Suma de pÃ©rdidas:** S = Xâ‚ + â€¦ + Xâ‚™ con E[S] = E[Xâ‚] + â€¦ + E[Xâ‚™]; con independencia, Var(S) = Var(Xâ‚) + â€¦ + Var(Xâ‚™).
- **Caso iid:** E[S] = nÎ¼, Var(S) = nÏƒÂ² y coeficiente de variaciÃ³n (1/âˆšn)Â·(Ïƒ/Î¼) (diversificaciÃ³n bajo independencia).
- **Modelo colectivo:** S = Xâ‚ + â€¦ + X_N, con N aleatorio. Entonces E[S] = Î¼Â·E[N] y Var(S) = E[N]Â·ÏƒÂ² + Var(N)Â·Î¼Â².
- **Compound Poisson:** E[S] = Î»Î¼ y Var(S) = Î»(ÏƒÂ² + Î¼Â²).

---

## 12. Ley de los Grandes NÃºmeros y SimulaciÃ³n

- El promedio muestral converge a Î¼; la frecuencia empÃ­rica fluctÃºa y converge sin ser monÃ³tona (simulaciÃ³n Bernoulli con p = 0.08).
- Una cartera grande **no elimina el riesgo sistemÃ¡tico** (choques comunes).

---

## 13. Datos de MÃ©xico: INEGI (EDR)

- **EDR 2024:** 819 672 defunciones registradas, 74 variables (archivo DEFUN24), 4 930 fuentes informantes y tasa bruta nacional de **630 por 100 mil**.

| AÃ±o | Defunciones registradas | Tasa por 100 mil |
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

- **Conteo â‰  tasa bruta â‰  probabilidad qâ‚“:** 630 por 100 mil equivale a 0.0063, pero agrega edades y sexos; qâ‚“ estÃ¡ condicionada a la edad.
- **Ejemplo:** 10 000 exposiciones con esa tasa dan 63, solo un benchmark agregado, no una tarifa.
- **Ocurrencia vs. residencia:** CDMX tuvo la tasa mÃ¡s alta (863) y Quintana Roo la menor (490) por entidad de ocurrencia; es un caso de sesgo de selecciÃ³n.
- **Leer el diccionario antes del CSV:** `EDAD` mezcla minutos, horas, dÃ­as, meses y aÃ±os. Flujo: `raw` â†’ script â†’ `processed` â†’ comparar con cifras oficiales.

---

## 14. Ejemplos Resueltos

- **Al menos una reclamaciÃ³n en 5 aÃ±os** (p = 0.03): 1 âˆ’ 0.97âµ â‰ˆ 0.1413.
- **Binomial(100, 0.02):** â„™(N = 3) con `stats.binom.pmf`.
- **Poisson(4.5):** â„™(N > 7) = 1 âˆ’ cdf(7).
- **Exponencial(0.25):** â„™(T > 3) = e^(âˆ’0.75) â‰ˆ 0.4724 y E[T] = 4.
- **Deducible exponencial:** E[(X âˆ’ d)â‚Š] = e^(âˆ’Î»d)/Î» â‰ˆ 6 065.31 (con Î» = 1/10 000 y d = 5 000).
- **Compound Poisson(120)** con media 8 000 y desviaciÃ³n estÃ¡ndar 12 000: E[S] = 960 000, Var(S) = 24 960 000 000 y SD â‰ˆ 157 987.

---

## 15. Errores Conceptuales a Evitar

1. Confundir densidad con probabilidad.
2. Confundir una tasa agregada con qâ‚“.
3. Usar independencia por comodidad.
4. Elegir Poisson solo porque la variable es un conteo.
5. Interpretar la esperanza como una predicciÃ³n.
6. Redondear demasiado pronto.
7. Editar datos crudos.
8. Subir informaciÃ³n sensible a GitHub.
9. Guardar solamente el notebook.
10. Concluir "Python dio este nÃºmero" sin explicaciÃ³n matemÃ¡tica.

---

## 16. PrÃ¡ctica Guiada

- **Parte A (repositorio):** cuenta con 2FA, `.venv`, `requirements.txt`, estructura de carpetas, al menos 4 commits, `pytest -q` sin errores y Actions en verde.
- **Parte B (notebook):** `01_probabilidad_actuarial.ipynb` con Bernoulli, binomial, Poisson, exponencial, grandes nÃºmeros, lectura de datos y secciÃ³n final "Conclusiones actuariales".
- **Parte C (datos de MÃ©xico):** DataFrame EDR 2015-2024, dos grÃ¡ficas, cambio porcentual 2023-2024, mÃ¡ximo de la serie, explicar por quÃ© 630 no es qâ‚“ y citar la fuente.
- **Parte D (entrega reproducible):** otra persona debe poder clonar, crear un entorno limpio, instalar dependencias y ejecutar todo en orden.

---

## 17. Ejercicios y Lista de VerificaciÃ³n

- **31 ejercicios:** A fundamentos (1-7), B momentos (8-12), C distribuciones (13-18), D agregados (19-22), E MÃ©xico (23-27), F GitHub (28-31).
- **Lista de verificaciÃ³n:** distinguir Î©, evento y variable aleatoria; usar condicional y Bayes; saber cuÃ¡ndo la independencia es un supuesto; calcular momentos; simular con semilla; manejar Git y `.venv`; documentar fuentes; no confundir tasa bruta con qâ‚“.

---

## Referencias

- INEGI: EstadÃ­sticas de Defunciones Registradas (EDR) 2024 y archivo DEFUN24.
- GitHub Docs, VS Code, Python (entornos virtuales) y Pro Git.
- Dickson, Hardy y Waters: *Actuarial Mathematics for Life Contingent Risks*.
- Klugman, Panjer y Willmot: *Loss Models: From Data to Decisions*.