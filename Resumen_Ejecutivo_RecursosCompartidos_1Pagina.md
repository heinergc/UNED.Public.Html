# Resumen ejecutivo de actualizacion

Proyecto: RecursosCompartidos (UNED.Ami.sln)  
Fecha: 2026-06-08  
Audiencia: Jefatura / toma de decision

## 1) Estado actual en una frase

La plataforma RecursosCompartidos esta operativa pero tecnologicamente en riesgo alto: combina tecnologias legacy, build no portable y configuraciones sensibles expuestas.

## 2) Que se encontro

- La solucion principal integra 21 proyectos de codigo y 2 de documentacion tecnica.
- Coexisten tres stacks:
  - netcoreapp2.2 (predominante, fuera de soporte)
  - netcoreapp3.1 (fuera de soporte)
  - ASP.NET MVC 5 en .NET Framework 4.6.1 (UNED.Ami.RUI)
- El build local con dotnet build --no-restore falla por restore no resuelto y por dependencias de compilacion web clasica para el proyecto RUI.
- Hay alta dependencia de integraciones externas: SQL Server, Active Directory, SAUR y AS400.
- Se detectaron credenciales y configuraciones sensibles en archivos de configuracion y pruebas.

## 3) Riesgos de negocio

- Seguridad: exposicion de credenciales de servicios y bases de datos.
- Continuidad operativa: plataforma sobre versiones sin soporte y build fragil.
- Tiempo de respuesta a incidentes: solucion heterogenea dificulta soporte y cambios.
- Riesgo de regresion: pruebas actuales con dependencias de ambiente real.

## 4) Recomendacion ejecutiva

Aprobar una modernizacion controlada por fases, priorizando:

1. Saneamiento de seguridad (secretos y rotacion de credenciales).
2. Estabilizacion de cadena de build y restore reproducible.
3. Definicion de estrategia para UNED.Ami.RUI (aislar, mantener controlado o modernizar).
4. Actualizacion gradual de proyectos .NET Core y dependencias.
5. Endurecimiento de pruebas y regresion integral antes de produccion.

## 5) Esfuerzo y costo estimado

Rango total estimado (sin IA): 1.236 a 2.232 horas.

Epicas criticas (P1):

- EP-03 Seguridad y secretos: 120 a 200 h
- EP-02 Framework y dependencias: 200 a 360 h
- EP-05 Frontend legacy y build mixto: 140 a 300 h
- EP-07 Pruebas: 220 a 420 h

Total critico P1: 680 a 1.280 horas.

## 6) Cronograma ejecutivo recomendado

Para una dedicacion de 30 h/semana (6 h funcionales por dia):

- Ola critica (P1): 23 a 43 semanas
- Primera ola propuesta para estabilizacion base: 25 semanas (742 h)

## 7) Decisiones que requiere jefatura

1. Definir politica institucional de version objetivo (.NET LTS aprobada).
2. Autorizar rotacion inmediata de credenciales y canal seguro de secretos.
3. Aprobar estrategia oficial para el modulo legacy UNED.Ami.RUI.
4. Confirmar disponibilidad de ambientes de prueba para SQL/AD/SAUR/AS400.
5. Validar presupuesto y equipo para ejecutar EP-03, EP-02, EP-05 y EP-07 como ruta critica.

## 8) Criterio de exito

Se considera exitosa la fase 1 cuando:

- No existan secretos activos en repositorio.
- El build sea reproducible en entorno controlado.
- Exista ruta aprobada para RUI y modernizacion .NET.
- Se complete regresion minima con evidencia y sin defectos criticos abiertos.

## 9) Referencia

Este resumen sintetiza el analisis tecnico detallado contenido en el reporte extenso de RecursosCompartidos.