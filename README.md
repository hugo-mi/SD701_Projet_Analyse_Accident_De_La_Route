# SD701_Projet : Analyse d'un dataset d'accident de la route

**SD-701 : Projet de Data Mining**
_Analyse d’une base de données des accidents corporels de la circulation routières_

## Sommaire


**1. Présentation du projet** <br>
**2. Objectif du projet** <br>
**3. Présentation du Dataset** <br>
**4. Pre-processing** <br>
**5. Exploration des données** <br>
**6. Sélection des variables** <br>
**7. Application de l'algorithme Apriori (règles d'associations)** <br>
**8. Application de l'algorithme Regression Logistique** <br>
**9. Conclusion & Discusion** <br>
**10. Requirements** <br>

## 1/ Présentation du projet

Dans le cadre du projet SD 701 nous souhaitons analyser une base de données des accidents corporels de la circulation routières. 
En appliquant différentes méthodes de DataMining sur un ensemble de jeux de données relatifs aux accidents de la route nous souhaitons déterminer les facteurs qui influencent la gravité d’un accident. 
Le contexte de l’étude pourrait être la suivante : 
Il s’avère que la plupart des personnes impliquées dans un accident de la route ne sont pas blessés ou ne présentent que des blessures artificielles. Cependant, il se peut que des personnes gravement blessées nécessitent une prise en charge à l’hôpital. Dans une situation de crise sanitaire, où tous les hôpitaux se retrouvent en surcharge de patients, il peut être utile de diminuer les accidents graves admis en soins intensifs. Pour ce faire, une campagne de prévention s’appuyant sur les facteurs influençant la gravité pourrait aider a diminuer le nombre d’accidents graves. 
Pour connaître les facteurs influençant la gravité d’un accident, l’option retenu serait de diminuer la probabilité qu’une personne ait un accident sachant que cette dernière à déjà eu un accident. (Formule de Bayes). En ce sens connaître les facteurs influençant la gravité des accidents permettraient des diminuer la probabilité qu’une personne soit hospitalisée. 


## 2/ Objectif du projet

L'objectif de ce projet est de déterminer les facteurs influençant la gravité des accidents. Sur la base de cette analyse l’agence régional de santé pourra alors fixer les orientations de sa future campagne nationale de lutte contre les accidents de la route. 


## 3/ Présentation du « Dataset »

Pour chaque accident corporel (soit un accident survenu sur une voie ouverte à la circulation publique, impliquant au moins un véhicule et ayant fait au moins une victime ayant nécessité des soins), des saisies d’information décrivant l’accident sont effectuées par l’unité des forces de l’ordre (police, gendarmerie, etc.) qui est intervenue sur le lieu de l’accident. Ces saisies sont rassemblées dans une fiche intitulée bulletin d’analyse des accidents corporels. L’ensemble de ces fiches constitue le fichier national des accidents corporels de la circulation dit " Fichier BAAC1" administré par l’Observatoire national interministériel de la sécurité routière "ONISR". Les bases de données, extraites du fichier BAAC, répertorient l'intégralité des accidents corporels de la circulation intervenus durant une année précise en France métropolitaine ainsi que les départements d’Outre-mer (Guadeloupe, Guyane, Martinique, La Réunion et Mayotte depuis 2012) avec une description simplifiée. Cela comprend des informations de localisation de l’accident, telles que renseignées ainsi que des informations concernant les caractéristiques de l’accident et son lieu, les véhicules impliqués et leurs victimes.
Description des bases de données annuelles des accidents corporels de la circulation routière - Années de 2005 à 2018

Source Dataset : data.gouv.fr (ministère de l’intérieur)

•	https://www.data.gouv.fr/fr/datasets/bases-de-donnees-annuelles-des-accidents-corporels-de-la-circulation-routiere-annees-de-2005-a-2019/

Notre base de données est composée de 4 fichiers csv :


•	**Caractéristique.csv** (58440 lignes) 

o	Décrit les circonstances générales de l’accident (types de collisions, luminosité, date)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/carac_table.png)


•	**Lieux.csv** (58440 lignes)

o	Décrit le lieu principal de l’accident (catégorie route, nb de voies, régime de circulation, surface de la chaussée,…)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/lieux_table.png)

•	**Véhicules.csv** (100710 lignes)

o	Décrit les véhicules impliqués dans l’accident (n° plaque immatriculations, type de véhicule, localisation du choc, manœuvre,…)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/vehic_table.png)


•	**Usagers.csv** (132978 lignes)

o	Décrit les usagers impliqués dans l’accident (place de l’usager dans le véhicule, gravité, trajet de l’usager,…)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/usag_table.png)

Chacune des variables contenues dans une rubrique est reliée aux variables des autres rubriques. Le n° d'identifiant de l’accident (Cf. **"_Num_Acc_"**) présent dans ces 4 rubriques permet d'établir un lien entre toutes les variables qui décrivent un accident. Quand un accident comporte plusieurs véhicules, il est également possible de relier chaque véhicule à ses occupants. Ce lien est fait par la variable **_Num_veh_**.

## 4/ Pre-processing

L'objectif de la phase de pre-processing est d'étudier la distrution des valeurs de chaque covariable afin d'effectuer un choix sur les variables à garder et celle à rejeter et ce tout en prenant en compte le contexte de notre étude. Il s'avère que pour la plupart des variables ont été re-encodées en suivant la stratégie one-hot encoding. 

Voici un exemple pour le ré-encodage de la variable **_catu_** de la table **_Usagers_**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/one_hot_encoding.png)

Les choix que nous avons entrepris pour ré-encoder chacune des variables sont décrits dans le notebook **Projet_EGVD_PreProcessing**

**Distribution des variables de la table **_Caractéristique_**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/distrib_table_carac1.png)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/distrib_table_carac2.png)

**Distribution des variables de la table **_Lieux_**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/distrib_table_lieux1.png)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/distrib_table_lieux2.png)

**Distribution des variables de la table **_Usagers_**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/distrib_table_usag.png)

**Distribution des variables de la table Véhicules**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/distrib_table_vehic1.png)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/distrib_table_vehic2.png)


**Jeu de données nettoyé**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/dataset_cleaned.png)

## 5/ Exploration des données

Pour la section fouille de données nous avons choisi d'effectuer des analyses descriptives plus poussées qui sont les suivantes

### 1ère Visualisation : Carte de France interactive du nombre d\'accident par département en 2019

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/acc_dep.png)

**Interprétation :**
A travers cette visualisation nous constatons que c'est en région île de france que se produit le plus d'accident. Sans surprise la carte met en lumière le fait que c'est dans les départements où la population est la plus élevé que se produit le plus d'accident. (Bouches-du-rhônes, Rhône, Haut de Seine, Essone, Seine St Denis, Paris)

A l'inverse dans les régions les moins denses en termes de population, le nombre d'accident y ait beaucoup plus faible.

### 2ème Visualisation : Carte de France interactive pour localiser tous les accidents d'un département en particulier en 2019

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/carte_acc.png)

**Interprétation :**
Encore une fois ce graphique confirme l'interpréation que nous avons faites juste avant. Ce sont dans les endroits où la densité de population est la plus forte que se produit le plus d'accident. A noter toutefois que cela ne signifie pas que ce sont dans ces endroits les plus peuplés que se réalisent les accidents les plus graves.

### 3ème Visualisation : Evolution du nombre des accidents en 2019 en fonction de la saison

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/acc_saison.png)

**Interprétation :**
Au regard de l'histogramme nous observons, que c'est en été que se produit le plus d'accident. Cependant la différence du nombre d'accident entre l'été, l'automne et le printemps n'est pas marquante contairement à la saison d'hiver. En effet, le delta du nombre d'accident entre l'été et l'hiver est conséquent. (4000 accidents de moins en hiver qu'en été)

### 4ème Visualisation : Evolution du nombre des accidents en 2019

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/evolution_acc_2019.png)

**Interprétation :**
En se fiant à la moyenne des accidents par mois (courbe orange), nous remarquons que de début Janvier à mi Mai, le nombre des accidents augmentent. Toutefois on constate que ce nombre stagne de début Février jusqu'à mi Mai.

Par ailleurs de mi Mai à mi Juillet août nous assistons à une augmentation considérable du nombre d'accident puis l'inverse de produit de mi Juillet à fin Août. cela peut être contre-intuitif car c'est à ce moment de l'année que nous assistons aux grands chassés croisées entre d'un côté les retours de vancance et de l'autre les départs en vacance. On peut donc en déduire que le nombre d'accident entre le début des vancances scolaires jusqu'à mi-juillet est bien supérieur qu'à la fin des vacances scolaire (baisse non négligeable du nombre d'accident entre mi Août et début Septembre).

A la rentrée scolaire nous constatons une augmentation du nombre d'accident. En fin d'année nous observons une baisse du nombre des accidents

**Conclusion de phase d'analyse :**
A travers cette analyse exploratoire, nous avons observé que c'est pendant la période d'Hiver que nous avons le moins d'accident et ce malgré le fait que les conditions météorologiques soient mauvaises (neige, verglas, pluie,...) durant cette période de l'année.

Enfin, ce sont dans les endroits dans lesquels la densité de population est la plus forte que se produit le plus d'accident. Maintenant, il nous reste à confirmer si ce sont dans ces régions de la France que se produit les accidents les plus grave, c'est ce que nous allons tenter d'observer à l'aide d'outil de machine learning

## 6/ Sélection des variables

La sélection des variables est une étape important dans la chaine de création de valeurs à partir de données car elle permet de centré notre étude pour nous concentré uniquement les données ayant un impact significatif sur notre variable cible. 
En général, il est recommandé d'éviter d'avoir des variables corrélées dans un jeu de données. En effet, un groupe de features fortement corrélées n'apportera pas d'informations supplémentaires (ou juste très peu), mais augmentera la complexité de l'algorithme, augmentant ainsi le risque d'erreurs.
De plus la multicolinéarité rend difficile l'interprétation de nos coefficients et réduit la capacité de notre modèle à identifier des variables indépendantes statistiquement significatives.
Nous chercherons donc à supprimer les variables fortement corrélés à l'aide de la librairie python **_featurewiz_**. 
La matrice de corrélation est la suivante : 

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/matrice_corr.png)

Ensuite nous avons appliqué l'algorithme **_featurewiz _**est une bibliothèque de python qui permet de trouver les meilleures caractéristiques dans notre ensemble de données si nous lui donnons un dataframe et le nom de la variable cible. Il fera ce qui suit :

- Il supprime automatiquement les caractéristiques fortement corrélées (la limite est fixée à 0,70 mais nous pouvons la changer dans l'argument d'entrée).

- Si plusieurs caractéristiques sont corrélées entre elles, laquelle supprimer ? Dans un tel conflits, l'algorithme supprimera la caractéristique ayant le **mutual information score** le plus faible.

- Pour finir l'algorithme fera une sélection récursive des caractéristiques en utilisant l'algorithme XGBoost pour trouver les meilleures caractéristiques en utilisant XGBoost.

Le résultat de **_featurewiz _** est le suivant : 

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/featurewize_output.png)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/featurewize_output1.png)

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/featurewize_output2.png)

**La liste des features en sortie de notre algorithme _featurewize_**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/featurewize_output3.png)

Ensuite, pour appliquer notre  **𝑅é𝑔𝑟𝑒𝑠𝑠𝑖𝑜𝑛   𝐿𝑜𝑔𝑖𝑠𝑡𝑖𝑞𝑢𝑒**  et  l'algorithme **𝐴𝑃𝑟𝑖𝑜𝑟𝑖** nous souhaitons conserver uniquement 10 variables. Pour cela utilisons une Wrapper methods, en particulier de la **_RFE _** (acronyme pour Recursive Feature Elimination).
Le principe est simple : on fournit un modèle à l'algorithme **_RFE _** (ici LogisticRegression), qui est ajusté sur le jeu de données complet. L'importance de chaque feature est estimée et il supprime la ou les features les moins importantes, puis il recommence. Une fois le nombre de features cible atteint, on retourne le jeu de features qui a fourni le meilleur résultat sur le jeu d'entrainement.
RFE est un algorithme de sélection de caractéristiques de type wrapper. Cela signifie qu'un algorithme d'apprentissage automatique différent est fourni et utilisé au cœur de la méthode, est enveloppé (wrapped) par **_RFE _** et utilisé pour aider à sélectionner les fonctionnalités.

Techniquement, **_RFE _** est un algorithme de sélection de caractéristiques de style wrapper qui utilise également la sélection de caractéristiques basée sur un filtre en interne.

**_RFE_** fonctionne en recherchant un sous-ensemble de fonctionnalités en commençant par toutes les fonctionnalités du jeu de données d'apprentissage et en supprimant avec succès les fonctionnalités jusqu'à ce que le nombre souhaité reste.

Ceci est réalisé en ajustant l'algorithme d'apprentissage automatique utilisé dans le noyau du modèle, en classant les fonctionnalités par importance, en supprimant les fonctionnalités les moins importantes et en réajustant le modèle. Ce processus est répété jusqu'à ce qu'un nombre spécifié de fonctionnalités reste.

Notre sélection final des 10 meilleurs features pour nos modèles est la suivante : 

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/wrapper.png)

## 7/ Application de l'algorithme Apriori (règles d'associations)

Afin de répondre à l'objectif de notre étude qui on le rappel est de trouver les facteurs aggravant d'un accident, nous avons choisi d'appliquer l'algorithme Apriori. En utilisant cet algorithme nous espérons pouvoir déterminer les ensembles des éléments fréquents causant des accidents graves. 
Pour ce faire, il procède en identifiant les éléments individuels fréquents dans la base de données et en les étendant à des ensembles d'éléments de plus en plus grands tant que ces ensembles d'éléments apparaissent suffisamment souvent dans la base de données.
Les itemsets fréquents déterminés par **Apriori ** peuvent être utilisés pour déterminer des règles d'association qui mettent en évidence des tendances générales dans la base de données : cela a des applications dans des domaines tels que  l'analyse du panier de commandes pour un site de e-commerce.

L'algorithme Apriori fait intervenir, 3 métriques qui sont les suivantes : 

**Le support :** il représente la fiabilité. Ce critère permet de fixer un seuil en dessous duquel les règles ne sont pas considérées comme fiables. le support est utilisé pour mesurer l'abondance ou la fréquence (souvent interprétée comme une signification ou une importance) d'un ensemble d'éléments dans une base de données. Il est nécéssaire de fixer un seuil de support minimal à respecter pour éviter de sélectionner des règles d’association qui seraient trop rares. 
Le support se calcul de la manière suivante :

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/support.png)

**La confiance :** elle représente la précision de la règle et peut être vue comme la probabilité conditionnelle Conf(A -> C) = P(A|C). En clair La confiance d'une règle A -> C est la probabilité de voir le conséquent dans une transaction étant donné qu'elle contient aussi l'antécédent. La métrique n'est ni symétrique ni dirigée ; par exemple, la confiance pour A -> C est différente de la confiance pour C->A. La confiance est de 1 (maximale) pour une règle A->C si le conséquent et l'antécédent se produisent toujours ensemble. 
La confiance se calcul de la manière suivante :

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/confidence.png)

**Le lift :** il caractérise l’intérêt de la règle, sa force. Un lift > 1 indique qu’il existe bien un lien entre les 2 éléments. La métrique lift est couramment utilisée pour mesurer combien de fois l'antécédent et la conséquence d'une règle A->C se produisent ensemble par rapport à ce à quoi nous nous attendrions s'ils étaient statistiquement indépendants. 
Le lift se calcul de la manière suivante : 

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/lift.png)

Une « Bonne règle » correspond à un règle dont le support et la confiance sont élevées. 

Nous avons utilisé l'algorithme _**Apriori**_, de la librairie **_Apyori_**. Nous avons appliqué l'algorithme sur l'ensemble des variables qui ont été sélectionné par l'algorithme des sélections de variables. Ensuite, les valeurs de ces variables ont été ré-encodé en variable catégorielle pour faciliter l'interprétation du résultat généré par Apriori. A noter que chaque ligne de notre jeu de données correspond à une transaction. Etant donné que nous souhaitons obtenir les règles d'association qui ont comme conséquence notre variable cible binaire, nous avons sélectionné les lignes dont la valeur de de notre variable cible était égale à 1. 

Pour un support minimum fixé à 0.0045, une confiance minimum fixé à 0.2 et un lift minimum fixé à 4 et 2 éléments minimums engagés dans la transaction nous avons obtenu le résultatc suivant:

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/output_reglog.png)

#### Interprétation

L'algorithme, Apriori a été long à executer sur nos machines. Par conséquent, nous n'avons pas pu executer l'algorithme Apriori sur l'ensemble de notre jeu de données car le temps de calcul est long. Nous nous sommes contentés d'analyser les 200 premières lignes de notre jeu de données ce qui nous apris 26 minutues or notre dataframe contient 21000 lignes. L'inconvénient est que cet échantillon peut ne pas être représentatif de l'ensemble de note dataset.

Les informations que nous pouvons tirer du résultat généré par Apriori sont les suivantes : 
Premièrement lorsque plusieurs véhicules sont impliqués dans un accident (entre 3 à 5 véhicules) les conséquences d'un accident peuvent être lourde jusqu'à provoquer des blesées grave ou même des morts. Cela se comprend facilement car dès lors qu'un certains nombres de véhicules sont engagés dans un accident plus les chocs sont violents. Cela correspond en fait à un carambolage. 

Une autre information importante ressort du résultat qui est que lorsque le choc entre les véhicules n'est pas un choc arrière, l'accident à plus de probabilité d'être grave. Il serait interessant alors de voir si le choc avant peut être un facteur important dans le degré de gravité d'un accident. Enfin, en analysant le résultat de l'algorithme nous nous apercevons qu'en province, les accidents sont plus généralement plus grave qu'en région île de france. Cela peut s'expliquer par le fait que les routes départementale la vitesse est plus elevée qu'en agglomération (ville). Et cela est d'autant plus parlant car malgré le fait que notre analyse exploratoire montre qu'il se produit beaucoup plus d'accident en île-de-france que dans l'ensemble des autres régions, les accidents graves se situent hors de l'île de france et plus généralement en dehors des villes (agglomération). Cela signifie que la plupart des accidents graves se produisent sur des axes routier comme les départementales et nationnales ou autoroute.

Par ailleurs en analysant le résultat nous pouvons dire que lorsque les personnes impliquées dans l'accident ne portent aucun équipement de protection individuelle, l'accident à une forte probabilité d'engendrer des bléssés graves et/ou des morts.

Enfin, la raison des trajets des personnes et notamment lorsqu'il s'agit d'un trajet loisir, semble renforcer la gravité d'un accident. On peut imaginer que lors d'un trajet loisir, le conducteur et moins concentré sur sa conduite et se laisse divertir facilement.

Globalement, les résultats généré par Apriori ne sont pas choquant mais ils auraient pu être déduit facilement par un expert métier dans le domaine de la sécurité routière. Autrement dit, l'algorithme Apriori ne nous a pas permis par de révéler des facteurs dont on ignorait qu'ils pouvaient avoir un impact sur la gravité d'un accident. 

## 8/ Application  de l'algorithme Regression Logistique

L'algorithme Regression Logistique a été executé sur les variables sélectionnées par l'algorithme de sélection de variable. 

Pour trouver les meilleurs paramètres à fournir à l'algorithme de Regression Logistiques, nous avons réalisé un **Grid Search**
Souvent, les effets des hyperparamètres sur un modèle sont connus, mais comment définir au mieux un hyperparamètre et des combinaisons d'hyperparamètres qui sont en interaction pour un jeu de données. Il existe souvent des heuristiques générales ou des règles empiriques pour configurer les hyperparamètres.
Une bonne approche consiste à rechercher différentes valeurs pour les hyperparamètres du modèle et choisir un sous-ensemble qui atteint les meilleures performances sur un ensemble de données, c'est ce qu'on appelle l' optimisation des hyperparamètres. Le résultat d'une optimisation d'hyperparamètres est un ensemble unique d'hyperparamètres performants que nous pouvons utiliser pour configurer notre modèle.

Ensuite, nous pouvons définir l'espace de recherche de nos hyperparamètres :

La régression logistique nécessite trois paramètres « C », « pénalité » et « solver » qui seront optimisés par GridSearchCV. Nous avons donc défini ces trois paramètres sous forme de liste de valeurs dont GridSearchCV sélectionnera la meilleure valeur de paramètre. 
À la fin du grid search, le meilleur score et la configuration d'hyperparamètres ayant obtenu les meilleures performances sont retournés. Nous pouvons voir que la meilleure configuration atteint une précision d'environ  74 , pour des valeurs spécifiques du solveur égale à  𝑙𝑖𝑏𝑙𝑛𝑖𝑒𝑎𝑟 , pour une pénalité  𝑙2  et pour un  𝐶=0.01 . C'est avec ce tunning des hyperparamètre que l'on atteint le score optimal.

*Les coéfficients de la Régression Logistique son les suivants :** 

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/reg_log1.png)

**Analyse des coéfficients de la régression logistique**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/reglog_formula.png)

**En passant les coéfficients à l'exponentielle pour améliorer leur interprêtation**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/reg_log.png)

**regardons l'Odds ratio ou Rapport des cotes ( 𝑅𝐶 )**

![](https://github.com/hugo-mi/SD701_Projet_Analyse_Accident_De_La_Route/blob/development/img/Reglog2.png)

Au regard des résultats de la régression logistique, les accidents aux conséquences les plus lourdes : être gravement bléssé ou décédé, concerne des accidents qui ont lieux en dehors des agglomérations sur des routes départementales là où la vitesse est plus élevée qu'en ville.

Les rapports des cotes des coéfficients  𝑟𝑒𝑔 _ 𝐼𝐷𝐹  et  𝑎𝑔𝑔  nous indique qu'il y a moins d'accident grave en agglomération et en île de France, pourtant c'est là où la densité de la population est le plus élevé. Cela peut s'expliquer par le fait que la vitesse est moins élevé en ville ; ce qui cause moins d'accident grave.

Les individus qui conduisent des deux roues ressortent des accidents avec des blessure plus grave que ceux qui conduisent des véhicules. Les personnes âgés sont plus sensibles aux chocs et ressortent des accidents avec des bléssure plus importantes.

## 9/ Conclusion & Discusion

A travers l'enseignement SD701, nous avons réalisé conjointement un pré-traitement des données, une exploration des données avec la réalisation de différentes visualisation permettant de mieux comprendre le dataset ainsi que l'application de deux algorithmes de machine learning. 

L'algorithme de regression linéaire confirme les résultats de l'algorithme Apriori notamment sur l'influence de la variable _choc_avant_. En effet, Apriori a montré que lorsque aucun choc arrière n'avait eu lieu lors de l'accident, la gravité de l'accident était élevé. Cela signifie qu'un choc avant renforce la gravité d'un accident et cela a été confirmé par l'algorithme de Regression Logisitque (coefficient de _choc_avant_ > 1). Cela est confirmé par le coefficient de la variable _circ_bidirectionnelle_ car lorsque la route est à deux sens, le choc frontale se produit beaucoup plus souvent que sur une route à sens unique. De plus intuitivement, on comprend que lorsqu'un accident à lieu dans une voie à 2 sens, le nombre de voitures impliqués y est plus élevé. Cela est confirmé par l'algorithme Apriori qui met en exergue le fait que lorsque plusieurs véhicules participent à un accident la probabilité que les conséquences de l'accident soient lourdes (bléssés grave et/ou mort) est très élevée. Par ailleurs les deux algorithmes montrent que lorsque l'accident à lieu sur des types de route se situant en dehors des grandes villes (hors agglomération) comme les routes départementales, la gravité de l'accident est plus élevée. En effet, Apriori a montré que les accidents les plus graves ont lieu en pronvince (hors Paris) et ce malgré le fait qu'il y ai beaucoup plus d'accident en Région île de France. Cela peut s'expliquer par le simple fait qu'en Ile de France, les routes départementales se font plus rares qu'en province. 

Cependant, nous avons relevé des incohérences dans les données par exemple, le fait qu'il ait des vehicules avec une vitesse maximale autorisé supérieur à 50km/h alors que le véhicule circule sur une autoroute dont la VMA est de 130 km/h. Ces erreurs semblent être des erreurs de saisies, cependant nous n'avons pas voulu nous attarder sur la correction de ces erreurs car cela demanderait avant la confirmation d'un expert métier dans le domaine de la sécurité routière. La qualité des données jouent un rôle important dans le précision des algorithmes de machine learning. Un adage bien connu dans le monde de la science des données permet de résumer cela et stipule : Garbage in, garbage out. 

Enfin, tout au long de l'étape de pré-processing, nous avons du faire des choix pour aggréger les différents datasets ainsi que pour sélectionner les variables à garder et celle à rejeter. Cette étape a occupé la majeur partie de notre temps car nous devions nettoyé, compilé et joindre plusieurs dataframe entre eux. Nous avons voulu prendre le temps nécéssaire afin de nous assurer de la qualité de nos données avant d'y appliquer les différents algorithme de machine learning. 

## 10/ Requirements

``` python pip install pygal_maps_fr```

`pip install folium`

`pip install apyori`

`pip install featurewiz`
