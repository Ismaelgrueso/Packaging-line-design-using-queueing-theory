# Diseño de *packing lines* con Teoría de Colas (Allen–Cunneen, G/G/m)

Este cuaderno de Jupyter compara dos diseños de una zona de **packing** usando la aproximación de **Allen–Cunneen** (modelo **G/G/m**: *General arrivals / General service / m servers*). El objetivo es estimar el **tiempo medio de espera en cola** (`t_q`) y ver cómo cambia con la **variabilidad del proceso** (`CVP`).

- **Solución 1 (SOL1)**: **cola única → m estaciones en paralelo** (por defecto, `m=2`).
- **Solución 2 (SOL2)**: **k líneas dedicadas** (por defecto, `k=2`) con **reparto** del flujo, por ejemplo `[0.3, 0.7]`.

> Lectura de negocio: menor `t_q` ⇒ menor **CT** (*cycle time*), menor **WIP** (*work in process*, Ley de Little), mejor **servicio** y menos congestión.

---

## Fórmula (Allen–Cunneen)

$$
t_q \approx \frac{C_a^2 + C_s^2}{2}\;\cdot\;
\frac{\rho^{\sqrt{2(m+1)}-1}}{m(1-\rho)}\;\cdot s
$$

**Donde:**
- `m`: nº de estaciones en paralelo.
- `s = 1/μ`: tiempo medio de servicio (`t_p`).
- `ρ = λ / (m μ)`: **utilización (u)** = carga/capacidad (**debe ser < 1**).
- `C_a`: coef. de variación de **llegadas** (`CVA`).
- `C_s`: coef. de variación de **servicio/proceso** (`CVP`).

**Ley de Little**:
- `WIP = λ · CT` y `CT = t_q + t_p`.

---

## Qué contiene el cuaderno

1. **Datos de línea**
   - `ra`: tasa de llegadas (pedidos/min).
   - `rp`: tasa de proceso por estación (pedidos/min).
   - `ta` y `tp` están definidos, pero el cálculo usa `tp = 1/rp` para coherencia.

2. **Funciones**
   - `tq_allen_cunneen(ra, rp, m, CVA, CVP)`: calcula `t_q` para **G/G/m** con chequeo de estabilidad (`u<1`).
   - `tq_s1(...)`: *wrapper* para **cola única → m estaciones** (por defecto `m=2`).
   - `tq_s2(ra, ta, rp, tp, CVA, CVP, k=2, reparto=[...])`: **k líneas dedicadas**; calcula el `t_q` medio **ponderando** cada línea por su peso `w` (cada línea se evalúa como **G/G/1** con `λ_i = w_i · ra`).

3. **Barrido de CVP**
   - Se generan valores de `CVP` (en el notebook: `np.linspace(0, 3, 31)`), se calcula `t_q` para ambas soluciones y se construye un DataFrame con:
     - `CVP`, `SOLUCION 1`, `SOLUCION 2` y `DIFERENCIA (min) = SOLUCION 2 − SOLUCION 1`.

4. **Gráficas**
   - Curvas `t_q` vs **CVP** para **SOL1** y **SOL2**.

---

## Requisitos

```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install numpy pandas matplotlib notebook
