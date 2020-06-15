# Ejemplos de consultas

Se ha realizado un proceso de evaluación a través de unos datos de prueba generados, para comprobar la cobertura del vocabulario con respecto a los requisitos planteados.

Para ello se ha realizado la transformación a partir de algunos datos publicados por los ayuntamientos de Madrid y Zaragoza en sus portales de datos abiertos.

Los datos transformados y anotados con este vocabulario se han cargado en un Virtuoso endpoint disponible en: http://ciudadesabiertas.linkeddata.es/sparql.

A continuación, se presentan varias consultas que se han ejecutado en este endpoint y que han permitido evaluar el vocabulario de acuerdo con los requisitos que se plantearon en la actividad de especificación. 

El grafo de trabajo, con datos de prueba y skos, está en http://ciudadesabiertas.linkeddata.es/datosabiertos/grafo/transporte/trafico/observaciones-incidencias

## Consulta 1: Lista con la ubicación, el tipo y el nombre de los equipos (todos)

```
PREFIX estraf:<http://vocab.ciudadesabiertas.es/def/transporte/trafico#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
PREFIX skos-tipo-equipo:<http://vocab.linkeddata.es/datosabiertos/kos/transporte/trafico/tipo-equipo-trafico>
PREFIX sosa:<http://www.w3.org/ns/sosa/>
PREFIX dcterms:<http://purl.org/dc/terms/>
PREFIX sf:<http://www.opengis.net/ont/sf#>


SELECT  ?descripcion ?labelTipoEquipo ?ubicacionLat ?ubicacionLong  
WHERE { ?equipo a sosa:Sensor .
        ?equipo estraf:tipoEquipoTrafico ?tipoEquipo .
        ?tipoEquipo skos:prefLabel ?labelTipoEquipo .
        ?equipo dcterms:description ?descripcion .
        ?equipo estraf:ubicacionEquipoTrafico ?punto .
        ?punto a sf:Point .
        ?punto geo:lat ?ubicacionLat .
       ?punto geo:long ?ubicacionLong
}
```

## Consulta 2: Lista de incidencias de tráfico registradas en una fecha dada, por ejemplo, 01-04-2020

```
PREFIX estraf:<http://vocab.ciudadesabiertas.es/def/transporte/trafico#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
PREFIX skos-tipo-incidencia:<http://vocab.linkeddata.es/datosabiertos/kos/transporte/trafico/tipo-incidencia/>
PREFIX schema:<http://schema.org/>
PREFIX dct:<http://purl.org/dc/terms/>

SELECT  ?descripcion  ?labelTipoIncidencia  
WHERE { ?incidencia a estraf:IncidenciaPlanificada .
        ?incidencia estraf:tipoIncidencia ?tipoIncidencia .
        ?incidencia schema:startDate ?fechaIncidencia .
        ?tipoIncidencia skos:prefLabel ?labelTipoIncidencia .
        ?incidencia dct:description ?descripcion .
        FILTER (?fechaIncidencia = "2020-04-01T00:00:00"^^xsd:dateTime) 
}
```

## Consulta 3. ¿Cuáles son los valores de medición, en tiempo real, obtenidos por los dispositivos del ayuntamiento?

```
PREFIX estraf:<http://vocab.ciudadesabiertas.es/def/transporte/trafico#>
PREFIX time:<http://www.w3.org/2006/time#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
PREFIX skos-tipo-equipo-trafico:<http://vocab.linkeddata.es/datosabiertos/kos/transporte/trafico/tipo-equipo-trafico/>
PREFIX schema:<http://schema.org/>
PREFIX sosa:<http://www.w3.org/ns/sosa/>
PREFIX dcterms:<http://purl.org/dc/terms/>

SELECT ?id ?descripcion ?propiedad ?fechaHora ?resultado WHERE {
	?dispositivo a sosa:Sensor .
	?dispositivo dcterms:identifier ?id .
	?dispositivo dcterms:description ?descripcion .
	?observacion sosa:madeBySensor ?dispositivo .
	?observacion sosa:observedProperty ?propiedad .
	?observacion sosa:resultTime ?fechaHora .
	?observacion sosa:hasSimpleResult ?resultado
}
```
## Consulta 4. ¿Cuáles son los valores de carga en tiempo real registrados en el ayuntamiento?

```
PREFIX estraf:<http://vocab.ciudadesabiertas.es/def/transporte/trafico#>
PREFIX time:<http://www.w3.org/2006/time#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
PREFIX skos-tipo-equipo-trafico:<http://vocab.linkeddata.es/datosabiertos/kos/transporte/trafico/tipo-equipo-trafico/>
PREFIX schema:<http://schema.org/>
PREFIX sosa:<http://www.w3.org/ns/sosa/>
PREFIX dcterms:<http://purl.org/dc/terms/>

SELECT ?id ?descripcion  ?fechaHora ?resultado WHERE {
	?dispositivo a sosa:Sensor .
	?dispositivo dcterms:identifier ?id .
	?dispositivo dcterms:description ?descripcion .
	?observacion sosa:madeBySensor ?dispositivo .
	?observacion sosa:observedProperty <http://vocab.ciudadesabiertas.es/recurso/transporte/trafico/propiedadmediciontrafico/carga> .
	?observacion sosa:resultTime ?fechaHora .
	?observacion sosa:hasSimpleResult ?resultado
}

```

## Consulta 5. ¿Cuáles son los valores de intensidad en tiempo real registrados las estaciones que se encuentran en la calle sepúlveda?

```
PREFIX geosparql:<http://www.opengis.net/spec/geosparql>
PREFIX estraf:<http://vocab.ciudadesabiertas.es/def/transporte/trafico#>
PREFIX estraf-recurso:<http://vocab.ciudadesabiertas.es/recurso/transporte/trafico/>
PREFIX time:<http://www.w3.org/2006/time#>
PREFIX skos:<http://www.w3.org/2004/02/skos/core#>
PREFIX xsd:<http://www.w3.org/2001/XMLSchema#>
PREFIX skos-tipo-equipo-trafico:<http://vocab.linkeddata.es/datosabiertos/kos/transporte/trafico/tipo-equipo-trafico/>
PREFIX schema:<http://schema.org/>
PREFIX sosa:<http://www.w3.org/ns/sosa/>
PREFIX dcterms:<http://purl.org/dc/terms/>

SELECT ?id ?descripcion  ?fechaHora ?resultado WHERE {
	?dispositivo a sosa:Sensor .
	?dispositivo dcterms:identifier ?id .
	?dispositivo dcterms:description ?descripcion .
	?observacion sosa:madeBySensor ?dispositivo .
	?observacion sosa:observedProperty <http://vocab.ciudadesabiertas.es/recurso/transporte/trafico/propiedadmediciontrafico/intensidad> .
	?observacion sosa:resultTime ?fechaHora .
	?observacion sosa:hasSimpleResult ?resultado
	FILTER (CONTAINS(?descripcion,'SEPULVEDA'))
}
```
