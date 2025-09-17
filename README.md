

```markdown

## 🚀 Introducción

Este repositorio presenta una suite completa de algoritmos heurísticos para la **colocación de Funciones de Red Virtualizadas (VNF)** y **enrutamiento de servicios** en entornos de **Redes Terrestres-No Terrestres (TN-NTN) integradas**, completamente alineados con la formulación ILP formal documentada.

### Problema Abordado

El desafío principal es optimizar el despliegue de VNF y el enrutamiento de tráfico en una infraestructura heterogénea de tres capas:

- **🌍 Capa Terrestre (V_G)**: Nodos estáticos de alta capacidad
- **✈️ Capa Aérea (V_A)**: Nodos móviles con recursos moderados  
- **🛰️ Capa Espacial (V_S)**: Constelación multi-órbita (LEO, MEO, GEO)

### Objetivos de Optimización

- ⚡ **Minimización de latencia extremo a extremo**
- 🔋 **Minimización del consumo energético**
- 📊 **Maximización de la utilización de recursos**
- 🎯 **Optimización multi-objetivo balanceada**

## 🏗️ Arquitectura del Sistema

### Modelo de Red Formal

La red se representa como un **grafo dependiente del tiempo**:

```
G^t = (V, E^t)
```

Donde:
- **V**: Conjunto de nodos = V_G ∪ V_A ∪ V_S
- **E^t**: Conjunto de enlaces en el tiempo t
- **t ∈ T**: Slots de tiempo discretos

### Variables de Decisión

#### **Variables Principales**:
- **x^s_{i,v} ∈ {0,1}**: VNF i del servicio s colocada en nodo v
- **f^{s,i}_{uv}(t) ∈ {0,1}**: Flujo entre VNF i y i+1 del servicio s por enlace (u,v) en tiempo t

#### **Parámetros del Sistema**:
- **D^s_i**: Demanda de recursos de VNF i para servicio s (mCPU, MB)
- **R^s_i**: Tasa de datos entre VNF i y i+1 para servicio s (bps)
- **L^s_max**: Latencia máxima permitida para servicio s (ms)
- **C_v**: Capacidad computacional del nodo v
- **B_{uv}(t)**: Capacidad de ancho de banda del enlace (u,v)
- **L_{uv}(t)**: Latencia de propagación del enlace (u,v)

## 🔒 Restricciones Matemáticas

### Restricción 1: Capacidad Nodal
```
∑_s ∑_i D^s_i · x^s_{i,v} ≤ C_v, ∀v ∈ V
```
**Descripción**: Los recursos consumidos por todas las VNF en un nodo no pueden exceder su capacidad.

### Restricción 2: Capacidad de Enlace
```
∑_s ∑_i R^s_i · f^{s,i}_{uv}(t) ≤ B_{uv}(t), ∀(u,v) ∈ E^t, ∀t ∈ T
```
**Descripción**: El ancho de banda total usado en un enlace no puede exceder su capacidad.

### Restricción 3: Disponibilidad de Enlace
```
f^{s,i}_{uv}(t) = 0 si (u,v) ∉ E^t
```
**Descripción**: Solo se pueden usar enlaces que existen en la topología en tiempo t.

### Restricción 4: Conservación de Flujo y Orden SFC
```
∑_{u∈V} f^{s,i}_{uv}(t) - ∑_{w∈V} f^{s,i}_{vw}(t) = δ^s_{i,v}, ∀v ∈ V, ∀s, ∀i
```
**Descripción**: El flujo entrante debe igual al flujo saliente en nodos intermedios.

### Restricción 5: Latencia Extremo a Extremo
```
∑_{(u,v)∈E} L_{uv}(t) · f^{s,i}_{uv}(t) + ∑_{v∈V} L_{proc,v} · x^s_{i,v} ≤ L^s_max
```
**Descripción**: La latencia total (propagación + procesamiento) no puede exceder el límite del servicio.

## 🧮 Algoritmos Propuestos

### Algoritmo 1: TN-NTN VNF Placement Heuristic (Actualizado)

**Propósito**: Algoritmo heurístico principal con notación formal y validación de restricciones.

#### Características Principales:
- ✅ **Notación formal**: Usa variables x^s_{i,v} y f^{s,i}_{uv}(t)
- ✅ **Validación completa**: Verifica todas las 5 restricciones
- ✅ **Clasificación jerárquica**: Prioriza capas según capacidad
- ✅ **Adaptación temporal**: Incrementa horizonte temporal si es necesario

#### Pseudocódigo Simplificado:
```
Para cada servicio s ordenado por latencia:
  Calcular tiempo mínimo T_min^s
  Para t desde T_min^s hasta T:
    Generar rutas candidatas P_s^t
    Para cada ruta p en P_s^t:
      Colocar VNF con restricciones (X_s, F_s)
      Si ValidateConstraints(X_s, F_s, s):
        Aceptar servicio y actualizar recursos
        break
```

### Algoritmo 2: Validación de Restricciones (Nuevo)

**Propósito**: Verificación formal de todas las restricciones del problema.

#### Restricciones Verificadas:
1. ✅ **Capacidad Nodal**: `∑_s ∑_i D^s_i · x^s_{i,v} ≤ C_v`
2. ✅ **Capacidad de Enlace**: `∑_s ∑_i R^s_i · f^{s,i}_{uv}(t) ≤ B_{uv}(t)`
3. ✅ **Disponibilidad de Enlace**: `f^{s,i}_{uv}(t) = 0 si (u,v) ∉ E^t`
4. ✅ **Conservación de Flujo**: `∑_u f^{s,i}_{uv}(t) - ∑_w f^{s,i}_{vw}(t) = δ^s_{i,v}`
5. ✅ **Latencia Extremo a Extremo**: `∑ L_{uv}(t) · f^{s,i}_{uv}(t) + ∑ L_{proc,v} · x^s_{i,v} ≤ L^s_max`

### Algoritmo 3: Colocación Jerárquica de VNF

**Propósito**: Estrategia de colocación que respeta la jerarquía de recursos TN-NTN.

#### Jerarquía de Colocación:
1. **🥇 Nodos Terrestres (V_G)**: Primera prioridad (alta capacidad, baja latencia)
2. **🥈 Nodos GEO (V_S)**: Segunda prioridad (estables, capacidad moderada)
3. **🥉 Nodos MEO (V_S)**: Tercera prioridad (capacidad limitada, movilidad media)
4. **🏃 Nodos LEO (V_S)**: Último recurso (recursos muy limitados, alta movilidad)

### Algoritmo 4: Optimización Multi-Objetivo (Formalizado)

**Función Objetivo**:
```
min(α · P_total - β · U_total)
```

Donde:
- **P_total**: Consumo energético total
- **U_total**: Utilización total de recursos = `∑_{v∈V} (∑_s ∑_i D^s_i · x^s_{i,v})/C_v`
- **α, β**: Pesos de optimización

### Algoritmos Adicionales:

#### **Algoritmo 5**: Maximización de Utilización de Recursos
#### **Algoritmo 6**: Minimización de Consumo Energético  
#### **Algoritmo 7**: Reoptimización Dinámica
#### **Algoritmo 8**: Generación de Variables de Conservación de Flujo
#### **Algoritmo 9**: Cálculo de Tiempo Mínimo
#### **Algoritmo 10**: Maximización de Throughput y Utilización

## ✅ Verificación de Restricciones

### Estado de Implementación

| Restricción | Estado | Algoritmo | Líneas | Fórmula Matemática |
|-------------|--------|-----------|--------|-------------------|
| **Capacidad Nodal** | ✅ Completa | Algoritmo 2 | 4-9 | `∑_s ∑_i D^s_i · x^s_{i,v} ≤ C_v` |
| **Capacidad de Enlace** | ✅ Completa | Algoritmo 2 | 11-16 | `∑_s ∑_i R^s_i · f^{s,i}_{uv}(t) ≤ B_{uv}(t)` |
| **Disponibilidad de Enlace** | ✅ Completa | Algoritmo 2 | 17-23 | `f^{s,i}_{uv}(t) = 0 si (u,v) ∉ E^t` |
| **Conservación de Flujo** | ✅ Completa | Algoritmo 2 + 8 | 18-25 | `∑_u f^{s,i}_{uv}(t) - ∑_w f^{s,i}_{vw}(t) = δ^s_{i,v}` |
| **Latencia Extremo a Extremo** | ✅ Completa | Algoritmo 2 | 27-31 | `∑ L_{uv}(t) · f^{s,i}_{uv}(t) + ∑ L_{proc,v} · x^s_{i,v} ≤ L^s_max` |

### Corrección Implementada: Restricción de Disponibilidad de Enlace

**Problema Identificado**: La restricción 3 estaba implícitamente manejada pero no explícitamente validada.

**Solución Implementada**:
```latex
\State \Comment{Constraint 3: Link Availability}
\For{each service $s'$, VNF pair $i$, time $t$}
  \For{each link $(u,v) \in V \times V$}
    \If{$f^{s',i}_{uv}(t) = 1$ \textbf{and} $(u,v) \notin E^t$}
      \State \Return \textbf{false}
    \EndIf
  \EndFor
\EndFor
```

## 📊 Análisis de Variables y Flujo Actualizado

### Variables Globales del Sistema (Formalizadas)

| Variable | Tipo | Descripción | Dominio |
|----------|------|-------------|---------|
| `x^s_{i,v}` | Binaria | Colocación de VNF | {0,1} |
| `f^{s,i}_{uv}(t)` | Binaria | Enrutamiento de flujo | {0,1} |
| `D^s_i` | Parámetro | Demanda de recursos | ℝ⁺ (mCPU, MB) |
| `R^s_i` | Parámetro | Tasa de datos | ℝ⁺ (bps) |
| `L^s_max` | Parámetro | Latencia máxima | ℝ⁺ (ms) |
| `C_v` | Parámetro | Capacidad nodal | ℝ⁺ (mCPU, MB) |
| `B_{uv}(t)` | Parámetro | Capacidad de enlace | ℝ⁺ (bps) |
| `L_{uv}(t)` | Parámetro | Latencia de enlace | ℝ⁺ (ms) |
| `δ^s_{i,v}` | Variable | Conservación de flujo | {-1, 0, +1} |

### Métricas de Evaluación Formalizadas

#### **Métricas de Rendimiento**:
- **Throughput**: `|{s : ∃ colocación factible para s}|`
- **Utilización Total**: `∑_{v∈V} (∑_s ∑_i D^s_i · x^s_{i,v})/C_v`
- **Consumo Energético**: `P_total(X, F)`
- **Latencia Promedio**: `(1/|S|) ∑_s ∑_{(u,v)∈E} L_{uv}(t) · f^{s,i}_{uv}(t)`

#### **Métricas de Calidad**:
- **Tasa de Aceptación**: `|Servicios Aceptados| / |S|`
- **Balanceamiento de Carga**: `σ²(utilización por nodo)`
- **Violaciones de Restricciones**: `0` (garantizado por validación)

## 💻 Código LaTeX Actualizado

### Algoritmo de Validación de Restricciones Completo

```latex
\begin{algorithm}
\caption{Constraint Validation for VNF Placement (Complete)}
\label{alg:constraint-validation-complete}
\begin{algorithmic}[1]
\Require Placement $X_s$, routing $F_s$, service $s$, network $G^t$
\Ensure Boolean feasibility status
\State \Comment{Constraint 1: Node Capacity}
\For{each node $v \in V$}
  \State $totalDemand \gets \sum_{s'} \sum_{i} D^{s'}_i \cdot x^{s'}_{i,v}$
  \If{$totalDemand > C_v$}
    \State \Return \textbf{false}
  \EndIf
\EndFor
\State \Comment{Constraint 2: Link Capacity}
\For{each link $(u,v) \in E^t$}
  \State $totalFlow \gets \sum_{s'} \sum_{i} R^{s'}_i \cdot f^{s',i}_{uv}(t)$
  \If{$totalFlow > B_{uv}(t)$}
    \State \Return \textbf{false}
  \EndIf
\EndFor
\State \Comment{Constraint 3: Link Availability}
\For{each service $s'$, VNF pair $i$, time $t$}
  \For{each link $(u,v) \in V \times V$}
    \If{$f^{s',i}_{uv}(t) = 1$ \textbf{and} $(u,v) \notin E^t$}
      \State \Return \textbf{false}
    \EndIf
  \EndFor
\EndFor
\State \Comment{Constraint 4: Flow Conservation}
\For{each node $v \in V$, service $s'$, VNF $i$}
  \State $inFlow \gets \sum_{u \in V} f^{s',i}_{uv}(t)$
  \State $outFlow \gets \sum_{w \in V} f^{s',i}_{vw}(t)$
  \State $\delta^{s'}_{i,v} \gets \Call{ComputeFlowDelta}{s', i, v, X_s}$
  \If{$inFlow - outFlow \ne \delta^{s'}_{i,v}$}
    \State \Return \textbf{false}
  \EndIf
\EndFor
\State \Comment{Constraint 5: End-to-End Latency}
\State $totalLatency \gets \sum_{(u,v) \in E} L_{uv}(t) \cdot f^{s,i}_{uv}(t) + \sum_{v \in V} L_{proc,v} \cdot x^s_{i,v}$
\If{$totalLatency > L^s_{\max}$}
  \State \Return \textbf{false}
\EndIf
\State \Return \textbf{true}
\end{algorithmic}
\end{algorithm}
```

### Tabla de Notación Completa

```latex
\begin{table}[h]
\centering
\caption{Complete Notation and Symbols (Aligned with Documentation)}
\label{tab:notation-complete}
\begin{tabular}{|c|l|c|}
\hline
\textbf{Symbol} & \textbf{Description} & \textbf{Domain/Units} \\
\hline
$G^t = (V, E^t)$ & Time-dependent network graph at time $t$ & Graph \\
$V_G, V_A, V_S$ & Terrestrial, aerial, and space node sets & Sets \\
$S$ & Set of service requests & Set \\
$x^s_{i,v}$ & VNF $i$ of service $s$ placed on node $v$ & $\{0,1\}$ \\
$f^{s,i}_{uv}(t)$ & Flow between VNF $i$ and $i+1$ on link $(u,v)$ & $\{0,1\}$ \\
$D^s_i$ & Resource demand of VNF $i$ for service $s$ & mCPU, MB \\
$R^s_i$ & Data rate between VNF $i$ and $i+1$ for service $s$ & bps \\
$L^s_{\max}$ & Maximum end-to-end latency for service $s$ & ms \\
$L_{proc,v}$ & Processing latency at node $v$ & ms \\
$C_v$ & Computational capacity of node $v$ & mCPU, MB \\
$B_{uv}(t)$ & Bandwidth capacity of link $(u,v)$ at time $t$ & bps \\
$L_{uv}(t)$ & Propagation latency of link $(u,v)$ at time $t$ & ms \\
$\delta^s_{i,v}$ & Flow conservation variable & $\{-1, 0, +1\}$ \\
$I_s$ & Set of VNFs in service $s$ chain & Set \\
$T_{min}^s$ & Minimum time slots required for service $s$ & Integer \\
$P_{total}$ & Total power consumption & Watts \\
$U_{total}$ & Total resource utilization & $[0,1]$ \\
$\alpha, \beta, \gamma, \delta$ & Multi-objective optimization weights & $\mathbb{R}^+$ \\
\hline
\end{tabular}
\end{table}
```


## 📚 Referencias

1. **Documentación Principal**: DocumentacionILPVNF.pdf - Formulación ILP completa
2. **Paper Base**: "ILP Formulation for VNF Placement and Service Chaining in Integrated TN-NTN Environments"
