# Plan Detallado de Modernización UNED.PersonalAcademico - Tareas Críticas

**TL;DR**: Plan granular para actualizar el sistema desde .NET Core 2.2 a .NET 8.0 LTS mediante migración incremental (2.2→3.1→5.0→6.0→8.0), sanear seguridad eliminando secretos expuestos y rotar credenciales, y establecer cobertura mínima de pruebas. Duración estimada: 16 semanas, 673 horas, 1 desarrollador senior + apoyo puntual.

---

## FASE 0: PREPARACIÓN Y SEGURIDAD INMEDIATA (Semanas 1-5, 124h)

### 🔒 EP-03.1: Inventario y Contención de Secretos (Semana 1, 42h)

#### T01.1: Auditoría de secretos en configuración (8h)
- Leer `UNED.PersonalAcademico/appsettings.json` línea por línea
- Leer `UNED.PersonalAcademico.API/appsettings.json` línea por línea
- Identificar todos los valores sensibles:
  - Azure AD: `ClientId`, `ClientSecret`, `TenantId`
  - SAUR: URLs, credenciales, tokens
  - Oracle: cadenas de conexión, usuarios, passwords
  - SMTP: servidor, puerto, usuario, password
  - JWT: `SecretKey`, `Issuer`, `Audience`
  - APIs externas: Google Maps API key, Tribunal Supremo endpoints
  - AMI: endpoints, credenciales
- Crear matriz en Excel con columnas: `Secreto | Archivo | Línea | Sistema afectado | Responsable técnico`
- Documentar cada secreto sin copiar valores reales

#### T01.2: Clasificación de riesgo por secreto (10h)
- Evaluar cada secreto:
  - **Riesgo Crítico**: Oracle production, Azure AD ClientSecret, JWT SecretKey
  - **Riesgo Alto**: SMTP credentials, SAUR tokens
  - **Riesgo Medio**: API keys externas
- Identificar sistemas que dependen de cada credencial
- Contactar responsables de cada sistema para coordinar rotación:
  - DBA para Oracle
  - Team Azure AD para ClientSecret
  - Admin SMTP para correo
  - Admin SAUR para integraciones
- Documentar ventanas de cambio disponibles

#### T01.3: Crear variables de entorno por ambiente (12h)
- Definir nomenclatura estándar para variables:
  ```
  PA_AZUREAD_CLIENTID
  PA_AZUREAD_CLIENTSECRET
  PA_AZUREAD_TENANTID
  PA_ORACLE_CONNECTIONSTRING
  PA_JWT_SECRETKEY
  PA_JWT_ISSUER
  PA_SMTP_HOST
  PA_SMTP_PORT
  PA_SMTP_USER
  PA_SMTP_PASSWORD
  PA_SAUR_BASEURL
  PA_SAUR_TOKEN
  PA_GOOGLEMAPS_APIKEY
  ```
- Crear plantilla de variables para 3 ambientes: DEV, QA, PROD
- Documentar en archivo `VariablesAmbiente.md` (sin valores reales)
- Crear script PowerShell para configurar variables locales en desarrollo

#### T01.4: Modificar código para leer variables de entorno (12h)
- Modificar [UNED.PersonalAcademico/Startup.cs](UNED.PersonalAcademico/Startup.cs):
  - En método `ConfigureServices`, cambiar lecturas de `Configuration["AzureAd:ClientSecret"]` a `Environment.GetEnvironmentVariable("PA_AZUREAD_CLIENTSECRET") ?? Configuration["AzureAd:ClientSecret"]`
  - Aplicar patrón a todos los secretos identificados
- Modificar [UNED.PersonalAcademico.API/Startup.cs](UNED.PersonalAcademico.API/Startup.cs) de igual forma
- Validar que la aplicación inicie correctamente con variables de entorno en desarrollo
- Commit en rama `feature/remove-secrets`

### 🔒 EP-03.2: Rotación de Credenciales (Semanas 2-3, 36h)

#### T02.1: Rotar credenciales Oracle (12h, requiere DBA)
- Coordinar con DBA ventana de cambio
- Solicitar creación de nuevo usuario/password para conexión desde aplicación
- DBA ejecuta:
  ```sql
  CREATE USER UNED_PA_NEW IDENTIFIED BY <password_seguro>;
  GRANT CONNECT, RESOURCE TO UNED_PA_NEW;
  -- Copiar permisos del usuario anterior
  ```
- Actualizar variable `PA_ORACLE_CONNECTIONSTRING` en ambiente QA
- Probar conexión desde aplicación
- Validar que todos los procedimientos almacenados ejecuten correctamente
- Documentar rollback: mantener usuario antiguo activo 7 días

#### T02.2: Rotar Azure AD ClientSecret (8h)
- En Azure Portal, navegar a App Registrations → UNED Personal Academico
- Ir a "Certificates & secrets" → "New client secret"
- Copiar nuevo secret (solo se muestra una vez)
- Actualizar variable `PA_AZUREAD_CLIENTSECRET` en QA
- Probar login con Azure AD
- Validar claims y roles se obtienen correctamente
- Después de validación, eliminar secret antiguo en Azure

#### T02.3: Rotar JWT SecretKey (4h)
- Generar nuevo secret de 256 bits:
  ```powershell
  $bytes = New-Object byte[] 32
  [Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($bytes)
  [Convert]::ToBase64String($bytes)
  ```
- Actualizar variable `PA_JWT_SECRETKEY`
- Probar generación y validación de tokens en API
- Validar que endpoints protegidos con `[Authorize]` funcionan
- Reiniciar aplicación para invalidar tokens anteriores

#### T02.4: Rotar credenciales SMTP y SAUR (12h)
- SMTP:
  - Solicitar a admin de correo nuevo usuario/password para envío
  - Actualizar variables `PA_SMTP_USER`, `PA_SMTP_PASSWORD`
  - Probar envío de correo de prueba desde aplicación
- SAUR:
  - Coordinar con equipo SAUR nuevo token de integración
  - Actualizar variable `PA_SAUR_TOKEN`
  - Probar endpoints de integración SAUR
  - Validar respuestas exitosas

### 🔒 EP-03.3: Revisión de Autenticación y Autorización (Semanas 3-4, 26h)

#### T03.1: Auditar roles y permisos (10h)
- Revisar [UNED.PersonalAcademico/Startup.cs](UNED.PersonalAcademico/Startup.cs) método `ConfigureServices`:
  - Identificar configuración de `AddAuthentication`
  - Identificar esquemas: Cookies, JwtBearer, AzureAD
  - Documentar tiempos de expiración configurados
- Revisar todos los controladores con atributo `[Authorize]`:
  - Buscar con `grep_search`: `\[Authorize.*\]`
  - Documentar roles requeridos por endpoint
  - Crear matriz: `Controlador | Acción | Rol requerido | Módulo funcional`
- Identificar puntos donde se asignan claims o roles

#### T03.2: Validar configuración JWT (8h)
- En [UNED.PersonalAcademico.API/Startup.cs](UNED.PersonalAcademico.API/Startup.cs):
  - Verificar configuración de `JwtBearerOptions`:
    ```csharp
    ValidateIssuer = true,
    ValidateAudience = true,
    ValidateLifetime = true,
    ValidateIssuerSigningKey = true,
    ClockSkew = TimeSpan.Zero // Eliminar tolerancia de 5 min por defecto
    ```
  - Documentar `TokenValidationParameters` actuales
  - Ajustar expiración de tokens a máximo 1 hora
  - Agregar `ClockSkew = TimeSpan.Zero` para validación estricta
- Probar generación de token desde API
- Probar token expirado (debe fallar con 401)

#### T03.3: Revisar manejo de sesión MVC (8h)
- En [UNED.PersonalAcademico/Startup.cs](UNED.PersonalAcademico/Startup.cs):
  - Identificar configuración de `AddSession`
  - Documentar `IdleTimeout` actual
  - Ajustar a máximo 30 minutos si está más alto
  - Verificar configuración de cookies:
    ```csharp
    options.Cookie.HttpOnly = true;
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    options.Cookie.SameSite = SameSiteMode.Strict;
    ```
- Revisar uso de sesión en controladores
- Validar que información sensible no se almacena en sesión

### 🔒 EP-03.4: Pruebas de Seguridad (Semana 5, 20h)

#### T04.1: Crear casos de prueba de login (6h)
- Login Azure AD:
  - Usuario válido → acceso correcto
  - Usuario inválido → mensaje error claro
  - Usuario sin roles → acceso denegado a módulos protegidos
- Login local (si aplica):
  - Validar contraseña encriptada
  - Validar lockout después de 5 intentos fallidos

#### T04.2: Crear casos de prueba de autorización (8h)
- Por cada rol identificado en T03.1:
  - Usuario con rol correcto accede a funcionalidad
  - Usuario sin rol recibe 403 Forbidden
- Probar endpoints API con token:
  - Token válido → 200 OK
  - Token expirado → 401 Unauthorized
  - Token sin firma válida → 401 Unauthorized
  - Sin token → 401 Unauthorized

#### T04.3: Ejecutar pruebas manuales de seguridad (6h)
- Ejecutar cada caso de prueba en ambiente QA
- Documentar resultados en matriz: `Caso | Resultado | Defecto ID si falla`
- Registrar defectos encontrados en sistema de tracking
- Validar que no quedan secretos en archivos de configuración commiteados

---

## FASE 1: MIGRACIÓN .NET CORE 2.2 → 3.1 (Semanas 4-6, 95h)

### 📋 EP-02.1: Preparación de Migración 2.2→3.1 (Semana 4, 20h)

#### T05.1: Investigar breaking changes 2.2→3.1 (8h)
- Revisar documentación oficial Microsoft:
  - https://docs.microsoft.com/en-us/aspnet/core/migration/22-to-30
  - https://docs.microsoft.com/en-us/aspnet/core/migration/30-to-31
- Identificar cambios que afectan el proyecto:
  - `Microsoft.AspNetCore.App` ya no es metapackage, se usa framework reference
  - `IHostingEnvironment` obsoleto, reemplazar por `IWebHostEnvironment`
  - `UseMvc()` obsoleto, migrar a endpoint routing
  - JSON serialization cambios (Newtonsoft.Json vs System.Text.Json)
  - Authentication middleware debe ir después de Routing
- Crear checklist de cambios a aplicar

#### T05.2: Crear global.json para controlar SDK (2h)
- En la raíz del workspace crear [global.json](global.json):
  ```json
  {
    "sdk": {
      "version": "3.1.426",
      "rollForward": "latestPatch"
    }
  }
  ```
- Verificar SDK 3.1 instalado: `dotnet --list-sdks`
- Commit en rama `feature/migrate-to-netcore31`

#### T05.3: Crear rama y estrategia de rollback (4h)
- Crear rama desde main: `git checkout -b feature/migrate-to-netcore31`
- Documentar plan de rollback en [ROLLBACK.md](ROLLBACK.md)
- Configurar ambiente QA paralelo para pruebas sin afectar versión actual
- Notificar a equipo sobre inicio de migración

#### T05.4: Resolver dependencia circular BL→MVC (6h) *DEPENDE DE T05.3*
- Analizar por qué BL referencia MVC:
  - Buscar en [UNED.PersonalAcademico.BL](UNED.PersonalAcademico.BL/) uso de tipos del proyecto MVC
  - Típicamente son ViewModels o Helpers que deben estar en EL
- Mover clases necesarias de MVC a EL:
  - Identificar clases compartidas
  - Mover a [UNED.PersonalAcademico.EL](UNED.PersonalAcademico.EL/)
  - Ajustar namespaces
- Eliminar `<ProjectReference>` de BL a MVC en [UNED.PersonalAcademico.BL.csproj](UNED.PersonalAcademico.BL/UNED.PersonalAcademico.BL.csproj)
- Compilar: `dotnet build UNED.PersonalAcademico.sln`
- Validar 0 errores de compilación

### 📋 EP-02.2: Migrar Proyectos Compartidos Primero (Semana 5, 20h)

#### T06.1: Migrar UNED.Ami.AspNetCore.Mvc a 3.1 (8h)
- Abrir [Recursos Compartidos/RecursosCompartidos/DLLs/UNED.Ami.AspNetCore.Mvc/UNED.Ami.AspNetCore.Mvc.csproj](Recursos Compartidos/RecursosCompartidos/DLLs/UNED.Ami.AspNetCore.Mvc/UNED.Ami.AspNetCore.Mvc.csproj)
- Cambiar `<TargetFramework>netcoreapp2.2</TargetFramework>` a `<TargetFramework>netcoreapp3.1</TargetFramework>`
- Eliminar `<PackageReference Include="Microsoft.AspNetCore.App" />`
- Agregar framework reference:
  ```xml
  <ItemGroup>
    <FrameworkReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>
  ```
- Actualizar paquetes NuGet a versiones compatibles con 3.1
- Compilar: `dotnet build`
- Resolver errores de compilación uno por uno
- Incrementar versión del paquete a 2.0.0

#### T06.2: Migrar UNED.Ami.AspNetCore.Mvc.DataAnnotations a 3.1 (6h)
- Mismo proceso que T06.1 para [UNED.Ami.AspNetCore.Mvc.DataAnnotations.csproj](Recursos Compartidos/RecursosCompartidos/DLLs/UNED.Ami.AspNetCore.Mvc.DataAnnotations/UNED.Ami.AspNetCore.Mvc.DataAnnotations.csproj)
- Versión a 2.0.0
- Compilar y validar

#### T06.3: Migrar UNED.Ami.Mail a 3.1 (6h)
- Mismo proceso para [UNED.Ami.Mail.csproj](Recursos Compartidos/RecursosCompartidos/DLLs/UNED.Ami.Mail/UNED.Ami.Mail.csproj)
- Versión a 2.0.0
- Publicar paquetes internos en feed NuGet corporativo o carpeta local

### 📋 EP-02.3: Migrar Proyecto EL (Semana 5, 12h)

#### T07.1: Actualizar UNED.PersonalAcademico.EL.csproj (4h)
- Cambiar TargetFramework a `netcoreapp3.1`
- Eliminar metapackage, agregar framework reference
- Actualizar paquetes:
  - `log4net` 2.0.8 → 2.0.15
  - `Newtonsoft.Json` 12.0.2 → 13.0.1
  - `Oracle.ManagedDataAccess.Core` 2.19.31 → 3.21.1
  - `System.Data.SqlClient` 4.8.0 → 4.8.6
  - `NLog` 4.6.7 → 4.7.15
  - `Microsoft.Extensions.*` 2.2.0 → 3.1.32
- Compilar: `dotnet build UNED.PersonalAcademico.EL\UNED.PersonalAcademico.EL.csproj`

#### T07.2: Resolver errores de compilación en EL (8h)
- Error típico: tipos obsoletos
  - Reemplazar `IHostingEnvironment` por `IWebHostEnvironment`
- Revisar warnings uno por uno
- Ejecutar: `dotnet build` hasta 0 errores
- Warnings aceptables documentar en [WARNINGS.md](WARNINGS.md)

### 📋 EP-02.4: Migrar Proyecto DAL (Semana 5, 12h)

#### T08.1: Actualizar UNED.PersonalAcademico.DAL.csproj (4h)
- TargetFramework a `netcoreapp3.1`
- Actualizar paquetes:
  - `Microsoft.Extensions.DependencyInjection` 2.2.0 → 3.1.32
  - `Oracle.ManagedDataAccess.Core` 2.19.31 → 3.21.1
  - `System.Configuration.ConfigurationManager` 4.6.0 → 6.0.0
- Compilar proyecto DAL

#### T08.2: Validar conexión Oracle con nuevo driver (8h)
- Crear proyecto consola de prueba temporal
- Probar conexión a Oracle QA con `Oracle.ManagedDataAccess.Core` 3.21.1
- Ejecutar 5 procedimientos almacenados críticos:
  1. Obtener datos de funcionario
  2. Insertar capacitación
  3. Actualizar expediente
  4. Consultar plazas
  5. Generar reporte
- Validar que tipos de datos Oracle mapean correctamente
- Documentar cualquier incompatibilidad

### 📋 EP-02.5: Migrar Proyecto BL (Semana 6, 12h)

#### T09.1: Actualizar UNED.PersonalAcademico.BL.csproj (4h)
- TargetFramework a `netcoreapp3.1`
- Actualizar referencia a paquetes UNED.Ami.* versión 2.0.0
- Actualizar `AutoMapper` 9.0.0 → 10.1.1
- Compilar

#### T09.2: Ajustar configuración AutoMapper para v10 (8h)
- AutoMapper 10 tiene breaking changes en configuración
- Revisar [MappingProfile.cs](UNED.PersonalAcademico.BL/MappingProfile.cs)
- Cambios típicos:
  - `Mapper.Initialize` obsoleto → usar `services.AddAutoMapper()`
  - Validar perfiles siguen funcionando
- Crear pruebas unitarias de mapeo
- Validar que todos los mapeos compilan

### 📋 EP-02.6: Migrar Proyectos MVC y API (Semana 6, 19h)

#### T10.1: Actualizar UNED.PersonalAcademico.csproj (MVC) (8h)
- TargetFramework a `netcoreapp3.1`
- Eliminar paquetes obsoletos:
  - Eliminar `Microsoft.AspNetCore.Razor.Design`
  - Eliminar `Microsoft.AspNetCore.Mvc.Core` explícito
- Actualizar paquetes:
  - `AutoMapper.Extensions.Microsoft.DependencyInjection` 7.0.0 → 8.1.1
  - `DocumentFormat.OpenXml` 2.9.1 → 2.20.0
  - `Rotativa.AspNetCore` 1.1.1 → 1.2.0
  - Actualizar referencias UNED.Ami.* a 2.0.0
- Agregar framework reference:
  ```xml
  <FrameworkReference Include="Microsoft.AspNetCore.App" />
  ```

#### T10.2: Migrar Startup.cs de MVC a endpoint routing (11h)
- Abrir [UNED.PersonalAcademico/Startup.cs](UNED.PersonalAcademico/Startup.cs)
- En método `Configure`:
  - Eliminar `app.UseMvc(routes => { ... });`
  - Agregar antes de endpoints:
    ```csharp
    app.UseRouting();
    app.UseAuthentication(); // Debe ir después de UseRouting
    app.UseAuthorization();
    ```
  - Agregar al final:
    ```csharp
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllerRoute(
            name: "default",
            pattern: "{controller=Home}/{action=Index}/{id?}");
        endpoints.MapControllers();
    });
    ```
- Mover `UseAuthentication()` para que esté DESPUÉS de `UseRouting()`
- Compilar
- Resolver errores de orden de middleware

#### T10.3: Actualizar UNED.PersonalAcademico.API.csproj (8h)
- Mismo proceso que T10.1 para API
- Actualizar además:
  - `Quartz` 3.0.7 → 3.3.3
  - `Swashbuckle.AspNetCore` 4.0.1 → 5.6.3
  - `System.DirectoryServices` 4.7.0 → 7.0.0

#### T10.4: Migrar Startup.cs de API (8h)
- Mismo proceso que T10.2
- Validar configuración Swagger sigue funcionando con v5
- Actualizar documentación de API si es necesario

---

## FASE 2: MIGRACIÓN .NET CORE 3.1 → .NET 5 (Semanas 7-8, 48h)

### 📋 EP-02.7: Preparación Migración 3.1→5.0 (Semana 7, 12h)

#### T11.1: Investigar breaking changes 3.1→5.0 (8h)
- Revisar https://docs.microsoft.com/en-us/dotnet/core/compatibility/3.1-5.0
- Cambios principales:
  - Cambio de TFM: `netcoreapp3.1` → `net5.0`
  - ASP.NET Core ya no es separado, es todo .NET 5
  - Mejoras en System.Text.Json (evaluar migración desde Newtonsoft)
  - Cambios en Kestrel y hosting
- Crear checklist

#### T11.2: Actualizar global.json (2h)
- Cambiar SDK a `5.0.408`:
  ```json
  {
    "sdk": {
      "version": "5.0.408"
    }
  }
  ```
- Verificar SDK 5.0 instalado

#### T11.3: Crear rama migrate-to-net5 (2h)
- Desde rama `feature/migrate-to-netcore31` crear `feature/migrate-to-net5`
- Documentar plan de rollback

### 📋 EP-02.8: Migrar Todos los Proyectos a .NET 5 (Semana 7-8, 36h)

#### T12.1: Migrar proyectos compartidos UNED.Ami.* (8h)
- Para cada proyecto (Mvc, DataAnnotations, Mail):
  - Cambiar TargetFramework a `net5.0`
  - Actualizar paquetes a versiones .NET 5
  - Compilar
  - Incrementar versión a 3.0.0
  - Publicar

#### T12.2: Migrar EL, DAL, BL (12h)
- Para cada proyecto:
  - TargetFramework a `net5.0`
  - Actualizar paquetes NuGet:
    - `Oracle.ManagedDataAccess.Core` 3.21.1 → 3.21.50
    - `Microsoft.Extensions.*` 3.1.32 → 5.0.0
  - Compilar
  - Resolver errores

#### T12.3: Migrar MVC y API (12h)
- Para ambos proyectos:
  - TargetFramework a `net5.0`
  - Actualizar paquetes:
    - `Swashbuckle.AspNetCore` 5.6.3 → 5.6.3 (compatible)
    - `AutoMapper` 10.1.1 → 11.0.1
  - Compilar y resolver errores

#### T12.4: Validar aplicación funciona en .NET 5 (4h)
- Ejecutar aplicación MVC localmente
- Probar login, navegación, CRUD básico
- Ejecutar API y probar endpoints con Swagger
- Documentar cualquier comportamiento diferente

---

## FASE 3: MIGRACIÓN .NET 5 → .NET 6 (Semanas 8-9, 52h)

### 📋 EP-02.9: Preparación Migración 5.0→6.0 (Semana 8, 12h)

#### T13.1: Investigar breaking changes 5.0→6.0 (8h)
- Revisar https://docs.microsoft.com/en-us/dotnet/core/compatibility/6.0
- Cambios principales:
  - Nuevo modelo de hosting minimal (opcional, seguir usando Startup.cs)
  - Implicit usings (opcional)
  - Nullable reference types recomendado
  - C# 10 por defecto
  - Cambios en Razor Pages y Blazor (si se usan)
  - ASP.NET Core authentication cambios
- Checklist de cambios

#### T13.2: Decidir adopción de minimal hosting (4h)
- Evaluar si conviene migrar a Program.cs minimal
- **Recomendación**: Mantener Startup.cs por ahora (menos cambios)
- Documentar decisión en [DECISIONES_ARQUITECTURA.md](DECISIONES_ARQUITECTURA.md)

### 📋 EP-02.10: Migrar a .NET 6 (Semana 9, 40h)

#### T14.1: Actualizar global.json y preparar rama (4h)
- SDK a `6.0.426`
- Crear rama `feature/migrate-to-net6`

#### T14.2: Migrar proyectos compartidos (8h)
- TargetFramework a `net6.0`
- Actualizar paquetes a versiones .NET 6
- Versión a 4.0.0

#### T14.3: Migrar EL, DAL, BL (12h)
- TargetFramework a `net6.0`
- Actualizar paquetes:
  - `Oracle.ManagedDataAccess.Core` 3.21.50 → 3.21.100
  - `Microsoft.Extensions.*` 5.0.0 → 6.0.0
  - `log4net` 2.0.15 → 2.0.17
  - `Newtonsoft.Json` 13.0.1 → 13.0.3
  - `NLog` 4.7.15 → 5.0.4
- Compilar cada uno

#### T14.4: Migrar MVC y API (12h)
- TargetFramework a `net6.0`
- Actualizar paquetes:
  - `Swashbuckle.AspNetCore` 5.6.3 → 6.5.0
  - `AutoMapper` 11.0.1 → 12.0.1
  - `Quartz` 3.3.3 → 3.6.2
- Ajustar código si Swashbuckle 6 tiene breaking changes

#### T14.5: Validar y probar aplicación completa (4h)
- Smoke testing: iniciar MVC y API
- Probar flujos críticos
- Validar logs, errores

---

## FASE 4: MIGRACIÓN .NET 6 → .NET 8 (Semanas 9-10, 53h)

### 📋 EP-02.11: Preparación Migración 6.0→8.0 (Semana 9, 12h)

#### T15.1: Investigar breaking changes 6.0→8.0 (8h)
- Revisar https://docs.microsoft.com/en-us/dotnet/core/compatibility/8.0
- .NET 8 es LTS (soporte hasta noviembre 2026)
- Cambios principales:
  - Performance improvements
  - Minimal APIs enhancements
  - Native AOT support (probablemente no aplicable)
  - Rate limiting built-in
  - Authentication/Authorization updates
- Checklist de validación

#### T15.2: Plan de contingencia y rollback (4h)
- Dado que .NET 8 es la meta final, documentar plan de rollback completo
- Identificar dependencias que podrían no ser compatibles aún
- Validar que Oracle driver soporta .NET 8

### 📋 EP-02.12: Migrar a .NET 8 Final (Semana 10, 41h)

#### T16.1: Actualizar global.json (2h)
- SDK a `8.0.404` o más reciente
- Crear rama `feature/migrate-to-net8`

#### T16.2: Migrar proyectos compartidos (8h)
- TargetFramework a `net8.0`
- Actualizar todos los paquetes a versiones .NET 8
- Versión a 5.0.0
- Publicar paquetes

#### T16.3: Migrar EL, DAL, BL (12h)
- TargetFramework a `net8.0`
- Actualizar paquetes:
  - `Oracle.ManagedDataAccess.Core` → última versión compatible con .NET 8
  - `Microsoft.Extensions.*` 6.0.0 → 8.0.0
  - `log4net` → última versión
  - `Newtonsoft.Json` 13.0.3 → considerar migración a System.Text.Json
  - `NLog` 5.0.4 → 5.2.x
- Compilar cada proyecto
- Resolver warnings relacionados con nullable reference types

#### T16.4: Migrar MVC y API (15h)
- TargetFramework a `net8.0`
- Actualizar paquetes:
  - `Swashbuckle.AspNetCore` 6.5.0 → 6.6.2
  - `AutoMapper` 12.0.1 → 13.0.1
  - `Quartz` 3.6.2 → 3.8.0
  - `Rotativa.AspNetCore` → verificar compatibilidad .NET 8
- Revisar si hay paquetes obsoletos que deben reemplazarse

#### T16.5: Validación exhaustiva .NET 8 (4h)
- Ejecutar todas las funcionalidades manualmente
- Verificar middleware, filters, autenticación
- Validar Oracle, SAUR, SMTP, Azure AD
- Probar generación de reportes y PDFs
- Validar performance (no debería degradarse)

---

## FASE 5: PRUEBAS COMPLETAS (Semanas 10-16, 281h)

### 🧪 EP-07.1: Definición de Estrategia de Pruebas (Semana 10, 20h)

#### T17.1: Crear matriz de riesgo funcional (8h)
- Identificar módulos críticos del sistema:
  - Login y seguridad (CRÍTICO)
  - Expediente personal (CRÍTICO)
  - Aprobaciones (ALTO)
  - Catalogos (MEDIO)
  - Reportes (ALTO)
  - Integraciones (ALTO)
- Crear matriz: `Módulo | Funcionalidad | Prioridad | Usuarios afectados | Casos de prueba estimados`
- Priorizar 80/20: 20% funcionalidad que cubre 80% uso

#### T17.2: Definir cobertura mínima de pruebas unitarias (6h)
- Servicios críticos a cubrir:
  - AuthSaurBL
  - MailBL
  - Servicios de Expediente
  - Servicios de Aprobaciones
- Meta: mínimo 60% cobertura en BL
- Documentar en [PLAN_PRUEBAS.md](PLAN_PRUEBAS.md)

#### T17.3: Definir checklist de regresión funcional (6h)
- Por cada módulo de la matriz T17.1, definir:
  - Happy path principal
  - Casos de error comunes
  - Integraciones externas
  - Validaciones de negocio
- Template de caso de prueba:
  ```
  ID: TC-XXX
  Módulo: [Expediente]
  Funcionalidad: [Agregar capacitación]
  Precondiciones: [Usuario autenticado con rol X]
  Pasos: 1... 2... 3...
  Resultado esperado: [...]
  Prioridad: [Alta/Media/Baja]
  ```

### 🧪 EP-07.2: Diseño de Casos de Prueba (Semanas 10-11, 54h)

#### T18.1: Casos de prueba de Login y Seguridad (22h)
**Login Azure AD (8 casos, 8h):**
- TC-001: Login exitoso con usuario UNED válido
- TC-002: Login fallido con usuario inválido
- TC-003: Login con usuario sin roles asignados
- TC-004: Cierre de sesión correcto
- TC-005: Token expirado redirige a login
- TC-006: Doble autenticación si está habilitada
- TC-007: Claims y roles se obtienen correctamente
- TC-008: Redirección post-login a URL original

**Autorización por Roles (12 casos, 8h):**
- TC-010: Admin accede a mantenimiento de catálogos
- TC-011: Usuario regular no accede a mantenimiento
- TC-012: Funcionario accede a su expediente
- TC-013: Funcionario no accede a expediente ajeno
- TC-014: Aprobador accede a módulo de aprobaciones
- TC-015: Funcionario no accede a aprobaciones
- TC-020: API rechaza token inválido con 401
- TC-021: API acepta token válido
- TC-022: API valida roles en endpoints protegidos

**JWT API (6 casos, 6h):**
- TC-030: Generar token con credenciales válidas
- TC-031: Token contiene claims esperados
- TC-032: Token expira después de tiempo configurado
- TC-033: Refresh token (si existe)

#### T18.2: Casos de prueba de Expediente (32h)
**Información Personal (10 casos, 8h):**
- TC-100: Ver información personal propia
- TC-101: Editar información personal
- TC-102: Validación de campos obligatorios
- TC-103: Validación de formatos (email, teléfono, cédula)
- TC-104: Subir fotografía
- TC-105: Cambiar fotografía existente
- TC-106: Histórico de cambios visible

**Información Académica (10 casos, 8h):**
- TC-110: Agregar título académico
- TC-111: Editar título académico
- TC-112: Eliminar título académico
- TC-113: Subir documento de respaldo (PDF)
- TC-114: Ver documento subido
- TC-115: Validar institución de lista de catálogo
- TC-116: Validar grado académico de lista

**Capacitaciones (8 casos, 8h):**
- TC-120: Agregar capacitación
- TC-121: Editar capacitación pendiente de aprobación
- TC-122: No editar capacitación aprobada
- TC-123: Eliminar capacitación pendiente
- TC-124: Subir certificado de capacitación
- TC-125: Enviar capacitación a aprobación

**Idiomas (6 casos, 4h):**
- TC-130: Agregar idioma con nivel
- TC-131: Editar nivel de idioma
- TC-132: Eliminar idioma
- TC-133: Subir certificación de idioma

**Producción Académica (6 casos, 4h):**
- TC-140: Agregar publicación
- TC-141: Agregar proyecto de investigación
- TC-142: Editar producción académica
- TC-143: Subir documento de evidencia

#### T18.3: Casos de prueba de Aprobaciones (16h)
- TC-200: Listar items pendientes de aprobación
- TC-201: Aprobar capacitación
- TC-202: Rechazar capacitación con motivo
- TC-203: Ver histórico de aprobaciones
- TC-204: Notificación por correo al aprobar
- TC-205: Notificación por correo al rechazar
- TC-206: Filtrar por tipo de aprobación
- TC-207: Filtrar por estado
- TC-208: Aprobar lote de items
- TC-209: Validar permisos de aprobador

#### T18.4: Casos de prueba de Catálogos (8h)
- TC-300: Listar catálogo de países
- TC-301: Agregar nuevo país
- TC-302: Editar país existente
- TC-303: Inactivar país
- TC-304: Validar unicidad de código
- TC-305: Listar catálogo de grados académicos
- TC-306: CRUD de instituciones educativas
- TC-307: CRUD de áreas de conocimiento

#### T18.5: Casos de prueba de Integraciones (12h)
**Oracle (4 casos, 4h):**
- TC-400: Conexión exitosa a BD
- TC-401: Ejecución de SP de lectura
- TC-402: Ejecución de SP de escritura
- TC-403: Manejo de errores de BD

**SAUR (4 casos, 4h):**
- TC-410: Consultar datos desde SAUR
- TC-411: Autenticación SAUR exitosa
- TC-412: Manejo de timeout SAUR
- TC-413: Error de servicio SAUR no disponible

**SMTP (3 casos, 2h):**
- TC-420: Enviar correo de notificación
- TC-421: Correo con adjunto
- TC-422: Manejo de error SMTP

**Active Directory (3 casos, 2h):**
- TC-430: Validar usuario contra AD
- TC-431: Obtener información de usuario AD
- TC-432: Manejo de usuario no encontrado en AD

#### T18.6: Casos de prueba de Reportes (8h)
- TC-500: Generar reporte de expediente en PDF
- TC-501: Reporte contiene información completa
- TC-502: Reporte con fotografía
- TC-503: Generar reporte Excel de funcionarios
- TC-504: Filtros de reporte funcionan
- TC-505: Reporte de plazas por vencer
- TC-506: Enviar reporte por correo

### 🧪 EP-07.3: Implementación de Pruebas Unitarias (Semanas 11-12, 60h)

#### T19.1: Setup de infraestructura de pruebas (8h)
- Configurar [UNED.PersonalAcademico.UnitTests](UNED.PersonalAcademico.UnitTests/) para .NET 8
- Actualizar paquetes:
  - `Microsoft.NET.Test.Sdk` → última versión
  - `Moq` → última versión
  - `MSTest` o migrar a `xUnit`
- Configurar coverlet para cobertura de código:
  ```xml
  <PackageReference Include="coverlet.collector" Version="6.0.0" />
  ```
- Crear estructura de carpetas: `BL/`, `DAL/`, `API/`, `Helpers/`

#### T19.2: Pruebas unitarias de AuthSaurBL (12h)
Archivo: [AuthSaurBLTests.cs](UNED.PersonalAcademico.UnitTests/BL/AuthSaurBLTests.cs)
- Mockar dependencias (HttpClient, configuración)
- Test: Autenticación exitosa con credenciales válidas
- Test: Autenticación falla con credenciales inválidas
- Test: Timeout de servicio SAUR
- Test: Manejo de respuesta HTTP 500
- Test: Parse correcto de token de respuesta
- Test: Expiración de token

#### T19.3: Pruebas unitarias de MailBL (8h)
Archivo: [MailBLTests.cs](UNED.PersonalAcademico.UnitTests/BL/MailBLTests.cs)
- Mockar SMTP client
- Test: Envío de correo simple
- Test: Envío con adjunto
- Test: Correo con template
- Test: Manejo de error SMTP
- Test: Validación de formato de email

#### T19.4: Pruebas unitarias de servicios de Expediente (16h)
Archivos en [UNED.PersonalAcademico.UnitTests/BL/Expediente/](UNED.PersonalAcademico.UnitTests/BL/Expediente/)

**CapacitacionesBL (6h):**
- Test: Crear capacitación válida
- Test: Validación de campos obligatorios
- Test: Enviar a aprobación cambia estado
- Test: No editar capacitación aprobada

**TitulosAcademicosBL (5h):**
- Test: Agregar título válido
- Test: Validar grado académico existe en catálogo
- Test: Validar institución existe

**DocumentosBL (5h):**
- Test: Subir documento PDF
- Test: Validar tamaño máximo
- Test: Validar extensiones permitidas
- Test: Generar ruta de almacenamiento

#### T19.5: Pruebas unitarias de servicios de Catalogos (8h)
- Test: Listar todos los países
- Test: Crear país único
- Test: Validar código país único
- Test: Inactivar país en uso (debe fallar)
- Test: Búsqueda con filtros

#### T19.6: Pruebas unitarias de DAL (8h)
- Mockar Oracle connection
- Test: Ejecutar SP de lectura
- Test: Ejecutar SP de escritura
- Test: Manejo de excepción Oracle
- Test: Mapeo de parámetros correcto
- Test: Mapeo de resultados a objetos

### 🧪 EP-07.4: Ejecución de Regresión Funcional (Semanas 12-14, 92h)

#### T20.1: Preparar ambiente QA con .NET 8 (24h, requiere DevOps)
- Configurar servidor QA con .NET 8 runtime
- Configurar IIS/Kestrel para aplicación
- Configurar variables de entorno con secretos QA
- Publicar MVC: `dotnet publish -c Release UNED.PersonalAcademico\UNED.PersonalAcademico.csproj`
- Publicar API: `dotnet publish -c Release UNED.PersonalAcademico.API\UNED.PersonalAcademico.API.csproj`
- Copiar publicaciones al servidor
- Smoke test: aplicación inicia correctamente
- Validar conexión a Oracle QA
- Validar conexión a SAUR QA

#### T20.2: Preparar datos de prueba (23h)
- Coordinar con DBA:
  - Crear usuarios de prueba en BD QA
  - Crear datos base de catálogos
  - Crear 5 funcionarios de prueba con expedientes variados
  - Crear funcionario aprobador
- En Azure AD QA:
  - Crear usuarios de prueba
  - Asignar roles: Admin, Funcionario, Aprobador, Consulta
- Documentar credenciales de prueba en archivo seguro (no en repo)
- Crear script de datos semilla si no existe

#### T20.3: Ejecutar casos de prueba de Seguridad (12h)
- Ejecutar TC-001 a TC-033 (casos de T18.1)
- Por cada caso:
  - Ejecutar según pasos definidos
  - Capturar pantallas
  - Documentar resultado: ✅ Pasa / ❌ Falla
  - Si falla, crear issue en tracker con evidencia
- Llenar matriz de resultados:
  ```
  TC-ID | Descripción | Resultado | Defecto ID | Comentarios
  ```

#### T20.4: Ejecutar casos de prueba de Expediente (24h)
- Ejecutar TC-100 a TC-143 (casos de T18.2)
- Mismo proceso que T20.3
- Priorizar casos críticos primero
- Validar que documentos se suben correctamente
- Validar histórico de cambios

#### T20.5: Ejecutar casos de prueba de Aprobaciones (8h)
- Ejecutar TC-200 a TC-209
- Validar flujo completo: funcionario crea → aprobador recibe → aprobador aprueba → funcionario ve aprobado
- Validar correos se envían correctamente

#### T20.6: Ejecutar casos de prueba de Catálogos (4h)
- Ejecutar TC-300 a TC-307
- Validar CRUD completo de al menos 3 catálogos

#### T20.7: Ejecutar casos de prueba de Integraciones (8h)
- Ejecutar TC-400 a TC-432
- Para Oracle: ejecutar 10 SPs diferentes
- Para SAUR: validar endpoints principales
- Para SMTP: enviar correos reales a cuentas de prueba
- Para AD: validar autenticación de 3 usuarios diferentes

#### T20.8: Ejecutar casos de prueba de Reportes (6h)
- Ejecutar TC-500 a TC-506
- Validar PDFs se generan correctamente
- Validar Excel descarga correctamente
- Validar contenido de reportes es correcto

#### T20.9: Consolidar resultados de regresión (3h)
- Consolidar todas las matrices de resultados
- Calcular métricas:
  - Total casos ejecutados
  - % casos pasados
  - % casos fallidos
  - Defectos críticos abiertos
- Crear reporte ejecutivo de regresión
- Presentar a líder técnico

### 🧪 EP-07.5: Corrección de Defectos (Semanas 14-15, 60h)

#### T21.1: Priorizar defectos encontrados (4h)
- Clasificar defectos por severidad:
  - **Blocker**: Sistema no funciona, no se puede continuar
  - **Critical**: Funcionalidad principal no funciona
  - **High**: Funcionalidad secundaria no funciona
  - **Medium**: Comportamiento incorrecto pero hay workaround
  - **Low**: Cosmético, no afecta funcionalidad
- Priorizar por: Severidad > Usuarios afectados > Frecuencia
- Crear backlog priorizado

#### T21.2: Corregir defectos Blocker (16h)
- Típicamente: aplicación no inicia, error de conexión BD, login no funciona
- Asignar máxima prioridad
- Por cada defecto:
  - Reproducir localmente
  - Debugear y encontrar causa raíz
  - Implementar fix
  - Probar localmente
  - Crear prueba unitaria que valide el fix
  - Commit y push
  - Desplegar en QA
  - Re-ejecutar caso de prueba que falló
  - Cerrar defecto si pasa

#### T21.3: Corregir defectos Critical (20h)
- Típicamente: módulo principal no funciona, datos no se guardan, integración falla
- Mismo proceso que T21.2
- Meta: 100% defectos críticos cerrados

#### T21.4: Corregir defectos High (16h)
- Evaluar cuáles son imprescindibles para go-live
- Corregir los seleccionados
- Los no críticos pueden ir a backlog post-producción

#### T21.5: Validar fixes con re-ejecución de pruebas (4h)
- Re-ejecutar todos los casos que fallaron originalmente
- Validar que fixes no introdujeron regresiones nuevas
- Actualizar matriz de resultados

### 🧪 EP-07.6: Validación con Usuarios Finales (UAT) (Semanas 15-16, 31h)

#### T22.1: Preparar sesiones UAT (8h)
- Identificar usuarios clave:
  - 2 funcionarios que usan el sistema regularmente
  - 1 aprobador
  - 1 admin que mantiene catálogos
  - 1 usuario consulta
- Enviar invitaciones con fechas y horarios
- Preparar ambiente UAT (copia de QA con datos frescos)
- Crear guía de validación con escenarios:
  - "Actualice su información personal y académica"
  - "Agregue una capacitación y envíela a aprobación"
  - "Apruebe 3 capacitaciones pendientes"
  - "Genere el reporte de su expediente"
- Preparar formulario de feedback

#### T22.2: Ejecutar sesiones UAT (16h)
- Sesión 1 (4h): Funcionarios
  - Observar cómo usan el sistema
  - Tomar notas de comentarios
  - Registrar problemas encontrados
  - Capturar feedback cualitativo
- Sesión 2 (4h): Aprobador y Admin
  - Mismo proceso
- Sesión 3 (4h): Escenarios adicionales
  - Probar flujos menos comunes
  - Validar reportes
- Sesión 4 (4h): Pruebas de integraciones
  - Validar integraciones en ambiente productivo simulado

#### T22.3: Consolidar feedback y ajustes menores (5h)
- Transcribir todos los comentarios
- Clasificar:
  - **Debe corregirse antes de producción**
  - **Nice to have, puede ser después**
  - **No es defecto, es por diseño**
- Implementar ajustes críticos
- Documentar mejoras futuras en backlog

#### T22.4: Obtener aprobación formal (2h)
- Preparar acta de aceptación UAT
- Incluir:
  - Resumen de pruebas ejecutadas
  - Defectos corregidos
  - Defectos pendientes (si hay) con justificación
  - Comentarios de usuarios
  - Recomendación: Aprobado para producción / Requiere nueva iteración
- Reunión con stakeholders para firma
- Obtener aprobación formal por escrito

---

## RESUMEN DE ENTREGABLES

### Documentos
1. [VariablesAmbiente.md](VariablesAmbiente.md) - Inventario de secretos y variables
2. [ROLLBACK.md](ROLLBACK.md) - Plan de rollback por fase de migración
3. [WARNINGS.md](WARNINGS.md) - Warnings aceptables post-migración
4. [DECISIONES_ARQUITECTURA.md](DECISIONES_ARQUITECTURA.md) - Decisiones técnicas tomadas
5. [PLAN_PRUEBAS.md](PLAN_PRUEBAS.md) - Estrategia completa de pruebas
6. Matrices de casos de prueba (Excel)
7. Matrices de resultados de regresión (Excel)
8. Reporte ejecutivo de UAT
9. Acta de aceptación formal

### Código
1. Todos los proyectos migrados a .NET 8
2. global.json con SDK 8.0
3. Configuración de variables de entorno en Startup.cs
4. Pruebas unitarias con >60% cobertura en BL
5. Scripts de configuración de ambiente
6. Paquetes UNED.Ami.* actualizados a versión 5.0.0

### Infraestructura
1. Ambiente QA configurado con .NET 8
2. Variables de entorno configuradas en QA
3. Credenciales rotadas en todos los sistemas
4. Documentación de despliegue

---

## DEPENDENCIAS CRÍTICAS EXTERNAS

| Dependencia | Responsable | Requerido para | Cuándo |
|-------------|-------------|----------------|--------|
| Nuevo usuario Oracle QA | DBA | T02.1 | Semana 2 |
| Nuevo ClientSecret Azure AD | Admin Azure | T02.2 | Semana 2 |
| Nueva credencial SMTP | Admin Correo | T02.4 | Semana 3 |
| Nuevo token SAUR | Equipo SAUR | T02.4 | Semana 3 |
| Servidor QA con .NET 8 | DevOps | T20.1 | Semana 12 |
| Usuarios de prueba en Azure AD | Admin Azure | T20.2 | Semana 12 |
| Datos de prueba en BD QA | DBA | T20.2 | Semana 12 |
| Usuarios clave para UAT | RRHH/Gestión | T22.1 | Semana 15 |

---

## RIESGOS Y MITIGACIONES

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Oracle driver incompatible con .NET 8 | Media | Alto | Validar en T08.2 temprano, buscar alternativas |
| Rotativa no funciona en .NET 8 | Media | Alto | Validar en T16.4, evaluar alternativa (DinkToPdf, PuppeteerSharp) |
| Defectos críticos en regresión | Alta | Alto | Buffer de 60h para correcciones en T21 |
| Usuarios no disponibles para UAT | Media | Medio | Identificar y confirmar usuarios en semana 8 |
| Credenciales no se pueden rotar | Baja | Crítico | Coordinar con responsables desde semana 1 |
| Breaking changes no documentados | Media | Alto | Testing exhaustivo en cada fase |

---

## VERIFICACIÓN POR FASE

### ✅ Checklist Fase 0 (Seguridad)
- [ ] Matriz de secretos completa con 0 secretos en código
- [ ] Variables de entorno documentadas
- [ ] Código lee desde variables de entorno
- [ ] Oracle: credencial rotada y validada
- [ ] Azure AD: ClientSecret rotado y validado
- [ ] JWT: SecretKey rotado y validado
- [ ] SMTP: credencial rotada y validada
- [ ] SAUR: token rotado y validado
- [ ] Roles y permisos documentados
- [ ] JWT expira en máximo 1 hora
- [ ] Sesión MVC expira en máximo 30 min
- [ ] Cookies con HttpOnly, Secure, SameSite
- [ ] Casos de prueba de seguridad ejecutados
- [ ] 0 secretos en repositorio validado con grep

### ✅ Checklist Fase 1 (.NET 3.1)
- [ ] global.json con SDK 3.1
- [ ] Rama migrate-to-netcore31 creada
- [ ] Dependencia circular BL→MVC eliminada
- [ ] Paquetes UNED.Ami.* migrados a 2.0.0
- [ ] EL migrado a netcoreapp3.1, compila sin errores
- [ ] DAL migrado a netcoreapp3.1, compila sin errores
- [ ] Conexión Oracle validada con driver 3.21.1
- [ ] BL migrado a netcoreapp3.1, compila sin errores
- [ ] AutoMapper v10 configurado correctamente
- [ ] MVC migrado, endpoint routing implementado
- [ ] API migrada, endpoint routing implementado
- [ ] Swagger funciona correctamente
- [ ] Aplicación inicia y login funciona
- [ ] 0 errores de compilación en toda la solución

### ✅ Checklist Fase 2 (.NET 5)
- [ ] global.json con SDK 5.0
- [ ] Todos los proyectos con TargetFramework net5.0
- [ ] Paquetes actualizados a versiones .NET 5
- [ ] Paquetes UNED.Ami.* en versión 3.0.0
- [ ] Compila sin errores
- [ ] Aplicación funciona igual que en 3.1
- [ ] Validación de funcionalidades críticas OK

### ✅ Checklist Fase 3 (.NET 6)
- [ ] global.json con SDK 6.0
- [ ] Todos los proyectos con TargetFramework net6.0
- [ ] Paquetes actualizados a versiones .NET 6
- [ ] Paquetes UNED.Ami.* en versión 4.0.0
- [ ] Swashbuckle 6.x funcionando
- [ ] Compila sin errores
- [ ] Aplicación funciona correctamente
- [ ] Oracle driver actualizado y validado

### ✅ Checklist Fase 4 (.NET 8)
- [ ] global.json con SDK 8.0
- [ ] Todos los proyectos con TargetFramework net8.0
- [ ] Paquetes actualizados a versiones .NET 8
- [ ] Paquetes UNED.Ami.* en versión 5.0.0
- [ ] Oracle driver compatible validado
- [ ] Rotativa o alternativa funciona
- [ ] Compila sin errores
- [ ] Warnings de nullable documentados
- [ ] Performance igual o mejor
- [ ] Todas las integraciones funcionan

### ✅ Checklist Fase 5 (Pruebas)
- [ ] Plan de pruebas aprobado
- [ ] Matriz de casos de prueba completa (150+ casos)
- [ ] Infraestructura de pruebas unitarias configurada
- [ ] Pruebas unitarias implementadas (>60% cobertura BL)
- [ ] Ambiente QA con .NET 8 configurado
- [ ] Datos de prueba creados
- [ ] Regresión funcional ejecutada (100%)
- [ ] Matriz de resultados consolidada
- [ ] Defectos Blocker corregidos (100%)
- [ ] Defectos Critical corregidos (100%)
- [ ] Defectos High evaluados y corregidos según prioridad
- [ ] Re-ejecución de casos fallidos OK
- [ ] Sesiones UAT completadas
- [ ] Feedback de usuarios documentado
- [ ] Ajustes críticos implementados
- [ ] Acta de aceptación firmada

---

## PRÓXIMOS PASOS DESPUÉS DE ESTE PLAN

1. **Preparación de Producción** (EP-09):
   - Configurar ambiente productivo con .NET 8
   - Configurar variables de entorno producción
   - Plan de despliegue detallado con rollback
   - Ventana de mantenimiento coordinada
   - Monitoreo post-despliegue

2. **Mejoras Post-Migración** (Backlog):
   - Migrar de Newtonsoft.Json a System.Text.Json
   - Evaluar migración a minimal APIs
   - Implementar rate limiting de .NET 8
   - Habilitar nullable reference types
   - Mejorar cobertura de pruebas a >80%
   - Implementar pruebas de integración automatizadas
   - Pipeline CI/CD completo
   - Monitoreo y observabilidad (APM)

3. **Deuda Técnica Restante** (EP-04, EP-05):
   - Resolver 114 warnings originales
   - Refactorizar arquitectura de capas
   - Normalizar uso de AutoMapper
   - Implementar patrones CQRS donde aplique
   - Documentación técnica completa
