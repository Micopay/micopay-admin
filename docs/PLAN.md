# Plan de la consola de administración

Versión pública del plan. Describe el producto y su diseño.

## 1. Propósito

Crear una consola interna en `admin.micopay.com.mx` para que el equipo entienda qué pasa con una persona o una operación sin consultar la base de datos a mano.

Debe responder tres preguntas: quién es el usuario, qué le impide operar y qué está pasando con su operación.

Es exclusiva para personal interno. Un portal para que los agentes administren su propio negocio sería otro producto.

## 2. Alcance

| Incluido | Fuera del alcance |
|---|---|
| Acceso individual con verificación en dos pasos | Mover, congelar o desviar fondos |
| Directorio de usuarios y búsqueda | Editar saldos o direcciones Stellar |
| Ficha de usuario, de consulta | Aprobar KYC a mano |
| Notas internas e historial del equipo | Marcar una operación como completada |
| Estado de cada operación en MicoPay y en el contrato | Suspender cuentas o editar perfiles, en la primera versión |
| Preparación de agentes en MicoPay y en Stellar | Crear wallets o actuar en nombre de un usuario |

## 3. Pantallas

| Pantalla | Para qué sirve |
|---|---|
| Usuarios | Buscar por ID, usuario, dirección Stellar o ID de operación, y abrir la ficha |
| Agentes | Ver a cada agente con su veredicto de preparación |
| Operaciones | Ver el estado en MicoPay junto al del contrato y las diferencias |
| Disputas y casos | Consultar los casos abiertos y su contexto |
| Actividad | Ver el historial de acciones del equipo |

La ficha muestra por separado la cuenta, la pertenencia a la red, la disponibilidad y la verificación de identidad. Cada indicador es una dimensión distinta.

La interfaz separa siempre el entorno de servicio de la red de activos. «Producción · Stellar testnet» es una combinación válida, y la etiqueta «Producción» no significa dinero real.

Sigue el sistema visual de la app: Archivo, papel y tinta, verde para lo digital y naranja para el efectivo, canto vivo y sombra sólida. Un error de red nunca se muestra como un cero o como «sin problemas».

## 4. Arquitectura

- **Aplicación web:** React, TypeScript y Vite, en este repositorio.
- **API y reglas:** en MicoPay Protocol, bajo `/api/admin`. No hay otra base de usuarios.
- **Hosting:** aplicación estática con un proxy del mismo origen hacia la API.
- **Identidad:** un proveedor con verificación en dos pasos y operadores con nombre.

```text
Operador → admin.micopay.com.mx → /api/admin → backend de MicoPay → PostgreSQL
                                                     ↓
                                     Stellar (solo lectura del escrow y las cuentas)
```

Cada petición se autoriza en el servidor. La dirección del panel y el proxy no son una protección. El navegador no guarda secretos ni datos de usuarios para uso sin conexión.

## 5. Estado en la cadena

En un escrow de autocustodia, la fuente de verdad es el contrato.

- Cada operación se identifica por contrato y `trade_id`, porque hay un contrato por activo.
- La consola muestra lo leído en el contrato, cuándo se leyó y el enlace a stellar.expert de la red correcta.
- Si la lectura falla, muestra «No verificable». Nunca infiere un estado ni lo reescribe.

## 6. Preparación de agentes

| Revisión | Fuente |
|---|---|
| Alta, verificación de identidad, zona y términos | MicoPay |
| Disponibilidad o pausa | MicoPay, mostrada aparte |
| La cuenta existe y tiene XLM para comisiones | Horizon |
| Trustline y saldo del activo exacto | Horizon |
| Cuenta autorizada por el emisor | Horizon |

El veredicto dice «Listo para operar» o «No puede operar», con el motivo y la acción que el agente debe hacer en su propia wallet. Si una fuente falla, el resultado es «No verificable».

## 7. Casos de soporte

Soporte no puede mover fondos. La consola muestra el caso, su evidencia y lo que el contrato todavía permite.

Ninguna acción del panel reembolsa una operación con constancia de efectivo o con un reclamo abierto.

## 8. Privacidad

- Solo se muestran los campos necesarios, y las respuestas son específicas para el panel.
- Los documentos de identidad se quedan con su proveedor.
- Las notas internas no guardan documentos ni secretos.
- Los datos de prueba son sintéticos; nunca se copia una base real al repositorio.

## 9. Fases

| Fase | Entrega |
|---|---|
| 1 | Acceso de operadores y base de la API |
| 2 | Directorio, ficha, búsqueda y disputas de consulta |
| 3 | Notas e historial del equipo |
| 4 | Estado en la cadena y diferencias |
| 5 | Preparación de agentes |
| Posterior | Edición acotada de cuentas, equipo y roles, métricas e integraciones |
