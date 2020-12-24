# SPARQL-Covid19
Analyzing covid19 data using Sparql
Ce projet sera traité dans la deuxième partie de TP.

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

![Classes](https://github.com/AyoubAj-CNN/SPARQL-Covid19/blob/main/classes.PNG)
![Properties](https://github.com/AyoubAj-CNN/SPARQL-Covid19/blob/main/properties.PNG)

-----------------
* Requête 1: 

Elle permet de récupérer les albums d'un artiste, avec la date de sortie, le nombre de pistes, et une image.
On peut changer "Radiohead" par le nom d'un autre artiste/groupe (Jean Michel Jarre par exemple) pour avoir une liste de ses albums.
On peut avoir des répétitions car le même album peut par exemple être publié par des maisons de disques différentes, à des dates différentes. 
Ceci est du au fait que Musicbrainz vise à cataloguer toutes les sorties officielles, et non pas les albums en eux-mêmes.

	PREFIX vocab: <http://dbtune.org/musicbrainz/resource/vocab/>
	PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
	PREFIX dc: <http://purl.org/dc/elements/1.1/>
	PREFIX mo: <http://purl.org/ontology/mo/>
	PREFIX foaf: <http://xmlns.com/foaf/0.1/>

	SELECT ?title ?date ?ntracks ?image
	WHERE {
	  ?artist foaf:name "Radiohead".
	  ?album foaf:maker ?artist;
			 rdf:type mo:Record;
			 dc:title ?title;
			 mo:image ?image;
			 vocab:tracks ?ntracks;
			 dc:date ?date.
	}
![1](figures/1.png)
![2](figures/d1.png)

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
