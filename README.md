:fr: [fr](#fr-correction-du-bug-dencodage-des-caract%C3%A8res-de-gliffy)
## :us: Fixed of the Gliffy Chrome App characters encoding bug (de, fr and ru)
The Chrome [Gliffy Diagrams](https://chrome.google.com/webstore/detail/gliffy-diagrams/bhmicilclplefnflapjmnngmkkkkpfad?hl=en) allows you to draw diagrams with ease and works offline.

But the latest version (`1.0.32`) of the application suffers from a problem of encoding the wording of the translations:
- `GLIFFY.DICTIONARY.de: "Einige Ihrer Ãnderungen sind nicht gespeichert."...`
- `GLIFFY.DICTIONARY.fr: "Vos modifications n'ont pas Ã©tÃ© enregistrÃ©es"...`
- `GLIFFY.DICTIONARY.ru: "Ð»Ð¸ÑÑ Ð½ÐµÑÐ¾ÑÑÐ°Ð½ÐµÐ½Ð½ÑÐµ Ð´Ð°Ð½Ð½ÑÐµ."... `

### Gliffy, Inc. does not maintain its stand-alone version (664467 users)
> Hi François,
> Thanks for reaching out, and I'm sorry you've run into this!
The free Chrome app is a limited version of Gliffy Online, so it does not have any other features or capabilities. We, unfortunately, will not be doing any updates to the Chrome app.  :-1:

### Fixed bug with modified `editor.html` :ambulance:
1. Close the Gliffy Diagrams application
1. Identify the [`Profile Path`](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/user_data_dir.md#Current-Location) by typing `chrome://version` in the Chrome URL bar
1. Move to `your_profile_path\Extensions\bhmicilclplefnflapjmnngmkkkkpfad\1.0.32_0` which is the application folder
1. Rename the `editor.html` file to `editor.html.origin` for security
1. Replace the original file by copying the `editor.html` file from the repository
1. Launch Gliffy Diagrams and see the change: :+1:
    - :de: `Entität-Beziehung ...`
    - :fr: `Entité-Relation ...`
    - :ru: `Объект-Взаимосвязь ...` 
### Explanation of the correction :mag:
The application uses a javascript template system ([handlebars.js](https://github.com/wycats/handlebars.js)) with internationalization `i18n` labels:
```html
editor.html
<a ... href="#gliffy-sidebar-com-gliffy-library-basic"> {{i18n SIDEBAR_BASIC_SHAPES}} </a>
```
#### Labels
The labels are stored in a dictionary by language:
```js
editor.html
1983 GLIFFY.DICTIONARY.de = $.extend(GLIFFY.DICTIONARY.de, {...,"SIDEBAR_BASIC_SHAPES": "Grundlegende Formen"}
```
They are then "read" in this dictionary when using the template:
```js
js/editor.js
38 {result = this.dict[str]}
```
But they are not encoded correctly (double encoding utf-8), I modified `editor.js` and the [correct encoding is restored](https://stackoverflow.com/questions/13356493/decode-utf-8-with-javascript/34926911#answer-13691499):
```js
js/editor.js
38 {result = decodeURIComponent(escape(this.dict[str])}}
```
#### Correct encoding of `editor.html` :trophy:
I applied the correct decoding directly to the entire `editor.html` file for:
- avoid running `decodeURIComponent(escape())` statements each time a label is displayed
- make it easy to correct labels that are now "in the clear"
```html
<!DOCTYPE html>
<html>    
    <head>                     
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">               
        <title>Fix Gliffy editor.html encoding
        </title>
        <script> 
        function convert() {
           var textIn = document.getElementById("in");
           var textOut =  document.getElementById("out");
           textOut.value= decodeURIComponent(escape(textIn.value));
        }   
        </script>          
    </head>    
    <body>
    <h1>Fix Gliffy editor.html encoding</h1>
    <textarea id="in"></textarea>          
    <br>
    <textarea id="out"></textarea>          
    <br>        
    <button onclick="convert();">Conversion</button>    
    </body>
</html>
```
## :fr: Correction du bug d'encodage des caractères de Gliffy
L'application Chrome [Diagrammes de Gliffy](https://chrome.google.com/webstore/detail/gliffy-diagrams/bhmicilclplefnflapjmnngmkkkkpfad?hl=fr) permet de dessiner des diagrammes en toute simplicité et fonctionne en mode hors connexion.

Mais la dernière version (`1.0.32`) de l'application souffre d'un problème d'encodage des libellés des traductions :
- `GLIFFY.DICTIONARY.de : "Einige Ihrer Änderungen sind nicht gespeichert."...`
- `GLIFFY.DICTIONARY.fr : "Vos modifications n'ont pas Ã©tÃ© enregistrÃ©es."...` 
- `GLIFFY.DICTIONARY.ru : "Ð»Ð¸ÑÑ Ð½ÐµÑÐ¾ÑÑÐ°Ð½ÐµÐ½Ð½ÑÐµ Ð´Ð°Ð½Ð½ÑÐµ."...`

### Gliffy, Inc. ne maintient sa version autonome (664 467 utilisateurs) 
> Hi François,
> Thanks for reaching out, and I'm sorry you've run into this!
> The free Chrome app is a limited version of Gliffy Online, therefore it does not have as many features or capabilities. We, unfortunately, won't be doing any updates to the Chrome app as we are choosing to focus our efforts on our online product. :-1: 

### Correction du bug avec le fichier `editor.html` modifié :ambulance:
1. Fermez l'application Diagrammes de Gliffy
1. Identifiez le [`Chemin d'accès au profil`](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/user_data_dir.md#Current-Location) en tapant `chrome://version` dans le barre d'URL de Chrome
1. Déplacez vous dans `votre_chemin_de_profil\Extensions\bhmicilclplefnflapjmnngmkkkkpfad\1.0.32_0` qui est le dossier de l'application
1. Renommez par sécurité le fichier `editor.html` en `editor.html.origin`
1. Remplacez le fichier d'origine en y copiant le fichier `editor.html` du dépôt
1. Lancez Diagrammes de Gliffy et constatez le changement : `Entité-Relation` `Réseau` `Modifier > Rétablir` :+1:
#### Correction des libellés
Les libellés du dictionnaire français se trouvent à la ligne 1988, il est facile d'y apporter des modifications/corrections (exemple "barre latÈrale" a été corrigée en "barre latérale").
### Explication de la correction :mag:
L'application utilise un système de template javascript ([handlebars.js](https://github.com/wycats/handlebars.js)) avec internationalisation `i18n` des libellés :
```html
editor.html
<a ... href="#gliffy-sidebar-com-gliffy-library-basic"> {{i18n SIDEBAR_BASIC_SHAPES}} </a>
```
#### Les libellés
Les libellés sont stockés dans un dictionnaire par langue :
```js
editor.html
1988 GLIFFY.DICTIONARY.fr = $.extend(GLIFFY.DICTIONARY.fr, {...,"SIDEBAR_BASIC_SHAPES":"Formes de base",...}
```
Ils sont ensuite "lus" dans ce dictionnaire au moment de lors de l'utilisation du template :
```js
js/editor.js
38 {result=this.dict[str]}
```
Mais ils ne sont pas encodés correctement (double encodage utf-8), j'ai modifié `editor.js` et l'[encodage correct est rétabli](https://stackoverflow.com/questions/13356493/decode-utf-8-with-javascript/34926911#answer-13691499) :
```js
js/editor.js
38 {result=decodeURIComponent(escape(this.dict[str]))}
```
#### Encodage correct de `editor.html` :trophy:
J'ai appliqué directement le décodage correct à tout le fichier `editor.html` pour :
- éviter d'exécuter les instructions `decodeURIComponent(escape())` à chaque affichage d'un libellé
- permettre  de corriger facilement les libellés qui sont maintenant "en clair"

```html
<!DOCTYPE html>
<html>    
    <head>                     
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">               
        <title>Fix Gliffy editor.html encoding
        </title>
        <script> 
        function convert() {
           var textIn = document.getElementById("in");
           var textOut =  document.getElementById("out");
           textOut.value= decodeURIComponent(escape(textIn.value));
        }   
        </script>          
    </head>    
    <body>
    <h1>Fix Gliffy editor.html encoding</h1>
    <textarea id="in"></textarea>          
    <br>
    <textarea id="out"></textarea>          
    <br>        
    <button onclick="convert();">Conversion</button>    
    </body>
</html>
```
