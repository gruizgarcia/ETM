# ETM renta — Base de desarrollo v0.1

## Estado y alcance exacto

Repositorio inicial preparado para importación mediante abapGit offline. No es una orden CTS ni una solución ETM terminada. No se ha importado ni compilado en SAP desde este entorno.

Incluye 27 objetos de repositorio: 6 reportes PROG, 16 clases de soporte/aplicación, 1 clase de excepción y 4 interfaces INTF. Cada objeto tiene fuente ABAP y metadata XML. Incluye pruebas ABAP Unit para el cálculo de ejemplo.

ZSD_RENTA_LIQUIDAR ejecuta únicamente el ejemplo de octubre de 2026: apertura 0, envíos 500 el día 1 y 200 el día 10, devolución 300 el día 20. Salida y retorno cuentan. Resultado: 16,600 pieza-día, 83,000 MXN antes de impuestos y 400 piezas al cierre. El resto de reportes indica “no implementado”. Las clases de persistencia, autorización y adaptadores elevan error por diseño; no deben usarse como implementaciones productivas.

Las interfaces son contratos mínimos iniciales: se ampliarán para cabecera/posiciones, lotes, tarifas, referencias y mensajes según el diseño definitivo. No equivalen todavía a todas las firmas del documento técnico.

## Todavía no incluido

13 tablas y estructuras DDIC; tipos tabla DDIC; objetos de bloqueo generados; transacciones ZRT_*; objetos de autorización SU20/SU21 y roles PFCG; SU24; objeto SLG0 y clase SE91; configuración funcional; jobs; BAPIs/APIs activadas; integración SD, FI o CFDI; carga productiva de archivos. No se simula su creación mediante archivos JSON. OBJETOS_PENDIENTES.md detalla el siguiente lote.

## Plataforma

Base pensada para ABAP clásico en S/4HANA On-Premise o Private Edition, con sintaxis moderna y SAP GUI/SALV. No es ABAP Cloud ni aplicación Fiori. Confirmar versión y políticas antes de importar.

## Importación inicial en DEV

1. Guardar ETM_ABAPGIT_BASE_v0_1.zip sin descomprimir ni volver a comprimir.
2. Entrar en SAP DEV con un desarrollador autorizado. La guía oficial recomienda idioma EN para el flujo offline; los textos del código siguen en español.
3. Crear o elegir un paquete Z transportable nuevo y exclusivo, por ejemplo ZSD_RENTA_BASE, con capa y sistema destino definidos con Basis. No usar $TMP si se desea transporte.
4. Abrir abapGit y elegir New Offline. Nombre: ETM Renta Base v0.1. Seleccionar ese paquete.
5. Import ZIP: elegir el ZIP de repositorio entregado, no otro ZIP de documentos.
6. Revisar el diff y la lista de objetos. Detenerse ante conflictos o nombres existentes; no sobrescribir desarrollos previos sin evaluación.
7. Pull ZIP. Asignar una orden Workbench DEV y la tarea del desarrollador cuando abapGit lo solicite. No inventar número de orden fuera de SAP.
8. Revisar el log de deserialización y activación. Comprobar que los 27 objetos estén activos. Resolver inconsistencias antes de continuar; una comprobación XML local no sustituye el compilador SAP.
9. Si la activación masiva deja dependencias pendientes: excepción, interfaces (MASTER primero), clases, reportes y pruebas; reactivar todo desde herramientas SAP.
10. Ejecutar controles de sintaxis y ATC aplicables. Ejecutar ABAP Unit en ZCL_RENTA_CALC.

## Pruebas en DEV

- SE38: ZSD_RENTA_LIQUIDAR; dejar P_DEMO marcado; F8. Comprobar 16,600 / 83,000 / 400 y detalle ALV de 31 días.
- Retorno del 20 de octubre: cobrables 700, saldo final 400; el día 21 cobra 400.
- Sin P_DEMO: debe indicar modo real no implementado y terminar.
- Los otros cinco reportes deben informar operación no implementada sin modificar datos.
- Las pruebas ABAP Unit tienen que ejecutarse en SAP; aquí solo se verificaron metadata, consistencia del ZIP y matemáticas independientes.

## Orden de transporte

Verificar lista de objetos en SE09/SE10. Para esta base se esperan 6 PROG y 21 objetos OO (17 CLAS y 4 INTF), además de los objetos de paquete y subobjetos registrados automáticamente por SAP. El conteo visible de entradas puede variar por esos subobjetos. Liberar solo después de aceptación técnica. Basis importa en QA por STMS según procedimiento. Esta base no debe liberarse a productivo como si fuera la solución terminada.

Roles, seguridad y customizing se añadirán mediante sus procedimientos y transportes correspondientes cuando estén implementados y validados. No se crean usuarios ni asignaciones personales con este ZIP.

## Reversión de instalación de la base

Si aparecen conflictos antes del Pull, cancelar sin sobrescribir. Si la importación ya creó objetos, revisar impacto y dependencias con el desarrollador. No borrar un paquete entero ni desinstalar abapGit automáticamente: el procedimiento correcto depende de qué objetos se hayan creado y de sus órdenes/usuarios.

## Fuentes de formato y procedimiento

- https://docs.abapgit.org/user-guide/projects/offline/import-zip.html
- https://docs.abapgit.org/user-guide/reference/packages.html
- https://docs.abapgit.org/user-guide/reference/supported.html
- Metadatos CLAS/PROG basados en el formato abapGit público. No se afirma certificación ni prueba de deserialización en el sistema destino.
