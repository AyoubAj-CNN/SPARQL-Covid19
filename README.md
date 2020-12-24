# SPARQL-Covid19
Analyzing covid19 data using Sparql

This project will be discussed in the second part of this homework.

## TP1 Linked Data - Ajarra Ayoub
----------------------
# Première partie : 14 requêtes sur DBPedia
----------------------
1. Query 1: Afficher les URLs des lyonnais (i.e. personnes nées à Lyon)

        PREFIX foaf: <http://xmlns.com/foaf/0.1/>
        PREFIX dbo: <http://dbpedia.org/ontology/>
        PREFIX dbr: <http://dbpedia.org/resource/>
        SELECT ?p ?n where{
          ?p a              dbo:Person;
             dbo:birthPlace dbr:Lyon;
             foaf:name ?n.
             FILTER(!contains(?n,","))
        }

	
2. Query 2: Afficher les URLs et les noms (foaf:name) des lyonnais

        PREFIX foaf: <http://xmlns.com/foaf/0.1/>
        PREFIX dbo: <http://dbpedia.org/ontology/>
        PREFIX dbr: <http://dbpedia.org/resource/>
        SELECT ?p ?n ?d where{
          ?p a              dbo:Person;
             dbo:birthPlace dbr:Lyon;
             foaf:name ?n;
             dbo:birthDate ?d.
             FILTER(!contains(?n,","))
        }

3. Query 3: Afficher les noms des lyonnais ne contenant pas de virgule (fonction contains)

        PREFIX foaf: <http://xmlns.com/foaf/0.1/>
        PREFIX dbo: <http://dbpedia.org/ontology/>
        PREFIX dbr: <http://dbpedia.org/resource/>
        SELECT ?p ?n ?d ?b where{
          ?p dbo:birthPlace dbr:Lyon;
             foaf:name ?n;
             dbo:birthDate ?d.
             FILTER(!contains(?n,","))
        }

4. Query 4: Afficher les noms (sans virgule) et dates de naissance des lyonnais


        PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
        PREFIX foaf: <http://xmlns.com/foaf/0.1/>
        PREFIX dbo: <http://dbpedia.org/ontology/>
        PREFIX dbr: <http://dbpedia.org/resource/>
        SELECT ?p ?n ?d ?b where{
          ?p dbo:birthPlace dbr:Lyon;
             foaf:name ?n;
             dbo:birthDate ?d.
             FILTER(!contains(?n,","))
          	 FILTER(?d >= "1900"^^xsd:gYear)
        }


5. Query 5 : Afficher les noms et dates de naissance des lyonnais nés après 1900


        PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
        PREFIX foaf: <http://xmlns.com/foaf/0.1/>
        PREFIX dbo: <http://dbpedia.org/ontology/>
        PREFIX dbr: <http://dbpedia.org/resource/>
        SELECT ?p ?n ?d ?b where{
          ?p dbo:birthPlace dbr:Lyon;
             foaf:name ?n;
             dbo:birthDate ?d.
             FILTER(!contains(?n,","))
          	 FILTER(?d >= "1900"^^xsd:gYear)
          OPTIONAL{?p dbo:deathDate ?death}
        }


6. Query 6: Afficher les noms et dates de naissance des lyonnais nés après 1900 avec, le cas échéant, leur date de décès

        PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
        PREFIX foaf: <http://xmlns.com/foaf/0.1/>
        PREFIX dbo: <http://dbpedia.org/ontology/>
        PREFIX dbr: <http://dbpedia.org/resource/>
        SELECT ?p ?n   where{
          ?p dbo:birthPlace dbr:Lyon;
             foaf:name ?n;
             dbo:deathPlace	dbr:Lyon.
        }

7. Query 7: Afficher les noms de tous les lyonnais morts à Lyon

        PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
        PREFIX foaf: <http://xmlns.com/foaf/0.1/>
        PREFIX dbo: <http://dbpedia.org/ontology/>
        PREFIX dbr: <http://dbpedia.org/resource/>
        SELECT ?p ?n ?x where{
          ?p dbo:birthPlace dbr:Lyon;
             foaf:name ?n;
             dbo:deathPlace ?x.
        }

8. Query 8: Afficher les noms de tous les lyonnais morts hors de France

    	PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
    	PREFIX foaf: <http://xmlns.com/foaf/0.1/>
    	PREFIX dbo: <http://dbpedia.org/ontology/>
    	PREFIX dbr: <http://dbpedia.org/resource/>
    	SELECT ?n {
    		?p dbo:birthPlace dbr:Lyon;
    		foaf:name ?n;
    		dbo:deathPlace ?dplace.
    		?dplace dbo:country ?country.
    	  FILTER (?country!=dbr:France)
    	}

9. Query 9: Afficher toutes les villes françaises dont le maire est natif

    	PREFIX dbr: <http://dbpedia.org/resource/>
    	PREFIX dbo: <http://dbpedia.org/ontology/>
    	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    	PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    	SELECT ?t {
    	  ?town dbo:country dbr:France.
    	  ?town dbo:mayor ?m.
    	  ?mayor dbo:birthPlace ?t.
    	}

10. Query 10 : Afficher tous les maires français (i.e. d’une ville de France) nés hors de France

    	PREFIX dbr: <http://dbpedia.org/resource/>
    	PREFIX dbo: <http://dbpedia.org/ontology/>
    	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    	PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    	SELECT ?m{
    	  ?town dbo:country dbr:France.
    	  ?town dbo:mayor ?m.
    	  ?mayor dbo:birthPlace ?mayorbplace.
    	  ?mayorbplace dbo:country ?mayorbcountry.
    	  FILTER(?mayorbcountry!=dbr:France)
    	}
    	
11. Query 11: Afficher tous les maires nés hors du pays où ils sont maires

    	PREFIX dbr: <http://dbpedia.org/resource/>
    	PREFIX dbo: <http://dbpedia.org/ontology/>
    	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    	PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    	SELECT ?m{
    	  ?town dbo:mayor ?m.
    	  ?town dbo:country ?c.
    	  ?mayor dbo:birthPlace ?mayorbplace.
    	  ?mayorbplace dbo:country ?mayorbcountry.
    	  FILTER(?mayorbcountry!=?c)
    	}
    	
12. Query 12: Afficher le nombre de maires français nés hors de France

    	PREFIX dbr: <http://dbpedia.org/resource/>
    	PREFIX dbo: <http://dbpedia.org/ontology/>
    	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    	PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    	SELECT (count(DISTINCT ?m) AS ?nm) {
    	  ?town dbo:country dbr:France.
    	  ?town dbo:mayor ?m.
    	  ?m dbo:birthPlace ?mayorbplace.
    	  ?mayorbplace dbo:country ?mayorbcountry.
    	  FILTER(?mayorbcountry!=dbr:France)
    	}
    	
13. Query 13 and 14: Afficher pour 10 villes françaises le nombre de natifs présents dans DBPedia, les 10 villes ayant le plus de natifs dans DBPedia.


    	PREFIX dbr: <http://dbpedia.org/resource/>
    	PREFIX dbo: <http://dbpedia.org/ontology/>
    	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
    	PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
    	SELECT ?town (count(DISTINCT ?p) as ?np) {
    		?town dbo:country dbr:France.
    		?p dbo:birthPlace ?town.
    	}
    	ORDER BY DESC(?np)
    	LIMIT 10
	
---------------------------------
## Deuxième partie: TP de la Séance 5
---------------------------------
J'ai choisi de travailler sur l'ensemble de données COVID-19, source ouverte publié par le New York Times, et les transformer en RDF. Ce sujet lié au données covid19 me passione beaucoup, j'ai travaillé sur plusieurs projets liés au thème, notamment le forecasting du nombre de cas au Maroc et en Tunisie.

* Source de données:
https://github.com/nytimes/covid-19-data

* Classes et propriétés liées à la base de données choisie:

![Classes](https://github.com/AyoubAj-CNN/SPARQL-Covid19/blob/main/classes.PNG "Classes")


![Properties](https://github.com/AyoubAj-CNN/SPARQL-Covid19/blob/main/properties.PNG "Properties")

-------------------------------
* Description de la base de données:

- Nous utiliserons la requête ci-dessous pour lister toutes les states (et territoires) dans la base de données:

		SELECT DISTINCT ?name
		{
		    ?country :state ?state.
		    ?state rdfs:label ?name
		}
		ORDER BY ?name

Résultat :

![Result1](https://github.com/AyoubAj-CNN/SPARQL-Covid19/blob/main/result1.PNG "States")

- Nous utiliserons la requête ci-dessous pournombre total de cas et de décès au niveau national:

		SELECT ?date (sum(?cases) as ?totalCases) (sum(?deaths) as ?totalDeaths)
		{
		    ?report :cases ?cases ;  
			    :deaths ?deaths ;      
			    :date ?date
		}
		GROUP BY ?date
		ORDER BY DESC(?date)
		
Résultat:

![Result2](https://github.com/AyoubAj-CNN/SPARQL-Covid19/blob/main/result2.PNG "cases and deaths")
--------------------------------------------
* Requête 1: 

 cette recherche ci-dessous permet de calculer le nombre de cas par habitant pour les comtés en recherchant les valeurs de population dans les wikidata.

		PREFIX wd: <http://www.wikidata.org/entity/>
		PREFIX wdt: <http://www.wikidata.org/prop/direct/>
		SELECT ?countyName ?cases ?population ?percentCases
		{
		    {SELECT (max(?d) as ?date) { ?r :date ?d} }

		    ?report 
			:cases ?cases;
			:date ?date;
			:county [
			    rdfs:label ?countyName ;
			    :fips ?fips
			]

		    SERVICE <https://query.wikidata.org/sparql>
		    {
			[
			    wdt:P1082 ?population ;
			    wdt:P882 ?fips
			]
		    }
		    BIND(roundHalfToEven((?cases / ?population) * 100,2) AS ?percentCases)
		}
		ORDER BY desc(?percentCases)



Résultat:

![Result3](https://github.com/AyoubAj-CNN/SPARQL-Covid19/blob/main/result3.PNG)

* Requête 2 :

Cette requête donne le nombre de collaborations d'un artiste.

	PREFIX foaf: <http://xmlns.com/foaf/0.1/>
	PREFIX rel: <http://purl.org/vocab/relationship/>
	SELECT (count(distinct ?collaboratorname) as ?count)
	WHERE {
		?artist foaf:name "Damon Albarn".
		?collaborator rel:collaboratesWith ?artist.
		?collaborator foaf:name ?collaboratorname.
	}
![3](figures/2.png)
![4](figures/d2.png)
