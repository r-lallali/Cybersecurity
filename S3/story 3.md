story 3

US 3:Analyse statique

EN TANT QUE reverse engineer,

JE VEUX analyser statiquement le malware,

AFIN DE comprendre sa structure, ses dépendances et ses fonctionnalités.

L'analyse statique du malware consiste à rechercher dans nos fichiers tous ce qui peut être trovuer sans executer notre fichier. Pour cela nous allons avoir besoin de 
différents outils : Detect It Easy (DIE) et Ghidra. Ce premier est un logiciel qui sert à identifier le type de fichier mais aussi le compilateur et d'autres informations
concernat le fichier tels que les imports effectuer dans celui ci. Le second est un décompilateur. Il va transformer le langage machine du fichier executable en langage plus
compréhensible par un humain.

Grâce à notre première analyse effectuer avec DIE, nous pouvons constater que le malware a été codé en C++ et compilé avec Mingw. Nous avons ensuite pus regarder les sections.
les sections sont divisé en plusieurs catégories, .data, .text, ... La section .text correspond à du texte présent dans le code, .data correspond à du contenu présent dans les
variables. Nous pouvons voir que dans la section .text, aucun texte n'est vraiment lisible mais nous retrouvons cependant dans .data certains texte lisible comme Xcopy qui
correspond à une commande windows pour créer une copie d'un fichier ou alors du texte présent dans l'invité de commande.
Nous pouvons aussi retrouvé les imports de nos executable, notamment, nous pouvons voir que RES contient certains apport de dll tel quel QT5core.dll, libgcc_s_dw2-1.dll ou
encore libwiinpthread-1.dll.
Dans les string, nous pouvons aussi retrouvons quelques extrais déchiffrable. Ces strings sont des morceaux de code que DIE va essayer de convertir en ascii.

Une fois cette première analyse effectuer, nous pouvons partir d'une première idée que le premier executable RES.exe permet de créer une copie de lui même et va executer une
fonction. L'autre fichier va être appelé va

Une fois ces premières idées sur ce que peut faire ce malware, nous pouvons nous lancer dans sa décompilation avec Ghidra. Le code décompiler sera en assembleur, un langage très
bas niveau, très proche du langage machine. Les noms de variable et des fonctions ne sont pas écrit en clair, nous retrouvons surtout les adresses mémoires de ce qui est appelé.

Nous commençons l'analyse du code décompilation de RES.exe. Nous retrouvons en grande majorité du code servant à l'initialisation donc du code pas lié à la dangerosité de notre
malware. Après avoir analyser de nombreuse fonction, je trouve enfin ma première fonction importante en recherchant des mots trouvé dans mes string de DIE, notamment grâce la
phrase "codé par le magnifique Hafnium !". Cette fonction "FUN_004035a0", elle est la fonction principale de cepremier executable. Elle permet d'initialiser QT, affiche le 
message "codé par le magnifique Hafnium !", crée un thread secondaire tout en installant sa persistance dans le dossier WindSyst. Nous pouvons voir que ce thread appel un 
pointeur qui pointe vers une fonction qui ne se situe pas dans ce fichier executable. 

Nous poursuivons notre analyse cette fois avec le code décompilé de Env.exe. Comme dans le premier fichier, nous retrouvons en grande majorité du code qui ne nous intéresse
pas. Après quelque recherche, nous arrivons à trouver la fonction principale de ce fichier, FUN_00401630. Cette dernière ava appeler les autres fonctions qui vont être 
importantes pour nous. On va notamment y trouver une fonction qui gert la création d'un interface SMTP. Il s'agit d'un interface permettant d'envoyer des mail. La fonction
FUN_004016b0 configure elle le serveur SMTP, on y retrouve le destinataire de ces mails.La fonction FUN_004016b0quant à elle permet la lecture d'un fichier de log traçant les actions effectuer par
l'utilisateur.