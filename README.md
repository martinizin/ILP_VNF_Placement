# ILP_VNF_Placement
Development of some heuristic algorithms for the case of study "An ILP Formulation for VNF Placement and Service Chaining in Integrated TN-NTN Environments "


```markdown

## 📋 Tabla de Contenidos

- [Introducción](#introducción)
- [Arquitectura del Sistema](#arquitectura-del-sistema)
- [Algoritmos Propuestos](#algoritmos-propuestos)
  - [Algoritmo 1: TN-NTN VNF Placement Heuristic](#algoritmo-1-tn-ntn-vnf-placement-heuristic)
  - [Algoritmo 2: Colocación Jerárquica de VNF](#algoritmo-2-colocación-jerárquica-de-vnf)
  - [Algoritmo 3: Optimización Multi-Objetivo](#algoritmo-3-optimización-multi-objetivo)
  - [Algoritmo 4: Reoptimización Dinámica](#algoritmo-4-reoptimización-dinámica)
  - [Algoritmo 5: Maximización de Utilización](#algoritmo-5-maximización-de-utilización)
  - [Algoritmo 6: Minimización Energética](#algoritmo-6-minimización-energética)
- [Análisis de Variables y Flujo](#análisis-de-variables-y-flujo)
- [Código LaTeX](#código-latex)
- [Implementación](#implementación)
- [Referencias](#referencias)
- [Contribuciones](#contribuciones)

## 🚀 Introducción

Este repositorio presenta una suite completa de algoritmos heurísticos para la **colocación de Funciones de Red Virtualizadas (VNF)** y **enrutamiento de servicios** en entornos de **Redes Terrestres-No Terrestres (TN-NTN) integradas**.

### Problema Abordado

El desafío principal es optimizar el despliegue de VNF y el enrutamiento de tráfico en una infraestructura heterogénea de tres capas:

- **🌍 Capa Terrestre**: Nodos estáticos de alta capacidad
- **✈️ Capa Aérea**: Nodos móviles con recursos moderados  
- **🛰️ Capa Espacial**: Constelación multi-órbita (LEO, MEO, GEO)

### Objetivos de Optimización

- ⚡ **Minimización de latencia extremo a extremo**
- 🔋 **Minimización del consumo energético**
- 📊 **Maximización de la utilización de recursos**
- 🎯 **Optimización multi-objetivo balanceada**

## 🏗️ Arquitectura del Sistema

### Modelo de Red

La red se representa como un **grafo dependiente del tiempo**:

```
G^t = (V, E^t)
```

Donde:
- **V**: Conjunto de nodos = V_G ∪ V_A ∪ V_S
- **E^t**: Conjunto de enlaces en el tiempo t
- **t ∈ T**: Slots de tiempo discretos

### Jerarquía de Recursos

| Capa | Capacidad Computacional | Movilidad | Latencia |
|------|------------------------|-----------|----------|
| **Terrestre (V_G)** | 🟢 Alta | 🔴 Estático | 🟢 Baja |
| **Aérea (V_A)** | 🟡 Moderada | 🟡 Media | 🟡 Media |
| **GEO (V_S)** | 🟡 Moderada | 🔴 Estático | 🔴 Alta |
| **MEO (V_S)** | 🟡 Limitada | 🟡 Media | 🟡 Media |
| **LEO (V_S)** | 🔴 Muy Limitada | 🟢 Alta | 🟢 Baja |

## 🧮 Algoritmos Propuestos

### Algoritmo 1: TN-NTN VNF Placement Heuristic

**Propósito**: Algoritmo heurístico principal que maneja la colocación de VNF considerando la dinámica temporal de la red.

#### Variables Clave:
- `G^t = (V, E^t)`: Grafo de red dependiente del tiempo
- `S`: Conjunto de solicitudes de servicios
- `Φ`: Solución de colocación y enrutamiento
- `T_min^s`: Tiempo mínimo requerido para el servicio s

#### Flujo del Algoritmo:
1. **Inicialización**: Clasificar nodos por capa y predecir conectividad
2. **Procesamiento de servicios**: Ordenar por prioridad (latencia crítica)
3. **Búsqueda temporal adaptativa**: Incrementar horizonte temporal si es necesario
4. **Colocación y enrutamiento**: Encontrar rutas factibles y colocar VNF

#### Relevancia para TN-NTN:
- ✅ Maneja topología dinámica satelital
- ✅ Considera heterogeneidad de recursos por capa
- ✅ Adaptación temporal para ventanas de conectividad

### Algoritmo 2: Colocación Jerárquica de VNF

**Propósito**: Implementa estrategia de colocación que prioriza nodos según su capacidad y confiabilidad.

#### Jerarquía de Colocación:
1. **🥇 Nodos Terrestres**: Primera prioridad (alta capacidad)
2. **🥈 Nodos GEO**: Segunda prioridad (estables, capacidad moderada)
3. **🥉 Nodos MEO**: Tercera prioridad (capacidad limitada)
4. **🏃 Nodos LEO**: Último recurso (recursos muy limitados)

#### Variables Específicas:
- `terrestrialCandidates`: Nodos terrestres candidatos
- `geoCandidates, meoCandidates, leoCandidates`: Nodos satelitales por órbita
- `placedNode`: Nodo seleccionado para colocación

#### Relevancia Crítica:
- 🎯 **Optimización de recursos**: Utiliza primero nodos con mayor capacidad
- ⚡ **Minimización de latencia**: Prioriza nodos terrestres para VNF críticas
- 🛡️ **Tolerancia a limitaciones**: Degradación gradual cuando recursos no están disponibles

### Algoritmo 3: Optimización Multi-Objetivo

**Propósito**: Balancea múltiples objetivos conflictivos mediante función de utilidad ponderada.

#### Función Objetivo:
```
score = α × NormalizeEnergy(energy) + 
        β × NormalizeUtilization(utilization) + 
        γ × NormalizeLatency(latency)
```

#### Pesos de Optimización:
- **α**: Peso para minimización energética
- **β**: Peso para maximización de utilización  
- **γ**: Peso para minimización de latencia

#### Trade-offs Considerados:
- ⚖️ **Energía vs Utilización**: Mayor utilización puede requerir más energía
- ⚖️ **Latencia vs Energía**: Rutas más cortas pueden consumir más energía
- ⚖️ **Utilización vs Latencia**: Consolidación puede aumentar latencia

### Algoritmo 4: Reoptimización Dinámica

**Propósito**: Adapta la solución cuando cambia la topología de red debido a movilidad satelital.

#### Variables de Adaptación:
- `Δ`: Conjunto de cambios topológicos
- `affectedServices`: Servicios impactados por cambios
- `G'^t`: Red actualizada después de cambios

#### Estrategia de Reoptimización:
1. **🔍 Identificación**: Detectar servicios afectados por cambios
2. **✅ Verificación**: Comprobar si colocación actual sigue siendo válida
3. **🔄 Migración**: Buscar nueva colocación si es necesario
4. **⏸️ Suspensión**: Suspender temporalmente si no hay alternativas

#### Beneficios:
- 🔄 **Continuidad de servicio**: Mantiene servicios activos durante cambios
- ⚡ **Eficiencia**: Solo reoptimiza servicios realmente afectados
- 🎯 **Gestión inteligente**: Libera y reasigna recursos dinámicamente

### Algoritmo 5: Maximización de Utilización

**Propósito**: Maximiza el aprovechamiento de recursos de infraestructura costosa.

#### Variables de Eficiencia:
- `S_sorted`: Servicios ordenados por eficiencia de recursos
- `θ_util`: Umbral mínimo de utilización
- `utilScore`: Puntuación de utilización por colocación

#### Estrategia:
1. **📊 Evaluación**: Calcular score de utilización para cada colocación
2. **🎯 Selección**: Elegir colocación con mayor utilización
3. **✅ Validación**: Verificar que supere umbral mínimo

### Algoritmo 6: Minimización Energética

**Propósito**: Minimiza consumo energético, crítico para nodos espaciales con energía limitada.

#### Variables Energéticas:
- `P`: Modelo de consumo energético
- `energyEfficientNodes`: Nodos ordenados por eficiencia energética
- `activeNodes`: Nodos ya activos en la red

#### Estrategia de Consolidación:
1. **🔋 Reutilización**: Intentar colocar en nodos ya activos
2. **⚡ Activación eficiente**: Si no es posible, activar nodo más eficiente
3. **📊 Evaluación**: Verificar que consumo esté bajo umbral

## 📊 Análisis de Variables y Flujo

### Variables Globales del Sistema

| Variable | Descripción | Relevancia TN-NTN |
|----------|-------------|-------------------|
| `G^t = (V, E^t)` | Grafo dependiente del tiempo | Captura dinamismo satelital |
| `V_G, V_A, V_S` | Nodos por capa | Arquitectura jerárquica |
| `S` | Solicitudes de servicios | Carga de trabajo |
| `Φ` | Solución de colocación | Resultado optimizado |
| `T` | Horizonte temporal | Predictibilidad orbital |

### Métricas de Evaluación

#### Métricas de Rendimiento:
- **📈 Throughput**: Servicios procesados exitosamente
- **⏱️ Latencia promedio**: Tiempo extremo a extremo
- **📊 Utilización de recursos**: Porcentaje de capacidad usada
- **🔋 Consumo energético**: Energía total consumida

#### Métricas de Calidad:
- **✅ Tasa de aceptación**: Servicios aceptados vs rechazados
- **⚖️ Balanceamiento de carga**: Distribución de carga entre nodos
- **🔄 Estabilidad**: Frecuencia de reoptimizaciones
- **🛡️ Robustez**: Tolerancia a fallos de nodos/enlaces

## 💻 Código LaTeX

### Preámbulo Requerido

```latex
\usepackage{algorithm}
\usepackage{algorithmic}
% O alternativamente:
% \usepackage[ruled,vlined]{algorithm2e}
```

### Ejemplo: Algoritmo Principal

```latex
\begin{algorithm}
\caption{TN-NTN VNF Placement Heuristic}
\label{alg:tnntv-vnf-placement}
\begin{algorithmic}[1]
\REQUIRE Network $G^t = (V, E^t)$, Service requests $S$, Time horizon $T$
\ENSURE VNF placement and routing solution $\Phi$
\STATE $\Phi \leftarrow \emptyset$
\STATE $V_G, V_A, V_S \leftarrow$ ClassifyNodesByLayer$(V)$
\STATE $M \leftarrow$ PredictConnectivity$(G^t, T)$
\STATE $S' \leftarrow$ SortServicesByPriority$(S)$
\FOR{each service $s \in S'$}
    \STATE $T_{min}^s \leftarrow \lceil \sum_{i \in I_s} \frac{\delta_s \varepsilon}{c_s \tau} \rceil$
    \STATE $t \leftarrow T_{min}^s$
    \STATE $serviceStatus \leftarrow$ FALSE
    \WHILE{$t \leq T$ AND $serviceStatus =$ FALSE}
        \STATE $P_s^t \leftarrow$ GenerateCandidatePaths$(s, G^t, t)$
        \IF{$P_s^t \neq \emptyset$}
            \FOR{each path $p \in P_s^t$}
                \STATE $placement \leftarrow$ PlaceVNFsOnPath$(s, p, G^t)$
                \IF{$placement.feasible$}
                    \STATE $\Phi \leftarrow \Phi \cup \{s, p, placement\}$
                    \STATE UpdateNetworkResources$(G^t, placement)$
                    \STATE $serviceStatus \leftarrow$ TRUE
                    \STATE \textbf{break}
                \ENDIF
            \ENDFOR
        \ELSE
            \STATE $t \leftarrow t + 1$
        \ENDIF
    \ENDWHILE
    \IF{$serviceStatus =$ FALSE}
        \STATE RejectService$(s)$
    \ENDIF
\ENDFOR
\RETURN $\Phi$
\end{algorithmic}
\end{algorithm}
```

### Tabla de Notación

```latex
\begin{table}[h]
\centering
\caption{Notation and Symbols}
\label{tab:notation}
\begin{tabular}{|c|l|}
\hline
\textbf{Symbol} & \textbf{Description} \\
\hline
$G^t = (V, E^t)$ & Time-dependent network graph at time $t$ \\
$V_G, V_A, V_S$ & Terrestrial, aerial, and space node sets \\
$S$ & Set of service requests \\
$\Phi$ & VNF placement and routing solution \\
$T_{min}^s$ & Minimum time slots required for service $s$ \\
$P_s^t$ & Set of candidate paths for service $s$ at time $t$ \\
$\alpha, \beta, \gamma$ & Multi-objective optimization weights \\
$\theta_{util}, \theta_{energy}$ & Utilization and energy thresholds \\
$\Delta$ & Set of topology changes \\
$I_s$ & Set of VNFs in service $s$ chain \\
\hline
\end{tabular}
\end{table}
```

## 🛠️ Implementación

### Estructura de Clases Recomendada

```java
// Clase principal de red TN-NTN
public class TNNTNNetwork {
    private List<Node> terrestrialNodes;
    private List<Node> aerialNodes;
    private List<Node> spaceNodes; // LEO, MEO, GEO
    private List<Link> links;
    private Map<Integer, ConnectivityMatrix> connectivityByTime;
}

// Clase de servicio
public class Service {
    private String serviceId;
    private Node sourceNode;
    private Node destinationNode;
    private List<VNF> vnfChain;
    private double latencyRequirement;
    private double bandwidthRequirement;
    private int priority;
}

// Clase de colocación de VNF
public class VNFPlacement {
    private Map<VNF, Node> vnfToNodeMapping;
    private Map<Node, Double> resourceConsumption;
    private boolean feasible;
    private double totalLatency;
    private double totalEnergy;
}
```

### Algoritmo Principal en Java

```java
public class TNNTNVNFOptimizer {
    
    public VNFPlacementSolution optimize(TNNTNNetwork network, 
                                       List<Service> services, 
                                       OptimizationObjectives objectives) {
        
        VNFPlacementSolution solution = new VNFPlacementSolution();
        
        // Clasificar nodos por capa
        classifyNodesByLayer(network);
        
        // Predecir conectividad
        predictConnectivity(network);
        
        // Ordenar servicios por prioridad
        List<Service> sortedServices = sortServicesByPriority(services);
        
        for (Service service : sortedServices) {
            // Aplicar algoritmo heurístico
            VNFPlacement placement = findOptimalPlacement(service, network, objectives);
            
            if (placement.isFeasible()) {
                solution.addService(service, placement);
                network.updateResources(placement);
            } else {
                solution.addRejectedService(service);
            }
        }
        
        return solution;
    }
}
```

## 📚 Referencias

1. **Documento Base**: "ILP Formulation for VNF Placement and Service Chaining in Integrated TN-NTN Environments"

2. **Paper de Referencia**: "Joint Network Function Placement and Routing Optimization in Dynamic Software-Defined Satellite-Terrestrial Integrated Networks"
