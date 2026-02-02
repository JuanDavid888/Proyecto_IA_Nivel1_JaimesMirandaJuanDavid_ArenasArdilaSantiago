# AgendaBot – Documentación Técnica

## 1. Descripción General del Proyecto

AgendaBot es un bot conversacional desarrollado sobre **Telegram** y orquestado mediante **n8n Community Edition**, cuyo objetivo es permitir a los usuarios **gestionar citas, tareas, hábitos, listas y recordatorios** de forma guiada, sin depender de servicios pagos ni APIs con tarjeta de crédito. El almacenamiento persistente se realiza en **Google Sheets**, actuando como base de datos ligera y auditable.

El flujo está diseñado bajo un enfoque **100% conversacional**, donde el usuario interactúa exclusivamente mediante **opciones numéricas**, garantizando claridad, control y trazabilidad de cada acción.

---

## 2. Funcionalidades Principales

### 2.1 Menú Principal Conversacional

* Presenta siempre las opciones disponibles al usuario.
* No asume intención: toda acción se inicia por selección explícita.
* Ofrece una recomendación de uso (ej. empezar por Agenda).
* Permite salida segura en cualquier momento.

### 2.2 Gestión de Citas (Agenda)

* Agendar nuevas citas mediante un **flujo guiado por pasos**.
* Validación de formato de fecha y hora.
* Confirmación explícita antes de guardar.
* Generación automática de `id_cita` incremental.
* Consulta, cancelación, reprogramación y cierre de citas.

### 2.3 Gestión de Tareas

* Registro de tareas con prioridad y fecha objetivo.
* Estados controlados (pendiente, completada, cancelada).
* Asociación directa con el usuario creador.

### 2.4 Gestión de Hábitos

* Creación de hábitos personales con frecuencia definida.
* Horario de recordatorio configurable.
* Estados activos o pausados.
* Base preparada para automatizaciones periódicas.

### 2.5 Listas e Ítems

* Creación de listas personalizadas (compras, pendientes, etc.).
* Asociación de múltiples ítems por lista.
* Control de estado por ítem.


### 2.6 Registro de Logs

* Registro de cada interacción relevante.
* Captura de pantalla, opción elegida y resultado.
* Útil para auditoría, debugging y mejora del flujo.

### 2.7 Manejo de Sesiones

* Persistencia del estado conversacional.
* Control de pantalla actual y paso del wizard.
* Almacenamiento de datos parciales entre mensajes.
* Prevención de pérdida de contexto.

---

## 3. Arquitectura del Flujo en n8n

### 3.1 Entrada Principal

* **Telegram Trigger**: punto único de entrada.
* Todo mensaje inicia o continúa una sesión.

### 3.2 Control de Sesión

* Verificación de sesión activa en la hoja `SESSIONS`.
* Redirección automática a la pantalla correcta según `pantalla_actual`.
* Limpieza de sesión al finalizar flujos o cancelar.

### 3.3 Lógica Central

* Nodos `Switch` para navegación por menús.
* Nodos `Code` para:

  * Validaciones de formato.
  * Generación de IDs.
  * Construcción de mensajes dinámicos.
* Nodos de Google Sheets para lectura y escritura.

---

## 4. Puntos Críticos para el Mantenimiento

### 4.1 Google Sheets como Base de Datos

* No eliminar columnas existentes.
* No cambiar nombres de hojas sin actualizar el flujo.
* Evitar espacios o caracteres invisibles en IDs.
* Mantener formatos consistentes (fecha, hora, estados).

### 4.2 Generación de IDs

* Los IDs dependen de la lectura completa de la hoja.
* Asegurar que los nodos `Get row(s)` tengan `Return All = true`.
* Evitar ejecuciones paralelas sin control si el flujo crece.

### 4.3 Manejo de Sesiones

* Cualquier cambio en menús debe reflejarse en `pantalla_actual`.
* No reutilizar nombres de pantallas sin control.
* Limpiar `datos_parciales` al finalizar flujos.

### 4.4 Mensajería

* Mantener siempre:

  * Opciones numeradas.
  * Opción de cancelar.
  * Mensajes claros y consistentes.
* No cambiar el orden de opciones sin revisar validaciones.


