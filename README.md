# Bot de Soporte Técnico Nivel 1

**Trabajo Práctico Integrador — Organización Empresarial**  
Tecnicatura Universitaria en Programación — Universidad Tecnológica Nacional

---

## Descripción

Bot de Telegram que automatiza el proceso de soporte técnico nivel 1 para
**TeleNet S.A.**, un proveedor ficticio de internet y telefonía. El bot guía
al cliente paso a paso para resolver problemas frecuentes y, si no es posible
resolverlos, genera un ticket automático y deriva el caso a un técnico especialista.

El flujo está modelado con **BPMN 2.0** e implementado mediante una
**máquina de estados** que persiste en un archivo Excel (`soporte.xlsx`).

---

## Integrante

| Nombre | Apellido  |
| ------ | --------- |
| Javier | Emperador |

---

## Estructura del proyecto

```
JEmperador-UTN-TUPaD-TPI-OE/
├── Datos/
│   └── soporte.xlsx          # Base de datos Excel (se genera automáticamente)
├── Documentacion/
│   ├── DICCIONARIO_DATOS.md  # Descripción de entidades y campos
│   ├── ESTADOS_BOT.md        # Máquina de estados y tabla de transiciones
│   └── MANUAL_DE_USUARIO.md  # Guía de uso del bot
├── .env                      # Token del bot (no incluido en el repositorio)
├── .env.example              # Ejemplo de configuración
├── .gitignore                # Excluye .env y archivos innecesarios
├── main.py                   # Código fuente principal
├── README.md                 # Este archivo
└── requirements.txt          # Dependencias del proyecto
```

---

## Requisitos

- Python 3.10 o superior
- Una cuenta de Telegram
- Un bot creado en [@BotFather](https://t.me/BotFather)

---

## Instrucciones de uso

**1. Clonar el repositorio:**

```bash
git clone [URL_DEL_REPOSITORIO]
cd JEmperador-UTN-TUPaD-TPI-OE
```

**2. Instalar dependencias:**

```bash
pip install -r requirements.txt
```

**3. Configurar el token:**

```bash
cp .env.example .env
```

Editá el archivo `.env` y completá tu token:

```
TOKEN=tu_token_aqui
```

El token lo obtenés hablando con [@BotFather](https://t.me/BotFather) en Telegram con `/newbot`.

**4. Ejecutar el bot:**

```bash
python main.py
```

Al iniciarse por primera vez, el bot crea automáticamente el archivo
`Datos/soporte.xlsx` con las hojas `Clientes`, `Sesiones` e `Historial`,
y carga tres clientes de ejemplo para pruebas.

---

## Comandos disponibles

| Comando     | Descripción                                     |
| ----------- | ----------------------------------------------- |
| `/start`    | Inicia o reinicia una consulta de soporte       |
| `/cancelar` | Cancela la consulta actual                      |
| `/estado`   | Muestra en qué paso del proceso está el usuario |
| `/ayuda`    | Muestra la lista de comandos disponibles        |

---

## Flujo del bot

```
/start
  └─► Solicita número de cliente (CLI-XXXX)
        ├─► Número inválido (hasta 3 intentos) → FALLIDO
        └─► Número válido
              └─► Muestra categorías de problemas (1 al 5)
                    └─► Cliente selecciona categoría
                          ├─► Categorías 1-4: muestra soluciones frecuentes
                          │     ├─► ¿Se resolvió? Sí → CERRADO
                          │     └─► ¿Se resolvió? No → solicita email
                          └─► Categoría 5: solicita email directo
                                └─► Email válido → genera ticket → TICKET_GENERADO
```

---

## Clientes de ejemplo precargados

| Número   | Nombre           | Servicio               |
| -------- | ---------------- | ---------------------- |
| CLI-0001 | García, Laura    | Internet Fibra 100Mbps |
| CLI-0002 | Martínez, Carlos | Internet + Telefonía   |
| CLI-0003 | López, Ana       | Internet Fibra 50Mbps  |

Para agregar más clientes, editá la hoja `Clientes` del archivo `Datos/soporte.xlsx` directamente.

---

## Manejo de errores

| Situación                     | Comportamiento del bot                                           |
| ----------------------------- | ---------------------------------------------------------------- |
| Formato de número incorrecto  | Informa el error, muestra el formato correcto, descuenta intento |
| Número no registrado en BD    | Informa que no está registrado, descuenta intento                |
| 3 intentos fallidos           | Cierra la sesión con estado FALLIDO                              |
| Categoría fuera de rango      | Informa el error y repite las opciones                           |
| Confirmación inválida         | Pide que responda únicamente "sí" o "no"                         |
| Email con formato inválido    | Informa el error y solicita nuevamente                           |
| Inactividad por 3 minutos     | Cierra la sesión automáticamente                                 |
| Archivo o multimedia recibido | Informa que solo se acepta texto                                 |

---

## Dependencias

```
python-telegram-bot==21.0.1
openpyxl==3.1.2
python-dotenv==1.0.1
```

---

## Documentación

- [**Manual de usuario**](Documentacion/MANUAL_DE_USUARIO.md)
- [**Diccionario de datos**](Documentacion/DICCIONARIO_DATOS.md)
- [**Maquina de estados**](Documentacion/ESTADOS_BOT.md)
- [**Capturas de bot**](Documentacion/Capturas)
- [**BPMN AS-IS y TO-BE**](Documentacion/BPMN)
