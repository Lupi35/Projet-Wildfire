# Projet Wildfire

En 2021, aux États-Unis, 58 968 feux de forêt ont été recensés soit près de 2,8 millions d’hectares brûlés . Ces incendies sont d’origine naturelle (foudre) ou humaine, 
la cause pouvant être intentionnelle, involontaire ou liée aux infrastructures. Si la plupart d’entre eux sont insignifiants, et n’occasionnent que peu de dégats, 
certains sont infiniment plus destructeurs et embrasent des milliers d’hectares de végétation sur plusieurs jours. 
Outre les conséquences dramatiques sur l’environnement (destruction d’écosystème animal et végétal, fragilisation des sols), ces incendies ont des impacts économiques 
(perte d’activités, impact sur le tourisme), sanitaires (pollution de l’air) et sociaux (délocalisation). 
Ȇtre informé en temps réel et être proactif sur les mesures à prendre en cas d’un départ de feu sont donc une nécessité pour les autorités. 
Dans quelle mesure peut-on s’appuyer sur les statistiques pour anticiper les risques que représentent ces feux de forêt ? Peut-on construire un modèle de Machine Learning 
capable de prédire la superficie approximative des terres réduites en cendres à partir de données historiques ?

## Notre dataset

Le dataset est une base de données spatiales des feux de forêt ayant eu lieu aux États-Unis entre 1992 et 2015. Cette base de données comprend 1,88 million 
d’enregistrements d’incendies géoréférencés, représentant un total de 57 millions d’hectares brûlés au cours d’une période de 24 ans. Elle a été créée au niveau 
national par le gouvernement américain dans le cadre du Fire Program Analysis (FPA), une unité chargée d’analyser les occurrences des incendies afin d’établir 
un processus commun et inter-agences pour la planification stratégique et la budgétisation de la gestion des feux de forêt.  

Il est disponible sur Kaggle sous format .sqlite à cette adresse :  
https://www.kaggle.com/datasets/rtatman/188-million-us-wildfires

### Ajouts de données complémentaires

Pour chaque incendie, le dataset nous rapporte les coordonnées géographiques (latitude, longitude) de l’incendie, la date à laquel il s’est produit, la cause responsable du départ du feu et la taille de ce feu à travers la superficie totale de végétation brûlée. 

Afin de pouvoir prédire l’ampleur d’un incendie, il est indispensable de croiser ces données avec des données météorologiques, les conditions climatiques (vent, chaleur, hygrométrie) ayant une grande influence sur le développement et la propagation des feux de forêt.
Pour récupérer ces données, nous nous sommes appuyés sur la librairie meteostat, base de données météorologiques et climatiques mondiale fournissant des données détaillées pour des milliers de stations météorologiques. Voici un lien vers la documentation :
https://dev.meteostat.net/python/

Afin d’avoir des données exhaustives, nous avons récupéré les informations suivantes :

•	températures moyennes des 30, 10, 3 jours précédents l’incendie et idem pour la période suivant le départ de l’incendie ;

•	température moyenne, minimale et maximale de la journée de départ de l’incendie ;

•	total des précipitations des 30, 10, 3 jours précédents l’incendie et idem pour la période suivant le départ de l’incendie ;

•	précipitations de la journée de départ de l’incendie.

La taille d’un incendie dépend également du temps de réaction des autorités et des moyens existants pour le maîtriser. Un des facteurs que nous avons donc considéré est le nombre de casernes de pompiers disponibles sur un territoire donné (ici, nous avons choisi le niveau étatique) et la distance entre le point de départ du feu et la caserne la plus proche.     
Nous avons obtenu la liste des casernes de pompiers sur le site du département de la Sécurité intérieure des États-Unis (U.S. Department of Homeland Security). Le jeu de données est téléchargeable ici :
https://hifld-geoplatform.opendata.arcgis.com/datasets/0ccaf0c53b794eb8ac3d3de6afdb3286_0/explore?location=40.553343%2C-120.631622%2C4.32
Dans ce dataset, le système de coordonnées utilisé pour localiser les casernes n’est pas indiqué. Les coordonnées ne sont donc pas directement exploitables. En revanche, l’adresse des casernes est renseignée. L’étape préalable pour récupérer ces données a donc été de récupérer les coordonnées géographiques des casernes à partir de leur adresse. Une fois les coordonnées récupérées, nous avons calculé la distance entre la caserne la plus proche et chaque départ de feu.

Dans la même optique, nous avons récupéré les densités de population au niveau de chaque État partant du postulat que plus un territoire est densément peuplé, plus le départ de feu était signalé rapidement et plus les autorités pouvaient réagir de manière efficace pour limiter la propagation de l’incendie. Les données ont été récupérées sur Wikipédia à cette adresse :
https://en.wikipedia.org/wiki/List_of_states_and_territories_of_the_United_States_by_population_density

Enfin, nous souhaitions enrichir un peu plus notre dataset en ajoutant des données concernant le type de végétation brûlé dans les incendies, certaines essences de bois se consumant plus rapidement que d’autres et impactant, de fait, la progression d’un incendie. Néanmoins, nous n’avons pas été en mesure de trouver de données exploitables à croiser avec notre dataset. Nous avons, en revanche, ajouté la couverture forestière par État, un feu de forêt ayant plus de chance de s’étendre là où la végétation est plus importante.  Les données ont été collectées sur Wikipédia à cette adresse :
https://en.wikipedia.org/wiki/Forest_cover_by_state_and_territory_in_the_United_States

## Les objectifs

L’objectif de ce travail est, dans un premier temps, d’étudier l’évolution des feux de forêt aux États-Unis entre 1995 et 2015 et de voir si nous pouvons en dégager une tendance générale. 
Dans la partie Data Visualisation, nous chercherons donc à comprendre : 

•	Si les feux de forêt sont plus ou moins fréquents dans le temps et si les dégats engendrés plus ou moins importants ;

•	l’impact des conditions météorologiques sur ces évolutions ;

•	quels territoires sont les plus concernés par ces incendies ;

•	les causes principales de ces incendies.

Dans un second temps, nous essaierons de voir s’il est possible de construire un modèle de Machine Learning capable de prédire la superficie relative de la couverture végétale brûlée. 
