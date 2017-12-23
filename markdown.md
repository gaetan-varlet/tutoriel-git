# Markdown, langage de balisage léger

### Paragraphes
Ceci est un paragraphe de texte.

Ceci est un autre paragraphe de texte !
```
Ceci est un paragraphe de texte.

Ceci est un autre paragraphe de texte !
```

### Le retour à la ligne
Ligne sans espace à la fin.
Ligne avec un espace à la fin.
Ligne avec 2 espaces à la fin.  
Troisième ligne.
```
Ligne sans espace à la fin.
Ligne avec un espace à la fin.
Ligne avec 2 espaces à la fin.  
Troisième ligne.
```

### Emphase faible (italique)
Voici un mot *important* à mon sens
```
Voici un mot *important* à mon sens
```

### Emphase forte (gras)
Voici des mots **très importants**, j'insiste !
```
Voici des mots **très importants**, j'insiste !
```

### Les titres
```
# Titre de niveau 1

## Titre de niveau 2

### Titre de niveau 3
```

### Les listes à puces
- Une puce
- Une autre puce
- Et encore une autre puce !
```
- Une puce
- Une autre puce
- Et encore une autre puce !
```

### Les listes de tâches
- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request
```
- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request
```

### Les listes à puces numérotées
1. Et de un
2. Et de deux
3. Et de trois
```
1. Et de un
2. Et de deux
3. Et de trois
```

### Les citations
> Ceci est un texte cité. Vous pouvez répondre
> à cette citation en écrivant un paragraphe
> normal juste en-dessous !
```
> Ceci est un texte cité.
> Vous pouvez répondre à cette citation en écrivant un paragraphe normal juste en-dessous !
```

On peut aussi mettre des listes dans les citations :

> **Note:**
> - StackEdit is accessible offline after the application has been loaded for the first time.
> - Your local documents are not shared between different browsers or computers.
```
> **Note:**
> - StackEdit is accessible offline after the application has been loaded for the first time.
> - Your local documents are not shared between different browsers or computers.
```

### Bloc de code
Voici un code en Java :
````java
```java
int main(){
  System.out.println("Hello world!\n");
  return 0;
}
```
````

### Code en ligne
`System.out.println()` permet d'afficher du texte en Java
```
`System.out.println()` permet d'afficher du texte en Java
```

### Les liens
Rendez-vous sur le [Site du Zéro](http://www.siteduzero.com) pour tout apprendre à partir de Zéro !
```
Rendez-vous sur le [Site du Zéro](http://www.siteduzero.com) pour tout apprendre à partir de Zéro !
```

### Les images
Le texte entre crochets est affiché si l'image n'est pas chargée.  
Le titre entre guillemets est lu par les navigateurs textuels et affiché au survol de l'image
![Google logo](https://www.google.fr/images/srpr/logo11w.png "google logo")
```
![Google logo](https://www.google.fr/images/srpr/logo11w.png "google logo")
```

### Barre de séparation
-----------------
```
-----------------
```

### Les tableaux

| Header 1      |     2 header    |   header 3 |
| ------------- |: -------------: | ---------: |
| 1 Online      |        1        |      value |
| Line 2        |        2        |      value |
| 3 Online      |        3        |      value |

```
| Header 1      |     2 header    |   header 3 |
| ------------- |: -------------: | ---------: |
| 1 Online      |        1        |      value |
| Line 2        |        2        |      value |
| 3 Online      |        3        |      value |

```

### Echappement des caractères

Pour afficher les caractères suivants, il faut les échapper avec un antislash `\` :
```
\   backslash
`   backtick
*   asterisk
_   underscore
{}  curly braces
[]  square brackets
()  parentheses
#   hash mark
+   plus sign
-   minus sign (hyphen)
.   dot
!   exclamation mark
```
