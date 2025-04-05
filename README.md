# Solutions des Exercices Prolog - README

## Table des matières
1. [Exercice 7: inverser(L, LL) et palindrome(L)](#exercice-7-inverserL-LL-et-palindromeL)
2. [Exercice 8: not_membre(A, L)](#exercice-8-not_membreA-L)
3. [Exercice 9: delete(A, L, LL)](#exercice-9-deleteA-L-LL)
4. [Exercice 10: Dernier élément d'une liste](#exercice-10-dernier-élément-dune-liste)
5. [Exercice 11: est_trié(L)](#exercice-11-est_triéL)

---

## Exercice 7: inverser(L, LL) et palindrome(L)

### Code

```prolog
% Cas de base: inverser une liste vide donne une liste vide
inverser([], []).

% Cas récursif: inverser la queue et ajouter la tête à la fin
inverser([H|T], LL) :-
    inverser(T, RT),    % D'abord inverser la queue T en RT
    append(RT, [H], LL). % Ensuite ajouter la tête H à la fin de RT

% Définition du prédicat palindrome
palindrome(L) :-
    inverser(L, L).
```

### Explication détaillée

#### Signification des variables:
- **H** (Head): Le premier élément de la liste (la "tête")
- **T** (Tail): Tous les éléments restants (la "queue")
- **RT** (Reversed Tail): La queue T après avoir été inversée
- **LL** (Result List): La liste finale inversée complète

#### Fonctionnement:
1. **Cas de base**: Si la liste est vide, son inverse est aussi une liste vide.
2. **Cas récursif**: Pour inverser une liste non vide:
   - On sépare la liste en tête (H) et queue (T)
   - On inverse récursivement la queue pour obtenir RT
   - On ajoute la tête à la fin de RT pour obtenir la liste inversée complète

#### Exemple détaillé: inverser([a,b,c], Résultat)

**Appel initial**: `inverser([a,b,c], Résultat)`
1. La liste n'est pas vide, donc on utilise la 2ème règle
2. On identifie:
   - `H` = a (la tête)
   - `T` = [b,c] (la queue)
3. Pour continuer, Prolog doit d'abord résoudre `inverser([b,c], RT)`

**Premier appel récursif**: `inverser([b,c], RT)`
1. La liste n'est pas vide, donc on utilise la 2ème règle
2. On identifie:
   - `H` = b (la tête)
   - `T` = [c] (la queue)
3. Pour continuer, Prolog doit d'abord résoudre `inverser([c], RT2)`

**Deuxième appel récursif**: `inverser([c], RT2)`
1. La liste n'est pas vide, donc on utilise la 2ème règle
2. On identifie:
   - `H` = c (la tête)
   - `T` = [] (la queue est vide)
3. Pour continuer, Prolog doit d'abord résoudre `inverser([], RT3)`

**Troisième appel récursif**: `inverser([], RT3)`
1. La liste est vide, donc on utilise la 1ère règle (cas de base)
2. Selon cette règle, `RT3` = []
3. Cette branche de récursion est terminée

**Remontée de la récursion**: Résolution de `inverser([c], RT2)`
1. Maintenant que nous savons que `RT3` = [], nous pouvons continuer
2. `append(RT3, [H], RT2)` devient `append([], [c], RT2)`
3. Cela donne `RT2` = [c]

**Remontée de la récursion**: Résolution de `inverser([b,c], RT)`
1. Maintenant que nous savons que `RT2` = [c], nous pouvons continuer
2. `append(RT2, [H], RT)` devient `append([c], [b], RT)`
3. Cela donne `RT` = [c,b]

**Remontée finale**: Résolution de `inverser([a,b,c], Résultat)`
1. Maintenant que nous savons que `RT` = [c,b], nous pouvons continuer
2. `append(RT, [H], LL)` devient `append([c,b], [a], Résultat)`
3. Cela donne `Résultat` = [c,b,a]

#### Visualisation
```
                     inverser([a,b,c], Résultat)
                     /                       \
        inverser([b,c], RT)                append(RT, [a], Résultat)
        /                  \                     |
inverser([c], RT2)   append(RT2, [b], RT)      [c,b,a]
/                \         |
inverser([], RT3)  append(RT3, [c], RT2)
     |                    |
    []                   [c]
                           \
                           [c,b]
```

### Le prédicat palindrome

Un palindrome est une séquence qui se lit de la même façon dans les deux sens. Le prédicat `palindrome(L)` vérifie simplement si une liste est égale à son inverse.

```prolog
palindrome(L) :-
    inverser(L, L).
```

**Exemples**:
- `palindrome([r,a,d,a,r])` retourne `true` car l'inverse de [r,a,d,a,r] est [r,a,d,a,r].
- `palindrome([1,2,3,4])` retourne `false` car l'inverse de [1,2,3,4] est [4,3,2,1].

---

## Exercice 8: not_membre(A, L)

### Code

```prolog
% Cas de base: aucun élément n'est membre d'une liste vide
not_membre(_, []).

% Cas récursif: l'élément n'est pas dans la liste s'il n'est pas la tête
% et n'est pas dans la queue
not_membre(X, [H|T]) :-
    X \= H,         % X est différent de la tête
    not_membre(X, T). % Et X n'est pas dans la queue
```

### Explication

Le prédicat `not_membre(X, L)` vérifie si un élément X n'appartient pas à une liste L.

1. **Cas de base**: N'importe quel élément n'est pas membre d'une liste vide.
2. **Cas récursif**: Un élément X n'est pas dans une liste non vide si:
   - X est différent de la tête (H)
   - X n'est pas dans la queue (T)

**Exemple**: `not_membre(c, [a,b,d])`
1. c ≠ a, donc on continue
2. c ≠ b, donc on continue
3. c ≠ d, donc on continue
4. On arrive à une liste vide, le cas de base est satisfait
5. Résultat: `true`

**Contre-exemple**: `not_membre(b, [a,b,d])`
1. b ≠ a, donc on continue
2. b = b, donc la condition échoue et le prédicat retourne `false`

---

## Exercice 9: delete(A, L, LL)

### Code pour la suppression de la première occurrence

```prolog
% Cas de base: si X est la tête, supprimer en retournant juste la queue
delete(X, [X|T], T).

% Cas récursif: si X n'est pas la tête, garder la tête et chercher dans la queue
delete(X, [H|T], [H|T1]) :-
    X \= H,
    delete(X, T, T1).
```

### Code pour la suppression de toutes les occurrences

```prolog
% Cas de base: rien à supprimer d'une liste vide
delete_all(_, [], []).

% Si X correspond à la tête, l'ignorer et continuer avec la queue
delete_all(X, [X|T], L) :-
    delete_all(X, T, L).

% Si X ne correspond pas à la tête, garder la tête et continuer avec la queue
delete_all(X, [H|T], [H|L]) :-
    X \= H,
    delete_all(X, T, L).
```

### Explication détaillée de delete(A, L, LL)

#### Signification des variables:
- **X** : L'élément à supprimer
- **L** : La liste d'origine
- **LL** : La liste résultante (sans la première occurrence de X)
- **H** : Premier élément de la liste (tête)
- **T** : Reste de la liste (queue)
- **T1** : La queue après suppression de X

#### Fonctionnement:
1. **Cas de base**: Si l'élément à supprimer (X) est la tête de la liste, alors on retourne simplement la queue.
2. **Cas récursif**: Si X n'est pas la tête, on garde la tête dans le résultat et on cherche X dans la queue.

#### Exemple détaillé: delete(b, [a,b,c,b,d], Résultat)

**Appel initial**: `delete(b, [a,b,c,b,d], Résultat)`
1. La tête est `a` et l'élément à supprimer est `b`
2. `a` ≠ `b`, donc nous utilisons la 2ème règle
3. On identifie:
   - `X` = b (élément à supprimer)
   - `H` = a (la tête)
   - `T` = [b,c,b,d] (la queue)
4. Nous gardons `a` dans le résultat et continuons avec la queue
5. Pour continuer, Prolog doit résoudre `delete(b, [b,c,b,d], T1)`

**Premier appel récursif**: `delete(b, [b,c,b,d], T1)`
1. La tête est `b` et l'élément à supprimer est `b`
2. `b` = `b`, donc nous utilisons la 1ère règle (cas de base)
3. On identifie:
   - `X` = b (élément à supprimer)
   - `[X|T]` = [b,c,b,d] donc `T` = [c,b,d]
4. Selon cette règle, `T1` = [c,b,d]
5. Cette branche de récursion est terminée

**Remontée finale**: Résolution de `delete(b, [a,b,c,b,d], Résultat)`
1. Maintenant que nous savons que `T1` = [c,b,d], nous pouvons finaliser
2. `[H|T1]` devient `[a|[c,b,d]]` = [a,c,b,d]
3. Donc `Résultat` = [a,c,b,d]

#### Visualisation:
```
delete(b, [a,b,c,b,d], Résultat)
    |
    +-- H = a, X = b, T = [b,c,b,d]
    |   (a ≠ b, donc on garde a et on continue)
    |
    +-- delete(b, [b,c,b,d], T1)
        |
        +-- X = b, [X|T] = [b,c,b,d], T = [c,b,d]
        |   (b = b, donc on retourne juste T)
        |
        +-- T1 = [c,b,d]
    |
Résultat = [a|T1] = [a,c,b,d]
```

### Explication détaillée de delete_all(X, L, LL)

#### Exemple détaillé: delete_all(b, [a,b,c,b,d], Résultat)

**Appel initial**: `delete_all(b, [a,b,c,b,d], Résultat)`
1. La tête est `a` et l'élément à supprimer est `b`
2. `a` ≠ `b`, donc nous utilisons la 3ème règle
3. On identifie:
   - `X` = b (élément à supprimer)
   - `H` = a (la tête)
   - `T` = [b,c,b,d] (la queue)
4. Nous gardons `a` dans le résultat et continuons avec la queue
5. Pour continuer, Prolog doit résoudre `delete_all(b, [b,c,b,d], L)`

**Premier appel récursif**: `delete_all(b, [b,c,b,d], L)`
1. La tête est `b` et l'élément à supprimer est `b`
2. `b` = `b`, donc nous utilisons la 2ème règle
3. Cette règle nous dit d'ignorer cet élément et de continuer
4. Pour continuer, Prolog doit résoudre `delete_all(b, [c,b,d], L)`

**Deuxième appel récursif**: `delete_all(b, [c,b,d], L)`
1. La tête est `c` et l'élément à supprimer est `b`
2. `c` ≠ `b`, donc nous utilisons la 3ème règle
3. On identifie:
   - `X` = b (élément à supprimer)
   - `H` = c (la tête)
   - `T` = [b,d] (la queue)
4. Nous gardons `c` dans le résultat et continuons
5. Pour continuer, Prolog doit résoudre `delete_all(b, [b,d], L1)`

**Troisième appel récursif**: `delete_all(b, [b,d], L1)`
1. La tête est `b` et l'élément à supprimer est `b`
2. `b` = `b`, donc nous utilisons la 2ème règle
3. Cette règle nous dit d'ignorer cet élément et de continuer
4. Pour continuer, Prolog doit résoudre `delete_all(b, [d], L1)`

**Quatrième appel récursif**: `delete_all(b, [d], L1)`
1. La tête est `d` et l'élément à supprimer est `b`
2. `d` ≠ `b`, donc nous utilisons la 3ème règle
3. On identifie:
   - `X` = b (élément à supprimer)
   - `H` = d (la tête)
   - `T` = [] (la queue)
4. Nous gardons `d` dans le résultat et continuons
5. Pour continuer, Prolog doit résoudre `delete_all(b, [], L2)`

**Cinquième appel récursif**: `delete_all(b, [], L2)`
1. La liste est vide, donc nous utilisons la 1ère règle (cas de base)
2. Selon cette règle, `L2` = []
3. Cette branche de récursion est terminée

**Remontée de la récursion**:
1. `delete_all(b, [d], L1)` : L2 = [], donc L1 = [d|L2] = [d]
2. `delete_all(b, [b,d], L1)` : L1 = [d] (l'élément b est ignoré)
3. `delete_all(b, [c,b,d], L)` : L1 = [d], donc L = [c|L1] = [c,d]
4. `delete_all(b, [b,c,b,d], L)` : L = [c,d] (l'élément b est ignoré)
5. `delete_all(b, [a,b,c,b,d], Résultat)` : L = [c,d], donc Résultat = [a|L] = [a,c,d]

#### Visualisation:
```
delete_all(b, [a,b,c,b,d], Résultat)
    |
    +-- 3ème règle : garder a
    |   delete_all(b, [b,c,b,d], L)
    |       |
    |       +-- 2ème règle : ignorer b
    |       |   delete_all(b, [c,b,d], L)
    |       |       |
    |       |       +-- 3ème règle : garder c
    |       |       |   delete_all(b, [b,d], L1)
    |       |       |       |
    |       |       |       +-- 2ème règle : ignorer b
    |       |       |       |   delete_all(b, [d], L1)
    |       |       |       |       |
    |       |       |       |       +-- 3ème règle : garder d
    |       |       |       |       |   delete_all(b, [], L2)
    |       |       |       |       |       |
    |       |       |       |       |       +-- 1ère règle : L2 = []
    |       |       |       |       |
    |       |       |       |       +-- L1 = [d|L2] = [d]
    |       |       |       |
    |       |       |       +-- L1 = [d] (b ignoré)
    |       |       |
    |       |       +-- L = [c|L1] = [c,d]
    |       |
    |       +-- L = [c,d] (b ignoré)
    |
    +-- Résultat = [a|L] = [a,c,d]
```

---

## Exercice 10: Dernier élément d'une liste

### Code

```prolog
% Cas de base: dans une liste à un seul élément, cet élément est le dernier
dernier([X], X).

% Cas récursif: le dernier élément d'une liste est le dernier élément de sa queue
dernier([_|T], X) :-
    dernier(T, X).
```

### Explication

Le prédicat `dernier(L, X)` trouve le dernier élément X d'une liste L.

1. **Cas de base**: Si la liste ne contient qu'un seul élément, alors cet élément est le dernier.
2. **Cas récursif**: Pour une liste plus longue, le dernier élément est le dernier élément de la queue.

**Exemple**: `dernier([1,2,3,4], X)`
1. La liste a plusieurs éléments, donc on utilise la 2ème règle
2. On ignore la tête (1) et on cherche le dernier élément dans [2,3,4]
3. On ignore la tête (2) et on cherche dans [3,4]
4. On ignore la tête (3) et on cherche dans [4]
5. [4] est une liste avec un seul élément, donc on utilise la 1ère règle
6. X = 4

---

## Exercice 11: est_trié(L)

### Code

```prolog
% Cas de base: liste vide et liste à un élément sont toujours triées
est_trié([]).
est_trié([_]).

% Cas récursif: une liste est triée si premier ≤ second et le reste est trié
est_trié([A, B|T]) :-
    A =< B,
    est_trié([B|T]).
```

### Explication

Le prédicat `est_trié(L)` vérifie si une liste L est triée en ordre croissant.

1. **Cas de base**:
   - Une liste vide est toujours triée.
   - Une liste avec un seul élément est toujours triée.
2. **Cas récursif**: Une liste avec au moins deux éléments est triée si:
   - Le premier élément (A) est inférieur ou égal au deuxième élément (B)
   - ET la liste commençant par B jusqu'à la fin est également triée

**Exemple**: `est_trié([1,2,3,4])`
1. 1 ≤ 2, donc on continue et on vérifie si [2,3,4] est trié
2. 2 ≤ 3, donc on continue et on vérifie si [3,4] est trié
3. 3 ≤ 4, donc on continue et on vérifie si [4] est trié
4. [4] est une liste avec un seul élément, donc elle est triée (cas de base)
5. Résultat: `true`

**Contre-exemple**: `est_trié([1,3,2,4])`
1. 1 ≤ 3, donc on continue et on vérifie si [3,2,4] est trié
2. 3 > 2, la condition échoue
3. Résultat: `false`
