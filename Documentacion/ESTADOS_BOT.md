# Máquina de Estados — Bot Soporte Técnico Nivel 1

## Estados

| Nombre                   | Descripción                                         |
| ------------------------ | --------------------------------------------------- |
| `INICIO`                 | Estado inicial, se activa con /start                |
| `ESPERANDO_NUMERO`       | Bot aguarda número de cliente                       |
| `ESPERANDO_CATEGORIA`    | Bot aguarda selección de categoría                  |
| `ESPERANDO_CONFIRMACION` | Bot aguarda confirmación si el problema se resolvió |
| `CERRADO`                | Caso resuelto exitosamente                          |
| `TICKET_GENERADO`        | Caso derivado a técnico                             |
| `ESPERANDO_EMAIL`        | Bot aguarda email de contacto para el técnico       |
| `FALLIDO`                | 3 intentos fallidos de validación                   |

---

## Tabla de Transiciones

| Estado actual            | Input usuario  | Condición              | Acción del bot                           | Estado siguiente         |
| ------------------------ | -------------- | ---------------------- | ---------------------------------------- | ------------------------ |
| `INICIO`                 | `/start`       | —                      | Saluda y solicita número de cliente      | `ESPERANDO_NUMERO`       |
| `ESPERANDO_NUMERO`       | Número         | Válido en BD           | Muestra categorías                       | `ESPERANDO_CATEGORIA`    |
| `ESPERANDO_NUMERO`       | Número         | Inválido, intentos < 3 | Informa error, pide reintentar           | `ESPERANDO_NUMERO`       |
| `ESPERANDO_NUMERO`       | Número         | Inválido, intentos = 3 | Informa que no puede continuar           | `FALLIDO`                |
| `ESPERANDO_CATEGORIA`    | Categoría      | Válida                 | Muestra soluciones frecuentes            | `ESPERANDO_CONFIRMACION` |
| `ESPERANDO_CATEGORIA`    | Cualquiera     | Inválida               | Informa error, repite opciones           | `ESPERANDO_CATEGORIA`    |
| `ESPERANDO_CONFIRMACION` | "Sí"           | —                      | Cierra el caso                           | `CERRADO`                |
| `ESPERANDO_CONFIRMACION` | "No"           | —                      | Genera ticket, solicita email            | `ESPERANDO_EMAIL`        |
| `ESPERANDO_EMAIL`        | Email válido   | —                      | Registra email, confirma ticket generado | `TICKET_GENERADO`        |
| `ESPERANDO_EMAIL`        | Input inválido | No es email válido     | Informa error, repite solicitud          | `ESPERANDO_EMAIL`        |
| `ESPERANDO_CONFIRMACION` | Cualquiera     | Inválida               | Informa error, repite pregunta           | `ESPERANDO_CONFIRMACION` |
| `CERRADO`                | Cualquiera     | —                      | Caso cerrado, sugiere /start             | `CERRADO`                |
| `TICKET_GENERADO`        | Cualquiera     | —                      | Informa ticket abierto con número        | `TICKET_GENERADO`        |
| `FALLIDO`                | `/start`       | —                      | Reinicia el proceso                      | `ESPERANDO_NUMERO`       |

---

## Datos a persistir por usuario

| Campo                    | Tipo   | Descripción                                            |
| ------------------------ | ------ | ------------------------------------------------------ |
| `usuario_id`             | entero | ID de Telegram del usuario                             |
| `estado_actual`          | texto  | Estado actual en la máquina                            |
| `intentos_numero`        | entero | Contador de intentos fallidos (máx 3)                  |
| `numero_cliente`         | texto  | Número validado del cliente                            |
| `categoria_seleccionada` | texto  | Categoría elegida por el usuario                       |
| `email_contacto`         | texto  | Email de contacto ingresado antes de generar el ticket |
| `ticket_id`              | texto  | ID del ticket generado (si aplica)                     |

> **Nota:** El estado `INICIO` no se persiste en el Excel. Es transitorio: se activa con `/start` y transiciona inmediatamente a `ESPERANDO_NUMERO`.
