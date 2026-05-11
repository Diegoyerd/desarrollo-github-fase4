## ▶️ Cómo ejecutar el proyecto

### Requisitos

- Python 3.8 o superior

- No requiere librerías externas (solo módulos estándar: `abc`, `re`, `logging`, `datetime`)

### Pasos

```bash

# 1. Clonar el repositorio

git clone https://github.com/[usuario]/software-fj-fase4.git

# 2. Ingresar a la carpeta del proyecto

cd software-fj-fase4

# 3. Ejecutar el sistema

python main.py

```

Los resultados se muestran en consola y los eventos quedan registrados en `logs/software_fj.log`.

---

## 🧪 Simulaciones incluidas

El sistema ejecuta **17 operaciones** de prueba que cubren escenarios válidos e inválidos:

| # | Descripción | Resultado esperado |

|---|---|---|

| 1 | Registrar cliente válido | ✅ Éxito |

| 2 | Cliente con correo inválido | ❌ `ErrorValidacion` |

| 3 | Cliente con teléfono inválido | ❌ `ErrorValidacion` |

| 4 | Crear servicio de sala válido | ✅ Éxito |

| 5 | Crear servicio de alquiler válido | ✅ Éxito |

| 6 | Crear servicio de asesoría válido | ✅ Éxito |

| 7 | Servicio con tarifa negativa | ❌ `ErrorValidacion` |

| 8 | Tarifa no numérica con excepción encadenada | ❌ `ErrorValidacion` (encadenada desde `ValueError`) |

| 9 | Crear reserva válida | ✅ Éxito |

| 10 | Confirmar reserva | ✅ Éxito |

| 11 | Procesar reserva con IVA y descuento | ✅ Costo calculado |

| 12 | Cancelar reserva ya procesada | ❌ `ErrorReserva` |

| 13 | Reserva con duración excesiva | ❌ `ErrorReserva` |

| 14 | Reserva sobre servicio no disponible | ❌ `ErrorServicioNoDisponible` |

| 15 | Procesar reserva sin confirmar | ❌ `ErrorReserva` |

| 16 | Crear reserva duplicada activa | ❌ `ErrorReservaDuplicada` |

| 17 | Crear reserva tras cancelar la duplicada | ✅ Éxito |

---

## 🛡️ Técnicas de manejo de excepciones implementadas

| Técnica | Dónde se aplica |

|---|---|

| `try / except` | `ejecutar_operacion()` — captura errores sin detener el sistema |

| `try / except / else` | `procesar()` — el bloque `else` solo corre si no hay error |

| `try / except / finally` | `procesar()` — el `finally` siempre registra el intento |

| Excepciones personalizadas | Jerarquía propia basada en `ErrorSistema` |

| Encadenamiento (`raise ... from`) | `crear_servicio_con_encadenamiento()` |

| Logging de errores y eventos | Cada operación registra su resultado en `software_fj.log` |

---

## 👥 Integrantes del equipo

Diego Yerdfrey Tocora González
Bladimir Cárdenas Montañez
Sebastián Torres Acosta
Ariana Jimena Pérez Garrido
Brayan Estiben Agudelo Gonzalez

## 📄 Licencia

Proyecto académico desarrollado para la **Universidad Nacional Abierta y a Distancia — UNAD**.  

Uso exclusivo con fines educativos.

 
# 🖥️ Sistema Integral de Gestión — Software FJ

### Fase 4 · Programación Orientada a Objetos y Manejo de Excepciones
> Curso: Programación · Código: 213023 
> Universidad Nacional Abierta y a Distancia — UNAD  
> Escuela de Ciencias Básicas, Tecnología e Ingeniería (ECBTI)  
> Programa: Ingeniería de Sistemas

---

## 📋 Descripción del proyecto

**Software FJ** es un sistema de gestión de reservas desarrollado en Python que permite registrar clientes, crear servicios y administrar reservas con un ciclo de vida completo (creación, confirmación, procesamiento y cancelación).

El proyecto aplica los principios de la **Programación Orientada a Objetos (POO)** y un **manejo robusto de excepciones**, garantizando que el sistema sea estable y nunca se detenga ante errores inesperados.

---

## 🎯 Objetivos del sistema

- Registrar y validar clientes con sus datos personales.

- Crear y gestionar servicios de tres tipos: reserva de sala, alquiler de equipo y asesoría especializada.

- Administrar reservas con transiciones de estado controladas.

- Detectar y manejar errores mediante excepciones personalizadas.

- Registrar todos los eventos y errores en un archivo de logs.

---

## 🗂️ Estructura del proyecto

```

software_fj/

│

├── main.py                  # Archivo principal con la simulación completa

├── README.md                # Documentación del proyecto

├── logs/

│   └── software_fj.log      # Registro de eventos y errores del sistema

└── requirements.txt         # Dependencias del proyecto (solo librería estándar)

```

---

## 🧱 Arquitectura del sistema (Clases principales)

```

EntidadSistema  (Clase abstracta base)

├── Cliente

├── Servicio  (Clase abstracta)

│   ├── ReservaSala

│   ├── AlquilerEquipo

│   └── AsesoriaEspecializada

└── Reserva

```

### Descripción de cada clase

| Clase | Descripción |

|---|---|

| `EntidadSistema` | Clase abstracta base. Define el identificador y el método `mostrar_informacion()`. |

| `Cliente` | Representa un cliente. Valida nombre, correo y teléfono con encapsulación. |

| `Servicio` | Clase abstracta para servicios. Define el contrato de `calcular_costo()`, `describir_servicio()` y `validar_parametros()`. |

| `ReservaSala` | Servicio de reserva de sala. Calcula costo por horas con límite de 8 horas. |

| `AlquilerEquipo` | Servicio de alquiler de equipo. Agrega garantía del 10% al costo base. |

| `AsesoriaEspecializada` | Servicio de asesoría. Aplica recargo del 15% si supera 3 horas. |

| `Reserva` | Integra cliente y servicio. Maneja estados: Creada → Confirmada → Procesada / Cancelada. |

---

## ⚠️ Excepciones personalizadas

El sistema define una jerarquía propia de excepciones para controlar cada tipo de error:

```

ErrorSistema  (Excepción base del sistema)

├── ErrorValidacion          → Datos de entrada incorrectos

├── ErrorReserva             → Operaciones inválidas sobre reservas

│   └── ErrorReservaDuplicada → Reserva con mismos datos ya activa

├── ErrorServicioNoDisponible → Intento de reservar un servicio inactivo

└── ErrorCalculoCosto         → Parámetros inválidos en el cálculo de costo

```

---

## 🔄 Ciclo de vida de una reserva

```

[Creada] ──────────► [Confirmada] ──────────► [Procesada]

    │                     │

   └──────────────────────┴──────────────► [Cancelada]

```

- Solo se puede **confirmar** una reserva en estado `Creada`.

- Solo se puede **procesar** una reserva en estado `Confirmada`.

- No se puede **cancelar** una reserva ya `Procesada`.

---

## ▶️ Cómo ejecutar el proyecto

### Requisitos

- Python 3.8 o superior
- No requiere librerías externas (solo módulos estándar: `abc`, `re`, `logging`, `datetime`)

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/[usuario]/software-fj-fase4.git

# 2. Ingresar a la carpeta del proyecto
cd software-fj-fase4

# 3. Ejecutar el sistema
python main.py
```

Los resultados se muestran en consola y los eventos quedan registrados en `logs/software_fj.log`.

---

## 🧪 Simulaciones incluidas

El sistema ejecuta **17 operaciones** de prueba que cubren escenarios válidos e inválidos:

| # | Descripción | Resultado esperado |
|---|---|---|
| 1 | Registrar cliente válido | ✅ Éxito |
| 2 | Cliente con correo inválido | ❌ `ErrorValidacion` |
| 3 | Cliente con teléfono inválido | ❌ `ErrorValidacion` |
| 4 | Crear servicio de sala válido | ✅ Éxito |
| 5 | Crear servicio de alquiler válido | ✅ Éxito |
| 6 | Crear servicio de asesoría válido | ✅ Éxito |
| 7 | Servicio con tarifa negativa | ❌ `ErrorValidacion` |
| 8 | Tarifa no numérica con excepción encadenada | ❌ `ErrorValidacion` (encadenada desde `ValueError`) |
| 9 | Crear reserva válida | ✅ Éxito |
| 10 | Confirmar reserva | ✅ Éxito |
| 11 | Procesar reserva con IVA y descuento | ✅ Costo calculado |
| 12 | Cancelar reserva ya procesada | ❌ `ErrorReserva` |
| 13 | Reserva con duración excesiva | ❌ `ErrorReserva` |
| 14 | Reserva sobre servicio no disponible | ❌ `ErrorServicioNoDisponible` |
| 15 | Procesar reserva sin confirmar | ❌ `ErrorReserva` |
| 16 | Crear reserva duplicada activa | ❌ `ErrorReservaDuplicada` |
| 17 | Crear reserva tras cancelar la duplicada | ✅ Éxito |

---

## 🛡️ Técnicas de manejo de excepciones implementadas

| Técnica | Dónde se aplica |
|---|---|
| `try / except` | `ejecutar_operacion()` — captura errores sin detener el sistema |
| `try / except / else` | `procesar()` — el bloque `else` solo corre si no hay error |
| `try / except / finally` | `procesar()` — el `finally` siempre registra el intento |
| Excepciones personalizadas | Jerarquía propia basada en `ErrorSistema` |
| Encadenamiento (`raise ... from`) | `crear_servicio_con_encadenamiento()` |
| Logging de errores y eventos | Cada operación registra su resultado en `software_fj.log` |

---

## 👥 Integrantes del equipo

| Nombre | Rol en el proyecto |
|---|---|
| [Nombre 1] | [Ej: Clases base y excepciones] |
| [Nombre 2] | [Ej: Servicios y cálculo de costos] |
| [Nombre 3] | [Ej: Clase Reserva y estados] |
| [Nombre 4] | [Ej: Simulación y operaciones] |
| [Nombre 5] | [Ej: README y documentación] |

---

## 📄 Licencia

Proyecto académico desarrollado para la **Universidad Nacional Abierta y a Distancia — UNAD**.  
Uso exclusivo con fines educativos.

 