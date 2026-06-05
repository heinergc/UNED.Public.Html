# Reporte funcional y tecnico de actualizacion

Proyecto: `UNED.PersonalAcademico` / Expediente Digital  
Fecha de analisis: 2026-06-02  
Alcance revisado: solucion principal, proyectos `.csproj`, configuraciones, controladores, capas BL/DAL/EL, pruebas unitarias y compilacion local.

## 1. Resumen ejecutivo

El sistema corresponde a una solucion ASP.NET Core empresarial para gestion de expediente digital de personal academico/administrativo. La solucion principal contiene 6 proyectos: MVC web, API REST, BL, DAL, EL y pruebas unitarias. Se identificaron 119 controladores entre frontend MVC y API, modulos de catalogos, expediente, aprobaciones, plazas, reportes, seguridad, integraciones SAUR/AMI/Active Directory, correo, Oracle y generacion documental.

El estado tecnico general es **estable para compilacion**, pero **legacy y de riesgo alto para operacion**: compila con `dotnet build --no-restore` usando SDK 9.0.314, con 0 errores y 114 advertencias. El riesgo mas importante es que todos los proyectos principales apuntan a `netcoreapp2.2`, version fuera de soporte. Microsoft indica que .NET Core 2.2 llego a fin de vida el 2019-12-23 y que las versiones EOL ya no reciben actualizaciones de seguridad. Fuentes: [Microsoft .NET support policy](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core), [descarga .NET Core 2.2](https://dotnet.microsoft.com/en-us/download/dotnet/2.2), [anuncio .NET Blog](https://devblogs.microsoft.com/dotnet/net-core-2-2-will-reach-end-of-life-on-december-23-2019/).

Prioridad general: **critica**. Recomendacion: ejecutar una modernizacion controlada hacia una version LTS vigente, idealmente .NET 8 LTS o la version institucional aprobada, con fase previa de seguridad, rotacion de secretos y pruebas de regresion.

## 2. Diagnostico funcional

| Modulo | Funcion observada | Actualizacion requerida |
|---|---|---|
| Catalogos | Mantenimiento de paises, grados, areas, periodos, tipos, estados, categorias, instituciones | Validar reglas, normalizar CRUD, pruebas funcionales |
| Expediente | Informacion personal, familiar, academica, laboral, idiomas, capacitaciones, documentos, premios, proyectos y produccion academica | Regresion completa por alto impacto usuario |
| Aprobaciones | Aprobacion de capacitaciones, documentos, experiencia, idiomas, proyectos, premios y produccion | Revisar flujos, roles y estados |
| Plazas | Control e historial de plazas, vencimientos y notificaciones | Validar procesos programados y Oracle |
| Seguridad | Login, roles, SAUR, Azure AD, JWT/API, sesion MVC | Reestructurar secretos, autenticacion y autorizacion |
| Reportes/documentos | Reportes, OpenXML, Rotativa/wkhtmltopdf, archivos adjuntos | Validar dependencias, formatos y almacenamiento |
| Integraciones | SAUR, AMI, Active Directory, SMTP, Google Maps, Tribunal Supremo/Gometa | Revisar credenciales, endpoints y resiliencia |

## 3. Diagnostico tecnico

| Aspecto | Hallazgo | Riesgo |
|---|---|---|
| Version .NET | `netcoreapp2.2` en MVC, API, BL, DAL, EL y UnitTests | Critico por fin de soporte |
| Compatibilidad actual | SDK 9 compila, pero advierte `NETSDK1138` | Compilacion no garantiza soporte en runtime |
| NuGet | Advertencias NU1902/NU1903/NU1904 en `Microsoft.AspNetCore.App`, `Microsoft.NETCore.App`, `DotNetZip`, `AutoMapper`, `Oracle.ManagedDataAccess.Core`, `log4net`, `Newtonsoft.Json`, `System.Data.SqlClient` | Vulnerabilidades moderadas, altas y criticas |
| Arquitectura | Separacion por capas MVC/API/BL/DAL/EL, pero BL referencia al proyecto MVC | Acoplamiento no deseado y migracion mas compleja |
| Datos | Oracle por procedimientos almacenados y `Oracle.ManagedDataAccess.Core` | Dependencia fuerte de BD y paquetes vulnerables |
| Seguridad | Secretos visibles en `appsettings.json`: client secret, API keys, passwords, JWT key, SMTP y cadena Oracle | Critico: rotacion inmediata |
| Autenticacion | Uso de sesion, `UseAuthentication`, JWT/API y SAUR/AD | Requiere revision integral de roles, claims y expiracion |
| Despliegue | Hay `pubxml`, ASP.NET Core Web y rutas IIS/logs Windows | Falta confirmacion de ambiente productivo exacto |
| Calidad | 114 advertencias, metodos async sin await, APIs obsoletas, conflictos de ensamblados | Deuda tecnica media/alta |
| Pruebas | UnitTests con pocos archivos frente al tamano funcional | Cobertura insuficiente para migracion |

## 4. Que se debe actualizar y por que

| Elemento | Estado actual | Problema | Riesgo de no actualizar | Beneficio | Prioridad | Complejidad |
|---|---|---|---|---|---|---|
| Framework .NET | .NET Core 2.2 | EOL desde 2019-12-23 | Exposicion a vulnerabilidades y obsolescencia | Soporte, seguridad, rendimiento | Alta | Alta |
| Paquetes NuGet | Versiones antiguas con NU190x | Vulnerabilidades conocidas | Incidentes de seguridad | Reduccion de superficie de ataque | Alta | Alta |
| Secretos | En archivos de configuracion | Exposicion directa | Compromiso de sistemas externos | Gestion segura y auditoria | Alta | Media |
| Autenticacion/autorizacion | Mixta: sesion, Azure AD, SAUR, JWT | Riesgo de inconsistencias | Accesos indebidos | Modelo de seguridad gobernable | Alta | Alta |
| DAL Oracle | Procedimientos y helper propio | Paquete vulnerable y acoplamiento | Fallos de compatibilidad | Acceso a datos actualizado | Alta | Media |
| Arquitectura | BL referencia MVC | Acoplamiento circular funcional | Migracion lenta y pruebas fragiles | Separacion limpia de responsabilidades | Media | Alta |
| Pruebas | Cobertura baja | Regresion no detectable | Fallos en produccion | Confianza para modernizar | Alta | Media |
| Despliegue | `pubxml`, rutas Windows/logs | Configuracion no gobernada | Fallos por ambiente | Publicacion repetible | Media | Media |
| Codigo async/obsoleto | 114 advertencias | Deuda tecnica | Rendimiento y mantenibilidad | Codigo mas claro y robusto | Media | Media |
| Documentacion | Evidencia parcial | Conocimiento tacito | Dependencia de personas | Transferencia y soporte | Media | Baja |

## 5. Estimacion de horas hombre sin IA

| Tarea | Perfil | Min | Max | Justificacion |
|---|---:|---:|---:|---|
| Analisis inicial y levantamiento | Arquitecto / Analista | 40 | 64 | Inventario tecnico-funcional y alcance |
| Revision de codigo y dependencias | Senior | 56 | 96 | 6 proyectos principales, 119 controladores |
| Documentacion funcional | Analista funcional | 48 | 80 | Modulos de expediente, catalogos, plazas y aprobaciones |
| Documentacion tecnica | Arquitecto / Senior | 40 | 72 | Arquitectura, integraciones, despliegue |
| Refactorizacion arquitectura | Senior / Arquitecto | 96 | 180 | Separacion BL/MVC, acoplamientos y patrones |
| Actualizacion .NET/NuGet | Senior | 120 | 220 | Migracion 2.2 a LTS vigente y compatibilidad |
| Seguridad y secretos | Senior / DevOps | 80 | 140 | Rotacion, vault, JWT, AD, SAUR, SMTP |
| Acceso a datos Oracle | Senior / DBA | 60 | 120 | Paquetes, procedimientos, pruebas con BD |
| Pruebas unitarias | Developer / QA | 80 | 150 | Cobertura minima de servicios/controladores criticos |
| Pruebas funcionales | QA / Analista | 80 | 140 | Flujos principales y regresion |
| Correccion de errores | Senior / Intermedio | 70 | 140 | Defectos posteriores a migracion |
| Validacion usuarios | Analista / QA | 32 | 64 | UAT y ajustes menores |
| Publicacion pruebas | DevOps | 24 | 48 | Ambiente QA/UAT |
| Publicacion produccion | DevOps / Senior | 24 | 48 | Paso a produccion y monitoreo inicial |
| Documentacion final | Analista / Senior | 24 | 40 | Cierre, manuales y bitacora |

Total estimado: **876 a 1.542 horas hombre**.

## 6. Primer reporte: tareas epicas

| Codigo | Epica | Descripcion funcional | Justificacion | Prioridad | Complejidad | Perfil | Min | Max | Riesgo si no se realiza | Resultado |
|---|---|---|---|---|---|---|---:|---:|---|---|
| EP-01 | Analisis funcional y tecnico | Inventario completo del sistema | Base de plan realista | P1 | Media | Arquitecto/Analista | 40 | 64 | Alcance incompleto | Diagnostico aprobado |
| EP-02 | Actualizacion .NET y NuGet | Migrar framework y paquetes | 2.2 EOL y vulnerabilidades | P1 | Alta | Senior | 120 | 220 | Riesgo de seguridad | Solucion moderna compilable |
| EP-03 | Seguridad y autenticacion | Secretos, roles, JWT, AD, SAUR | Secretos visibles | P1 | Alta | Senior/DevOps | 80 | 140 | Compromiso de credenciales | Seguridad saneada |
| EP-04 | Arquitectura y capas | Reducir acoplamientos | BL referencia MVC | P2 | Alta | Arquitecto | 96 | 180 | Migracion fragil | Capas limpias |
| EP-05 | Correccion tecnica | Async, obsoletos, conflictos | 114 advertencias | P2 | Media | Senior | 70 | 140 | Deuda acumulada | Build con menos advertencias |
| EP-06 | Acceso a datos | Oracle/procedimientos | Paquetes vulnerables | P2 | Media | Senior/DBA | 60 | 120 | Fallos datos | DAL validado |
| EP-07 | Pruebas | Unitarias y funcionales | Cobertura baja | P1 | Media | QA/Dev | 160 | 290 | Regresion no detectada | Suite minima y UAT |
| EP-08 | Documentacion | Funcional, tecnica, operativa | Conocimiento tacito | P3 | Baja | Analista/Senior | 64 | 112 | Soporte dificil | Documentos formales |
| EP-09 | Preparacion despliegue | QA, produccion, configuracion | Despliegue no evidenciado completo | P2 | Media | DevOps | 48 | 96 | Paso a produccion riesgoso | Pipeline/procedimiento |
| EP-10 | Validacion final | Usuarios y cierre | Confirmar operacion | P2 | Media | Analista/QA | 32 | 64 | Rechazo usuario | Acta de aceptacion |

## 7. Segundo reporte: epicas con subtareas

| Epica | Subtarea | Descripcion | Motivo | Perfil | Min | Max | Dependencia | Entregable | Criterio |
|---|---|---|---|---|---:|---:|---|---|---|
| EP-01 | ST-01 | Inventariar proyectos, modulos y dependencias | Alcance | Arquitecto | 16 | 24 | Codigo | Inventario | Validado por lider |
| EP-01 | ST-02 | Levantar flujos funcionales | Mapa funcional | Analista | 24 | 40 | Usuarios clave | Matriz funcional | Cobertura de modulos |
| EP-02 | ST-01 | Evaluar ruta .NET 2.2 a LTS | Modernizacion | Senior | 24 | 40 | EP-01 | Plan migracion | Ruta aprobada |
| EP-02 | ST-02 | Actualizar paquetes vulnerables | Seguridad | Senior | 40 | 80 | ST-01 | Paquetes actualizados | Build exitoso |
| EP-02 | ST-03 | Corregir errores de compatibilidad | Compilacion | Senior | 56 | 100 | ST-02 | Solucion compilable | 0 errores |
| EP-03 | ST-01 | Retirar secretos de appsettings | Exposicion | DevOps | 24 | 40 | Accesos | Config segura | Sin secretos en repo |
| EP-03 | ST-02 | Rotar credenciales expuestas | Contencion | DevOps/Senior | 24 | 48 | Dueños sistemas | Credenciales nuevas | Validado por sistemas |
| EP-03 | ST-03 | Revisar roles/JWT/AD/SAUR | Acceso | Senior | 32 | 52 | ST-01 | Matriz seguridad | Pruebas acceso |
| EP-04 | ST-01 | Eliminar dependencia BL -> MVC | Acoplamiento | Arquitecto | 40 | 80 | EP-02 | BL desacoplada | Sin referencia MVC |
| EP-04 | ST-02 | Normalizar servicios y DI | Mantenibilidad | Senior | 32 | 60 | ST-01 | DI revisada | Pruebas pasan |
| EP-04 | ST-03 | Revisar manejo de errores | Robustez | Senior | 24 | 40 | ST-02 | Middleware/documento | Errores controlados |
| EP-05 | ST-01 | Corregir async sin await | Calidad | Intermedio | 24 | 48 | Build | Warnings reducidos | Revision tecnica |
| EP-05 | ST-02 | Reemplazar APIs obsoletas | Mantenibilidad | Senior | 32 | 64 | EP-04 | Codigo actualizado | Sin obsoletos criticos |
| EP-05 | ST-03 | Resolver conflictos ensamblados | Compatibilidad | Senior | 14 | 28 | EP-02 | Dependencias alineadas | Sin MSB3277 |
| EP-06 | ST-01 | Validar Oracle y procedimientos | Datos | DBA/Senior | 24 | 48 | BD pruebas | Matriz SP | Ejecucion controlada |
| EP-06 | ST-02 | Actualizar proveedor Oracle | Seguridad | Senior | 20 | 40 | EP-02 | Driver actualizado | Conexion OK |
| EP-06 | ST-03 | Pruebas CRUD principales | Regresion | QA/DBA | 16 | 32 | ST-01 | Evidencia pruebas | Casos aprobados |
| EP-07 | ST-01 | Definir estrategia de pruebas | Cobertura | QA | 16 | 24 | EP-01 | Plan QA | Aprobado |
| EP-07 | ST-02 | Crear pruebas unitarias criticas | Regresion | Dev/QA | 60 | 120 | EP-04 | Suite pruebas | Cobertura minima |
| EP-07 | ST-03 | Ejecutar pruebas funcionales | UAT | QA/Analista | 80 | 146 | Ambiente QA | Evidencia | Casos cerrados |
| EP-08 | ST-01 | Documentar arquitectura | Transferencia | Arquitecto | 24 | 40 | EP-04 | Documento tecnico | Revisado |
| EP-08 | ST-02 | Documentar procesos funcionales | Soporte | Analista | 40 | 72 | Usuarios | Manual funcional | Aprobado |
| EP-09 | ST-01 | Preparar ambiente QA | Despliegue | DevOps | 24 | 48 | Config segura | Ambiente listo | Smoke test OK |
| EP-09 | ST-02 | Preparar paso a produccion | Operacion | DevOps/Senior | 24 | 48 | UAT | Plan despliegue | Rollback definido |
| EP-10 | ST-01 | Validacion usuarios | Aceptacion | Analista/QA | 24 | 48 | EP-07 | Acta UAT | Firmada |
| EP-10 | ST-02 | Cierre y lecciones aprendidas | Gobierno | Analista | 8 | 16 | ST-01 | Informe cierre | Aprobado |

Total subtareas detalladas: **26**.

## 8. Matriz de priorizacion

| Epica | Impacto funcional | Impacto tecnico | Riesgo operativo | Esfuerzo | Urgencia | Prioridad |
|---|---|---|---|---|---|---|
| EP-02 | Alto | Alto | Alto | Alto | Alta | P1 critica |
| EP-03 | Alto | Alto | Alto | Medio | Alta | P1 critica |
| EP-07 | Alto | Medio | Alto | Alto | Alta | P1 critica |
| EP-04 | Medio | Alto | Medio | Alto | Media | P2 importante |
| EP-05 | Medio | Medio | Medio | Medio | Media | P2 importante |
| EP-06 | Alto | Medio | Alto | Medio | Media | P2 importante |
| EP-09 | Medio | Medio | Alto | Medio | Media | P2 importante |
| EP-10 | Alto | Bajo | Medio | Bajo | Media | P2 importante |
| EP-01 | Medio | Medio | Medio | Bajo | Alta | P2 importante |
| EP-08 | Medio | Bajo | Bajo | Bajo | Baja | P3 recomendable |

### Costo estimado en horas de tareas criticas

La siguiente tabla separa las epicas clasificadas como **P1 critica**, indicando el costo estimado en horas hombre para su ejecucion manual, sin intervencion de inteligencia artificial.

| Epica | Nombre de la epica | Impacto funcional | Impacto tecnico | Riesgo operativo | Esfuerzo | Urgencia | Prioridad | Horas minimas | Horas maximas | Perfil principal |
|---|---|---|---|---|---|---|---|---:|---:|---|
| EP-02 | Actualizacion .NET y NuGet | Alto | Alto | Alto | Alto | Alta | P1 critica | 120 | 220 | Desarrollador senior .NET |
| EP-03 | Seguridad y autenticacion | Alto | Alto | Alto | Medio | Alta | P1 critica | 80 | 140 | Desarrollador senior / DevOps |
| EP-07 | Pruebas | Alto | Medio | Alto | Alto | Alta | P1 critica | 160 | 290 | QA / Desarrollador .NET |

| Indicador | Horas |
|---|---:|
| Total minimo tareas criticas | 360 |
| Total maximo tareas criticas | 650 |

### Detalle operativo para historias de usuario y sprints

La siguiente tabla descompone las epicas criticas en tareas base para iniciar la creacion de historias de usuario, criterios de aceptacion y planificacion de sprints. La propuesta asume sprints de 2 semanas y debe ajustarse segun disponibilidad real del equipo, acceso a ambientes, dependencias institucionales y prioridad definida por la jefatura del proyecto.

| Epica | Sprint sugerido | Tarea base para historia de usuario | Enfoque de historia de usuario | Perfil responsable | Horas minimas | Horas maximas | Dependencias | Entregable esperado | Criterio base de aceptacion |
|---|---|---|---|---|---:|---:|---|---|---|
| EP-03 | Sprint 1 | Inventariar secretos y configuraciones sensibles | Como equipo tecnico, necesito identificar secretos expuestos para definir acciones de mitigacion. | DevOps / Senior .NET | 8 | 16 | Acceso al codigo y configuraciones | Matriz de secretos y configuraciones sensibles | Existe inventario validado sin publicar valores secretos |
| EP-03 | Sprint 1 | Retirar secretos de `appsettings.json` | Como administrador tecnico, necesito mover secretos fuera del codigo fuente para reducir riesgo de exposicion. | DevOps | 16 | 24 | Matriz de secretos | Configuracion basada en variables de entorno o gestor seguro | El repositorio no contiene secretos activos |
| EP-03 | Sprint 1 | Rotar credenciales expuestas | Como responsable de seguridad, necesito reemplazar credenciales expuestas para invalidar accesos comprometidos. | DevOps / Sistemas externos | 24 | 48 | Coordinacion con responsables de Oracle, SMTP, SAUR, AMI, AD y APIs | Credenciales rotadas y documentadas en canal seguro | Las credenciales anteriores no son funcionales y las nuevas operan correctamente |
| EP-03 | Sprint 2 | Revisar autenticacion y autorizacion | Como usuario autorizado, necesito que el sistema valide correctamente roles y permisos para proteger funciones sensibles. | Senior .NET / Analista | 20 | 32 | Credenciales saneadas | Matriz de roles, permisos y flujos de acceso | Los roles criticos tienen reglas documentadas y probadas |
| EP-03 | Sprint 2 | Validar JWT, sesion y expiracion | Como responsable tecnico, necesito verificar tiempos de sesion y tokens para evitar accesos indebidos. | Senior .NET | 12 | 20 | Configuracion segura | Configuracion de seguridad validada | Token y sesion expiran segun politica definida |
| EP-02 | Sprint 2 | Definir ruta de migracion .NET | Como arquitecto, necesito definir la version objetivo y estrategia de migracion para reducir riesgo tecnico. | Arquitecto .NET | 16 | 24 | Inventario tecnico | Documento de ruta de migracion | Version objetivo y fases aprobadas |
| EP-02 | Sprint 2 | Actualizar paquetes NuGet vulnerables en rama controlada | Como desarrollador, necesito actualizar dependencias vulnerables para reducir riesgos de seguridad. | Senior .NET | 24 | 48 | Ruta de migracion | Rama tecnica con paquetes actualizados | La solucion compila o registra errores controlados de compatibilidad |
| EP-02 | Sprint 3 | Migrar proyectos principales a version .NET objetivo | Como equipo tecnico, necesito migrar MVC, API, BL, DAL, EL y pruebas a una version soportada. | Senior .NET / Arquitecto | 40 | 80 | Paquetes revisados | Proyectos migrados | Los proyectos restauran paquetes y compilan en ambiente controlado |
| EP-02 | Sprint 3 | Corregir errores de compilacion por migracion | Como desarrollador, necesito resolver incompatibilidades para obtener una solucion funcional. | Senior .NET | 32 | 60 | Migracion inicial | Solucion compilable | `dotnet build` finaliza con 0 errores |
| EP-02 | Sprint 4 | Validar compatibilidad de MVC, API y middleware | Como equipo tecnico, necesito confirmar que rutas, filtros, Swagger, sesiones y middleware funcionan tras la migracion. | Senior .NET / QA | 24 | 40 | Build migrado | Evidencia tecnica de compatibilidad | MVC y API responden en ambiente de pruebas |
| EP-02 | Sprint 4 | Actualizar documentacion tecnica de dependencias | Como mantenedor, necesito documentar cambios de framework y paquetes para soporte futuro. | Senior .NET | 8 | 16 | Migracion validada | Documento tecnico actualizado | Dependencias y cambios quedan trazables |
| EP-07 | Sprint 1 | Definir plan de pruebas criticas | Como QA, necesito identificar escenarios prioritarios para validar la modernizacion. | QA / Analista funcional | 16 | 24 | Inventario funcional | Plan de pruebas criticas | Plan aprobado por lider tecnico y funcional |
| EP-07 | Sprint 2 | Diseñar casos de prueba para seguridad y login | Como QA, necesito validar accesos, roles y sesiones para evitar regresiones de seguridad. | QA / Analista | 16 | 28 | Matriz de roles | Casos de prueba de seguridad | Casos cubren login, roles, token y expiracion |
| EP-07 | Sprint 3 | Diseñar casos de prueba para expediente y catalogos | Como usuario funcional, necesito confirmar que los procesos principales siguen operando correctamente. | QA / Analista | 24 | 40 | Mapa funcional | Casos de prueba funcionales | Casos cubren expediente, catalogos, documentos y aprobaciones |
| EP-07 | Sprint 3 | Crear pruebas unitarias iniciales para servicios criticos | Como desarrollador, necesito automatizar pruebas basicas para detectar regresiones tempranas. | Developer .NET / QA tecnico | 40 | 80 | Servicios estabilizados | Suite inicial de pruebas unitarias | Las pruebas se ejecutan en pipeline o comando local |
| EP-07 | Sprint 4 | Ejecutar regresion funcional completa | Como equipo de proyecto, necesito verificar que los flujos criticos funcionan despues de la migracion. | QA / Analista funcional | 48 | 90 | Ambiente QA migrado | Evidencia de pruebas ejecutadas | Defectos criticos registrados y priorizados |
| EP-07 | Sprint 5 | Corregir defectos detectados en pruebas | Como desarrollador, necesito corregir defectos para estabilizar la version candidata. | Senior .NET / Intermedio | 40 | 80 | Resultado de regresion | Defectos corregidos | Defectos criticos cerrados o con plan aceptado |
| EP-07 | Sprint 5 | Validacion final con usuarios clave | Como usuario clave, necesito validar los procesos modernizados antes de produccion. | Analista funcional / QA | 16 | 32 | Defectos criticos corregidos | Acta o evidencia UAT | Usuarios clave aprueban o documentan observaciones |

| Indicador de planificacion critica | Valor estimado |
|---|---:|
| Total tareas base para historias | 18 |
| Sprints sugeridos | 5 |
| Horas minimas plan operativo critico | 424 |
| Horas maximas plan operativo critico | 802 |

Nota: este detalle operativo puede superar el rango resumido de 360 a 650 horas porque incluye actividades preparatorias para historias de usuario, documentacion de aceptacion, validacion por sprint y estabilizacion. Para planificacion contractual se recomienda usar el rango ampliado cuando el equipo no cuente con pruebas automatizadas ni ambientes totalmente preparados.

## 9. Riesgos del proyecto

| Riesgo | Causa probable | Impacto | Probabilidad | Mitigacion |
|---|---|---|---|---|
| Framework sin soporte | `netcoreapp2.2` | Alto | Alta | Migrar a LTS |
| Secretos expuestos | Credenciales en JSON | Critico | Alta | Rotar y mover a vault/variables |
| Vulnerabilidades NuGet | Paquetes antiguos | Alto | Alta | Actualizar y auditar |
| Regresion funcional | Baja cobertura pruebas | Alto | Alta | Plan QA y pruebas automatizadas |
| Fallos en Oracle | Procedimientos y driver antiguo | Alto | Media | Ambiente BD de pruebas y DBA |
| Acoplamiento entre capas | Referencia BL -> MVC | Medio | Media | Refactor gradual |
| Despliegue no repetible | Configuracion manual/pubxml | Medio | Media | Pipeline o runbook |
| Integraciones externas inestables | SAUR/AMI/AD/SMTP/API externas | Alto | Media | Pruebas de contrato y monitoreo |

## 10. Supuestos de estimacion

La solucion compila actualmente con 0 errores y 114 advertencias. Existe acceso al codigo fuente completo. Se asume acceso a base de datos Oracle de pruebas, ambiente QA, credenciales institucionales para rotacion, usuarios clave para validacion y permiso para actualizar paquetes internos UNED. No se incluyen cambios funcionales nuevos ni rediseño visual completo. No se incluyen tiempos administrativos externos ni ventanas de aprobacion institucional.

Informacion faltante que afecta la estimacion: version real de IIS/runtime productivo, arquitectura de servidores, pipeline actual, cobertura real de pruebas manuales, inventario de procedimientos Oracle, criticidad de cada modulo, SLA productivo y politica institucional de version .NET objetivo.

## 11. Resultado final

| Indicador | Resultado |
|---|---|
| Total de epicas | 10 |
| Total de subtareas detalladas | 26 |
| Total minimo estimado | 876 horas |
| Total maximo estimado | 1.542 horas |
| Perfil mas requerido | Desarrollador senior .NET / Arquitecto .NET |
| Nivel general de riesgo | Alto / Critico por seguridad y EOL (End of Life)|
| Recomendacion final | Modernizacion controlada, iniciando por seguridad y framework |
| Orden recomendado | EP-01, EP-03, EP-02, EP-04, EP-06, EP-05, EP-07, EP-09, EP-10, EP-08 |

## Evidencia tecnica local usada

- `UNED.PersonalAcademico.sln`: 6 proyectos principales.
- `UNED.PersonalAcademico.csproj`, `UNED.PersonalAcademico.API.csproj`, `UNED.PersonalAcademico.BL.csproj`, `UNED.PersonalAcademico.DAL.csproj`, `UNED.PersonalAcademico.EL.csproj`, `UNED.PersonalAcademico.UnitTests.csproj`: `TargetFramework netcoreapp2.2`.
- `UNED.PersonalAcademico/appsettings.json` y `UNED.PersonalAcademico.API/appsettings.json`: configuraciones con secretos y endpoints.
- `dotnet build UNED.PersonalAcademico.sln --no-restore`: 0 errores, 114 advertencias.

## 12. Cronograma de trabajo propuesto para EP-03, EP-02 y EP-07

Este cronograma se basa en la experiencia tecnica levantada en el analisis: solucion ASP.NET Core legacy en `netcoreapp2.2`, dependencias vulnerables, secretos expuestos en configuracion, baja cobertura de pruebas y alta necesidad de regresion funcional. La propuesta organiza las epicas criticas en 5 sprints de 2 semanas, iniciando el lunes 2026-06-08 y finalizando el viernes 2026-08-14.

### Supuestos del cronograma

- Equipo minimo recomendado: 1 desarrollador senior .NET, 1 DevOps o responsable tecnico de ambientes, 1 QA funcional/tecnico y apoyo puntual de DBA o responsables de integraciones.
- Disponibilidad esperada: entre 30 y 40 horas semanales por perfil principal.
- Se requiere acceso a ambiente QA, base de datos Oracle de pruebas, responsables de SAUR/AMI/AD/SMTP y usuarios clave para validacion.
- Las actividades de EP-07 inician desde el primer sprint para acompanar la migracion, no solo al final.
- La rotacion de credenciales depende de aprobaciones institucionales y puede ejecutarse en paralelo con ajustes tecnicos.

### Cronograma por sprint

| Sprint | Fechas | Epica principal | Actividades | Entregables | Criterio de cierre |
|---|---|---|---|---|---|
| Sprint 1 | 2026-06-08 al 2026-06-19 | EP-03 / EP-07 | Inventario de secretos, configuraciones sensibles, endpoints e integraciones. Definicion del plan de pruebas criticas. Identificacion de flujos de login, roles, sesion, JWT, expediente, catalogos y aprobaciones. | Matriz de secretos sin exponer valores, matriz inicial de riesgos de seguridad, plan QA critico. | Secretos e integraciones identificados; plan de pruebas aprobado por lider tecnico/funcional. |
| Sprint 2 | 2026-06-22 al 2026-07-03 | EP-03 / EP-02 / EP-07 | Retiro de secretos de `appsettings.json`, definicion de variables de entorno o mecanismo seguro, inicio de rotacion de credenciales, revision de roles/autenticacion, definicion de ruta de migracion .NET y diseno de casos de prueba de seguridad. | Configuracion saneada en rama controlada, matriz de roles y permisos, ruta de migracion .NET aprobada, casos de prueba de seguridad. | Repositorio sin secretos activos; estrategia .NET objetivo documentada; casos de seguridad listos para ejecucion. |
| Sprint 3 | 2026-07-06 al 2026-07-17 | EP-02 / EP-07 | Actualizacion de paquetes NuGet vulnerables, migracion inicial de proyectos MVC, API, BL, DAL, EL y UnitTests a version soportada, correccion de errores iniciales de compatibilidad, diseno de casos para expediente y catalogos, inicio de pruebas unitarias criticas. | Rama migrada inicial, paquetes actualizados, bitacora de incompatibilidades, casos funcionales prioritarios, suite unitaria inicial. | La solucion restaura paquetes y avanza hacia compilacion controlada; casos funcionales principales estan definidos. |
| Sprint 4 | 2026-07-20 al 2026-07-31 | EP-02 / EP-07 | Correccion de errores de compilacion, validacion de middleware, sesiones, rutas MVC, API, Swagger, Oracle e integraciones principales. Ejecucion de regresion funcional en QA sobre flujos criticos. | Solucion compilable, evidencia tecnica de compatibilidad, reporte de regresion funcional, defectos priorizados. | `dotnet build` finaliza con 0 errores; MVC/API responden en QA; defectos criticos quedan registrados y priorizados. |
| Sprint 5 | 2026-08-03 al 2026-08-14 | EP-07 / EP-02 / EP-03 | Correccion de defectos detectados, reejecucion de pruebas criticas, validacion final de usuarios clave, cierre de documentacion tecnica de dependencias, seguridad y pruebas. | Defectos criticos corregidos o con plan aceptado, evidencia UAT, documento tecnico actualizado, acta o correo de aceptacion funcional. | Usuarios clave aprueban la version candidata; no quedan defectos criticos abiertos sin plan de mitigacion. |

### Vista por epica

| Epica | Duracion estimada | Inicio | Fin | Horas minimas | Horas maximas | Dependencias clave | Resultado esperado |
|---|---:|---|---|---:|---:|---|---|
| EP-03 Seguridad y autenticacion | 2 sprints principales, con seguimiento hasta cierre | 2026-06-08 | 2026-08-14 | 80 | 140 | Accesos a sistemas externos, responsables de credenciales, politica institucional de secretos | Secretos fuera del codigo, credenciales rotadas, roles y autenticacion documentados y probados. |
| EP-02 Actualizacion .NET y NuGet | 3 sprints principales | 2026-06-22 | 2026-08-14 | 120 | 220 | Ruta de migracion aprobada, paquetes compatibles, ambiente QA | Solucion migrada a version soportada, paquetes vulnerables actualizados y build sin errores. |
| EP-07 Pruebas | 5 sprints transversales | 2026-06-08 | 2026-08-14 | 160 | 290 | Mapa funcional, ambiente QA, datos de prueba, usuarios clave | Plan QA, casos de prueba, suite unitaria minima, regresion funcional y validacion UAT. |

### Hitos de control

| Fecha objetivo | Hito | Responsable sugerido | Evidencia |
|---|---|---|---|
| 2026-06-19 | Aprobacion del plan de seguridad y pruebas | Lider tecnico / QA | Matriz de secretos y plan QA |
| 2026-07-03 | Configuracion saneada y ruta .NET aprobada | DevOps / Arquitecto .NET | Rama controlada y documento de migracion |
| 2026-07-17 | Migracion tecnica inicial completada | Senior .NET | Bitacora tecnica y avance de compilacion |
| 2026-07-31 | Build estable y regresion funcional ejecutada | Senior .NET / QA | Build 0 errores y reporte de defectos |
| 2026-08-14 | Version candidata validada | QA / Usuarios clave | Evidencia UAT y cierre tecnico |

### Distribucion sugerida de esfuerzo

| Perfil | Participacion principal | Enfoque |
|---|---|---|
| Arquitecto / Senior .NET | Sprints 2, 3, 4 y 5 | Ruta de migracion, actualizacion de proyectos, compatibilidad y correccion tecnica. |
| DevOps / Seguridad | Sprints 1, 2 y seguimiento | Secretos, variables de entorno, rotacion de credenciales, ambientes y configuracion segura. |
| QA / Analista funcional | Sprints 1 a 5 | Plan de pruebas, casos funcionales, regresion, evidencia y UAT. |
| DBA / responsables de integraciones | Apoyo puntual en sprints 2, 3 y 4 | Oracle, SAUR, AMI, AD, SMTP, validacion de conectividad y credenciales. |

### Riesgos especificos del cronograma

| Riesgo | Impacto en cronograma | Mitigacion |
|---|---|---|
| Retraso en rotacion de credenciales | Puede extender EP-03 mas alla del Sprint 2 | Iniciar gestion institucional desde la primera semana y separar configuracion tecnica de aprobaciones externas. |
| Paquetes no compatibles con la version .NET objetivo | Puede aumentar esfuerzo de EP-02 | Migrar en rama controlada, priorizar paquetes criticos y registrar decisiones tecnicas. |
| Ambiente QA incompleto | Bloquea regresion de EP-07 | Preparar ambiente desde Sprint 1 y validar conectividad antes del Sprint 4. |
| Baja disponibilidad de usuarios clave | Retrasa UAT | Agendar validaciones desde Sprint 3 con escenarios concretos y tiempos definidos. |
| Defectos criticos posteriores a migracion | Puede requerir sprint adicional de estabilizacion | Mantener pruebas desde el inicio y priorizar flujos de mayor impacto operativo. |

### Recomendacion de ejecucion

El orden recomendado es iniciar con EP-03 y EP-07 en paralelo, porque la seguridad y las pruebas reducen el riesgo de la migracion. EP-02 debe comenzar formalmente en el Sprint 2, una vez que exista ruta tecnica aprobada y configuracion saneada. Para planificacion contractual, se recomienda reservar una holgura adicional de 1 sprint si el ambiente QA, la base de datos Oracle o la rotacion de credenciales dependen de terceros institucionales.
