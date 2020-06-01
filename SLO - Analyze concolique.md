# SLO - Analyze concolique



## 4 Analyser une Fonction Simple

**Q 4.1**

```
Cette option limite le nombre de tests que l'on génère uniquement aux états qui couvrent du nouveau code.
Sans cette option, on se retrouverait avec plus de 6000 cas de tests
```

**Q 4.2**

```
.ptr.err : fichier contenant les erreurs de types store & load à des endroits invalides de la mémoire

.ktest   : fichiers de test de KLEE, ils contiennent des données symboliques permettant de reproduire le chemin que KLEE a suivi
```

**Q 4.3**

```
prend 2 int en paramètre,
si le premier param est plus grand que le 2e, inverse leur valeurs.
	si le premier paramètre est toujours plus grand que le 2e, 	alors on écrit un 	         message d'erreur dans stderr et on arrête l'exécution du code
	
ensuite on retourne la valeur de x-y (donc 2e param - premier param


sinon, on renvoit la valeur du premier param - celle du 2e.

```

**Q 4.4**

```
la partie "assert(0)" du code n'est jamais atteinte, du à la condition du if qui ne sera jamais vraie
```





## 5 Labyrinthe

**Q 5.1**

```
Lorsqu'une erreur est relevée, cela indique que nous sommes sur un chemin gagnant
```





## 6 Keygen avec KLEE

**Q 6.1**

```
--optimize      : 	optimise le code avant son exécution avec plusieurs optimisations du                     compilateur (LLVM optimisation)
--libc          : 	ajoute une libraire avant l'exécution du prog
--posix-runtime : 	permet d'ajouter une librairie supplémentaire


libc et posix-runtime ensemble permettent également de "redéfinir" la méthode main du prog avec une fonction spéciale "klee_init_env" qui permet d'altérer les commandes "normales" afin de supporter notamment les arguments symboliques.


--sym-arg       :	permet d'indiquer le nombre et la taille des arguments que l'on passe                     à posix-runtime
```

**Q 6.2**

```

```





## 7 Algorithmes de Tri

**Q 7.1**

```
test et compare le bubble_sort et le insertion_sort d'un même tableau de int
```

**Q 7.2**

```
il faut ajouter le flag suivant pour autoriser les calls de fonctions externes avec des arguments symboliques :
--allow-external-sym-calls
```

**Q 7.3**

```
il faut ajouter le flag suivant pour autoriser les calls de fonctions externes avec des arguments symboliques :
--allow-external-sym-calls
```





## 8 Comparaison avec le Fuzzer

**Q 8.1**

```
KLEE n'ayant aucune paramètre entrant "aléatoires" on obtient forcément une meilleure performance
```

