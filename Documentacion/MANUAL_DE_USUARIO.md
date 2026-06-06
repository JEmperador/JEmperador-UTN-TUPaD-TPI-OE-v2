# Manual de Usuario — Bot de Soporte Técnico Nivel 1

## TeleNet S.A.

---

## ¿Qué es este bot?

El bot de soporte técnico de TeleNet S.A. es un asistente automatizado disponible en Telegram que te permite reportar y resolver problemas técnicos de tu servicio de internet o telefonía sin necesidad de llamar a un agente.

El bot te guía paso a paso: primero intenta resolver tu problema con soluciones frecuentes y, si no es suficiente, genera un ticket automáticamente para que un técnico especialista te contacte.

---

## ¿Cómo acceder?

1. Abrí Telegram en tu celular o computadora
2. Buscá el bot por su nombre de usuario: **@TeleNetSoporteBot**
3. Presioná **Iniciar** o escribí `/start`

---

## Comandos disponibles

| Comando     | Descripción                                       |
| ----------- | ------------------------------------------------- |
| `/start`    | Inicia o reinicia una consulta de soporte         |
| `/cancelar` | Cancela la consulta actual y descarta el progreso |
| `/estado`   | Muestra en qué paso del proceso estás             |
| `/ayuda`    | Muestra la lista de comandos disponibles          |

---

## ¿Qué necesitás para usar el bot?

Únicamente tu **número de cliente**, que encontrás en tu factura mensual de TeleNet S.A.

El formato es: **CLI-XXXX** (por ejemplo: `CLI-0042`)

---

## Flujo completo paso a paso

### Paso 1 — Iniciá la consulta

Escribí `/start`. El bot te dará la bienvenida y te pedirá tu número de cliente.

```
👋 ¡Bienvenido al Soporte Técnico de TeleNet S.A.!

Para comenzar, ingresá tu número de cliente.
Formato: CLI-XXXX (lo encontrás en tu factura)
```

---

### Paso 2 — Ingresá tu número de cliente

Escribí tu número de cliente en el formato indicado.

```
Vos:  CLI-0001
Bot:  ✅ Cliente CLI-0001 verificado.

      Seleccioná el número de tu problema:

        1 — Conectividad (sin internet)
        2 — Velocidad lenta
        3 — Telefonía (sin línea)
        4 — Facturación y pagos
        5 — Otro problema
```

> ⚠️ Tenés **3 intentos** para ingresar un número válido. Si los agotás, la sesión se cerrará automáticamente.

---

### Paso 3 — Seleccioná la categoría del problema

Respondé con el número que corresponda a tu problema (del 1 al 5).

**Ejemplo para categoría 1 — Conectividad:**

```
Vos:  1
Bot:  📋 Conectividad (sin internet) — Pasos a seguir:

      • Reiniciá el router desenchufándolo 30 segundos.
      • Verificá que todos los cables estén bien conectados.
      • Consultá si hay un corte en tu zona en nuestra app.

      ¿El problema se resolvió? Respondé sí o no.
```

> **Nota:** Si seleccionás la opción **5 — Otro problema**, el bot te derivará directamente a un técnico sin mostrar soluciones previas.

---

### Paso 4A — El problema se resolvió ✅

Si las soluciones sugeridas funcionaron, respondé **sí**.

```
Vos:  sí
Bot:  ✅ ¡Nos alegra que se haya resuelto!

      Tu caso fue cerrado exitosamente.
      Escribí /start si necesitás hacer otra consulta.
```

El proceso finaliza aquí. Tu caso queda registrado con estado **CERRADO**.

---

### Paso 4B — El problema NO se resolvió 🎫

Si las soluciones no funcionaron, respondé **no**.

```
Vos:  no
Bot:  Entendido. Para derivar tu caso a un técnico
      necesito tu email de contacto.
      Ejemplo: tucorreo@gmail.com
```

---

### Paso 5 — Ingresá tu email de contacto

El técnico especialista usará este email para contactarte.

```
Vos:  mimail@gmail.com
Bot:  🎫 Ticket generado: TKT-0001

      Un técnico especialista revisará tu caso y se
      contactará al email mimail@gmail.com a la brevedad.

      Guardá tu número de ticket por si necesitás
      hacer seguimiento.
```

El proceso finaliza aquí. Tu caso queda registrado con estado **TICKET GENERADO**.

---

## Categorías de problemas y soluciones frecuentes

### 1 — Conectividad (sin internet)

- Reiniciá el router desenchufándolo 30 segundos
- Verificá que todos los cables estén bien conectados
- Consultá si hay un corte en tu zona en la app de TeleNet

### 2 — Velocidad lenta

- Reiniciá el router y esperá 2 minutos
- Verificá cuántos dispositivos están conectados
- Realizá un test de velocidad en fast.com

### 3 — Telefonía (sin línea)

- Verificá que el cable del teléfono esté bien conectado al módem
- Reiniciá el módem y esperá que reconecte
- Probá con otro teléfono si tenés

### 4 — Facturación y pagos

- Revisá tu bandeja de entrada por el email de factura
- Ingresá a la app de TeleNet para ver el estado de tu cuenta
- Verificá que tu medio de pago esté vigente

### 5 — Otro problema

El bot derivará tu caso directamente a un técnico especialista.

---

## Errores frecuentes y cómo resolverlos

| Situación                          | Mensaje del bot                                                | Qué hacer                                                                       |
| ---------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Formato de número incorrecto       | "Formato incorrecto. El número debe tener el formato CLI-XXXX" | Revisá que el número tenga el formato correcto, por ejemplo `CLI-0042`          |
| Número no registrado               | "El número CLI-XXXX no está registrado en nuestro sistema"     | Verificá el número en tu factura. Si el problema persiste contactá a soporte    |
| 3 intentos fallidos                | "Superaste el límite de intentos"                              | Escribí `/start` para iniciar una nueva sesión                                  |
| Categoría inválida                 | "Opción inválida. Por favor ingresá un número del 1 al 5"      | Respondé únicamente con un número entre 1 y 5                                   |
| Respuesta inválida en confirmación | "Por favor respondé únicamente sí o no"                        | Escribí solo `sí` o `no`                                                        |
| Email inválido                     | "El email ingresado no es válido"                              | Verificá que el email tenga el formato correcto, por ejemplo `nombre@gmail.com` |
| Inactividad de 3 minutos           | "Tu sesión expiró por inactividad"                             | Escribí `/start` para iniciar una nueva sesión                                  |
| Archivo o imagen enviada           | "Este tipo de mensaje no es válido"                            | El bot solo acepta texto. Respondé con texto                                    |

---

## Estados de la consulta

A lo largo del proceso tu consulta puede estar en uno de estos estados. Podés consultarlo en cualquier momento con `/estado`.

| Estado                   | Significado                                             |
| ------------------------ | ------------------------------------------------------- |
| `ESPERANDO_NUMERO`       | El bot espera que ingreses tu número de cliente         |
| `ESPERANDO_CATEGORIA`    | El bot espera que selecciones la categoría del problema |
| `ESPERANDO_CONFIRMACION` | El bot espera que confirmes si el problema se resolvió  |
| `ESPERANDO_EMAIL`        | El bot espera tu email de contacto para el técnico      |
| `CERRADO`                | Tu consulta fue resuelta exitosamente                   |
| `TICKET_GENERADO`        | Se generó un ticket y un técnico te contactará          |
| `FALLIDO`                | La sesión terminó por intentos fallidos o cancelación   |

---

## Consejos de uso

- **Sesión activa:** si cerrás Telegram y volvés a abrirlo, podés retomar la consulta donde la dejaste escribiendo `/estado`.
- **Inactividad:** si no respondés en 3 minutos, la sesión se cierra automáticamente. Escribí `/start` para comenzar de nuevo.
- **Cancelar:** podés escribir `/cancelar` en cualquier momento para abandonar el proceso sin completarlo.
- **Múltiples problemas:** si tenés más de un problema, completá una consulta y luego escribí `/start` para iniciar otra.

---

## Contacto

Si tenés dudas que el bot no puede resolver:

📧 **soporte@telenet.com.ar**
