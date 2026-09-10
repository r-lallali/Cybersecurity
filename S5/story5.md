story 5)

US 5:Évaluation des risques

EN TANT QUE responsable sécurité,

JE VEUX évaluer l’impact du malware sur la machine,

AFIN DE définir des mesures de mitigation adaptées.

Après tous nos tavaux d'analyse, nous pouvons en arriver à la conclusion que notre malware est un keylogger. Il s'agit d'un virrus qui va récupérer tous ce qu'un utilisateur va taper sur son clavier. Il faut en
général très attention à ce genre de malware puisqu'ils peuvent permettre de retrouver les mot de passe des personnes infecté.
Dans notre cas, notre virrus créer une copie de lu même à la racine de notre ordinateur, dans un dossier pouvant laisser penser qu'il s'agit de fichier nécessaire pour le fonctionnement de windows. Nous avons pu 
remarquer qu'il y a de l'activité avec ce malware uniquement lorsque l'on tape dans le terminal qu'il ouvre precedemment. Il stock dans un fichier log ce l'utilisateur tape sur son clavier.Nous remarqons aussi qu'il y a une erreur lorsque env.exe cherche à être executé, il manque 
un dll. C'est pourtant ici que nous avons pu trouver les fonctions permettant l'envoie d'un mail. 

Nous constatons donc que le malware que l'on étudie ne récupère pas tous ce que l'utilisateur tape sur son clavier et est incapable d'envoyer des données. Il est donc inofensif.