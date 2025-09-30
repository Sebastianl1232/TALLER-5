# Taller en Clase – Aplicando el análisis semántico y la generación de código

## Objetivo
Aplicar los conceptos de análisis semántico, código intermedio, optimización y código destino en ejercicios prácticos distintos a los vistos en la explicación.

## Parte 1 – Detectando errores semánticos

### Fragmento de código a analizar:
```cpp
float precio = 19.99;
int unidades = 3;
string total = precio * unidades;
```

### Análisis del código:

#### 1. ¿Existe error semántico?
```cpp
Sí, existe un error semántico.
```

#### 2. ¿Por qué ocurre?
```cpp
El error semántico ocurre por incompatibilidad de tipos. Específicamente:

Se intenta asignar el resultado de una operación aritmética (`precio * unidades`) a una variable de tipo `string`
La multiplicación entre `float` y `int` produce un resultado de tipo `float` (59.97)
No existe una conversión implícita automática de `float` a `string` en C++
El compilador no puede realizar esta conversión sin una instrucción explícita

Detalles técnicos:
`precio` es de tipo `float` (19.99)
`unidades` es de tipo `int` (3)
`precio * unidades` resulta en un `float` (59.97)
`total` está declarado como `string`, pero se le intenta asignar un `float`
```

#### 3. Dos formas de corregirlo:

##### **Solución 1: Cambiar el tipo de la variable `total`**
```cpp
float precio = 19.99;
int unidades = 3;
float total = precio * unidades;  // Cambiar string por float
```

##### **Solución 2: Convertir explícitamente a string**
```cpp
#include <string>

float precio = 19.99;
int unidades = 3;
string total = std::to_string(precio * unidades);  // Conversión explícita
```
## Parte 2 – Construcción de código intermedio

### Instrucción a convertir:
```
x = (m + n) * (p - q) / r
```

#### Proceso paso a paso:

1. **Identificar sub-expresiones**: `(m + n)`, `(p - q)`, multiplicación y división
2. **Asignar variables temporales**: T1, T2, T3, T4
3. **Descomponer en operaciones simples**

#### Código de tres direcciones resultante:

```
T1 = m + n          // Evaluar primera sub-expresión (m + n)
T2 = p - q          // Evaluar segunda sub-expresión (p - q)  
T3 = T1 * T2        // Multiplicar los resultados de las sub-expresiones
T4 = T3 / r         // Dividir el resultado anterior entre r
x = T4              // Asignar el resultado final a x
```

### Análisis de la conversión

**Orden de evaluación respetado:**
1. Los paréntesis tienen prioridad
2. La multiplicación antes que división (mismo nivel, izquierda a derecha)
3. Se preserva la semántica de la expresión original


## Parte 3 – Optimización de expresiones

### Fragmento de código a analizar:
```javascript
let area = (base * altura) / 2 + (0 * altura);
```

### Análisis de optimización:

#### 1. ¿Qué operación puede eliminarse?

**Pueden eliminarse dos operaciones:**

1. **Multiplicación por cero**: `(0 * altura)`
   - Cualquier número multiplicado por cero siempre resulta en cero
   - Esta operación es redundante independientemente del valor de `altura`

2. **Suma de cero**: `+ 0`
   - Sumar cero a cualquier número no cambia el resultado
   - La operación completa `+ (0 * altura)` equivale a `+ 0`

#### 2. Código optimizado:

##### **Versión original:**
```javascript
let area = (base * altura) / 2 + (0 * altura);
```

##### **Versión optimizada:**
```javascript
Optimizado: area = (base * altura) / 2
```

## Parte 4 – Generación de código destino

### Instrucción a traducir:
```
k = (x + y) - (z * 2)
```

### Traducción a pseudocódigo de ensamblador:

#### **Instrucciones disponibles:**
- **LOAD**: Carga un valor en el acumulador
- **ADD**: Suma al acumulador
- **SUB**: Resta del acumulador  
- **MUL**: Multiplica el acumulador
- **STORE**: Guarda el resultado en una variable

#### **Análisis de la expresión:**
1. **Sub-expresión 1**: `(x + y)`
2. **Sub-expresión 2**: `(z * 2)`
3. **Operación final**: Restar sub-expresión 2 de sub-expresión 1

#### **Código de ensamblador resultante:**

```assembly
; Evaluar (x + y)
LOAD x          ; Acumulador = x
ADD y           ; Acumulador = x + y
STORE temp1     ; temp1 = x + y

; Evaluar (z * 2)  
LOAD z          ; Acumulador = z
MUL 2           ; Acumulador = z * 2
STORE temp2     ; temp2 = z * 2

; Operación final: temp1 - temp2
LOAD temp1      ; Acumulador = x + y
SUB temp2       ; Acumulador = (x + y) - (z * 2)
STORE k         ; k = resultado final
```

### Análisis del proceso de generación:

#### **Estrategia de traducción:**

1. **Descomposición**: Se divide la expresión en sub-expresiones manejables
2. **Variables temporales**: Se usan `temp1` y `temp2` para almacenar resultados intermedios
3. **Uso del acumulador**: Todas las operaciones pasan por el acumulador

#### **Mapeo de operaciones:**

| **Operación de alto nivel** | **Instrucciones de ensamblador** |
|---------------------------|--------------------------------|
| `x + y`                   | `LOAD x; ADD y`               |
| `z * 2`                   | `LOAD z; MUL 2`               |
| `resultado1 - resultado2`  | `LOAD temp1; SUB temp2`       |
| Asignación a `k`          | `STORE k`                     |

---