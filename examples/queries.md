# Ejemplos de consultas

Se ha realizado un proceso de evaluación a través de unos datos de prueba generados, para comprobar la cobertura del vocabulario con respecto a los requisitos planteados.

Para ello se ha realizado la transformación a partir de algunos datos publicados por los ayuntamientos de Madrid y Zaragoza en sus portales de datos abiertos.

Los datos transformados y anotados con este vocabulario se han cargado en un Virtuoso endpoint disponible en: http://ciudadesabiertas.linkeddata.es/sparql.

A continuación, se presentan varias consultas que se han ejecutado en este endpoint y que han permitido evaluar el vocabulario de acuerdo con los requisitos que se plantearon en la actividad de especificación. 

## Consulta 1: ¿Cuál es la ubicación de los equipos de control del tráfico del ayuntamiento?

Asumimos que por Equipos de Control del Tráfico del Ayuntamiento nos referimos a todos los Equipos de Tráfico, que incluyen equipos de control y dispositivos de medición.

```
PREFIX geosparql:<http://www.opengis.net/doc/IS/geosparql/1.0#>
PREFIX estraf:<http://vocab.ciudadesabiertas.es/recurso/transporte/trafico#>
PREFIX time:<http://www.w3.org/2006/time#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
PREFIX skos-tipo-equipo:<http://vocab.linkeddata.es/datosabiertos/kos/transporte/trafico/tipo-equipo-trafico/>

SELECT  ?descripcion  ?ubicacionLat ?ubicacionLong ?tipoEquipo
WHERE { ?equipo a estraf:EquipoTrafico .
        ?equipo estraf:tipoEquipoTrafico ?tipoEquipo .
        ?equipo estraf:descripcion ?descripcion .
        ?equipo estraf:ubicacionEquipo ?punto .
        ?punto a geosparql:Point .
        ?punto geosparql:lat ?ubicacionLat .
        ?punto geosparql:long ?ubicacionLong
        #FILTER (lang(?labelTipoEquipo)="es")
}
```
## Consulta 2: ¿Cuáles son las incidencias de tráfico registradas en el ayuntamiento en fecha 2020-03-08?
```
PREFIX geosparql:<http://www.opengis.net/spec/geosparql>
PREFIX estraf:<http://vocab.ciudadesabiertas.es/def/transporte/trafico#>
PREFIX time:<http://www.w3.org/2006/time#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
PREFIX inc:<http://vocab.linkeddata.es/datosabiertos/kos/transporte/trafico/tipo-incidencia/>
PREFIX schema:<http://schema.org/>

SELECT  ?descripcion  
WHERE { ?incidencia a estraf:Incidencia .
        ?incidencia estraf:tipoIncidencia ?tipoIncidencia .
        ?incidencia schema:startDate ?fechaIncidencia .
	  ?incidencia estraf:descripcion ?descripcion .
        FILTER (?fechaIncidencia = "2020-03-08T15:00:00+02:00"^^xsd:dateTime) 

}
```

## Consulta 3: ¿Cuáles son las incidencias de tráfico registradas en el ayuntamiento desde 2020-03-09?

```
PREFIX geosparql:<http://www.opengis.net/spec/geosparql>
PREFIX estraf:<http://vocab.ciudadesabiertas.es/def/transporte/trafico#>
PREFIX time:<http://www.w3.org/2006/time#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
PREFIX inc:<http://vocab.linkeddata.es/datosabiertos/kos/transporte/trafico/tipo-incidencia/>
PREFIX schema:<http://schema.org/>

SELECT  ?descripcion  
WHERE { ?incidencia a estraf:Incidencia .
        ?incidencia estraf:tipoIncidencia ?tipoIncidencia .
        ?incidencia schema:startDate ?fechaIncidencia .
       ?incidencia estraf:descripcion ?descripcion .
        FILTER (?fechaIncidencia > "2020-03-09T00:00:00+02:00"^^xsd:dateTime) 

}
```
