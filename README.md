## 1. Week-ends Reddit

Cette question utilise des données dérivées de l'archive Reddit Comment, qui est une collection de chaque commentaire Reddit, distribué sous forme de 150 Go de JSON compressé.

Le fichier fourni `reddit-counts.json.gz` contient un décompte du nombre de commentaires publiés quotidiennement dans chaque subreddit de province canadienne et dans /r/canada lui-même.
(Les valeurs différeront légèrement des valeurs actuelles: les fuseaux horaires peuvent ne pas être définis correctement, il y aura donc des commentaires classés de manière incorrecte vers minuit : je suis prêt à vivre avec cela.)
Encore une fois, le format est du JSON compressé ligne par ligne. Il s'avère que Pandas (≥0.21) peut gérer la compression automatiquement et nous n'avons pas besoin de décompresser explicitement :
```bash
counts = pd.read_json(sys.argv[1], lignes=True)
```
1.1 Test T de Student
Question: y a-t-il un nombre différent de commentaires Reddit publiés en semaine qu'en week-end ?

Pour cette question, nous examinerons uniquement les valeurs (1) en 2012 et 2013, et (2) dans le subreddit /r/canada. Commencez par créer un DataFrame avec les données fournies et séparez les jours de la semaine des week-ends.

Astuce : recherchez datetime.date.weekday soit 5 ou 6.

Complétez le programme reddit_weekends.py pour cette question et suivez le bloc-notes reddit_weekends.ipynb. Des notes vous seront attribuées pour votre travail dans le fichier python, ainsi que pour les chiffres et les réponses courtes dans le cahier. Le fichier python a également une configuration de code à exécuter pour vous ; vous pouvez prendre le fichier de données comme input via la ligne de commande :

bash
Copy
Edit
python3 reddit_weekends.py ./data/reddit-counts.json.gz
Notez que la sortie produite par la commande ci-dessus ne sera pas marquée; nous testerons vos fonctions directement via des tests unitaires comme d'habitude.

1.2 Solution 1: la transformation des données pourrait nous aider.
Complétez les graphiques dans reddit_weekends.ipynb
Jetez un oeil à l'histogramme des données. Vous remarquerez qu'il est biaisé : c'est la raison pour laquelle il n'était pas distribué normalement dans la dernière partie. Transformez les décomptes afin que les données n'échouent pas au test de normalité. Options probables pour les transformations : np.log, np.exp, np.sqrt, counts**2. Choisissez celle qui se rapproche le plus d'une distribution normale. [A moins que j'ai raté quelque chose, aucun d'entre eux ne passera le test de normalité. Le mieux que je puisse obtenir : une variable avec des problèmes de normalité, une correcte ; pas de problèmes de variance égale.]

1.3 Solution 2 : le théorème central limite pourrait nous aider.
Complétez reddit_weekends.py:central_limit_theorem()
Complétez les graphiques dans reddit_weekends.ipynb
Le théorème central limite dit que si nos nombres sont suffisamment grands et que nous examinons les moyennes de l'échantillon, alors le résultat devrait être normal. Essayons cela : nous combinerons tous les jours de semaine et de week-end de chaque paire année/semaine et prendrons la moyenne de leurs décomptes (non transformés).

Astuce : vous pouvez obtenir une "année" et un "numéro de semaine" à partir des deux premières valeurs renvoyées par date.isocalendar(). Cette année et ce numéro de semaine vous donneront un identifiant pour la semaine. Utilisez Pandas pour regrouper par cette valeur et agréger en prenant la moyenne. Remarque : l'année renvoyée par isocalendar n'est pas toujours la même que l'année de la date (autour de la nouvelle année). Utilisez l'année de l'isocalendar, qui est correcte dans ce cas.

1.4 Solution 3 : un test non paramétrique pourrait nous aider.
Complétez reddit_weekends.py:mann_whitney_u_test()
L'autre option que nous avons dans notre boîte à outils : un test statistique qui ne se soucie pas autant de la forme de son input. Le test U de Mann – Whitney ne suppose pas de valeurs distribuées normalement ni que la variance des deux distributions sont égales.

Effectuez un test U sur les décomptes (initiaux non transformés, non agrégés). Notez que nous devrions faire ici un test bilatéral, qui correspondra aux autres analyses. Assurez-vous que les arguments de la fonction sont corrects.

Encore une fois, notez que nous modifions subtilement la question à nouveau. Si nous parvenons à une conclusion à cause d'un test U, c'est quelque chose comme "il n'est pas également probable qu'il y ait un plus grand nombre de commentaires le week-end par rapport aux jours de semaine".

1.5 Questions
Répondez à ces questions dans la section spécifiée dans reddit_weekends.ipynb :

Laquelle des quatre transformations suggérées vous rapproche le plus de satisfaire les hypothèses d'un test T ?
Parmi les quatre approches, laquelle réussit le mieux à répondre à la question initiale : "y a-t-il un nombre différent de commentaires Reddit publiés en semaine et le week-end ?" Pourquoi ?



