# Introducción

**Documentación para caso uno de:**
- **Carlos García Molina (2024181023)**
- **Carlos Abarca Mora (2024138701)**

**Nombre de la aplicación:** Payment Assistant  
**Descripción breve:** Aplicación que ofrece servicios de ayuda para realizar pagos automáticos mediante IA de voz.

## Interacción por Voz y Registro de Pagos 

**Funcionalidad:**  
- La aplicación permite a los usuarios registrar pagos recurrentes mediante comandos de voz.  
- Se utiliza procesamiento de lenguaje natural para identificar detalles como el servicio, monto, fecha y frecuencia.  
- Cada interacción se confirma con el usuario antes de ser almacenada.

**Tablas involucradas:**  
- **`pay_user`**: Almacena la información personal y de autenticación (ID, nombre, contraseña, etc.).  
- **`ia_interactions`**: Registra cada interacción, incluyendo la transcripción de voz (en su campo `audio_transcription`) y las respuestas generadas.  
- **`ia_session`**: Gestiona y vincula las sesiones de interacción con la IA, incluyendo la configuración y estado de cada sesión activa.  
- **`ia_type`**: Define los tipos de interacciones o acciones que la IA puede manejar.  

Esta combinación permite que el sistema capture con precisión las órdenes emitidas por voz y las vincule a la identidad del usuario, facilitando el seguimiento y la mejora continua de la experiencia. Esta funcionalidad la utilizan aplicaciones como Google Assistant y Amazon Alexa para permitir a los usuarios interactuar de manera natural y sin fricciones. Los usuarios se benefician de la rapidez y facilidad de uso, reduciendo errores de entrada manual y haciendo el proceso mucho más intuitivo.

---

## Gestión de Cuentas Bancarias y Métodos de Pago 

**Funcionalidad:**  
- Los usuarios pueden configurar sus cuentas bancarias, registrando datos como el número de cuenta, nombre del banco y saldo.  
- La aplicación permite vincular métodos de pago (como tarjetas o transferencias) para autorizar transacciones automáticas.  
- Se pueden editar o eliminar cuentas y métodos de pago cuando sea necesario.  

**Tablas involucradas:**  
- **`pay_available_method`**: Almacena información detallada de métodos de pago disponibles.  
- **`pay_method`**: Detalla los métodos de pago, incluyendo token, fechas, secretos para integraciones, etc.  
- **`pay_contact`**: Permite almacenar datos de contacto asociados a un usuario, incluyendo correos o teléfonos para notificaciones.  
- **`pay_contact_info_type`**: Clasifica los distintos tipos de contacto (teléfono, email, etc.) vinculados a `pay_contact`.  

Estas tablas aseguran que toda la información financiera y de contacto esté correctamente vinculada a cada usuario, permitiendo transacciones seguras y verificables. Esta funcionalidad la utilizan aplicaciones como Mint para la gestión de cuentas y la monitorización de saldos. Los usuarios se benefician al tener toda su información financiera consolidada y actualizada, lo que permite transacciones seguras y verificables.

---

## Ejecución de Pagos y Registro de Transacciones 

**Funcionalidad:**  
- Una vez que se confirma un pago (mediante notificaciones SMS o email), la aplicación ejecuta la transacción de forma automática.  
- Se registra cada transacción, capturando datos como el monto, la fecha, el método de pago utilizado y el estado (éxito o fallo).  
- Se mantiene un historial completo para análisis y consulta futura.  

**Tablas involucradas:**  
- **`payments`**: Registra cada pago realizado.  
- **`transactions`**: Almacena detalles ampliados de cada transacción, incluyendo referencias y estado.  
- **`userbalance`**: Controla el saldo y registra los movimientos financieros asociados al usuario.  
- **`payment_notifications`**: Gestiona el envío de recordatorios y confirmaciones de pago.  

Este conjunto de tablas permite un seguimiento detallado de todas las transacciones, garantizando que cada pago se ejecute y se registre correctamente para futuros análisis y auditorías. Esta funcionalidad la utilizan aplicaciones como Braintree Payments, que ofrecen un sistema robusto para procesar pagos y mantener un historial detallado de transacciones. Los usuarios se benefician al contar con un registro transparente de sus operaciones, lo que facilita el seguimiento y la resolución de cualquier inconveniente.

---

## Planes y Monetización 

**Funcionalidad:**  
- La aplicación debe ofrecer una opción gratuita que permita un pago único al mes.  
- Además, se deben ofrecer planes de suscripción escalables basados en la cantidad de pagos y transferencias mensuales que el usuario requiera.  
- El sistema gestionará de forma automática la facturación y la renovación de estas suscripciones.  

**Tablas involucradas:**  
- **`subscriptions`**: Define los planes disponibles en la aplicación, incluyendo descripciones y detalles relevantes.  
- **`features_per_plan`**: Especifica las características adicionales que se incluyen en cada plan, permitiendo diferenciar la oferta en función del nivel de servicio.  
- **`plan_prices`**: Detalla el precio, la periodicidad (mensual, anual, etc.) y la vigencia de cada plan de suscripción.  
- **`plan_per_user`**: Relaciona el plan de suscripción con cada usuario, permitiendo saber qué plan ha seleccionado cada cliente.  
- **`user_plan_limit`**: Establece restricciones específicas, como la cantidad máxima de pagos o transferencias mensuales que se permiten en cada plan.  

Este modelo permite gestionar de manera integral las suscripciones del usuario, garantizando que el plan gratuito se mantenga con un solo pago mensual, mientras que los planes de suscripción premium se ajustan a la cantidad de transacciones requeridas y se renuevan automáticamente, facilitando la facturación continua. Esta funcionalidad la utilizan servicios como Stripe Checkout para gestionar suscripciones y planes de precios de forma dinámica. Los usuarios se benefician de una oferta flexible que se adapta a sus necesidades, facilitando la comparación y elección de un plan que maximice su valor.

---

## Programación y Recordatorios 

**Funcionalidad:**  
- La aplicación programa la ejecución de pagos según la recurrencia definida (diaria, semanal, mensual, etc.) y envía recordatorios para confirmar los pagos.  
- Los recordatorios se gestionan mediante notificaciones que se envían al usuario, garantizando que no se omitan transacciones importantes.  

**Tablas involucradas:**  
- **`schedule`**: Configura la recurrencia y el número de repeticiones.  
- **`schedule_details`**: Registra los detalles de cada ejecución programada, como la fecha base y la próxima ejecución.  
- **`payment_notifications`**: Gestiona el envío y registro de notificaciones y recordatorios (sustituyendo la antigua tabla `Notificacion`).  

Este sistema asegura que los pagos se realicen a tiempo y que el usuario siempre reciba la información necesaria para autorizar cada transacción. Esta funcionalidad la utilizan aplicaciones como Rocket Money o Prism Bill Pay, que programan recordatorios y ayudan a gestionar las fechas de pago. Los usuarios se benefician al evitar olvidos y multas, manteniendo un flujo de pagos puntual y organizado.

---

## Seguridad, Roles y Permisos 

**Funcionalidad:**  
- Para proteger el sistema y la información del usuario, se implementa un robusto sistema de roles y permisos.  
- Esto define qué acciones puede realizar cada usuario y asegura que solo personal autorizado acceda a determinadas funcionalidades.  

**Tablas involucradas:**  
- **`pay_roles`**: Define los distintos roles dentro del sistema.  
- **`pay_modules`**: Organiza las funcionalidades en módulos o áreas.  
- **`pay_permissions`**: Establece los permisos asociados a cada módulo o funcionalidad.  
- **`pay_rolespermissions`**: Relaciona roles con los permisos correspondientes.  
- **`pay_user_roles`**: Asocia cada usuario a uno o varios roles.  
- **`pay_user_permissions`**: Permite asignar permisos individuales directamente a un usuario, si fuera necesario.  

Estas tablas forman la columna vertebral del sistema de seguridad, asegurando que el acceso a funciones críticas esté adecuadamente controlado. Esta funcionalidad la utilizan aplicaciones como Auth0 para implementar controles de acceso basados en roles (RBAC). Los usuarios y administradores se benefician al contar con un sistema que restringe el acceso a funciones sensibles, aumentando la seguridad del sistema.

---

## Integración con APIs Externas y Gestión de Logs 

**Funcionalidad:**  
- La aplicación se integra con APIs externas para procesar pagos y enviar SMS, lo que permite automatizar la ejecución de transacciones.  
- Además, se registra cada evento y error en un sistema de logs que facilita la auditoría y el monitoreo del sistema.  

**Tablas involucradas:**  
- **`ia_session`**: Maneja las credenciales y la configuración para integraciones con servicios de IA (incluyendo `api_url`), y puede emplearse como base para otras integraciones externas.  
- **`logs`**: Almacena el registro completo de eventos y errores para auditoría.  
- **`logtypes`**: Clasifica los tipos de logs generados.  
- **`logsources`**: Registra la fuente de cada evento o error.  
- **`logserenty`**: Define la “gravedad” (severity) o categoría adicional para los registros.  

Estas tablas aseguran que la integración externa y el registro de actividades se realicen de forma transparente y eficiente. Esta funcionalidad la utilizan aplicaciones como Stripe y Twilio para integrar pagos y envíos de SMS, mientras que plataformas como Datadog y Splunk gestionan los logs y eventos del sistema. Los usuarios se benefician al contar con procesos automatizados y un sistema de monitoreo que garantiza la estabilidad y la seguridad del servicio.

---

## Soporte Internacional y Datos Complementarios 

**Funcionalidad:**  
- Para adaptarse a un mercado global, Payment Assistant incorpora soporte para diferentes monedas, idiomas y países.  
- Esto permite que la aplicación ajuste los formatos, precios y la experiencia de usuario de acuerdo con la región.  

**Tablas involucradas:**  
- **`pay_country`**: Relaciona países con sus datos de contacto (`pay_contact`) y la información relevante.  
- **`pay_culture`**: Define la moneda y el idioma utilizados en la aplicación, vinculándose a un país en particular.  

Estas tablas complementarias permiten que la aplicación opere de forma personalizada y escalable en diferentes mercados internacionales. Esta funcionalidad la utilizan aplicaciones como Revolut y Wise, que gestionan múltiples monedas y ofrecen soporte multilingüe. Los usuarios se benefician de una plataforma que les permite operar en su moneda local y recibir información adaptada a su región, lo que mejora la experiencia global del servicio.

---

## Otros Datos Complementarios 

**Funcionalidad:**  
- Algunas tablas adicionales aseguran una clasificación más fina de las transacciones y facilitan el análisis de datos.  

**Tablas involucradas:**  
- **`features_plan`**: Gestiona características específicas de los planes (complementando `features_per_plan`).  
- **`plan_per_user`**: Mantiene las suscripciones activas de cada usuario, junto con los límites definidos en `user_plan_limit`.  

Esta funcionalidad es crucial para sistemas financieros que requieren un análisis detallado y categorización precisa de cada transacción o plan. Empresas de análisis de datos y auditoría financiera utilizan esta aproximación para identificar patrones, optimizar procesos y mejorar la precisión en la gestión de transacciones.

---

# Lista de Identidades Basadas en la Base de Datos

En el contexto de *Payment Assistant*, varias tablas representan o gestionan las distintas “identidades” que participan en el sistema. A continuación se muestra una lista de esas identidades principales y su función:

1. **pay_user**  
   - **Función:** Almacena la información esencial de cada usuario, como su nombre, apellidos, teléfono, fecha de nacimiento, contraseñas y estado de activación.  
   - **Clave Primaria:** `user_id`  
   - **Notas:** Es la entidad central para identificar a cada persona que ingresa y utiliza el sistema.

2. **pay_roles**  
   - **Función:** Define los distintos roles dentro del sistema (por ejemplo, administrador, usuario estándar, etc.).  
   - **Clave Primaria:** `roleid`  
   - **Notas:** Permite agrupar permisos y otorgar funciones específicas a cada grupo de usuarios.

3. **pay_permissions**  
   - **Función:** Establece los permisos asociados a ciertos módulos o acciones (lectura, escritura, eliminación, etc.).  
   - **Clave Primaria:** `permissions_id`  
   - **Notas:** Sirve para detallar qué acciones concretas se pueden realizar dentro del sistema.

4. **pay_modules**  
   - **Función:** Organiza las distintas funcionalidades en “módulos” o secciones del sistema (por ejemplo, módulo de pagos, módulo de suscripciones).  
   - **Clave Primaria:** `moduleid`  
   - **Notas:** Cada módulo se asocia con uno o varios permisos.

5. **pay_rolespermissions**  
   - **Función:** Relaciona los roles con los permisos específicos que cada rol posee.  
   - **Clave Primaria:** `rolespermissions_id`  
   - **Notas:** Permite saber, a nivel de sistema, qué rol está autorizado a qué permiso.

6. **pay_user_roles**  
   - **Función:** Asocia cada usuario a uno o varios roles; por ejemplo, un usuario podría tener el rol de administrador y de editor al mismo tiempo.  
   - **Clave Primaria:** `userrolesid`  
   - **Notas:** Define la pertenencia de un usuario a un rol concreto, facilitando la asignación de permisos masivos.

7. **pay_user_permissions**  
   - **Función:** Permite asignar permisos individuales a cada usuario, por encima o aparte de los roles asignados.  
   - **Clave Primaria:** `rolepermissionsid`  
   - **Notas:** Útil para conceder excepciones o permisos puntuales a determinados usuarios sin cambiar la configuración de roles.

8. **ia_session**  
   - **Función:** Gestiona las sesiones activas de interacción con inteligencia artificial y guarda el estado de cada sesión.  
   - **Clave Primaria:** `session_id`  
   - **Notas:** Representa la “identidad” de una sesión de IA en curso, vinculada a un `user_id`.

9. **ia_type**  
   - **Función:** Define los distintos tipos de interacciones de la IA (por ejemplo, comandos de voz, confirmaciones, cancelaciones, etc.).  
   - **Clave Primaria:** `type_id`  
   - **Notas:** Cada tipo de interacción ayuda a clasificar las respuestas y acciones que el sistema de IA soporta.

10. **ia_interactions**  
   - **Función:** Registra las interacciones de IA (texto, voz, resultados, etc.) dentro de cada sesión, permitiendo un historial.  
   - **Clave Primaria:** `interactions_id`  
   - **Notas:** Vincula la información de la tabla `ia_type` y la tabla `ia_session` para rastrear cada comando o respuesta.

---

# Identidades y Funcionalidades en Payment Assistant

A continuación se describe el mapa de entidades en *Payment Assistant* y cómo cada tabla satisface los requerimientos de usuarios, permisos, archivos y adjuntos, administración de organizaciones, transacciones financieras, suscripciones, bitácoras, soporte internacional y mapeo general entre las tablas y los datos que manejan.

---

## 1. Usuarios y Perfiles de Usuarios

### Entidades Relacionadas
- **`pay_user`**  
- **`pay_user_roles`**  
- **`pay_contact`**  
- **`pay_contact_info_type`**  

**Descripción:**  
La tabla **`pay_user`** almacena datos básicos y de autenticación (nombre, apellidos, contraseña, teléfono, fecha de nacimiento, etc.). A través de **`pay_user_roles`**, cada usuario puede ligarse a uno o más roles administrativos o de consumo. Adicionalmente, **`pay_contact`** y **`pay_contact_info_type`** permiten al usuario definir métodos de contacto (email, teléfono, etc.) para recibir notificaciones o comunicación oficial de la plataforma.

---

## 2. Permisos, Roles, Accesos y Autorizaciones

### Entidades Relacionadas
- **`pay_roles`**
- **`pay_permissions`**
- **`pay_modules`**
- **`pay_rolespermissions`**
- **`pay_user_permissions`**

**Descripción:**  
El sistema de autorización se basa en los roles y permisos:

1. **Roles** (tabla **`pay_roles`**): define distintos perfiles (Administrador, Cliente, Soporte, etc.).  
2. **Permisos** (tabla **`pay_permissions`**): describe acciones puntuales (crear, leer, editar, eliminar, etc.), ligadas a **módulos** específicos.  
3. **Módulos** (tabla **`pay_modules`**): organizan el sistema en secciones (pago, suscripciones, reportes, etc.).  
4. **Relación Roles-Permisos** (tabla **`pay_rolespermissions`**): asigna uno o varios permisos a cada rol.  
5. **Permisos Individuales de Usuario** (tabla **`pay_user_permissions`**): permite dar a un usuario permisos directos especiales, al margen de sus roles.

---

## 3. Archivos, Adjuntos, Grabaciones, Streaming, y Media Files

### Entidades Relacionadas
- **`pay_method`** (campo `logoIconURL`)
- **`subscriptions`** (campo `logourl`)
- **`ia_interactions`** (campo `audio_transcription`)

**Descripción:**  
Aunque la base de datos no presenta una tabla específica para almacenar archivos o adjuntos, existen columnas para guardar rutas o binarios de íconos, grabaciones de audio, etc. Si se requiriera un sistema más robusto de gestión de archivos, se podría crear una tabla adicional (p.e. **`file_storage`**) que vincule los registros en la BD con un servicio externo (S3, Azure Blob, etc.).

---

## 4. Administración de Empresas / Estructuras Organizacionales

### Entidades Relacionadas
- **`pay_user`** (ampliable con un campo `empresa_id`)
- **`pay_country`** / **`pay_culture`** (para asociar localización)

**Descripción:**  
No existe una tabla llamada “empresa” por defecto. Sin embargo, se puede crear una tabla **`companies`** para manejar organizaciones y vincularla a **`pay_user`** mediante su `user_id`, si se desea soportar usuarios que pertenezcan a una empresa o grupo específico.

---

## 5. Transacciones, Pagos, Balances, Devoluciones y Reclamos

### Entidades Relacionadas
- **`payments`**
- **`transactions`**
- **`userbalance`**
- **`payment_notifications`**

**Descripción:**  
1. **`payments`**: registra cada pago realizado.  
2. **`transactions`**: guarda detalles de la transacción (referencias, estado).  
3. **`userbalance`**: mantiene el saldo y los movimientos de cada usuario.  
4. **`payment_notifications`**: gestiona avisos y confirmaciones de pago.

Las devoluciones o reclamos pueden manejarse con un campo adicional en **`transactions`** (ej. `transaction_type`) o en una tabla especializada (ej. **`refunds`**).

---

## 6. Suscripciones, Planes, Vigencias y Límites

### Entidades Relacionadas
- **`subscriptions`**
- **`features_per_plan`**
- **`plan_prices`**
- **`plan_per_user`**
- **`user_plan_limit`**

**Descripción:**  
- **`subscriptions`**: define planes con su descripción y logotipo.  
- **`features_per_plan`**: especifica características incluidas en cada plan.  
- **`plan_prices`**: controla costos y periodicidad de planes.  
- **`plan_per_user`**: relaciona el plan adquirido con cada usuario.  
- **`user_plan_limit`**: configura límites de uso según el plan contratado.

---

## 7. Bitácora o Bitácoras del Sistema

### Entidades Relacionadas
- **`logs`**
- **`logtypes`**
- **`logsources`**
- **`logserenty`**

**Descripción:**  
- **`logs`**: tabla principal con eventos y referencias.  
- **`logtypes`**: clasifica el tipo de evento (error, transacción, seguridad, etc.).  
- **`logsources`**: indica la fuente del evento.  
- **`logserenty`**: nivel de gravedad o severidad (info, warning, error).

---

## 8. Tablas para Soportar Internacionalización (Idiomas y Monedas)

### Entidades Relacionadas
- **`pay_country`**
- **`pay_culture`**

**Descripción:**  
- **`pay_country`**: lista de países y posibles vínculos con contactos u organizaciones.  
- **`pay_culture`**: idioma y moneda para cada país, permitiendo adaptar la interfaz o el procesamiento de pagos.

---

## 9. Mapeo y Validación de Diseño

La siguiente tabla ejemplifica el mapeo general entre cada entidad y los campos que maneja, así como el tipo de dato sugerido:

| **Entidad**               | **Función Principal**                                                                                       | **Campos Clave**                                 | **Tipo de Dato**         |
|---------------------------|-------------------------------------------------------------------------------------------------------------|---------------------------------------------------|--------------------------|
| `pay_user`                | Almacenar datos personales, autenticación y estado de cada usuario                                         | `name`, `last_name`, `phone`, `birth`, `password` | VARCHAR, DATE, BIT(1)    |
| `pay_roles`               | Definir roles para asignación de permisos                                                                   | `roleid`, `name_role`, `description`             | INT, VARCHAR             |
| `pay_permissions`         | Especificar acciones puntuales (crear, eliminar, etc.)                                                      | `permissions_id`, `description`, `code`          | INT, VARCHAR             |
| `pay_modules`             | Agrupar permisos por módulos o secciones                                                                    | `moduleid`, `name`                                | TINYINT(8), VARCHAR      |
| `pay_rolespermissions`    | Asociar un rol a uno o varios permisos                                                                      | `rolespermissions_id`, `enabled`                 | INT, BIT(1)              |
| `pay_user_roles`          | Asignar roles a un usuario                                                                                  | `userrolesid`, `roleid`                           | INT                      |
| `pay_user_permissions`    | Permisos directos adicionales por usuario                                                                   | `rolepermissionsid`, `permissions_id`, `enabled` | INT, BIT(1)              |
| `ia_session`              | Manejar la sesión de IA (token, config, API URL)                                                            | `session_id`, `user_id`, `session_token`         | INT, VARCHAR, JSON       |
| `ia_type`                 | Definir tipos de interacciones de IA                                                                         | `type_id`, `name`                                 | INT, VARCHAR             |
| `ia_interactions`         | Registrar comandos y respuestas de IA                                                                        | `interactions_id`, `ia_answer`, `audio_transcription` | INT, VARCHAR        |
| `pay_available_method`    | Guardar métodos de pago (tokens, máscaras de cuenta, etc.)                                                  | `available_method`, `mask_account`, `token`       | INT, VARCHAR             |
| `pay_method`              | Mantener la configuración de cada método de pago (keys, logos, JSON de config)                              | `metodo_id`, `name`, `secret_key`, `apiURL`       | INT, VARCHAR, JSON       |
| `payments`                | Registrar cada pago (monto, fecha, estado, descripción)                                                     | `payments_id`, `price`, `currency`               | BIGINT, DECIMAL, CHAR(3) |
| `transactions`            | Almacenar detalles de una transacción específica                                                            | `transaction_id`, `amount`, `description`         | INT, DECIMAL, VARCHAR    |
| `userbalance`             | Controlar el saldo de cada usuario y las operaciones que afectan su balance                                 | `balanceid`, `balance`, `lastbalance`            | INT, DECIMAL             |
| `payment_notifications`   | Notificar al usuario sobre eventos de pago                                                                  | `notificationid`, `message`, `details_id`         | INT, VARCHAR             |
| `subscriptions`           | Definir planes (nombre, descripción, logotipo)                                                              | `subscriptions_id`, `description`, `logourl`      | INT, VARCHAR, VARBINARY  |
| `features_per_plan`       | Indicar las características de un plan (valor, habilitado, referencia a `subscriptions_id`)                 | `featuresPerPlan_id`, `value`, `enable`           | INT, VARCHAR, BIT(1)     |
| `plan_prices`             | Especificar precios y vigencia de cada plan                                                                 | `prices_id`, `amount`, `endDate`                 | INT, DECIMAL, DATETIME   |
| `plan_per_user`           | Relacionar un usuario con un plan                                                                           | `planuser_id`, `user_id`, `prices_id`            | INT, DATETIME, BIT(1)    |
| `user_plan_limit`         | Definir límites de uso por suscripción (número máximo de transacciones, etc.)                               | `limitid`, `limit`, `planuser_id`                | INT                      |
| `schedule`                | Configurar la recurrencia y el fin de la programación (cantidad de repeticiones, fecha de finalización)      | `schedule_id`, `name`, `end_date`, `repetitions`  | INT, VARCHAR, DATE       |
| `schedule_details`        | Detalles sobre fechas concretas de la programación                                                          | `details_id`, `baseDate:`, `LastExecute`          | INT, DATE, DATETIME      |
| `logs`                    | Almacenar la bitácora con descripción, timestamps y referencias                                             | `logsid`, `description`, `referenceid1`          | INT, VARCHAR, DATETIME   |
| `logtypes`                | Definir tipos de logs (error, info, etc.)                                                                   | `logtypeid`, `name`, `ref1Desc`, `ref2Desc`       | INT, VARCHAR             |
| `logsources`              | Registrar la fuente del log (qué aplicación o subsistema lo generó)                                         | `logsourcesid`, `name`                            | INT, VARCHAR             |
| `logserenty`              | Definir severidad del log (critical, error, warning, info)                                                  | `logsrentyid`, `name`                             | INT, VARCHAR             |
| `pay_country`             | Lista de países, asociando un `contact_id` si se requiere                                                   | `country_id`, `name`, `contact_id`               | INT, VARCHAR             |
| `pay_culture`             | Define moneda y lenguaje por país                                                                           | `culture_id`, `currency`, `language`             | INT, VARCHAR             |

De esta forma, comprobamos que cada entidad y cada campo tienen el tipo de dato apropiado para sus necesidades, abarcando la totalidad de las funcionalidades de la plataforma: usuarios, permisos, archivos y adjuntos, estructuras organizacionales, transacciones financieras, suscripciones, bitácoras y soporte internacional.
