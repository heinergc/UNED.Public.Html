# Inventario de objetos de base de datos usados por UNED.PersonalAcademico

Fecha de generacion: 2026-06-05 16:02:03

Este inventario fue generado mediante revision estatica del codigo fuente. No se realizo conexion a Oracle ni consulta al diccionario de base de datos, por lo que el listado refleja los objetos referenciados por la aplicacion y puede requerir validacion con DBA para confirmar existencia, sinonimos, permisos y dependencias internas de paquetes.

## Resumen

| Categoria | Cantidad |
|---|---:|
| Clases de entidad/modelos `TBPE`/`TBPP` | 56 |
| Objetos Oracle referenciados en literales de codigo | 263 |

## Esquemas y origenes identificados

| Origen | Uso observado |
|---|---|
| `RHUMANOS` | Esquema principal usado por los modelos y procedimientos Oracle. |
| Oracle configurado por cadena de conexion | Acceso ejecutado desde `OracleManagerHelper` con `CommandType.StoredProcedure` y consultas escalares puntuales. |
| `ApiSAUR` | Integracion externa para autenticacion/autorizacion; no es objeto Oracle local en este inventario. |
| `ApiAMI` | Integracion externa usada por servicios compartidos; no se clasifica como objeto Oracle local. |

## Entidades / tablas inferidas por modelos

Estas clases representan entidades de negocio con nombres alineados a objetos `TBPE`/`TBPP`. Se listan como tablas inferidas porque el codigo las usa como modelos de datos y muchas incluyen propiedades de procedimientos almacenados.

| Objeto inferido | Modulo | Archivo | Linea |
|---|---|---|---:|
| `TBPECATAgenciaAcreditadoras` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATAgenciaAcreditadoras.cs` | 9 |
| `TBPECATCategSal` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATCategSal.cs` | 11 |
| `TBPECATEscudoFiscal` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATEscudoFiscal.cs` | 8 |
| `TBPECATEstadCiv` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstadCiv.cs` | 8 |
| `TBPECATEstados` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstados.cs` | 8 |
| `TBPECATEvaluacion` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATEvaluacion.cs` | 8 |
| `TBPECatGenero` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECatGenero.cs` | 8 |
| `TBPECATIdentificador` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATIdentificador.cs` | 9 |
| `TBPECATipoAgrupacion` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoAgrupacion.cs` | 8 |
| `TBPECATipoPublicacion` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoPublicacion.cs` | 8 |
| `TBPECATMotSalid` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATMotSalid.cs` | 8 |
| `TBPECATNivel` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATNivel.cs` | 10 |
| `TBPECATPais` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATPais.cs` | 11 |
| `TBPECATParentesto` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATParentesto.cs` | 7 |
| `TBPECATParticipacion` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATParticipacion.cs` | 8 |
| `TBPECATPertinencia` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATPertinencia.cs` | 8 |
| `TBPECATRangos` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATRangos.cs` | 8 |
| `TBPECATTipIden` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipIden.cs` | 8 |
| `TBPECATTipoCompromiso` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoCompromiso.cs` | 9 |
| `TBPECATTipoExpe` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoExpe.cs` | 8 |
| `TBPECATTipoInves` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoInves.cs` | 8 |
| `TBPECATTipoTelefono` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoTelefono.cs` | 8 |
| `TBPEDETExpLaboral` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEDETExpLaboral.cs` | 11 |
| `TBPEDETOrganizac` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEDETOrganizac.cs` | 6 |
| `TBPEDETPlazaRastreo` | Plazas | `.\UNED.PersonalAcademico.EL\Plaza\TBPEDETPlazaRastreo.cs` | 8 |
| `TBPEDETPlazas` | Plazas | `.\UNED.PersonalAcademico.EL\Plaza\TBPEDETPlazas.cs` | 8 |
| `TBPEDETPlazasMostrar` | Plazas | `.\UNED.PersonalAcademico.EL\Plaza\TBPEDETPlazas.cs` | 140 |
| `TBPEDETPremios` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEDETPremios.cs` | 8 |
| `TBPEDETProdAcad` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEDETProdAcad.cs` | 6 |
| `TBPEMAEActCapacit` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEActCapacit.cs` | 8 |
| `TBPEMAEArea` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEArea.cs` | 9 |
| `TBPEMAECapacitacion` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAECapacitacion.cs` | 9 |
| `TBPEMAECondPens` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAECondPens.cs` | 9 |
| `TBPEMAEConduPermiso` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEConduPermiso.cs` | 9 |
| `TBPEMAECorreos` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAECorreos.cs` | 8 |
| `TBPEMAEDocVarios` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEDocVarios.cs` | 8 |
| `TBPEMAEExpDigital` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEExpDigital.cs` | 7 |
| `TBPEMAEExperienciaAcademica` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEExperienciaAcademica.cs` | 9 |
| `TBPEMAEExpLaboral` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEExpLaboral.cs` | 6 |
| `TBPEMAEFamiliares` | Maestros / Detalles | `.\UNED.PersonalAcademico.EL\MaestrosDetalles\TBPEMAEFamiliares.cs` | 8 |
| `TBPEMAEFuncionarios` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEFuncionarios.cs` | 11 |
| `TBPEMAEFuncionariosPhoto` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEFuncionarios.cs` | 156 |
| `TBPEMAEGradoAcad` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEGradoAcad.cs` | 9 |
| `TBPEMAEIdioma` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEIdioma.cs` | 9 |
| `TBPEMAEInfAcademica` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEInfAcademica.cs` | 12 |
| `TBPEMAEInfIdiomas` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEInfIdiomas.cs` | 6 |
| `TBPEMAEInstitucion` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEInstitucion.cs` | 8 |
| `TBPEMAEPaqInfo` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEPaqInfo.cs` | 6 |
| `TBPEMAEPeriodo` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEPeriodo.cs` | 9 |
| `TBPEMAEProyectos` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAEProyectos.cs` | 6 |
| `TBPEMAETelefonos` | Expediente | `.\UNED.PersonalAcademico.EL\Expediente\TBPEMAETelefonos.cs` | 8 |
| `TBPEMAETipoDoc` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAETipoDoc.cs` | 9 |
| `TBPEMAETJorn` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAETJorn.cs` | 3 |
| `TBPEMAETJornada` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAETJornada.cs` | 8 |
| `TBPEReguladoraPension` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPEReguladoraPension.cs` | 11 |
| `TBPPMAEMovimiento` | Catalogos | `.\UNED.PersonalAcademico.EL\Catalogos\TBPPMAEMovimiento.cs` | 8 |

## Procedimientos, paquetes y otros objetos Oracle referenciados

La siguiente matriz lista objetos encontrados en literales de codigo, propiedades `*ProcedureName`, llamadas directas desde BL/DAL y tipos auxiliares usados para parametros.

| Objeto | Tipo | Modulo | Referencias |
|---|---|---|---|
| `PCKPESLACTCAPACITACION.ALLACTIVIDADESCUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEActCapacit.cs:46 |
| `PCKPESLACTCAPACITACION.SPPESLACTIVIDADPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEActCapacit.cs:40 |
| `PCKPESLCAPACITACION.ALLCAPACITACIONESCUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\CapacitacionApiModel.cs:80 |
| `PCKPESLCAPACITACION.SPPESLCAPAPORCONS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\CapacitacionApiModel.cs:86 |
| `PCKPESLCATAGENACRE.SPPESLAGENACREPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATAgenciaAcreditadoras.cs:55 |
| `PCKPESLCATAGENACRE.SPPESLTODOSAGENACRE` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATAgenciaAcreditadoras.cs:49 |
| `PCKPESLCATEGSAL.SPPESLCATEGSALPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATCategSal.cs:51 |
| `PCKPESLCATEGSAL.SPPESLTODOSCATEGSAL` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATCategSal.cs:57 |
| `PCKPESLCATESCFISCAL.SPPESLESCFISCALPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEscudoFiscal.cs:28 |
| `PCKPESLCATESCFISCAL.SPPESLTODOSTESCFISCAL` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEscudoFiscal.cs:34 |
| `PCKPESLCATESTADCIV.SPPESLESTADCIVPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstadCiv.cs:28 |
| `PCKPESLCATESTADCIV.SPPESLTODOSESTADCIV` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstadCiv.cs:34 |
| `PCKPESLCATEVALUACION.SPPESLEVALUACIONPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEvaluacion.cs:52 |
| `PCKPESLCATEVALUACION.SPPESLTODOSEVALUACION` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEvaluacion.cs:58 |
| `PCKPESLCATGENERO.SPPESLGENEROPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECatGenero.cs:27 |
| `PCKPESLCATGENERO.SPPESLTODOSGENEROS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECatGenero.cs:33 |
| `PCKPESLCATIDENTIFIC.SPPESLIDENTIFICADORPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATIdentificador.cs:29 |
| `PCKPESLCATIDENTIFIC.SPPESLTODOSIDENTIFICADOR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATIdentificador.cs:35 |
| `PCKPESLCATMOTSALD.SPPESLMOTSALDACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATMotSalid.cs:48 |
| `PCKPESLCATMOTSALD.SPPESLTMOTSALDPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATMotSalid.cs:42 |
| `PCKPESLCATNIVEL.SPPESLNIVELPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATNivel.cs:54 |
| `PCKPESLCATNIVEL.SPPESLTODOSNIVEL` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATNivel.cs:60 |
| `PCKPESLCATPAIS.SPPESLBUSQPAISPAG` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPais.cs:64 |
| `PCKPESLCATPAIS.SPPESLPAISES` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPais.cs:58 |
| `PCKPESLCATPAIS.SPPESLPAISPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPais.cs:52 |
| `PCKPESLCATPARENTESCO.SPPESLPARENTESCOPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATParentesto.cs:27 |
| `PCKPESLCATPARENTESCO.SPPESLTODOSPARENTESCO` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATParentesto.cs:33 |
| `PCKPESLCATPARTICIPACION.SPPESLPARTICIPACIONPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATParticipacion.cs:47 |
| `PCKPESLCATPARTICIPACION.SPPESLTODOSPARTICIPACION` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATParticipacion.cs:53 |
| `PCKPESLCATPERTINEN.SPPESLPERTINENPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPertinencia.cs:52 |
| `PCKPESLCATPERTINEN.SPPESLTODOSPERTINEN` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPertinencia.cs:58 |
| `PCKPESLCATPUBLICACION.SPPESLPUBLICACIONPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoPublicacion.cs:28 |
| `PCKPESLCATPUBLICACION.SPPESLTODOSTIPOPUBLICACION` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoPublicacion.cs:34 |
| `PCKPESLCATRANGO.SPPESLRANGOPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATRangos.cs:52 |
| `PCKPESLCATRANGO.SPPESLTODOSRANGO` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATRangos.cs:58 |
| `PCKPESLCATREGPENSI.SPPESLREGPENSIPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEReguladoraPension.cs:31 |
| `PCKPESLCATREGPENSI.SPPESLTODOSREGPENSI` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEReguladoraPension.cs:37 |
| `PCKPESLCATTIPCOMPROM.SPPESLTIPCOMPROMPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoCompromiso.cs:35 |
| `PCKPESLCATTIPCOMPROM.SPPESLTODOSTIPCOMPROM` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoCompromiso.cs:41 |
| `PCKPESLCATTIPIDEN.SPPESLTIPOIDENPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipIden.cs:29 |
| `PCKPESLCATTIPIDEN.SPPESLTODOSTIPOIDEN` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipIden.cs:35 |
| `PCKPESLCATTIPOAGRU.SPPESLTIPOAGRUPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoAgrupacion.cs:28 |
| `PCKPESLCATTIPOAGRU.SPPESLTODOSTIPOAGRU` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoAgrupacion.cs:34 |
| `PCKPESLCATTIPOEXPE.SPPESLTIPOEXPEPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoExpe.cs:44 |
| `PCKPESLCATTIPOEXPE.SPPESLTODOSTIPOEXPE` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoExpe.cs:50 |
| `PCKPESLCATTIPOINVES.SPPESLTIPOINVESPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoInves.cs:28 |
| `PCKPESLCATTIPOINVES.SPPESLTODOSTIPOINVES` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoInves.cs:34 |
| `PCKPESLCATTIPOTEL.SPPESLTIPOTELPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoTelefono.cs:35 |
| `PCKPESLCATTIPOTEL.SPPESLTODOSETIPOTEL` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoTelefono.cs:41 |
| `PCKPESLCONDPENS.SPPESLCONDPENS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAECondPens.cs:28 |
| `PCKPESLCONDUCAC.SPPESLCONDUCACIDPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEConduPermiso.cs:65 |
| `PCKPESLCONDUCAC.SPPESLTODOSCONDUCAC` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEConduPermiso.cs:71 |
| `PCKPESLCORREOFUNCI.SPPESLCORREOFUNCIPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAECorreos.cs:50 |
| `PCKPESLCORREOFUNCI.SPPESLTODOSCORFUNCI` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAECorreos.cs:56 |
| `PCKPESLDETPLAZA.SPPEINFOPLAZAPARAPERSONAL` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Plaza\ApiModel\InfoPlazaFuncionarioApiModel.cs:67 |
| `PCKPESLDETPLAZA.SPPESLDETALLEPLAZAS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Plaza\TBPEDETPlazas.cs:97 |
| `PCKPESLDETPLAZA.SPPESLDETALLEPLAZASPAG` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Plaza\TBPEDETPlazas.cs:103 |
| `PCKPESLDOCVARIOS.SPPESLOBTIENETODOSDOCVARIOSXID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEDocVarios.cs:47 |
| `PCKPESLDOCVARIOS.SPPESLOBTIENEUNDOCUMENTOXID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEDocVarios.cs:53 |
| `PCKPESLESTADOS.SPPESLESTADOPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstados.cs:49 |
| `PCKPESLESTADOS.SPPESLTODOSESTADOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstados.cs:55 |
| `PCKPESLEXPACA.ALLEXPACADEMICACUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoAcademicaApiModel.cs:50 |
| `PCKPESLEXPACA.SPPESLINFOACADPORCONS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoAcademicaApiModel.cs:56 |
| `PCKPESLEXPDIGITAL.SPPEDOCSINGRESADOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Expediente\DocumentosDigitalesBL.cs:92 |
| `PCKPESLEXPDIGITAL.SPPEOBTENEREXPDIGITAL` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEExpDigital.cs:42 |
| `PCKPESLEXPDIGITAL.SPPERECUENTODOCS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Expediente\DocumentosDigitalesBL.cs:78 |
| `PCKPESLEXPDIGITAL.TRAEREXPDIGITALCUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEExpDigital.cs:36 |
| `PCKPESLEXPLABORAL.ALLEXPLABORALESCUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETExpLaboral.cs:37 |
| `PCKPESLEXPLABORAL.SPPESLEXPLABPORCONS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETExpLaboral.cs:43 |
| `PCKPESLFUNCIONARIOS.SPPESLBUSQFUNPAG` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Busquedas\BusquedaFuncionario.cs:63 |
| `PCKPESLFUNCIONARIOS.SPPESLBUSQUEDAFUNCIONARIO` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Busquedas\BusquedaFuncionario.cs:57 |
| `PCKPESLFUNCIONARIOS.SPPESLFUNCINFLABORALXID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoLaboralApiModel.cs:185 |
| `PCKPESLFUNCIONARIOS.SPPESLFUNCIONARIOSXID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEFuncionarios.cs:136 |
| `PCKPESLGRADO.ALLGRADOSCUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEGradoAcad.cs:30 |
| `PCKPESLIDIOMA.SPPESLIDIOMAS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEIdioma.cs:30 |
| `PCKPESLINFIDIOMAS.ALLINFIDIOMASCUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoIdiomaApiModel.cs:56 |
| `PCKPESLINFIDIOMAS.SPPESLIDIOMASPORCONS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoIdiomaApiModel.cs:62 |
| `PCKPESLINSTITUCIONES.SPPESLINSTITUCIONES` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEInstitucion.cs:65 |
| `PCKPESLINSTITUCIONES.SPPESLINSTITUCIONPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEInstitucion.cs:71 |
| `PCKPESLMAEAREA.SPPESLAREAPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEArea.cs:39 |
| `PCKPESLMAEAREA.SPPESLTODOSAREA` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEArea.cs:45 |
| `PCKPESLMAEEXPACADEMICA.SPPESLEXPACADEMPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEExperienciaAcademica.cs:42 |
| `PCKPESLMAEEXPACADEMICA.SPPESLTODOSEXPACADEM` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEExperienciaAcademica.cs:48 |
| `PCKPESLMAEFAMIL.SPPESLFAMILIARPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\MaestrosDetalles\TBPEMAEFamiliares.cs:80 |
| `PCKPESLMAEFAMIL.SPPESLMOSTRARFAMILIARESPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\MaestrosDetalles\TBPEMAEFamiliares.cs:86 |
| `PCKPESLMAEMOVIM.SPPESLESCMOVIMPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPPMAEMovimiento.cs:37 |
| `PCKPESLMAEMOVIM.SPPESLTODOSTMOVIM` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPPMAEMovimiento.cs:43 |
| `PCKPESLMAEPERIODO.SPPESLPERIODOPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEPeriodo.cs:50 |
| `PCKPESLMAEPERIODO.SPPESLTODOSPERIODOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEPeriodo.cs:56 |
| `PCKPESLORGANIZAC.SPPESLORGANIZAC` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\OrganizacionApiModel.cs:22 |
| `PCKPESLORGANIZAC.SPPESLORGANIZACPORCONS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\OrganizacionApiModel.cs:28 |
| `PCKPESLPAQINFO.ALLPAQUETESINFOCUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\PaqueteInfoApiModel.cs:29 |
| `PCKPESLPAQINFO.SPPESLPAQINFOPORCONS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\PaqueteInfoApiModel.cs:35 |
| `PCKPESLPREMIOS.SPPESLPREMIOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETPremios.cs:50 |
| `PCKPESLPREMIOS.SPPESLPREMIOSPORCONS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETPremios.cs:56 |
| `PCKPESLPRODACAD.SPPESLPRODACAD` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProdAcadApiModel.cs:26 |
| `PCKPESLPRODACAD.SPPESLPRODACADPORCONS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProdAcadApiModel.cs:32 |
| `PCKPESLPROYECTOS.SPPESLPROYECTOPORCOD` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProyectosApiModel.cs:54 |
| `PCKPESLPROYECTOS.SPPESLPROYECTOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProyectosApiModel.cs:48 |
| `PCKPESLTELFUNCI.SPPESLTELFUNCIPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAETelefonos.cs:80 |
| `PCKPESLTELFUNCI.SPPESLTODOSTELFUNCI` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAETelefonos.cs:86 |
| `PCKPESLTIPDOCUMENTOS.SPPESLDESCTIPOSDOCUMENTOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAETipoDoc.cs:73 |
| `PCKPESLTIPDOCUMENTOS.SPPESLTIPOSDOCUMENTOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAETipoDoc.cs:67 |
| `RHUMANOS.PCKPESLACTCAPACITACION.SPPESLACTACTIVAS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\ActividadCapacitacionBL.cs:33 |
| `RHUMANOS.PCKPESLCATESCFISCAL.SPPESLESCFISCALACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\EscudoFiscalBL.cs:75 |
| `RHUMANOS.PCKPESLCATESTADCIV.SPPESLESTADCIVACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\EstadoCivilBL.cs:52 |
| `RHUMANOS.PCKPESLCATEVALUACION.SPPESLEVALUACIONACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoEvaluacionBL.cs:32 |
| `RHUMANOS.PCKPESLCATGENERO.SPPESLGENEROSACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\GeneroBL.cs:49 |
| `RHUMANOS.PCKPESLCATIDENTIFIC.SPPESLIDENTIFICADORACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\IdentificadorBL.cs:34 |
| `RHUMANOS.PCKPESLCATMOTSALD.SPPESLMOTSALDACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\MotivoSalidaBL.cs:34 |
| `RHUMANOS.PCKPESLCATNIVEL.SPPESLNIVELACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoNivelBL.cs:33 |
| `RHUMANOS.PCKPESLCATPAIS.SPPESLPAISESACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\PaisBL.cs:28 |
| `RHUMANOS.PCKPESLCATPAIS.SPPESLPAISESPORIDS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\PaisBL.cs:36 |
| `RHUMANOS.PCKPESLCATPAIS.SPPESLPAISPORID` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\PaisBL.cs:44 |
| `RHUMANOS.PCKPESLCATPARENTESCO.SPPESLPARENTESCOACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\ParentescoBL.cs:75 |
| `RHUMANOS.PCKPESLCATPARTICIPACION.SPPESLPARTICIPACIONACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoParticipacionBL.cs:33 |
| `RHUMANOS.PCKPESLCATPERTINEN.SPPESLPERTINENACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoPertinenciaBL.cs:33 |
| `RHUMANOS.PCKPESLCATPUBLICACION.SPPESLTIPOPUBLICACIONACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoPublicacionBL.cs:33 |
| `RHUMANOS.PCKPESLCATRANGO.SPPESLRANGOACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoRangosBL.cs:33 |
| `RHUMANOS.PCKPESLCATTIPCOMPROM.SPPESLTIPCOMPROMACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoCompromisoBL.cs:37 |
| `RHUMANOS.PCKPESLCATTIPIDEN.SPPESLTIPOIDENACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoIdentificacionBL.cs:75 |
| `RHUMANOS.PCKPESLCATTIPOAGRU.SPPESLTIPOAGRUACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoAgrupacionBL.cs:33 |
| `RHUMANOS.PCKPESLCATTIPOEXPE.SPPESLTIPOEXPEACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoExperienciaBL.cs:34 |
| `RHUMANOS.PCKPESLCATTIPOINVES.SPPESLTIPOINVESACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoInvestigaBL.cs:33 |
| `RHUMANOS.PCKPESLCATTIPOTEL.SPPESLTIPOTELACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoTelefonoBL.cs:32 |
| `RHUMANOS.PCKPESLDETPLAZA.SPPESLDIASFALTANTESPLAZAS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Expediente\PlazaDetalleBL.cs:196 |
| `RHUMANOS.PCKPESLDETPLAZA.SPPESLRASTREOFUNCPLAZA` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Expediente\PlazaDetalleBL.cs:168 |
| `RHUMANOS.PCKPESLDETPLAZA.SPPESLVERIFICAFUNCASING` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Expediente\PlazaDetalleBL.cs:100 |
| `RHUMANOS.PCKPESLDETPLAZA.SPPESLVERIFICARESOLUCION` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Expediente\PlazaDetalleBL.cs:79 |
| `RHUMANOS.PCKPESLDETPLAZA.SPPESLVERIFPLAZMOV` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Expediente\PlazaDetalleBL.cs:121 |
| `RHUMANOS.PCKPESLFUNCIONARIOS.SPPEFUNCIPORDEPENDENCIAS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\BusquedaBL.cs:34 |
| `RHUMANOS.PCKPESLINSTITUCIONES.SPPESLINSTITUCIONESPORIDS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\InstitucionBL.cs:53 |
| `RHUMANOS.PCKPESLINSTITUCIONES.SPPESLINSTITUCIONESPORTIPO` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\InstitucionBL.cs:45 |
| `RHUMANOS.PCKPESLMAEEXPACADEMICA.SPPESLEXPACADEMACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoExperienciaAcademicaBL.cs:61 |
| `RHUMANOS.PCKPESLMAEMOVIM.SPPESLMOVIMACTIVOS` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoMovimientoBL.cs:22 |
| `RHUMANOS.PCKPESLMAEMOVIM.SPPESLMOVIMACTIVOSPORPLAZA` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoMovimientoBL.cs:33 |
| `RHUMANOS.PCKPESLMAEPERIODO.SPPESLENPERIODOACTIVO` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\PeriodoBL.cs:50 |
| `RHUMANOS.PCKPESLTIPDOCUMENTOS.SPPESLTIPOSDOCUMENTOSXMOSTRAR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoDocumentoBL.cs:37 |
| `RHUMANOS.PCKPESLTIPJORN.ALLTIPJORDESCUR` | Paquete / procedimiento Oracle | General | .\UNED.PersonalAcademico.BL\Catalogos\TipoJornadaBL.cs:31 |
| `RHUMANOS.PCKPESLTIPJORN.ALLTIPOSJORNADATIEMPOSCUR` | Paquete / procedimiento Oracle | Catalogos | .\UNED.PersonalAcademico.BL\Catalogos\TipoJornadaBL.cs:24<br>.\UNED.PersonalAcademico.BL\Catalogos\TipoJornadaBL.cs:38 |
| `RHUMANOS.SPPEPRINC_FUNCIONARIOINFLAB` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.DAL\Expediente\InformacionLaboralDAL.cs:32 |
| `RHUMANOS.SPPEUPACTLZPLAZA` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.BL\Expediente\PlazaDetalleBL.cs:201 |
| `RHUMANOS.SPPEUPCAMBIARESTADOSDOCS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.BL\Expediente\DocumentosDigitalesBL.cs:133 |
| `RHUMANOS.SPPEUPCAMBIARESTADOXDOC` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.BL\Expediente\DocumentosDigitalesBL.cs:197 |
| `RHUMANOS.SPPEUPCAMBIARESTDOCVARIOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.BL\Expediente\DocumentosVariosBL.cs:92 |
| `SPPEDLCONDUCAC` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEConduPermiso.cs:96 |
| `SPPEDLCORREOFUNCI` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAECorreos.cs:81 |
| `SPPEDLDETORGANIZAC` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\OrganizacionApiModel.cs:46 |
| `SPPEDLDETPREMIOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETPremios.cs:74 |
| `SPPEDLDETPRODACAD` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProdAcadApiModel.cs:50 |
| `SPPEDLDOCUMENTO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEDocVarios.cs:65 |
| `SPPEDLESTADCIV` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstadCiv.cs:59 |
| `SPPEDLEXPACADEMICA` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEExperienciaAcademica.cs:73 |
| `SPPEDLEXPERIENCIALAB` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETExpLaboral.cs:61 |
| `SPPEDLFAMILIAR` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\MaestrosDetalles\TBPEMAEFamiliares.cs:111 |
| `SPPEDLGENERO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECatGenero.cs:58 |
| `SPPEDLMAECAPACITACION` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\CapacitacionApiModel.cs:104 |
| `SPPEDLMAEINFACADEMICA` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoAcademicaApiModel.cs:74 |
| `SPPEDLMAEINFIDIOMAS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoIdiomaApiModel.cs:80 |
| `SPPEDLMAEPAQINFO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\PaqueteInfoApiModel.cs:53 |
| `SPPEDLMAEPROYECTOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProyectosApiModel.cs:72 |
| `SPPEDLPAIS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPais.cs:83 |
| `SPPEDLTELFUNCI` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAETelefonos.cs:111 |
| `SPPEDLTIPOEXPE` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoExpe.cs:75 |
| `SPPEDLTIPOSDOCUMENTOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAETipoDoc.cs:85 |
| `SPPEDLTIPOTEL` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoTelefono.cs:66<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPPMAEMovimiento.cs:68 |
| `SPPEINACTCAPACIT` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEActCapacit.cs:52 |
| `SPPEINADMINFUNCIONARIOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEFuncionarios.cs:144 |
| `SPPEINAGENACRE` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATAgenciaAcreditadoras.cs:61 |
| `SPPEINAREA` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEArea.cs:51 |
| `SPPEINCAPAFUNCIONARIO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\CapacitacionApiModel.cs:92 |
| `SPPEINCATEGSAL` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATCategSal.cs:63 |
| `SPPEINCONDUCAC` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEConduPermiso.cs:77 |
| `SPPEINCORREOFUNCI` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAECorreos.cs:62 |
| `SPPEINDETORGANIZAC` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\OrganizacionApiModel.cs:34 |
| `SPPEINDETPLAZAS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Plaza\TBPEDETPlazas.cs:111 |
| `SPPEINDETPREMIOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETPremios.cs:62 |
| `SPPEINDETPRODACAD` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProdAcadApiModel.cs:38 |
| `SPPEINESCFISCAL` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEscudoFiscal.cs:40 |
| `SPPEINESTADCIV` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstadCiv.cs:40 |
| `SPPEINESTADOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstados.cs:61 |
| `SPPEINEVALUACION` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEvaluacion.cs:64 |
| `SPPEINEXPACADEM` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEExperienciaAcademica.cs:54 |
| `SPPEINEXPERIENCIALAB` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETExpLaboral.cs:49 |
| `SPPEINFAMILIAR` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\MaestrosDetalles\TBPEMAEFamiliares.cs:92 |
| `SPPEINGENERO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECatGenero.cs:39 |
| `SPPEINIDENTIFICADOR` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATIdentificador.cs:41 |
| `SPPEININSTITUCION` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEInstitucion.cs:77 |
| `SPPEINMAEINFACADEMICA` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoAcademicaApiModel.cs:62 |
| `SPPEINMAEINFIDIOMAS` | Procedimiento almacenado | Expediente | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoIdiomaApiModel.cs:68<br>.\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoIdiomaApiModel.cs:74 |
| `SPPEINMAEPAQINFO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\PaqueteInfoApiModel.cs:41 |
| `SPPEINMAEPROYECTOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProyectosApiModel.cs:60 |
| `SPPEINNIVEL` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATNivel.cs:66 |
| `SPPEINPAISES` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPais.cs:70 |
| `SPPEINPARENTESCO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATParentesto.cs:39 |
| `SPPEINPARTICIPACION` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATParticipacion.cs:59 |
| `SPPEINPERIODO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEPeriodo.cs:62 |
| `SPPEINPERTINEN` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPertinencia.cs:64 |
| `SPPEINRANGO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATRangos.cs:64 |
| `SPPEINREGPENSI` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEReguladoraPension.cs:43 |
| `SPPEINTELFUNCI` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAETelefonos.cs:92 |
| `SPPEINTIPCOMPROM` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoCompromiso.cs:47 |
| `SPPEINTIPIDEN` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipIden.cs:41 |
| `SPPEINTIPOAGRU` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoAgrupacion.cs:40 |
| `SPPEINTIPOEXPE` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoExpe.cs:56 |
| `SPPEINTIPOINVES` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoInves.cs:40 |
| `SPPEINTIPOPUBLICACION` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoPublicacion.cs:40 |
| `SPPEINTIPOSDOCUMENTOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAETipoDoc.cs:79 |
| `SPPEINTIPOTEL` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoTelefono.cs:47<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPPMAEMovimiento.cs:49 |
| `SPPEPRINCDOCUMENTO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEDocVarios.cs:59 |
| `SPPEUPACTCAPACIT` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEActCapacit.cs:58<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEActCapacit.cs:71 |
| `SPPEUPAGENACRE` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATAgenciaAcreditadoras.cs:67 |
| `SPPEUPAREA` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEArea.cs:57<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEArea.cs:70 |
| `SPPEUPCATEGSAL` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATCategSal.cs:69 |
| `SPPEUPCONDUCAC` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEConduPermiso.cs:83 |
| `SPPEUPCORREOFN` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAECorreos.cs:68 |
| `SPPEUPDETORGANIZAC` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\OrganizacionApiModel.cs:40 |
| `SPPEUPDETPREMIOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETPremios.cs:68 |
| `SPPEUPDETPRODACAD` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProdAcadApiModel.cs:44 |
| `SPPEUPESCFISCAL` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEscudoFiscal.cs:46<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATEscudoFiscal.cs:59 |
| `SPPEUPESTADCIV` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstadCiv.cs:46 |
| `SPPEUPESTADOS` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstados.cs:67<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATEstados.cs:80 |
| `SPPEUPEVALUACION` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATEvaluacion.cs:70<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATEvaluacion.cs:83 |
| `SPPEUPEXPACADEM` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEExperienciaAcademica.cs:60 |
| `SPPEUPEXPERIENCIALAB` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEDETExpLaboral.cs:55 |
| `SPPEUPFAMILIAR` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\MaestrosDetalles\TBPEMAEFamiliares.cs:98 |
| `SPPEUPFUNCIONARIOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEFuncionarios.cs:150 |
| `SPPEUPFUNCIONARIOSFOTO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAEFuncionarios.cs:170 |
| `SPPEUPGENERO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECatGenero.cs:45 |
| `SPPEUPIDENTIFICADOR` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATIdentificador.cs:47<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATIdentificador.cs:60 |
| `SPPEUPINSTITUCION` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEInstitucion.cs:83 |
| `SPPEUPLIBERAPLAZA` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Plaza\LiberarPlazaModel.cs:67 |
| `SPPEUPMAECAPACITACION` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\CapacitacionApiModel.cs:98 |
| `SPPEUPMAEINFACADEMICA` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\InfoAcademicaApiModel.cs:68 |
| `SPPEUPMAEPAQINFO` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\PaqueteInfoApiModel.cs:47 |
| `SPPEUPMAEPROYECTOS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\ApiModels\ProyectosApiModel.cs:66 |
| `SPPEUPNIVEL` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATNivel.cs:72<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATNivel.cs:85 |
| `SPPEUPPAIS` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPais.cs:76 |
| `SPPEUPPARENTESCO` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATParentesto.cs:45<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATParentesto.cs:58 |
| `SPPEUPPARTICIPACION` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATParticipacion.cs:65<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATParticipacion.cs:78 |
| `SPPEUPPERIODO` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEPeriodo.cs:68<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPEMAEPeriodo.cs:81 |
| `SPPEUPPERTINEN` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATPertinencia.cs:70<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATPertinencia.cs:83 |
| `SPPEUPRANGO` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATRangos.cs:70<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATRangos.cs:77 |
| `SPPEUPREGPENSI` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPEReguladoraPension.cs:49<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPEReguladoraPension.cs:62 |
| `SPPEUPTELFUNCI` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Expediente\TBPEMAETelefonos.cs:98 |
| `SPPEUPTIPCOMPROM` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoCompromiso.cs:53 |
| `SPPEUPTIPIDEN` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipIden.cs:47 |
| `SPPEUPTIPOAGRU` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoAgrupacion.cs:46<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoAgrupacion.cs:59 |
| `SPPEUPTIPOEXPE` | Procedimiento almacenado | General | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoExpe.cs:62 |
| `SPPEUPTIPOINVES` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoInves.cs:46<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoInves.cs:59 |
| `SPPEUPTIPOPUBLICACION` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoPublicacion.cs:46<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPECATipoPublicacion.cs:59 |
| `SPPEUPTIPOTEL` | Procedimiento almacenado | Catalogos | .\UNED.PersonalAcademico.EL\Catalogos\TBPECATTipoTelefono.cs:53<br>.\UNED.PersonalAcademico.EL\Catalogos\TBPPMAEMovimiento.cs:55 |
| `TBPECATCategSal` | Tabla / tipo de dato referenciado | API / Jobs | .\UNED.PersonalAcademico.API\Controllers\V1\CategoriaSalarialController.cs:17<br>.\UNED.PersonalAcademico.API\Controllers\V1\CategoriaSalarialController.cs:33<br>.\UNED.PersonalAcademico.API\Controllers\V1\CategoriaSalarialController.cs:69 |
| `TBPEDETORGANIZAC_INFO` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\OrganizacionBL.cs:55<br>.\UNED.PersonalAcademico.BL\Expediente\OrganizacionBL.cs:62 |
| `TBPEDETPREMIOS_INFO` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\PremiosBL.cs:55<br>.\UNED.PersonalAcademico.BL\Expediente\PremiosBL.cs:62 |
| `TBPEDETPRODACAD_INFO` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\ProdAcademicaBL.cs:55<br>.\UNED.PersonalAcademico.BL\Expediente\ProdAcademicaBL.cs:62 |
| `TBPEMAECAPACITACION_INFO` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\CapacitacionBL.cs:55<br>.\UNED.PersonalAcademico.BL\Expediente\CapacitacionBL.cs:62 |
| `TBPEMAEEXPERIENCIALABORAL` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\ExperienciaLaboralBL.cs:57<br>.\UNED.PersonalAcademico.BL\Expediente\ExperienciaLaboralBL.cs:64 |
| `TBPEMAEINFACADEMICA_ALL` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\InformacionAcademicaBL.cs:57<br>.\UNED.PersonalAcademico.BL\Expediente\InformacionAcademicaBL.cs:64 |
| `TBPEMAEINFIDIOMAS_INFO` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\InfomacionIdiomaBL.cs:55<br>.\UNED.PersonalAcademico.BL\Expediente\InfomacionIdiomaBL.cs:62 |
| `TBPEMAEPAQINFO_ALL` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\PaqueteInformaticoBL.cs:57<br>.\UNED.PersonalAcademico.BL\Expediente\PaqueteInformaticoBL.cs:64 |
| `TBPEMAEPROYECTO_INFO` | Tabla / tipo de dato referenciado | Expediente | .\UNED.PersonalAcademico.BL\Expediente\ProyectosBL.cs:55<br>.\UNED.PersonalAcademico.BL\Expediente\ProyectosBL.cs:62 |
| `TBPEMAETIPDOC_all` | Tabla / tipo de dato referenciado | General | .\UNED.PersonalAcademico.DAL\Catalogos\TipoDocumentoDAL.cs:34 |

## Observaciones

- La aplicacion ejecuta la mayoria de operaciones mediante procedimientos almacenados usando `OracleManagerHelper` y `AmiUnedData`.
- Varias clases definen `Schema`, `GetProcedureName`, `RecordProcedureName`, `PaginationProcedureName`, `InsertProcedureName`, `UpdateProcedureName`, `DeleteProcedureName`, `InsertAndQueryProcedureName` o `UpdateAndQueryProcedureName`.
- Los objetos con prefijo `PCKPESL...` corresponden a paquetes Oracle y procedimientos internos del paquete.
- Los objetos con prefijo `SPPE...` corresponden a procedimientos almacenados invocados directamente o configurados desde los modelos.
- Los objetos `TBPE...` y `TBPP...` se reportan como tablas o tipos inferidos a partir de modelos y referencias en codigo; se recomienda confirmar si son tablas fisicas, vistas, sinonimos o tipos PL/SQL con el DBA.
- No se incluyen dependencias internas que puedan existir dentro de cada paquete Oracle, porque esas dependencias no estan visibles en el codigo fuente C#.
