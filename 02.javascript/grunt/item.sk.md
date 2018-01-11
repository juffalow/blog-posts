---
title: Grunt
media_order: grunt-logo.png
published: true
date: '13-11-2015 17:40'
taxonomy:
    tag:
        - JavaScript
        - 'Vzorový projekt'
        - Grunt
header_image_file: grunt-logo.png
---

Ak pracuješ na nejakom väčšom projekte, určite máš napísaný nejaký ‘buildovací’ script, ktorý ti pomáha s niektorými často opakovanými úlohami. Takouto úlohou môže byť spájanie javascriptov, kompilovanie SASS / LESS súborov, presúvanie súborov alebo mazanie niektorých prebytočných súborov. Každá takáto úloha sa dá ( relatívne ) ľahko nadefinovať v Grunte, ktorý sa potom o všetko postará.

===

#### Príklad

Určite si robil na nejakom projekte, kde si si rozložil CSS a JavaScript veci do viacerých súborov, aby si v tom mal nejaký poriadok a prehľad. Potom ale vznikne problém, že keď je treba jednotlivé scripty použit, treba ich po jednom pridávať do html hlavičky. Tá potom vyzerá asi takto :

```
<head>
    ...
    <link href="subor1.css" rel="stylesheet">
    <link href="subor2.css" rel="stylesheet">
    <link href="subor3.css" rel="stylesheet">
    
    <script src="subor1.js"></script>
    <script src="subor2.js"></script>
    ...
</head>
```

Jednoduchým riešením môže byť ručne a pracne spájať jednotlivé súbory, čo časom isto omrzí, keďže po akejkolvek zmene je treba súbory nanovo pospájať. Riešením môže byť použitie nejakého **task managera**, ktorý by sa o všetko postaral. Takýchto task managerov je samozrejme viac. Medzi najznámejšie istotne patria **Grunt** a **Gulp**.

#### Čo to dokáže?

Dá sa povedať, že čokolvek, čo je treba. Od spájania súborov, minimalizovanie, presúvanie po testovanie funkčnosti a sledovanie zmien v súboroch. Takýchto modulov je dostupných strašne veľa a v prípade, že by neexistoval presne taký, aký je treba, nie je tažké vytvoriť si vlastný.

#### Inštalácia

Grunt a jeho pluginy sa inštalujú pomocou **[NPM](https://www.npmjs.com/)** správcu balíčkov pre **[Node.js](https://nodejs.org/)**.

```
$ npm install -g grunt-cli
```

Parameter `-g` inštaluje balíček `grunt-cli` globálne a bude nutné použiť príkaz <kbd>sudo</kbd> aby inštalácia prebehla úspešne, alebo vo Windowse spustiť príkazový riadok ako Administrátor.

**Pozor!** Toto je len akýsi spúšťač samotného Grunt-u. Ten sa nachádza v projektovom adresári hneď vedľa konfiguračného súbora Gruntfile spolu s ďaľšími závislosťami – modulmi.

## Projekt *GruntTest* ako príklad

Najlahšie a najlepšie bude si to ukázať na nejakom modelovom príklade. Na začiatok netreba nič zložité a veľké. Majme teda projekt, ktorý bude mat dva hlavné JavaScript súbory. Jeden `matematika.js`, kde budú funkcie `scitaj`, `odcitaj` a druhý `text.js` s funkciami `zmenNaVelkePismena` a `zmenNaMalePisemena`. Na stránke to má byť všetko iba v jednom súbore, bez nejakých zbytočných znakov a komentárov. Samozrejme, aby bolo jasne vidieť, ktoré súbory sú určené pre vývoj a ktoré pre produkciu, tak bude stromová štruktúra vyzerať nasledovne :

```
src ( source code )
    matematika.js
    text.js
www ( cisty kod urceny pre produkciu )
    index.html
    js
        grunttest.min.js ( vysledny javascript )
```

#### Súbor *package.json*, informácie o projekte a určenie závislostí

Súbor *package.json* patrí do koreňového adresára projektu a sú v ňom uložené projektové metadáta a závislosti – moduly, ktoré sa využívajú. Na jeho vytvorenie je možné použiť aj samotný NPM – príkazom <kbd>npm init</kbd>. Ten si vypýta názov projektu, verziu, autora, atd. a podla toho vygeneruje základný package.json súbor.

```
{
  "name": "GruntTest",
  "version": "1.0.0",
  "description": "Testovaci projekt",
  "main": "",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Matej 'juffalow' Jellus",
  "license": "ISC"
}
```

To by bol základny súbor bez akýchkoľvek zavislostí. V zadaní však je, že vo výsledku má byť iba jeden, minimalizovaný súbor. T.j. treba nájsť taký plugin, ktorý zjednotí súbory do jedného a ďalší, ktorý vymaže všetko nadbytočné. Zoznam takýchto pluginov je na oficialnej stránke [Grunt Plugins](http://gruntjs.com/plugins).

Po krátkom prezretí základnej ponuky pluginov je zrejemé, že vyhovuje *contrib-concat* s popisom „spájanie suborov“ a *contrib-uglify* s popisom „minimalizovanie súborov“. Tieto dva pluginy teda pridať ako závislosť do package.json. Vysledný súbor teda vyzerá takto :

```
{
  "name": "GruntTest",
  "version": "1.0.0",
  "description": "Testovaci projekt",
  "author": "Matej 'juffalow' Jellus",
  "devDependencies": {
    "grunt": "~0.4.5",
    "grunt-contrib-concat": "~0.5.1 ",
    "grunt-contrib-uglify": "~0.5.0"
  }
}
```

#### Inčtalácia Gruntu a pluginov

Každý projekt môže využívať inú verziu Gruntu a iné pluginy. Ak chceš nainštalovať všetky závislosti naraz, stačí spustiť príkaz :

```
$ npm install
```

Ten stiahne všetky potrebné balíčky a uloží ich do podadresára *node_modules* v projektovom adresári. Medzi nimi sa nachádza aj samotný Grunt, ktorý je spúšťaný z konzoly pomocou príkazu <kbd>grunt</kbd>.

Súbor *Gruntfile.js* a tvorba taskov

Tento súbor tiež patrí do koreňového adresára projektu. Sú v ňom definované a nakonfigurované úlohy, ktoré má Grunt spúšťať. Skladá sa z 4roch hlavných častí :

* hlavná funkcia – ktorá obaluje celú funkcionalitu, t.j. celý kód Gruntu musí byť definovaný v nej
* konfigurácia – vačšina pluginov vyžaduje aspoň základné nastavenia, napríklad ak sa majú spájať súbory, tak musí byť definovaný adresár, v ktorom sa súbory nachádzajú a cesta k súboru, do ktorého má byť uložený výstup
* načítanie pluginov – pluginy, ktoré sa využívajú musia byť reálne najskôr načítané ( niečo ako *include* )
* vlastné úlohy – nadefinované názvy úloh a ku nim pluginy, ktoré majú byť spustené

*Gruntfile.js*, ktorý bude riešit zadaný problém by mohol byť zapísany nasledovne :

```
// hlavna funkcia
module.exports = function(grunt) {

  // konfiguracia
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    concat: { // plugin, ktory spaja subory
      options: {
        separator: ';'
      },
      dist: {
        src: ['src/*.js'], // vstupom budu vsetky .js subory v adresari src
        dest: 'src/temp/<%= pkg.name %>.js' // vystupom bude jeden subor pomenovany ako nas projekt ( definovane v subore package.json -> name )
      }
    },
    uglify: { // plugin, ktory minimalizuje subory
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: '<%= concat.dist.dest %>', // vstupom je vystupny subor zo spajaceho pluginu
        dest: 'www/js/<%= pkg.name %>.min.js' // vystupom je minimalizovany subor v produkcnom adresari
      }
    }
  });

  // nacitanie pluginov
  grunt.loadNpmTasks('grunt-contrib-concat');
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // defaultna uloha
  // pluginy su vykonavane v poradi v akom su tu nadefinovane
  grunt.registerTask('default', ['concat', 'uglify']);

};
```

Keď je všetko takto pekne nadefinované a pripravené, stačí už len spustiť príkaz <kbd>grunt</kbd>, ktorý ak je zadaný bez parametra, tak vykoná defaultný task – v tomto prípade najskor pospája všetky .js súbory z adresára src do jedného súbora GruntTest.js v adresári /src/temp/. Potom spustí plugin na minimalizovanie, ktorý ako vstup dostane výstupný súbor zo spájania ( **pozor!** keďže je to takto definované, plugin na spájanie musí byť vykonaný pred minimalizovaním! ). Ten vymaže všetky nadbytočné biele znaky + komentáre a vytvorí tak jednoriadkový „škaredý“ / tažko čitatelný kód. Tento výstup je potom uložený do adresára *www/js/*.

## Vyčistenie projektu

Pri spájaní javascriptov vznikol súbor, ktorý je v projekte teraz naviac. Takýchto súborov sa môže časom vyskytnúť viac, prípadne sa môžu tvoriť iné, ktoré bude treba priebežne mazať. Na to môže byť vytvorená dalšia úloha a tá bude len mazať prebytočné veci.

#### Pridanie nového pluginu

Jedna z možností ako pridať nový plugin je dopísať ho do *package.json* ako ďalšiu závislosť a spustiť príkaz <kbd>npm install</kbd>. Prípadne jednoduchší spôsob je spustiť príkaz na inštalovanie konkrétneho pluginu s parametrom `--save-dev`, ktorý stiahne plugin a dopíše ho do *package.json* sám.

```
$ npm install <module> --save-dev
```

Takže znova na stránku [grunt pluginov](http://gruntjs.com/plugins), kde treba nájsť niečo, čo maže súbory / adresáre. Myslím, že *contrib-clean* s popisom „čisté súbory a priečinky“ by to mal zvládnuť.

```
$ npm install grunt-contrib-clean --save-dev
```

#### Vytvorenie novej úlohy

Plugin pridaný, zapísaný do závislostí, zostáva už len konfigurácia a vytvorenie úlohy v *Gruntfile.js*. To už nie je nič zložité, v podstate tri riadky naviac :

```
  // konfiguracia
  ...
  clean: ["src/temp/"]
  ...
```

```
  // nacitanie pluginu
  ...
  grunt.loadNpmTasks('grunt-contrib-clean');
  ...
```

```
  // nova uloha
  ...
  grunt.registerTask('vycisti', ['clean']);
  ...
```

#### Spúšťanie vlastných úloh

Funkčnosť je možné vyskúšať spustením príkazu <kbd>grunt vycisti</kbd>. Parametrom je názov tasku, ktorý bol vytvorený. Ak je Grunt spustený bez parametra, vykoná len defaultný task.

## Záver

To by malo byť asi všetko. Istotne môžem odporučiť pozrieť zoznam dalších pluginov. Ja aktuálne využívam *grunt-remove-logging*, ktorý vymaže všetky `console.log()`, *grunt-contrib-jshint* na validáciu JavaScriptov ( vid. [jshint.com](http://jshint.com/) ). Plánujem spraviť aj testy, ale to až niekedy keď bude čas ;-). Grunt je ale možné použiť aj na CSS súbory, kompilovanie SASS súborov a veľa dalšieho.

Celý projekt si môžeš pozrieť alebo stiahnuť z [GitHubu](https://github.com/juffalow/GruntTest).

#### Odkazy

[Oficiálna stránka Grunt](http://gruntjs.com/) kde môžeš tiež nájsť úvodný tutoriál, zoznam pluginov a celú dokumentáciu.
Pluginy a dokumentácia ku nim je na stránke [NPM správcu balíčkov](https://www.npmjs.com/). Odkazy na použité pluginy :

* [grunt-contrib-concat](https://www.npmjs.com/package/grunt-contrib-concat) – spájanie súborov
* [grunt-contrib-uglify](https://www.npmjs.com/package/grunt-contrib-uglify) – minimalizovanie súborov
* [grunt-contrib-clean](https://www.npmjs.com/package/grunt-contrib-clean) – mazanie súborov a adresárov
