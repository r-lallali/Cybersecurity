story 2

US 2:Vérification de la reconnaissance

EN TANT QUE analyste SOC,

JE VEUX vérifier si le malware est déjà connu,

AFIN DE déterminer s’il s’agit d’une menace nouvelle ou référencée.

Nous pouvons revenir sur notre savestate de notre ordinateur sain puisque nous aurons besoin d'internet pour cette story. 
Pour récupérer le hash de notre malware, nous devons créer un terminal powershell et executer la commande GetFile-Hash "chemin vers les executable" -Algorythme SHA256.
Cela nous donne le ash de nos fichiers.
Pour vérifier si ces derniers sont déja connu, nous pouvons comparer ces hash avec certains déjà présent sur virrustotal.
Le premier executable est considéré selon 26 sources sur 71 comme un fichier malveillant. Nous pouvons voir qu'il est notamment déjà utiliser pour certain keylogger.
Le second executable est considérer selon 49 sources sur 71 comme un fichier malveillant. Nous retrouvons une petite analyse de code qui indique que ce code est un keylogger
qui utilise le framework QT. Il créer aussi une persistance en créant un dossier WIndsyst dans lequel se retrouve ses différentes dll et ses 2 executables via Xcopy et établit
ses persistances dans windows run.

Nous pouvons donc en conclure que le malware est déjà connu.