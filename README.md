# INSPIRE Stored Queries
## Aanmaken in Geoserver

Een manier is:

* maak een XML bestand aan (bijvoorbeeld: ```gebiedsindelingen.xml```) met de code van de stored query. Voorbeeld, voor een WFS die deze 2 featuretypes ```cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_provincie_2012_gegeneraliseerd``` en ```cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_gemeente_2012_gegeneraliseerd ``` aanbiedt en die in 1 dataset terug moeten komen is dat:
```
<?xml version="1.0" encoding="UTF-8"?>
<wfs:StoredQueryDescription xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:gml="http://www.opengis.net/gml/3.2" id="urn:bgi:def:query:OGC-WFS::InspireStoredQueryDemo">
      <wfs:Title xml:lang="en">GetDataset gebiedsindelingen demo</wfs:Title>
      <wfs:Abstract xml:lang="en">Example INSPIRE Stored Query dataset returning 2 featuretypes</wfs:Abstract>
      <wfs:Parameter name="CRS" type="xsd:string"/>
      <wfs:Parameter name="DataSetIdCode" type="xsd:string"/>
      <wfs:Parameter name="DataSetIdNamespace" type="xsd:string"/>
      <wfs:Parameter name="Language" type="xsd:string"/>
      <wfs:QueryExpressionText isPrivate="false" language="urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression" returnFeatureTypes="cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_provincie_2012_gegeneraliseerd">
          <wfs:Query wfs:srsName="${CRS}" wfs:typeNames="cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_provincie_2012_gegeneraliseerd"/>
      </wfs:QueryExpressionText>
      <wfs:QueryExpressionText isPrivate="false" language="urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression" returnFeatureTypes="cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_gemeente_2012_gegeneraliseerd">
          <wfs:Query wfs:srsName="${CRS}" wfs:typeNames="cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_gemeente_2012_gegeneraliseerd"/>
      </wfs:QueryExpressionText>
</wfs:StoredQueryDescription>
```
De StoredQuery bevat dus per featuretype een element ```wfs:QueryExpressionText```. Voor een INSPIRE compliant query, zal elk featuretype van de service een element ```wfs:QueryExpressionText``` moeten krijgen.

* plaats het bestand ```gebiedsindelingen.xml``` in de Geoserver data dir in de directory:
```
{GEOSERVER_DATA_DIR}/wfs/stored_queries
```
Als deze directory nog niet bestaat, maak die dan gewoon eerst aan.

* doe een DescribeStoredQueries-request, Geoserver gaat dan de XML bestanden inlezen en stored queries aanmaken. Bijvoorbeeld:
[http://localhost:8080/geoserver/wfs?request=DescribeStoredQueries&service=WFS](http://localhost:8080/geoserver/wfs?request=DescribeStoredQueries&service=WFS) (let op de url met in dit geval 'geoserver').

* het kan zijn dat bij aanmaken van de stored queries Geoserver een kopie maakt van het bestand en die de bestandsnaam geeft met (opgeschoond) de id van de StoredQuery en extensie xml. Voorbeeld: ```id="urn:bgi:def:query:OGC-WFS::InspireStoredQueryDemo"``` wordt ```urnbgidefqueryOGCWFSInspireStoredQueryDemo.xml```. Na aanmaken van dat bestand, kan je het orginele bestand (```gebiedsindelingen.xml```) uit de directory ```{GEOSERVER_DATA_DIR}/wfs/stored_queries``` verwijderen.

### Tips
1. de voorbeelden hier negeren een aantal parameters in de query. Alleen CRS wordt gebruikt, want dat moet kunnen verschillen voor de service. INSPIRE vereist echter wel dat de parameters DataSetIdCode, DataSetIdNamespace en Language opgegeven kunnen worden. Vandaar dat ze wel gedefinieerd worden.
1. maak per featuretype van een service een ```wfs:QueryExpressionText``` (met in attribuut ```returnFeatureTypes``` de featuretypename) met een ```wfs:Query```(met ook in attribuut ```wfs:typeNames``` de featuretypename).
1. kies het ID van de query naar eigen keuze. INSPIRE schrijft niet voor hoe je het ID moet kiezen. In het voorbeeld heb ik een eigen URN gekozen (```urn:bgi:<etc>```)

## Voorbeelden

Voor 1 featuretype:
```
<?xml version="1.0" encoding="UTF-8"?>
<wfs:StoredQueryDescription xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:gml="http://www.opengis.net/gml/3.2" id="urn:bgi:def:query:OGC-WFS::InspireStoredQueryNatura2000">
      <wfs:Title xml:lang="en">GetDataset natura2000 demo</wfs:Title>
      <wfs:Abstract xml:lang="en">Example INSPIRE Stored Query dataset returning 1 featuretype</wfs:Abstract>
      <wfs:Parameter name="CRS" type="xsd:string"/>
      <wfs:Parameter name="DataSetIdCode" type="xsd:string"/>
      <wfs:Parameter name="DataSetIdNamespace" type="xsd:string"/>
      <wfs:Parameter name="Language" type="xsd:string"/>
      <wfs:QueryExpressionText isPrivate="false" language="urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression" returnFeatureTypes="natura2000:natura2000">
          <wfs:Query wfs:srsName="${CRS}" wfs:typeNames="natura2000:natura2000"/>
      </wfs:QueryExpressionText>
</wfs:StoredQueryDescription>
```


Voor een service met 2 featuretypes:
```
<?xml version="1.0" encoding="UTF-8"?>
<wfs:StoredQueryDescription xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:fes="http://www.opengis.net/fes/2.0" xmlns:wfs="http://www.opengis.net/wfs/2.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:gml="http://www.opengis.net/gml/3.2" id="urn:bgi:def:query:OGC-WFS::InspireStoredQueryDemo">
      <wfs:Title xml:lang="en">GetDataset gebiedsindelingen demo</wfs:Title>
      <wfs:Abstract xml:lang="en">Example INSPIRE Stored Query dataset returning 2 featuretypes</wfs:Abstract>
      <wfs:Parameter name="CRS" type="xsd:string"/>
      <wfs:Parameter name="DataSetIdCode" type="xsd:string"/>
      <wfs:Parameter name="DataSetIdNamespace" type="xsd:string"/>
      <wfs:Parameter name="Language" type="xsd:string"/>
      <wfs:QueryExpressionText isPrivate="false" language="urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression" returnFeatureTypes="cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_provincie_2012_gegeneraliseerd">
          <wfs:Query wfs:srsName="${CRS}" wfs:typeNames="cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_provincie_2012_gegeneraliseerd"/>
      </wfs:QueryExpressionText>
      <wfs:QueryExpressionText isPrivate="false" language="urn:ogc:def:queryLanguage:OGC-WFS::WFS_QueryExpression" returnFeatureTypes="cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_gemeente_2012_gegeneraliseerd">
          <wfs:Query wfs:srsName="${CRS}" wfs:typeNames="cbsgebiedsindelingen:cbsgebiedsindelingen_cbs_gemeente_2012_gegeneraliseerd"/>
      </wfs:QueryExpressionText>
</wfs:StoredQueryDescription>
```


## Gebruiken

DescribeStoredQueries:
[http://localhost:8080/geoserver/wfs?request=DescribeStoredQueries&service=WFS](http://localhost:8080/geoserver/wfs?request=DescribeStoredQueries&service=WFS)

GetFeature request met Stored Query (dus geen typenames in het request):

Voor: InspireStoredQueryNatura2000:
[http://localhost:8080/geoserver/wfs?service=WFS&request=GetFeature&version=2.0.0&StoredQuery_ID=urn:bgi:def:query:OGC-WFS::InspireStoredQueryNatura2000&CRS=EPSG:4258&Language=en](http://localhost:8080/geoserver/wfs?service=WFS&request=GetFeature&version=2.0.0&StoredQuery_ID=urn:bgi:def:query:OGC-WFS::InspireStoredQueryNatura2000&CRS=EPSG:4258)

Voor InspireStoredQueryDemo:
[http://localhost:8080/geoserver/wfs?service=WFS&request=GetFeature&version=2.0.0&StoredQuery_ID=urn:bgi:def:query:OGC-WFS::InspireStoredQueryDemo&CRS=EPSG:4258&Language=en](http://localhost:8080/geoserver/wfs?service=WFS&request=GetFeature&version=2.0.0&StoredQuery_ID=urn:bgi:def:query:OGC-WFS::InspireStoredQueryDemo&CRS=EPSG:4258&Language=en)
Let op: Langauge wordt genegeerd.
