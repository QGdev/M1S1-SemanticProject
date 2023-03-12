# Projet DANA 2022 - Web des Données & Web sémantique

> **:warning: ATTENTION:**\
>  Certains fichiers sont trop volumineux pour GITHUB, les commits originaux ont donc été modifiés pour exclure tous les fichiers dépassant 100MB (limite imposé par GITHUB). Néanmoins, les fichiers de génération sparql restent valides et utilisables avec TARQL pour retrouver les fichiers générés (mais maintenant absents).

## Participants
- Quentin GOMES DOS REIS
- Alan PROVOST
- Izzedine issa AHMAT

> **:warning: ATTENTION:**\
>  Certains fichiers sont volumineux, un tableau est disponible plus bas afin de savoir quel fichier Turtle télécharger en fonction de vos besoins. 

## Jeu de données utilisé
Pour ce projet, nous avons choisi de prendre le jeu de donnée proposé par [Eurostat](https://ec.europa.eu/eurostat/fr/).

Ce jeu de données représente les données du transport aérien de passagers par aéroports déclarants dans l'union européenne.
Il contient des données concernant le transport du fret, de courrier et de passagers mais également le nombre de sièges offerts.
La périodicité des données est multiple, nous avons des données annuelles, trimestrielles et mensuelles.

Il est sous la licence CC BY 4.0 et est disponible à l'adresse suivante [https://ec.europa.eu/eurostat/databrowser/view/AVIA_TF_ALA/default/table](https://ec.europa.eu/eurostat/databrowser/view/AVIA_TF_ALA/default/table).

## Sémantisation
En l'état, le fichier CSV n'est pas traitable au vu de sa taille (1,3Go), nous allons donc traiter uniquement les données annuelles, on utilise donc SPARQL et pour trancrire notre dataset en Turtle, avec le fichier dataset_crea_annuel.sparql.

![Exemple de notre schéma de données](dataset_diagram_exemple.png)

<!---
```plantuml
   @startuml
        skinparam componentStyle rectangle

        circle (http://example.org/airport/FR/LFPG)

        [LFPG] <-u- (http://example.org/airport/FR/LFPG) : "dbo:icaoLocationIdentifier"
        [FR] <-u- (http://example.org/airport/FR/LFPG) : "dbo:iso31661Code"
        [FRA] <-u- (http://example.org/airport/FR/LFPG) : "dbo:iso31661Code"
        [France] <-u- (http://example.org/airport/FR/LFPG) : "dbo:country"
        [dbo:Airport] <-u- (http://example.org/airport/FR/LFPG) : "rdf:type"

        circle (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU)

        [ex:AirportMetricValue] as type1
        type1 <-- (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU) : "rdf:type"
        (2011) <-- (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU) : "dbo:year"
        (360735) <-- (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU) : "rdf:value"
        [FLIGHT] <-- (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU) : "ex:unit"
        [CAF] <-- (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU) : "ex:measuredMetric"
        [INTL] <-- (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU) : "ex:coverageTransport"
        [LIC_EU] <-- (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU) : "ex:airline"


        circle (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL)


        [ex:AirportMetricValue] as type2
        type2 <-- (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL) : "rdf:type"
        (2010) <-- (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL) : "dbo:year"
        (45336.2) <-d- (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL) : "rdf:value"
        [T] <-- (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL) : "ex:unit"
        [FRM_BRD] <-- (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL) : "ex:measuredMetric"
        [NAT] <-- (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL) : "ex:coverageTransport"
        [TOTAL] <-- (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL) : "ex:airline"

        (http://example.org/airport/FR/LFPG) <-up- (http://example.org/measure/FR/LFPG/2011/CAF/INTL/LIC_EU) : "ex:declarativeAirport"
        (http://example.org/airport/FR/LFPG) <-up- (http://example.org/measure/FR/LFPG/2010/FRM_BRD/NAT/TOTAL) : "ex:declarativeAirport"
@enduml
-->

Une fois traité avec TARQL, on obtient un fichier Turtle contenant environ 11 536 926 triples, ci-dessus, voici un exemples de structure de données de notre fichier Turtle.

> **:warning: ATTENTION:**\
>  Les types des valeurs des mesures changent en fonction du type de mesure, si la mesure est une masse, le type sera float sinon le type sera entier. 


## Fichiers Turtle

Afin de permettre de la liaison avec le plus de groupes possibles, nous avons développé plusieurs variantes de notre dataset, voici, les différences entres les différentes versions...

| Fichier     | Format des codes pays <br> *(dbo:iso31661Code)*| Contient les noms de pays <br> *(dbo:country)* | Pour commande pour "re-compiler " <br> au format Turtle depuis la racine du projet |
| ----------- | ---------------------------------------------- | ---------------------------------------------- | ---------------------------------------------------------------------------------- |
| [data_original.ttl](https://gitlab.univ-nantes.fr/E192263G/semantic-project/-/blob/main/dataset/data_original.ttl) <br> *(Original, sans modifications)* | ISO31661-ALPHA2 <br> *(Sauf la Grèce qui est nommé en EL au niveau Européen)*  | Non | ```./tarql-1.2/bin/tarql --dedup 10000000  ./build/dataset_creation_original.sparql > dataset/data_original.ttl``` |
| [data_complete.ttl](https://gitlab.univ-nantes.fr/E192263G/semantic-project/-/blob/main/dataset/data_complete.ttl)  <br> *(Version finale, celle qui est retenue)* |  ISO31661-ALPHA2 *(La Grèce passe de EL à GL)* <br> ISO31661-ALPHA3 | Oui | ```./tarql-1.2/bin/tarql --dedup 10000000  ./build/dataset_creation_complete.sparql > dataset/data_complete.ttl``` |

Bien sûr, seuls les codes pays ont été modifiés, les codes ICAO et les différentes valeurs restent inchangées quelque soit la version.


## Requêtes locales

Nous avons effectué trois exemples de requêtes, les voici...
| Fichier | Résumé  |
| ------- | ------- | 
| [local1-Top20FrenchAirportsReportingTheBiggestNumberOfFlightsIn2020.sparql](https://gitlab.univ-nantes.fr/E192263G/semantic-project/-/blob/main/requests/local/local1-Top20FrenchAirportsReportingTheBiggestNumberOfFlightsIn2020.sparql) | Liste des 20 aéroports français ayant déclaré le plus de vols en 2020. |
| [local2-FrenchAirportsThatHaveTheBiggestFillingRateOnNationalFlights.sparql](https://gitlab.univ-nantes.fr/E192263G/semantic-project/-/blob/main/requests/local/local2-FrenchAirportsThatHaveTheBiggestFillingRateOnNationalFlights.sparql) | Les aéroports français dont les vols nationaux ont été les plus chargés en fonction des années |
| [local3-AirportThatOfferedMoreThan100000SeatsIn2020.sparql](https://gitlab.univ-nantes.fr/E192263G/semantic-project/-/blob/main/requests/local/local3-AirportThatOfferedMoreThan100000SeatsIn2020.sparql) | Aéroports offrant un nombre de sièges passagers supérieurs à 100 000 en 2020. |


## Requêtes locales

Nous avons effectué trois exemples de requêtes liées, les voici...

> **:warning: ATTENTION:**\
>  Dans les requêtes suivantes, certaines parties pourront parraître étrange mais il ne s'agit généralement que d'un correctif permettant de lier notre dataset à celui d'autres groupes.


| Fichier | Groupe de liaison | Résumé |
| ------- | ----------------- | ------ | 
| [grp3-IsHDIRelatedToNbPassengerFlightIn2017.sparql](https://gitlab.univ-nantes.fr/E192263G/semantic-project/-/blob/main/requests/linked/grp3-IsHDIRelatedToNbPassengerFlightIn2017.sparql) | 3 | Le nombre de vols de passagers est-il relatif à l’HDI d’un pays en 2017 ? |
| [grp4-EmittedCo2AndNbOfFlightsIn2010.sparql](https://gitlab.univ-nantes.fr/E192263G/semantic-project/-/blob/main/requests/linked/grp4-EmittedCo2AndNbOfFlightsIn2010.sparql) | 4 |Affichage de la quantité de Co2 émise et du nombre de vols par pays en 2010 |
| [grp7-isStartuoFundingRelatedToNbOfFlightIn2014.sparql](https://gitlab.univ-nantes.fr/E192263G/semantic-project/-/blob/main/requests/linked/grp7-isStartuoFundingRelatedToNbOfFlightIn2014.sparql) | 7 | Le nombre de vols est-il relatif au total des levées de fonds par pays en 2014 ? |
