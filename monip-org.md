# MONIP.org XSS ⭐️
### Bonjour, aujourd'hui je vais vous montrer comment j'ai trouvé une XSS sur le fameux site <a href="http://www.monip.org">MONIP.ORG</a>
<img src="https://i.imgur.com/eMqp2un.jpg"/>
<hr />

## C'est quoi une XSS ? 😶
Une XSS (Cross Site Scripting) est une faille permettant l'injection de code (JS/HTML/CSS).
### Un exemple s'impose
`coucou.php`
```diff
<?php 
$name = $_GET["name"];
echo "Coucou ".$name;
?>
```
Le petit bout de code ici présent est vulnérable, car la variable `$name` présente aucune sécurité contre les XSS <br />
Voici un petit test : <br />
`coucou.php?name=<h1>HXFF</h1>` <br />
Voilà Voilà, la page exécutera le HTML. <br />
### Il existe plusieurs types de Cross Site Scripting 😉
Reflected, Stored et Dom Based <br />
Aujourd'hui, nous allons seulement parler des XSS Reflected car sur <a href="http://monip.org">MONIP.ORG</a>, elle est Reflected, passons à l'exploitation.
### Exploitation du site MONIP.ORG 😎
Comme vous le savez, ces sites vous permettent de connaitre votre IP, mais sur certains, il est indiqué si oui ou non vous utilisez un proxy.<br />
J'ai donc eu l'idée d'injecter du code dans l'entête X-Forwarded-For.<br />
### C'est quoi X-Forwarded-For ? 😐
Voici la définition de Wikipedia : <br />
L'en-tête X-Forwarded-For (XFF) est un en-tête standard pour identifier l'adresse IP d'origine d'un client se connectant à un serveur Web via un proxy HTTP<br /><br />
Bon, place à la commande que j'ai utilisé (via cURL) : 
`curl -H "X-Forwarded-For: <script>alert(1);<script>" monip.org`<br />
<img src="https://i.imgur.com/mJN6rhg.png"/>
Le résultat de l'entête X-Forwarded-For est reflected (et interprété) sur le site monip.org, vous pouvez donc y injecter du code.
### Comment fixer la faille XSS ? 😇
Il y'a plusieurs moyen mais la meilleure façon selon moi reste htmlspecialchars ! 
```diff 
<?php
$name = htmlspecialchars($_GET["name"], ENT_QUOTES);
echo "Coucou ".$name;
?>
```

Voilà c'est déjà la fin ! j'espère que vous avez aimé le Writeup et le mini tuto sur les XSS ahah ! 

### by <a href="https://www.github.com/http-x-forwarded-for/">HXFF</a> (d'ou le pseudo...)
