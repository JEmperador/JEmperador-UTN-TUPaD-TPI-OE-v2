# Diccionario de Datos — Bot Soporte Técnico Nivel 1

## TeleNet S.A. — Proveedor de Internet y Telefonía

---

## Estructura del Excel (`soporte.xlsx`)

El archivo contiene tres hojas:

| Hoja        | Descripción                                                    |
| ----------- | -------------------------------------------------------------- |
| `Clientes`  | Registro maestro de clientes habilitados para usar el bot      |
| `Sesiones`  | Estado actual de cada conversación activa (máquina de estados) |
| `Historial` | Log completo de interacciones por cliente                      |

---

## Hoja 1: `Clientes`

Representa la base de clientes registrados. El bot consulta esta hoja para validar el número de cliente (Gateway 1).

| Campo                 | Tipo  | Longitud   | Obligatorio | Descripción                                   | Ejemplo                  |
| --------------------- | ----- | ---------- | ----------- | --------------------------------------------- | ------------------------ |
| `numero_cliente`      | Texto | 8 chars    | Sí          | Identificador único del cliente en el sistema | `CLI-0042`               |
| `nombre_completo`     | Texto | 100 chars  | Sí          | Nombre y apellido del titular del servicio    | `García, Laura`          |
| `telefono`            | Texto | 20 chars   | Sí          | Teléfono de contacto principal                | `+54 11 4567-8901`       |
| `email`               | Texto | 150 chars  | No          | Correo electrónico para contacto del técnico  | `laura@gmail.com`        |
| `servicio_contratado` | Texto | 50 chars   | Sí          | Tipo de servicio activo                       | `Internet Fibra 100Mbps` |
| `ticket_asociado`     | Texto | 12 chars   | No          | Último ticket generado para este cliente      | `TKT-0007`               |
| `fecha_alta`          | Fecha | dd-mm-yyyy | Sí          | Fecha de alta en el sistema                   | `15-03-2023`             |

---

## Hoja 2: `Sesiones`

Representa el estado actual de cada usuario en la máquina de estados. Se crea/actualiza con cada interacción del bot.

| Campo                    | Tipo       | Longitud         | Obligatorio | Descripción                                            | Ejemplo               |
| ------------------------ | ---------- | ---------------- | ----------- | ------------------------------------------------------ | --------------------- |
| `telegram_id`            | Entero     | —                | Sí          | ID único del usuario en Telegram                       | `123456789`           |
| `numero_cliente`         | Texto      | 8 chars          | No          | Número de cliente validado en el paso 1                | `CLI-0042`            |
| `estado_actual`          | Texto      | 30 chars         | Sí          | Estado actual en la máquina de estados                 | `ESPERANDO_CATEGORIA` |
| `intentos_numero`        | Entero     | —                | Sí          | Contador de intentos fallidos al validar número        | `1`                   |
| `categoria_seleccionada` | Texto      | 50 chars         | No          | Categoría de problema elegida por el cliente           | `Conectividad`        |
| `email_contacto`         | Texto      | 150 chars        | No          | Email ingresado al final del proceso                   | `laura@gmail.com`     |
| `ticket_id`              | Texto      | 12 chars         | No          | ID del ticket generado (si el problema no se resolvió) | `TKT-0007`            |
| `ultimo_contacto`        | Fecha/Hora | dd-mm-yyyy hh:mm | Sí          | Timestamp del último mensaje recibido (para timeout)   | `06-06-2025 14:32`    |

**Estados válidos para `estado_actual`:**

| Valor                    | Descripción                                                     |
| ------------------------ | --------------------------------------------------------------- |
| `INICIO`                 | Estado transitorio al enviar /start, no se persiste en el Excel |
| `ESPERANDO_NUMERO`       | Bot aguarda número de cliente                                   |
| `ESPERANDO_CATEGORIA`    | Bot aguarda selección de categoría de problema                  |
| `ESPERANDO_CONFIRMACION` | Bot aguarda confirmación si el problema se resolvió             |
| `ESPERANDO_EMAIL`        | Bot aguarda email de contacto (solo si se genera ticket)        |
| `CERRADO`                | Caso resuelto exitosamente                                      |
| `TICKET_GENERADO`        | Caso derivado a técnico especialista                            |
| `FALLIDO`                | 3 intentos fallidos de validación de número                     |

---

## Hoja 3: `Historial`

Registro completo de todas las interacciones. Nunca se modifica, solo se agregan filas.

| Campo            | Tipo       | Longitud         | Obligatorio | Descripción                                         | Ejemplo                                |
| ---------------- | ---------- | ---------------- | ----------- | --------------------------------------------------- | -------------------------------------- |
| `id_interaccion` | Entero     | —                | Sí          | Identificador correlativo de la fila                | `1`                                    |
| `telegram_id`    | Entero     | —                | Sí          | ID del usuario en Telegram                          | `123456789`                            |
| `numero_cliente` | Texto      | 8 chars          | No          | Número de cliente (puede ser vacío si no se validó) | `CLI-0042`                             |
| `fecha_hora`     | Fecha/Hora | dd-mm-yyyy hh:mm | Sí          | Timestamp exacto de la acción registrada            | `06-06-2025 14:33`                     |
| `accion`         | Texto      | 50 chars         | Sí          | Acción que ocurrió en esa interacción               | `NUMERO_VALIDADO`                      |
| `detalle`        | Texto      | 255 chars        | No          | Información adicional de la acción                  | `Categoría seleccionada: Conectividad` |
| `resultado`      | Texto      | 20 chars         | Sí          | Resultado de la acción                              | `EXITOSO` / `FALLIDO`                  |
| `ticket_id`      | Texto      | 12 chars         | No          | Ticket generado en esa interacción (si aplica)      | `TKT-0007`                             |

**Valores válidos para `accion`:**

| Valor                    | Descripción                                  |
| ------------------------ | -------------------------------------------- |
| `INICIO_SESION`          | El usuario envió /start                      |
| `NUMERO_VALIDADO`        | Número de cliente validado correctamente     |
| `NUMERO_INVALIDO`        | Número de cliente no encontrado en la BD     |
| `CATEGORIA_SELECCIONADA` | Cliente eligió una categoría de problema     |
| `SOLUCION_MOSTRADA`      | Bot mostró soluciones frecuentes             |
| `PROBLEMA_RESUELTO`      | Cliente confirmó que el problema se resolvió |
| `TICKET_GENERADO`        | Se generó un ticket para el técnico          |
| `EMAIL_REGISTRADO`       | Cliente proveyó email de contacto            |
| `SESION_FALLIDA`         | Sesión terminó por 3 intentos fallidos       |
| `TIMEOUT`                | Sesión cerrada por inactividad               |
| `CANCELADO`              | Usuario canceló con /cancelar                |

---

## Categorías de problemas disponibles

Estas son las opciones que el bot muestra al cliente en `ESPERANDO_CATEGORIA`:

| Código | Categoría                   | Soluciones frecuentes que muestra el bot                                 |
| ------ | --------------------------- | ------------------------------------------------------------------------ |
| `1`    | Conectividad (sin internet) | Reiniciar el router, verificar cables, verificar si hay corte en la zona |
| `2`    | Velocidad lenta             | Reiniciar router, verificar dispositivos conectados, test de velocidad   |
| `3`    | Telefonía (sin línea)       | Verificar cables del teléfono, reiniciar el módem                        |
| `4`    | Facturación y pagos         | Consultar la app, verificar email de factura, estado de cuenta           |
| `5`    | Otro problema               | Descripción libre, deriva directo a ticket                               |

---

## Reglas de negocio

| Regla     | Descripción                                                                                        |
| --------- | -------------------------------------------------------------------------------------------------- |
| **RN-01** | Un cliente solo puede tener una sesión activa a la vez (por `telegram_id`)                         |
| **RN-02** | Máximo 3 intentos para ingresar el número de cliente. Al tercer fallo, estado → `FALLIDO`          |
| **RN-03** | El email de contacto se solicita únicamente si se genera un ticket                                 |
| **RN-04** | El `ticket_id` se genera con formato `TKT-XXXX` donde XXXX es correlativo con ceros a la izquierda |
| **RN-05** | Una sesión expira por inactividad a los 3 minutos sin mensajes del usuario                         |
| **RN-06** | El campo `ticket_asociado` de la hoja `Clientes` se actualiza al generar un nuevo ticket           |
| **RN-07** | Toda acción queda registrada en `Historial`, sin excepción                                         |
