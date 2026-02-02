# AgendaBot – Especificación Funcional

## Artículo 1. Introducción

La organización **AgendaBot Services** requiere implementar un bot automatizado que permita **agendar, planificar y automatizar tareas básicas** sin depender de plataformas de pago ni servicios que exijan tarjeta de crédito.

---

## Capítulo II: Herramientas y Restricciones

### Artículo 2. Stack Tecnológico Permitido

El proyecto deberá implementarse con:

* **Telegram** (interfaz conversacional).
* **n8n Community Edition** (automatización y lógica).
* **Google Sheets** (almacenamiento de datos).

**No está permitido:**

* n8n Cloud con pago.
* APIs que requieran tarjeta de crédito.
* Entrenamiento de modelos, embeddings o RAG.

### Artículo 3. Enfoque Conversacional de la Automatización

AgendaBot funcionará bajo los siguientes principios:

* El usuario siempre elige qué hacer escribiendo un número.
* El bot siempre explica qué hace y qué opciones hay.
* El bot siempre sugiere una opción recomendada.
* El bot nunca asume intención.
* El bot siempre ofrece salida (volver o cancelar).

---

## Capítulo III: Modelo de Datos (Google Sheets)

### Artículo 4. Estructura del Documento `AgendaBot_DB`

El documento deberá contener las siguientes hojas:

### 📄 CITAS

* `id_cita`
* `fecha`
* `hora`
* `nombre`
* `motivo`
* `canal`
* `estado`
* `creado_por`
* `timestamp_creacion`

### 📄 TAREAS

* `id_tarea`
* `titulo`
* `prioridad`
* `estado`
* `fecha_objetivo`
* `creado_por`

### 📄 HABITOS

* `id_habito`
* `nombre`
* `frecuencia`
* `hora_recordatorio`
* `estado`

### 📄 LISTAS

* `id_lista`
* `nombre_lista`
* `tipo`
* `creado_por`

### 📄 ITEMS_LISTA

* `id_item`
* `id_lista`
* `item`
* `estado`

### 📄 USUARIOS

* `telegram_user`
* `nombre`
* `rol`
* `permitido`

### 📄 LOGS

* `timestamp`
* `telegram_user`
* `pantalla`
* `opcion_elegida`
* `resultado`

### 📄 SESSIONS

* `telegram_user`
* `pantalla_actual`
* `paso_actual`
* `datos_parciales`
* `timestamp_ultima_interaccion`

---

## Capítulo IV: Comunicación Humanizada del Bot

### Artículo 5. Principios de Mensajería

Cada mensaje del bot deberá:

* Saludar de forma cercana.
* Explicar brevemente qué puede hacer.
* Mostrar opciones numeradas.
* Sugerir una opción.
* Indicar cómo continuar o salir.

### Artículo 6. Mensaje de Bienvenida

```text
Hola, soy AgendaBot 👋

Estoy aquí para ayudarte a organizar tus citas, tareas y recordatorios
de forma sencilla y sin complicaciones.

Puedes escribirme en cualquier momento.
Para avanzar, solo tienes que escribir el número de la opción que quieras usar.

Menú principal:
0. Ayuda
1. Agenda (citas)
2. Tareas
3. Recordatorios
4. Hábitos
5. Listas
6. Reportes
7. Configuración
8. Administrador
```

**Mensaje de orientación:**

```text
Tip rápido: escribe solo el número (por ejemplo: 1) y presiona enviar.
```

### Artículo 7. Opción Inválida (Mensaje Global)

```text
Ups, esa opción no existe en este menú.

Por favor escribe uno de los números que ves en pantalla.

Estás en: Menú principal
Opciones disponibles: 0 al 8
```

---

## Capítulo V: Navegación por Menús Numéricos

### Artículo 8. Menú Principal

```text
¿En qué te puedo ayudar hoy?

Menú principal:
0. Ayuda
1. Agenda (citas)
2. Tareas
3. Recordatorios
4. Hábitos
5. Listas
6. Reportes
7. Configuración
8. Administrador
```

**Sugerencia:**

```text
Si quieres empezar rápido, te recomiendo la opción 1 (Agenda).
```

### Artículo 9. Menú Agenda

```text
Perfecto, vamos con tu agenda.

¿Qué deseas hacer?

1. Agendar una nueva cita
2. Consultar tu agenda
3. Reprogramar una cita
4. Cancelar una cita
5. Marcar una cita como completada
9. Volver al menú principal
```

---

## Capítulo VI: Flujos Guiados por Pasos (Wizard)

### Artículo 10. Flujo “Agendar Nueva Cita”

#### Paso 1 – Fecha

```text
Empecemos a crear tu cita.

Paso 1 de 6
¿Cuál es la fecha de la cita?

Formato: YYYY-MM-DD
Ejemplo: 2025-12-20

9. Cancelar
```

#### Paso 2 – Hora

```text
Paso 2 de 6
¿A qué hora será la cita?

Formato 24 horas (HH:MM)
Ejemplo: 14:30

9. Cancelar
```

#### Paso 3 – Nombre

```text
Paso 3 de 6
¿A nombre de quién es la cita?

Escribe el nombre completo.

9. Cancelar
```

#### Paso 4 – Motivo

```text
Paso 4 de 6
Cuéntame brevemente el motivo de la cita.

Ejemplo: Asesoría técnica

9. Cancelar
```

#### Paso 5 – Canal

```text
Paso 5 de 6
¿Cómo será la atención?

1. Presencial
2. Virtual
3. Llamada
9. Cancelar
```

#### Paso 6 – Confirmación

```text
Revisa la información de tu cita:

Fecha: 2025-12-20
Hora: 14:30
Cliente: Pedro Gómez
Motivo: Asesoría técnica
Canal: Virtual

¿Qué deseas hacer?
1. Confirmar y guardar
2. Editar información
3. Cancelar
```

#### Mensaje de Éxito

```text
¡Listo! Tu cita quedó agendada correctamente.

ID de la cita: CITA-014

¿Qué deseas hacer ahora?
1. Volver a Agenda
2. Ir al menú principal
```
