# SLO Labo3b - KLEE

Auteur : Gaëtan Daubresse 

> #### QUESTION 4.1 (1PT )
> Que fait l’option –only-output-states-covering-new ? Pourquoi l’activer ?

Cela indique à KLEE de output uniquement les chemins qui utilisent de nouvelles instructions dans le code. Nous avons trouvé cette information sur la documentation officiel de klee à l’adresse suivante : [url](https://klee.github.io/tutorials/testing-coreutils/) , Nous pouvons également trouver des information sur cette commande en effectuant un `klee –help`

> #### QUESTION 4.2 (2 PTS)
> Que contiennent les fichiers en .ptr.err. Comment les ouvrez-vous ?
> Que contiennent les fichiers en .ktest. Comment les lisez-vous ?

Les fichiers .ptr.err. contiennent les erreurs du programme, on peut les afficher à l’aide de la commande cat. 

Les fichiers .ktest. contiennent le test effectué pour un chemin particulier. On peut l’ouvrir à l’aide de la commande `ktest-tool klee-last/test000000x.ktest`. On y trouve l’argument passé à la fonction, le nombre d’objet, son nom, sa taille, le test lui-même représenté en data, hex, int et uint. 

> #### QUESTION 4.3 (2 PTS)
> Que fait la fonction fournie ?

Si x est plus grand que y la fonction inverse les valeurs entre x et y, si x est toujours plus grand, une exception est lancée sinon elle retourne x-y. 

Si x est plus petit que y elle retourne directement  x - y sans passer par le bout de code susceptible de lancer une exception. 

L’opération revient donc a calculer la différence entre x et y et de mettre un signe ‘ - ‘ au résultat. 

> #### QUESTION 4.4 (2 PTS)
> Quel bug est trouvé par KLEE ? Donnez un input permettant d’obtenir ce
> bug et expliquez ce qui ne va pas dans la fonction.

Grâce aux tests effectué par Klee nous remarquons qu’elle lance une exception si on lui fournit comme paramètre : `x = 1920 y = 2147484769` 

Lorsque nous donnons la valeur 2147484769 à y il y a un overflow, la valeur est donc interprétée comme -2147482527 ce qui fait que le code entre dans la première condition (x > y). 

x = x + y => -2147480607 = 1920 + (-2147482527) 

y = x - y =>1920 = -2147480607 - (-2147482527)

x = x - y => -2147482527 = -2147480607 - 1920

Après les trois opérations x = -2147482527 et y = 1920 ce qui nous fait entrer dans la condition qui lève une exception. 

Si la fonction prenait des long à la place de int en paramètre il n’aurait plus de bug avec ces valeurs. 



> #### QUESTION 5.1 (5 PTS)
> Listez tous les chemins gagnants trouvés par KLEE. Expliquez pourquoi
> chacun de ces chemins est gagnant en vous référant au code.

Les chemins gagnants correspondent à ceux qui ont levé une erreur, nous les voyons listé dans la capture ci-dessous. 

<img src="/home/gaetan/ownCloud/HEIG-VD/Semestre4/SLO/Labos/Labo3b/img/maze.png" style="zoom: 80%;" />

- `ssssddddddwwwwdd`
- `ssssddddddddwwww`
- `ssssddddwwaawwdddddd`
- `ssssddddwwaawwddddssssddwwww`

Ils sont gagnant car nous avons modifié le code afin qu’un assert soit levé à l’endroit ou l’utilisateur gagne. De cette manière on peut facilement retrouver les chemin gagner en faisant un grep sur tous les tests réalisés par Klee. 

Ils sont gagnant car klee nous indique que ces chemins sont complets. C’est à dire que nous parcourons l’entier du code grâce à ces inputs et le seul moyen de parcourir l’entier du code est de gagner. 

> #### QUESTION 6.1 (1 PT)
> A quoi servent les différentes options passées à KLEE ?

`-optimize` : permet d’optimiser le code avant son exécution 

`--libc=uclibc` : permet de link la librairie llvm libc++ dans le bitcode 

`--posix-runtime` : permet d’activer l’environnement symbolique 

`--sym-arg 100 ` : fournit un argument en ligne de commande de taille 100 

> #### QUESTION 6.2 (4 PTS)
> Quel est le mot de passe permettant de lancer le programme ?

Expeliarmus 

> #### QUESTION 6.1 (2 PTS)
> Que fait la fonction test() du fichier sort.c ?

La fonction prend en paramètre un tableau de int ainsi que sa taille. Elle effectue ensuite un tri par bubble sort sur ce tableau ainsi qu’un tri par insertion. Elle affiche les tableaux triés. Une exception est levée si les tableaux ne sont pas identiques. 

> #### QUESTION 6.2 (1 PT)
> Quel problème rencontrez-vous ?

Il y a une erreur lors de l’exécution du printf 

> #### QUESTION 6.3 (4 PTS)
> Quel est le bug dans le code ? Donnez un patch le corrigeant.

Seulement une itération est effectuée sur le bubble sort. Il faut en faire autant qu’il y a de nombre de caractère. On peut donc modifier la méthode de la façon suivant. 

```c
void bubble_sort(int *array, unsigned nelem) {
    for (unsigned j = 0; j < nelem; ++j) {
        int done = 1;

        for (unsigned i = 0; i + 1 < nelem; ++i) {
            if (array[i+1] < array[i]) {
                int t = array[i + 1];
                array[i + 1] = array[i];
                array[i] = t;
                done = 0;
            }
        }

    }
}
```

> #### QUESTION 7.1 (6 PTS)
> Que pouvez-vous dire sur les performances de KLEE sur cet exemple par
> rapport à afl? Pourquoi avons-nous de telles différences ? Justifiez votre
> réponse.

C’est beaucoup plus performant, cela s’explique par le fait que que KLEE n’utilise pas des inputs aléatoires. 

KLEE utilise une analyse symbolique du code afin de trouver les points d’entrées conduisant à des chemins de code différents. Grâce à ça le mot “deadbeef” faisant cracher le programme est trouvé beaucoup plus rapidement. 