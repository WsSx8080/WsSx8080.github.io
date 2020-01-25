# HackTheBox - Cartographer Writeup

>  Certains hackers développent un nouveau serveur de commande et de contrôle. Pouvez-vous entrer par effraction et voir ce qu'ils font ? 

En visitant le site, nous atterissons directement sur une page de connexion nous demandant un `username` et un `password`.

Dans ces cas là, bien souvent, la première chose que j'effectue est de vérifier si une faille sqli est présente ou non.
Donc pour vérifier, je rentre l'username `' or 1=1#` :

<img src="https://i.imgur.com/Dcq25uS.png">

> Oui mais pourquoi `' or 1=1#` ?

C'est le principe d'une injection sql. Imaginons que le code de la page soit quelque chose de ce genre :
```sql
"SELECT * FROM utilisateurs WHERE login='$login' AND password='$password'"
```
En ayant pris des valeurs comme `' or 1=1#` pour l'username et `jeankevin` pour le mot de passe, la requête va s'effectuer
comme ceci :
```sql
"SELECT * FROM utilisateurs WHERE login='' OR 1=1 # ' AND password='jeankevin'");
```
Donc dans ce cas de figure, on voit bien que la condition passe car 1=1 (jusqu'à preuve du contraire) et
ensuite on rajoute le `#` pour enlever le reste de la requête. Ainsi, nous sommes connectés.

Et ... Bingo ! ça nous amène vers une autre page dont le chemin d'accès est `/panel.php?info=home` qui nous indique que
le site est en construction.

Je regarde le code source :
```html
<html>
<head>
    <title>Cartographer - Panel</title>
    <link rel='stylesheet' href='style.css' type='text/css' />
</head>
<body>
    <div class="content-container">
        <div class="blur"></div>
    </div>
    <div class="loginform">
        <center>
            <img src="logo.png" /><br><br>
            <br>Cartographer<br>Is Still<br>Under Construction!        </center>
    </div>
</body>
</html>
```

Rien d'intéressant ... Cependant le chemin d'accès de la page me semble étrange. En effet, il n'est pas rare de trouver
de nombreuses failles dans des structures similaires (rfi, lfi etc.).

J'essaie donc d'accèder à `../../../../../../../../../../../etc/passwd`, sans succès, ça m'affiche `Not Found!`.

<img src="https://i.imgur.com/WtV7hAU.png">

Là je stagne ... Or à ce moment là
je viens de me rappeler que la difficulté du chall est facile, j'essaie donc d'accèder à `/panel.php?info=flag` et MIRACLE !
ça marche !

Ainsi j'accède au flag.

<img src="https://i.imgur.com/10MRE44.png">

## Conclusion

Un chall sympa à faire accessible aux débutants.

Challenge réalisé par : <a href="https://github.com/http-x-forwarded-for">HXFF</a> et <a href="https://github.com/Aaaaalpha">Alpha</a>.
