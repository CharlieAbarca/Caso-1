# Identidades y Funcionalidades en Payment Assistant

A continuación se describe el mapa de entidades en *Payment Assistant* y cómo cada tabla satisface los requerimientos de usuarios, permisos, archivos y adjuntos, administración de organizaciones, transacciones financieras, suscripciones, bitácoras, soporte internacional y mapeo general entre las tablas y los datos que manejan.

---

## 1. Usuarios y Perfiles de Usuarios

### Entidades Relacionadas
- **`pay_user`**  
- **`pay_user_roles`**  
- **`pay_contact`**  
- **`pay_contact_info_type`**  

#### Descripción
La tabla **`pay_user`** almacena datos básicos y de autenticación (nombre, apellidos, contraseña, teléfono, fecha de nacimiento, etc.). A través de **`pay_user_roles`**, cada usuario puede ligarse a uno o más roles administrativos o de consumo. Adicionalmente, **`pay_contact`** y **`pay_contact_info_type`** permiten al usuario definir métodos de contacto (email, teléfono, etc.) para recibir notificaciones o comunicación oficial de la plataforma.

**Campos Destacados**  
- `pay_user.name` / `pay_user.last_name`: cadenas de texto para nombres.  
- `pay_user.phone`: campo para almacenar números de teléfono.  
- `pay_user.birth`: almacena fecha de nacimiento.  
- `pay_user.password`: campo que guarda la contraseña con la fortaleza adecuada (idealmente con hash).  

**Tipo de Dato Adecuado**:  
- Se utilizan `VARCHAR` o `TEXT` para campos como nombre y contraseña.  
- `DATE` para nacimiento (`birth`).  
- `BIT(1)` para campos booleanos (p.ej. `active`, `delete`).  

---

## 2. Permisos, Roles, Accesos y Autorizaciones

### Entidades Relacionadas
- **`pay_roles`**  
- **`pay_permissions`**  
- **`pay_modules`**  
- **`pay_rolespermissions`**  
- **`pay_user_permissions`**  

#### Descripción
El sistema de autorización se basa en los roles y permisos:

1. **Roles** (tabla **`pay_roles`**): define distintos perfiles (Administrador, Cliente, Soporte, etc.).  
2. **Permisos** (tabla **`pay_permissions`**): describe acciones puntuales (crear, leer, editar, eliminar, etc.), ligadas a **módulos** específicos.  
3. **Módulos** (tabla **`pay_modules`**): organizan el sistema en secciones (pago, suscripciones, reportes, etc.).  
4. **Relación Roles-Permisos** (tabla **`pay_rolespermissions`**): asigna uno o varios permisos a cada rol.  
5. **Permisos Individuales de Usuario** (tabla **`pay_user_permissions`**): permite dar a un usuario permisos directos especiales, al margen de sus roles.

**Campos Destacados**  
- `pay_permissions.description`: Detalles del permiso.  
- `pay_roles.name_role`: Nombre del rol.  
- `pay_modules.name`: Nombre del módulo.  
- `pay_rolespermissions.enabled`: Campo tipo bit para indicar si un permiso está vigente.  

**Tipo de Dato Adecuado**:  
- `INT` para las llaves foráneas que relacionan roles, permisos y módulos.  
- `DATETIME` para `last_update` con el fin de seguir la pista de cambios.

---

## 3. Archivos, Adjuntos, Grabaciones, Streaming, y Media Files

### Entidades Relacionadas
- **`pay_method`** (campo `logoIconURL`)  
- **`subscriptions`** (campo `logourl`)  
- **`ia_interactions`** (campo `audio_transcription`)  

#### Descripción
Aunque la base de datos no presenta una tabla específica para *archivos* o *storage*, existen columnas para almacenar rutas o binarios de íconos, grabaciones de audio, etc. Por ejemplo:

- **`pay_method.logoIconURL`**: un campo binario o string que guarda la URL o el ícono del método de pago.  
- **`subscriptions.logourl`**: ruta o binario del logotipo asociado a cada suscripción.  
- **`ia_interactions.audio_transcription`**: transcripción del audio de la interacción con IA; en caso de requerirse, se podría almacenar un enlace (URL) a la grabación original.

Para un escenario más avanzado, se sugiere una tabla adicional llamada, por ejemplo, **`file_storage`**, que contenga el `file_id`, `file_url`, `metadata` y la referencia externa a la tabla que lo solicita. De esta forma se pueden almacenar y gestionar todos los archivos en un servicio externo (S3, Azure Blob, etc.), relacionándolos con los registros de la BD.

---

## 4. Administración de Empresas / Estructuras Organizacionales

### Entidades Relacionadas
- **`pay_user`** (relación con un campo adicional para *empresa_id* en caso de necesitarlo)  
- **`pay_country`** / **`pay_culture`** (para establecer países o culturas de empresas en caso de requerirse)  

#### Descripción
La base de datos no presenta una tabla específica llamada “empresa” o “compañía” (como tal), pero se pueden extender las tablas existentes:

- Si se desea gestionar empresas o estructuras organizacionales, puede crearse una tabla **`companies`** para almacenar datos de compañías y relacionarla con **`pay_user`** por su `user_id`, en caso de que un usuario pertenezca a dicha empresa.
- **`pay_country`** y **`pay_culture`** podrían usarse para definir la ubicación y configuración regional de cada empresa, en caso de expandir la aplicación a nivel internacional.

---

## 5. Transacciones, Pagos, Balances, Devoluciones, Reclamos, Precios y Renovaciones

### Entidades Relacionadas
- **`payments`**  
- **`transactions`**  
- **`userbalance`**  
- **`payment_notifications`**  

#### Descripción
Todas las operaciones monetarias están centralizadas en:

1. **`payments`**: cada pago realizado, ya sea por pago puntual o recurrente.  
2. **`transactions`**: detalla información adicional de cada pago, referencias externas, montos, fecha, etc.  
3. **`userbalance`**: controla el saldo global del usuario, reflejando recargas, congelaciones o ajustes.  
4. **`payment_notifications`**: notifica al usuario de pagos próximos o confirmaciones realizadas.

Las devoluciones o reclamos podrían integrarse con un campo adicional en **`transactions`** (por ejemplo, `transaction_type = 'REFUND'`) o con una tabla específica **`refunds`** si se requiere manejo complejo de reembolsos.

---

## 6. Suscripciones, Planes, Vigencias, Límites y Uso

### Entidades Relacionadas
- **`subscriptions`**  
- **`features_per_plan`**  
- **`plan_prices`**  
- **`plan_per_user`**  
- **`user_plan_limit`**  

#### Descripción
La plataforma ofrece distintos planes (versión gratuita, premium, etc.). Para ello:

1. **`subscriptions`**: define los planes (nombres, descripción, íconos).  
2. **`features_per_plan`**: detalla las características específicas que cada plan ofrece.  
3. **`plan_prices`**: controla los precios, la recurrencia (mensual, anual) y la vigencia.  
4. **`plan_per_user`**: relaciona un usuario con un plan adquirido, indicando fecha de adquisición y si el plan sigue vigente.  
5. **`user_plan_limit`**: especifica los límites por usuario, según el plan (p. ej., número máximo de pagos mensuales).

Para la renovación de planes y control de vigencias, se usan campos como `endDate` en **`plan_prices`** y `acquisition` en **`plan_per_user`**.

---

## 7. Bitácora o Bitácoras del Sistema

### Entidades Relacionadas
- **`logs`**  
- **`logtypes`**  
- **`logsources`**  
- **`logserenty`** (log severity)  

#### Descripción
Todo lo relacionado con la auditoría y el monitoreo se guarda en:

- **`logs`**: tabla principal de bitácoras, almacena la descripción del evento, hora y referencias a otros datos.  
- **`logtypes`**: define qué tipo de evento se ha registrado (error, transacción, seguridad, etc.).  
- **`logsources`**: identifica la fuente del log (un módulo específico, un subsistema, etc.).  
- **`logserenty`**: un nivel de gravedad (información, error, alerta) para cada log.

Esta estructura es fundamental para detectar y rastrear errores, monitorear las operaciones y mantener la trazabilidad de cualquier evento relevante en la plataforma.

---

## 8. Tablas para Soportar Internacionalización (Idiomas y Monedas)

### Entidades Relacionadas
- **`pay_country`**  
- **`pay_culture`**  

#### Descripción
Para ajustarse a múltiples regiones:

1. **`pay_country`**: define la lista de países y relaciona la entidad `contact_id` si fuera necesario.  
2. **`pay_culture`**: define la moneda y el idioma para cada país, indicando la referencia `pay_country_country_id`.

Esta sección soporta la internacionalización, proporcionando la moneda y el idioma correctos para el usuario o la organización.

---

## 9. Mapeo y Validación de Diseño

A continuación se muestra el mapeo general entre las entidades (tablas) y la información que manejan:

| **Entidad**               | **Función Principal**                                                                                       | **Campos Clave**                                 | **Tipo de Dato**       |
|---------------------------|-------------------------------------------------------------------------------------------------------------|---------------------------------------------------|------------------------|
| `pay_user`                | Almacenar datos personales, de autenticación y estado de cada usuario                                       | `name`, `last_name`, `phone`, `birth`, `password` | VARCHAR, DATE, BIT(1)  |
| `pay_roles`               | Definir roles para asignación de permisos                                                                   | `roleid`, `name_role`, `description`             | INT, VARCHAR           |
| `pay_permissions`         | Especificar acciones puntuales (crear, eliminar, etc.)                                                      | `permissions_id`, `description`, `code`          | INT, VARCHAR           |
| `pay_modules`             | Agrupar permisos por módulos o secciones                                                                    | `moduleid`, `name`                                | TINYINT(8), VARCHAR    |
| `pay_rolespermissions`    | Asociar un rol a uno o varios permisos                                                                      | `rolespermissions_id`, `enabled`                 | INT, BIT(1)            |
| `pay_user_roles`          | Asignar roles a un usuario                                                                                  | `userrolesid`, `roleid`                           | INT                    |
| `pay_user_permissions`    | Permisos directos adicionales por usuario                                                                   | `rolepermissionsid`, `permissions_id`, `enabled` | INT, BIT(1)            |
| `ia_session`              | Manejar la sesión de IA (token, config, API URL)                                                            | `session_id`, `user_id`, `session_token`         | INT, VARCHAR, JSON     |
| `ia_type`                 | Definir tipos de interacciones de IA                                                                         | `type_id`, `name`                                 | INT, VARCHAR           |
| `ia_interactions`         | Registrar comandos y respuestas de IA                                                                        | `interactions_id`, `ia_answer`, `audio_transcription` | INT, VARCHAR       |
| `pay_available_method`    | Guardar métodos de pago (tokens, máscaras de cuenta, etc.)                                                  | `available_method`, `mask_account`, `token`       | INT, VARCHAR           |
| `pay_method`              | Mantener la configuración de cada método de pago (keys, logos, JSON de config)                              | `metodo_id`, `name`, `secret_key`, `apiURL`       | INT, VARCHAR, JSON     |
| `payments`                | Registrar cada pago (monto, fecha, estado, descripción)                                                     | `payments_id`, `price`, `currency`               | BIGINT, DECIMAL, CHAR(3) |
| `transactions`            | Almacenar detalles de una transacción específica                                                            | `transaction_id`, `amount`, `description`         | INT, DECIMAL, VARCHAR  |
| `userbalance`             | Controlar el saldo de cada usuario y las operaciones que afectan su balance                                 | `balanceid`, `balance`, `lastbalance`            | INT, DECIMAL           |
| `payment_notifications`   | Notificar al usuario sobre eventos de pago                                                                  | `notificationid`, `message`, `details_id`         | INT, VARCHAR           |
| `subscriptions`           | Definir planes (nombre, descripción, logotipo)                                                              | `subscriptions_id`, `description`, `logourl`      | INT, VARCHAR, VARBINARY |
| `features_per_plan`       | Indicar las características de un plan (valor, habilitado, referencia a `subscriptions_id`)                 | `featuresPerPlan_id`, `value`, `enable`           | INT, VARCHAR, BIT(1)   |
| `plan_prices`             | Especificar precios y vigencia de cada plan                                                                 | `prices_id`, `amount`, `endDate`                 | INT, DECIMAL, DATETIME |
| `plan_per_user`           | Relacionar un usuario con un plan                                                                           | `planuser_id`, `user_id`, `prices_id`            | INT, DATETIME, BIT(1)  |
| `user_plan_limit`         | Definir límites de uso por suscripción (número máximo de transacciones, etc.)                               | `limitid`, `limit`, `planuser_id`                | INT                    |
| `schedule`                | Configurar la recurrencia y el fin de la programación (cantidad de repeticiones, fecha de finalización)      | `schedule_id`, `name`, `end_date`, `repetitions`  | INT, VARCHAR, DATE     |
| `schedule_details`        | Detalles sobre fechas concretas de la programación                                                          | `details_id`, `baseDate:`, `LastExecute`          | INT, DATE, DATETIME    |
| `logs`                    | Almacenar la bitácora con descripción, timestamps y referencias                                             | `logsid`, `description`, `referenceid1`          | INT, VARCHAR, DATETIME |
| `logtypes`                | Definir tipos de logs (error, info, etc.)                                                                   | `logtypeid`, `name`, `ref1Desc`, `ref2Desc`       | INT, VARCHAR           |
| `logsources`              | Registrar la fuente del log (qué aplicación o subsistema lo generó)                                         | `logsourcesid`, `name`                            | INT, VARCHAR           |
| `logserenty`              | Definir severidad del log (critical, error, warning, info)                                                  | `logsrentyid`, `name`                             | INT, VARCHAR           |
| `pay_country`             | Lista de países, asociando un `contact_id` si se requiere                                                   | `country_id`, `name`, `contact_id`               | INT, VARCHAR           |
| `pay_culture`             | Define moneda y lenguaje por país                                                                           | `culture_id`, `currency`, `language`             | INT, VARCHAR           |

Mediante esta tabla de referencia, se puede comprobar que cada entidad y cada campo tienen el tipo de dato apropiado (por ejemplo, `INT` para claves, `VARCHAR` para nombres y descripciones, `DECIMAL` para montos, `DATE`/`DATETIME` para fechas, `JSON` cuando se requiera guardado de datos estructurados, etc.). Asimismo, cubrimos la totalidad de las necesidades funcionales descritas (usuarios, permisos, archivos, organizaciones, transacciones, subscripciones, bitácoras e internacionalización).

