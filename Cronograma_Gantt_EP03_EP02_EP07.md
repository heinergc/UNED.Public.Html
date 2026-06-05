## 12. Cronograma tipo Gantt para EP-03, EP-02 y EP-07

Este cronograma se ajusta a la jornada indicada por el programador: **6 horas funcionales por dia, 5 dias por semana**, para una capacidad semanal de **30 horas**. La planificacion se calcula como trabajo principalmente ejecutado por una persona programadora, con apoyo puntual de DevOps, QA, DBA y responsables de integraciones cuando sea necesario.

Periodo propuesto: lunes 2026-06-08 al viernes 2026-11-13.  
Duracion total: 23 semanas.  
Capacidad semanal: 30 horas.  
Capacidad total disponible: 690 horas.  
Esfuerzo planificado: **673 horas**.  
Reserva tecnica aproximada: **17 horas**.

### Resumen de esfuerzo por epica

| Epica | Nombre | Horas planificadas | Semanas principales | Resultado esperado |
|---|---|---:|---|---|
| EP-03 | Seguridad y autenticacion | 124 | S1 a S7 | Secretos saneados, credenciales rotadas y reglas de acceso revisadas. |
| EP-02 | Actualizacion .NET y NuGet | 268 | S6 a S14 | Solucion migrada, paquetes actualizados, build estable y dependencias documentadas. |
| EP-07 | Pruebas unitarias y funcionales | 281 | S1 a S23 | Plan QA, casos, pruebas unitarias, regresion, defectos corregidos y UAT. |
| **Total** | **EP-03, EP-02 y EP-07** | **673** | **23 semanas** | **Version candidata validada tecnicamente.** |

### Semanas del cronograma

| Semana | Fechas | Capacidad |
|---|---|---:|
| S1 | 2026-06-08 al 2026-06-12 | 30 h |
| S2 | 2026-06-15 al 2026-06-19 | 30 h |
| S3 | 2026-06-22 al 2026-06-26 | 30 h |
| S4 | 2026-06-29 al 2026-07-03 | 30 h |
| S5 | 2026-07-06 al 2026-07-10 | 30 h |
| S6 | 2026-07-13 al 2026-07-17 | 30 h |
| S7 | 2026-07-20 al 2026-07-24 | 30 h |
| S8 | 2026-07-27 al 2026-07-31 | 30 h |
| S9 | 2026-08-03 al 2026-08-07 | 30 h |
| S10 | 2026-08-10 al 2026-08-14 | 30 h |
| S11 | 2026-08-17 al 2026-08-21 | 30 h |
| S12 | 2026-08-24 al 2026-08-28 | 30 h |
| S13 | 2026-08-31 al 2026-09-04 | 30 h |
| S14 | 2026-09-07 al 2026-09-11 | 30 h |
| S15 | 2026-09-14 al 2026-09-18 | 30 h |
| S16 | 2026-09-21 al 2026-09-25 | 30 h |
| S17 | 2026-09-28 al 2026-10-02 | 30 h |
| S18 | 2026-10-05 al 2026-10-09 | 30 h |
| S19 | 2026-10-12 al 2026-10-16 | 30 h |
| S20 | 2026-10-19 al 2026-10-23 | 30 h |
| S21 | 2026-10-26 al 2026-10-30 | 30 h |
| S22 | 2026-11-02 al 2026-11-06 | 30 h |
| S23 | 2026-11-09 al 2026-11-13 | 30 h |

### Cronograma tipo Gantt por tarea

Leyenda: `X` = semana con trabajo planificado.

| ID | Epica | Tarea | Horas | Inicio | Fin | Dependencia | S1 | S2 | S3 | S4 | S5 | S6 | S7 | S8 | S9 | S10 | S11 | S12 | S13 | S14 | S15 | S16 | S17 | S18 | S19 | S20 | S21 | S22 | S23 |
|---|---|---|---:|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| T01 | EP-03 | Inventariar secretos, endpoints y configuraciones sensibles | 12 | 2026-06-08 | 2026-06-12 | Codigo fuente | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T02 | EP-07 | Definir plan de pruebas criticas | 20 | 2026-06-08 | 2026-06-19 | Inventario | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T03 | EP-03 | Clasificar riesgos de seguridad | 10 | 2026-06-15 | 2026-06-19 | T01 |  | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T04 | EP-03 | Retirar secretos de appsettings.json | 20 | 2026-06-15 | 2026-06-26 | T01 |  | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T05 | EP-03 | Rotacion de credenciales expuestas | 36 | 2026-06-22 | 2026-07-10 | T01, externos |  |  | X | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T06 | EP-03 | Revisar roles, permisos, JWT, AD, SAUR | 26 | 2026-06-29 | 2026-07-17 | T04 |  |  |  | X | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T07 | EP-07 | Casos de prueba login, roles, JWT | 22 | 2026-07-06 | 2026-07-17 | T06 |  |  |  |  | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T08 | EP-03 | Pruebas tecnicas de seguridad | 16 | 2026-07-13 | 2026-07-24 | T06, T07 |  |  |  |  |  | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T09 | EP-02 | Definir ruta migracion .NET | 20 | 2026-07-13 | 2026-07-24 | Diagnostico |  |  |  |  |  | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T10 | EP-02 | Actualizar paquetes NuGet vulnerables | 36 | 2026-07-20 | 2026-08-07 | T09 |  |  |  |  |  |  | X | X | X |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| T11 | EP-02 | Migrar proyectos MVC, API, BL, DAL, EL | 60 | 2026-08-03 | 2026-08-21 | T09, T10 |  |  |  |  |  |  |  |  | X | X | X |  |  |  |  |  |  |  |  |  |  |  |  |
| T12 | EP-02 | Corregir errores de compilacion | 46 | 2026-08-17 | 2026-09-04 | T11 |  |  |  |  |  |  |  |  |  |  | X | X | X |  |  |  |  |  |  |  |  |  |  |
| T13 | EP-02 | Validar middleware, rutas, API, Swagger | 32 | 2026-08-24 | 2026-09-11 | T12 |  |  |  |  |  |  |  |  |  |  |  | X | X | X |  |  |  |  |  |  |  |  |  |
| T14 | EP-02 | Validar Oracle e integraciones | 24 | 2026-08-31 | 2026-09-11 | T12 |  |  |  |  |  |  |  |  |  |  |  |  | X | X |  |  |  |  |  |  |  |  |  |
| T15 | EP-02 | Documentacion tecnica framework | 12 | 2026-09-07 | 2026-09-18 | T13, T14 |  |  |  |  |  |  |  |  |  |  |  |  |  | X | X |  |  |  |  |  |  |  |  |
| T16 | EP-07 | Casos expediente, catalogos, aprobaciones | 32 | 2026-09-07 | 2026-09-25 | T02 |  |  |  |  |  |  |  |  |  |  |  |  |  | X | X | X |  |  |  |  |  |  |  |
| T17 | EP-07 | Pruebas unitarias servicios criticos | 60 | 2026-09-21 | 2026-10-16 | T10, T11 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | X | X | X | X |  |  |  |  |
| T18 | EP-07 | Preparar datos de prueba, escenarios y checklist de regresion | 23 | 2026-10-05 | 2026-10-16 | T16 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | X | X |  |  |  |  |
| T19 | EP-07 | Regresion funcional completa en QA | 69 | 2026-10-12 | 2026-10-30 | T13, T18 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | X | X | X |  |  |
| T20 | EP-07 | Corregir defectos detectados | 60 | 2026-10-26 | 2026-11-13 | T19 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | X | X | X |
| T21 | EP-07 | Reejecutar pruebas y evidencia UAT | 24 | 2026-11-02 | 2026-11-13 | T20 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | X | X |
| T22 | EP-07 | Validacion final con usuarios clave | 13 | 2026-11-09 | 2026-11-13 | T21 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | X |

### Carga semanal ajustada a 6 horas funcionales diarias

| Semana | Fechas | Horas asignadas | Capacidad semanal | Diferencia | Enfoque | Descripción para seguimiento gerencial |
|---|---|---:|---:|---:|---|---|
| S1 | 2026-06-08 al 2026-06-12 | 30,0 | 30 | 0,0 | Inventario de seguridad y plan QA | Se revisa el código para identificar contraseñas y datos sensibles que están expuestos, y se prepara el plan de pruebas inicial |
| S2 | 2026-06-15 al 2026-06-19 | 30,0 | 30 | 0,0 | Secretos y rotacion inicial | Se retiran las contraseñas visibles del código y se inicia la coordinación con otras áreas para cambiar credenciales comprometidas |
| S3 | 2026-06-22 al 2026-06-26 | 30,0 | 30 | 0,0 | Secretos y rotacion continuacion | Se continúa cambiando las contraseñas de base de datos, correo y conexiones externas en coordinación con administradores |
| S4 | 2026-06-29 al 2026-07-03 | 30,0 | 30 | 0,0 | Roles, permisos y casos de seguridad | Se revisa quién puede acceder a qué funciones del sistema y se diseñan pruebas para validar que los permisos funcionen correctamente |
| S5 | 2026-07-06 al 2026-07-10 | 30,0 | 30 | 0,0 | Pruebas de seguridad y casos QA | Se ejecutan pruebas para confirmar que los cambios de seguridad no afectan el acceso de usuarios autorizados y se documentan más casos de prueba |
| S6 | 2026-07-13 al 2026-07-17 | 30,0 | 30 | 0,0 | Seguridad tecnica y ruta .NET | Se finalizan ajustes de seguridad y se define el plan técnico para actualizar la plataforma de programación del sistema |
| S7 | 2026-07-20 al 2026-07-24 | 30,0 | 30 | 0,0 | Ruta .NET y paquetes NuGet | Se establece la estrategia de actualización tecnológica y se revisan las bibliotecas de software que tienen vulnerabilidades conocidas |
| S8 | 2026-07-27 al 2026-07-31 | 30,0 | 30 | 0,0 | Paquetes NuGet actualizacion | Se actualizan las bibliotecas de software a versiones más seguras y modernas, verificando que el sistema compile correctamente |
| S9 | 2026-08-03 al 2026-08-07 | 30,0 | 30 | 0,0 | Migracion de proyectos inicio | Se inicia el cambio de los 6 proyectos principales del sistema a la nueva versión de la plataforma de desarrollo |
| S10 | 2026-08-10 al 2026-08-14 | 30,0 | 30 | 0,0 | Migracion de proyectos continuacion | Se completa la migración de todos los proyectos y se realizan las primeras compilaciones para detectar incompatibilidades |
| S11 | 2026-08-17 al 2026-08-21 | 30,0 | 30 | 0,0 | Errores de compilacion | Se corrigen los errores técnicos que aparecen al compilar el sistema con la nueva versión de la plataforma |
| S12 | 2026-08-24 al 2026-08-28 | 30,0 | 30 | 0,0 | Compatibilidad y build | Se verifica que todas las partes del sistema funcionen juntas y se logra una compilación completa y estable |
| S13 | 2026-08-31 al 2026-09-04 | 30,0 | 30 | 0,0 | Middleware y validaciones | Se prueba que las rutas web, la API de servicios y las configuraciones de sesión funcionen correctamente después de la actualización |
| S14 | 2026-09-07 al 2026-09-11 | 30,0 | 30 | 0,0 | Oracle, integraciones y documentacion | Se valida la conexión a la base de datos Oracle y las integraciones con sistemas externos, y se actualiza la documentación técnica |
| S15 | 2026-09-14 al 2026-09-18 | 30,0 | 30 | 0,0 | Casos de prueba funcionales | Se preparan los escenarios de prueba detallados para validar expedientes, catálogos, documentos y procesos de aprobación |
| S16 | 2026-09-21 al 2026-09-25 | 30,0 | 30 | 0,0 | Pruebas unitarias inicio | Se crean pruebas automáticas para las funciones más críticas del sistema, que permiten detectar errores rápidamente |
| S17 | 2026-09-28 al 2026-10-02 | 30,0 | 30 | 0,0 | Pruebas unitarias continuacion | Se completa la suite de pruebas automáticas alcanzando una cobertura mínima aceptable de los servicios principales |
| S18 | 2026-10-05 al 2026-10-09 | 30,0 | 30 | 0,0 | Preparacion de datos, escenarios y checklist de regresion | Se prepara información de prueba realista y se documentan todos los escenarios que deben validarse para asegurar que nada se rompa |
| S19 | 2026-10-12 al 2026-10-16 | 30,0 | 30 | 0,0 | Regresion funcional | Se ejecuta el plan completo de pruebas funcionales en ambiente de calidad, validando expediente, catálogos y reportes |
| S20 | 2026-10-19 al 2026-10-23 | 30,0 | 30 | 0,0 | Regresion funcional continuacion | Se completan las pruebas funcionales restantes, incluyendo aprobaciones, plazas e integraciones con sistemas externos |
| S21 | 2026-10-26 al 2026-10-30 | 30,0 | 30 | 0,0 | Correccion de defectos | Se analizan y corrigen los errores funcionales detectados durante las pruebas, priorizando los de mayor impacto |
| S22 | 2026-11-02 al 2026-11-06 | 30,0 | 30 | 0,0 | Correccion de defectos y reejecucion | Se completan las correcciones de errores críticos y se vuelven a ejecutar las pruebas afectadas para confirmar las soluciones |
| S23 | 2026-11-09 al 2026-11-13 | 13,0 | 30 | 17,0 | Evidencia UAT, validacion final y reserva | Usuarios clave validan el sistema actualizado, se recopila evidencia de aceptación y se cierra formalmente el proyecto técnico |
| **Total** | **Periodo completo** | **673,0** | **690,0** | **17,0** | **EP-03, EP-02 y EP-07** | **Modernización completa del sistema con seguridad saneada, plataforma actualizada y funcionalidad validada** |

### Enfoque de datos de prueba y regresion

La actividad de datos de prueba y regresion consiste en preparar informacion controlada y escenarios funcionales para comprobar que los procesos existentes siguen funcionando despues de la actualizacion tecnica. No se trata solamente de probar pantallas, sino de validar que los cambios en .NET, paquetes NuGet, seguridad, configuracion, Oracle e integraciones no rompan flujos que ya eran parte de la operacion normal del sistema.

El trabajo incluye preparar usuarios y roles de prueba, expedientes con distintos estados, catalogos activos e inactivos, documentos adjuntos, casos de aprobacion, registros asociados a plazas, datos academicos, informacion laboral, capacitaciones, idiomas, proyectos, premios y produccion academica. Tambien se deben considerar datos vinculados con integraciones como Oracle, SAUR, AMI, Active Directory, SMTP y servicios externos que participen en los flujos principales.

La regresion funcional debe cubrir al menos los procesos criticos del expediente digital: ingreso al sistema, validacion de permisos, consulta de expediente, edicion de informacion personal, carga o consulta de documentos, uso de catalogos, aprobaciones, reportes basicos y validacion de servicios API cuando correspondan. Cada escenario debe tener resultado esperado, datos usados, evidencia de ejecucion y estado final: aprobado, fallido, bloqueado o pendiente.

El entregable esperado de esta actividad es un conjunto de datos y una matriz de pruebas que permita ejecutar la regresion de forma ordenada. Esta matriz debe indicar modulo, escenario, rol requerido, datos de entrada, pasos de prueba, resultado esperado, resultado obtenido, evidencia y observaciones. Con esto se facilita detectar defectos, repetir pruebas despues de correcciones y demostrar que la version candidata mantiene la funcionalidad principal.

### Hitos de control

| Fecha objetivo | Hito | Evidencia esperada |
|---|---|---|
| 2026-06-19 | Inventario de seguridad y plan QA inicial | Matriz de secretos, riesgos iniciales y plan de pruebas criticas |
| 2026-07-24 | Seguridad base revisada | Configuracion saneada, roles revisados y casos de seguridad definidos |
| 2026-08-07 | Paquetes NuGet y migracion inicial en ejecucion | Rama tecnica, paquetes actualizados y bitacora de compatibilidad |
| 2026-09-11 | Migracion tecnica estabilizada | Build estable, validacion MVC/API, Oracle e integraciones principales |
| 2026-10-09 | Pruebas unitarias y datos de regresion listos | Suite inicial, datos de prueba y checklist de regresion |
| 2026-11-06 | Defectos criticos corregidos | Registro de defectos, correcciones y reejecucion parcial |
| 2026-11-13 | Version candidata validada | Evidencia UAT y cierre tecnico de EP-03, EP-02 y EP-07 |

### Criterio profesional de estimacion

Las horas se estiman con base en una jornada real de programador de 6 horas funcionales diarias. El cronograma distribuye la carga en 30 horas semanales y por eso extiende la planificacion hasta 23 semanas. La mayor carga se concentra en EP-02 y EP-07, porque la migracion desde `.NET Core 2.2` puede generar errores de compatibilidad que solo aparecen al compilar, ejecutar MVC/API, validar Oracle y correr regresion funcional completa.
