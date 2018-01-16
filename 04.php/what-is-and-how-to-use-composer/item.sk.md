---
title: 'Čo to je Composer a ako ho použiť'
media_order: logo-composer.png
published: true
date: '28-01-2016 16:50'
taxonomy:
    tag:
        - php
        - Composer
        - 'Vzorový projekt'
slug: co-to-je-composer-a-ako-ho-pouzit
header_image_file: logo-composer.png
---

Ak sa učíš **PHP** alebo si skúsený programátor istotne poznáš slovo **Composer**. Ale…čo to je? Aký je jeho účel a prečo by si ho mal používať? Tu by som o ňom rád napísal pár vecí a ukázal ako sa používa. **PHPUnit** testovací framework by mohol byť vhodným a aj užitočným príkladom.

===

## Composer

Určite si už napísal niekoľko tried, ktoré riešia bežné úlohy ako napríklad obsluha dát, logovanie chýb, atď. Potom si vytvoril úplne nový projekt, kde si ich chcel použiť, ale! Musel si ich buď znova písať alebo ich hľadať a kopírovat spolu s ostatnými súbormi, ktoré boli potrebné. A to boli všetko triedy, ktoré si robil ty, takže vieš, čo všetko potrebujú. Ale čo ak by si chcel využiť triedy niekoho iného? Prípadne nejaký kamarát / kolega by chcel použiť tvoje, čo by asi bez tvojej pomoci bol dosť problém. Toto je situácia, kedy prichádza na rad Composer.

#### Využitie

Takže znova, chceš vytvoriť nový projekt. Spíšeš si všetky závislosti ( potrebné balíčky / triedy ), spustíš Composer a ten stiahne všetko za teba. Každá z týchto závislostí môže mať svoje vlastné závislosti, ale to už viac nie je tvoja starosť. Composer dotiahne všetky požadované knižnice.

Composer je teda správca balíčkov pre jazyk PHP. Je inšpirovaný nástrojom NPM od Node.js a nástrojom Bundler pre Ruby. Je to niečo podobné ako Maven v Jave.

#### composer.json

Súbor vo formáte json, kde sú definované závislosti a ďalšie metadáta. Hlavnú časť tvorí kľúč `require`, v ktorom povieš Composer-u, na ktorých balíčkoch je tvoj projekt zavislý.

Príklad *composer.json* súbora z php frameworku Symfony :

```
{
    "name": "symfony/symfony",
    "type": "library",
    "description": "The Symfony PHP framework",
    "keywords": ["framework"],
    "homepage": "https://symfony.com",
    "license": "MIT",
    "authors": [
        {
            "name": "Fabien Potencier",
            "email": "fabien@symfony.com"
        },
        {
            "name": "Symfony Community",
            "homepage": "https://symfony.com/contributors"
        }
    ],
    "require": {
        "php": ">=5.5.9",
        "doctrine/common": "~2.4",
        "twig/twig": "~1.23|~2.0",
        "psr/cache": "~1.0",
        "psr/log": "~1.0",
        "symfony/polyfill-apcu": "~1.0,>=1.0.2",
        "symfony/polyfill-intl-icu": "~1.0",
        "symfony/polyfill-mbstring": "~1.0",
        "symfony/polyfill-php56": "~1.0",
        "symfony/polyfill-php70": "~1.0",
        "symfony/polyfill-util": "~1.0"
    },
    ...
}
```

**Názov balíčka** pozostáva z mena autora a názvu projektu. Meno autora existuje len aby sa zamedzilo konfliktom pri názvoch. Dovoluje to dvom odlišným ľuďom vytvoriť knižnicu s tým istým názvom len s odlišným menom autora ako napríklad `igorw/json` a `saldaek/json`.

**Verzia balíčka** je typ obmedzenia pre Composer, takže stiahne iba konkrétne verzie alebo verzie zodpovedajúce obmedzeniu. ( Viac si o tom môžeš prečítať v [dokumentácii k Composer verziám](https://getcomposer.org/doc/articles/versions.md), ale to už samozrejme v angličtine )

#### composer.lock

Keď spustíš príkaz <kbd>composer install</kbd>, tak nájde verzie požadovaných balíčkov, ktoré vyhovujú daným podmienkam a stiahne ich do adresára *vendor*.

Po nainštalovaní závislostí Composer zapíše zoznam balíčkov a ich konkrétnych verzií do *composer.lock* súbora. Tento súbor by si mal commit-núť do repozitára aby si zabránil budúcemu stiahnutiu iných verzií. Dôvodom je príkaz `install`, ktorý skontroluje, či existuje lock súbor a ak áno, stiahne verzie balíčkov v ňom definované. To znamená, že server, ďalší programátori a všetci ostatní budú spúšťať aplikáciu s presne tými istými balíčkami.

<div class="alert alert-info" role="alert">Ak používaš <i>GIT</i> na verziovanie svojho kódu, tak adresár <i>vendor</i> môžeš pridať do <i>.gitignore</i>. Nepotrebuješ ukladať kód všetkých balíčkov vo svojom repozitári.</div>

#### autoload.php

Pre balíčky, ktoré maju definovanú informáciu o autoloade Composer vygeneruje *vendor/autoload.php* súbor. Tento súbor stačí includ-nuť a automatické loadovanie tried máš ako bonus. Toto robí používanie balíčkov veľmi jednoduchým.

Informáciu o autoloade môžeš pridať aj pre svoj vlastný kód vložením kľúča `autoload` do *composer.json*. S touto funkciou určuješ mapovanie mennýc priestorov do adresárov.

## Ukážka projektu *s využitím PHPUnit*

Vytvorte triedu na kontrolu či zadaný reťazec je [palindróm](https://sk.wikipedia.org/wiki/Palindr%C3%B3m) – postupnosť znakov, ktoré možno čítať v ľubovolnom smere.

Adresárová štruktúra :

```
ComposerExample
    src
    tests
        PalindromeTest.php
    composer.json
```

#### PHPUnit

Podľa testami riadeného vývoja začneme vytvorením príslušných testov. Čo na to potrebujeme… Istotne nejaký framework určený na testovanie. Na toto využijem známy PHPUnit. Takže ho vložím do *composer.json* :

```
{
    "require": {
        "phpunit/phpunit": "^4.6.0"
    },
    "autoload": {
        "psr-4": {
            "src\\" : "src"
        }
    }
}
```

A spustím príkaz <kbd>composer install</kbd>. Na výstupe je zoznam balíčkov a ich verzia, ktorá bola stiahnutá. Možeš ich nájsť v adresári vendor spolu s *autoload.php* súborom.

#### Testy

Popri tom ako vytváraš testy by si mal uvažovať o riešenom probléme a rovno navrhovať ako by to malo vo finále vyzerať. Táto časť ti pomôže lepšie pochopiť problém a odhalí rôzne scenáre, ktoré môžu nastať.
Týka sa zadanie len reťazcov? Smie niekto skúsiť zistiť, či číslo nie je palindróm? Možno v PHPčku typ premennej nehrá nejakú velkú rolu, ale pre iné jazyky to nemusí platiť.

Riešenie zahŕňa triedu, ktorá obsahuje funkcionalitu a metódu na určenie či zadaný reťazec je palindróm alebo nie. Názov triedy môže byť `Palindrome` a metóda `check()`. Je potrebné testovať oba prípady. Prípad, že reťazec je palindróm a prípad, že nie je. Testý teda môžu vyzerať takto :

```
class PalindromeTest extends PHPUnit_Framework_TestCase {

    private $palindrome;
    
    public function setUp() {
        $this->palindrome = new src\Palindrome();
    }

    public function testSuitableString() {
        $this->assertTrue($this->palindrome->check('racecar'));
        $this->assertTrue($this->palindrome->check('was it a car or a cat i saw'));
    }

    public function testNotSuitableString() {
        $this->assertFalse($this->palindrome->check('some meaningless text'));
    }
}
```

Teraz, keď je test pripravený, spusti príkaz <kbd>phpunit --bootstrap ./vendor/autoload.php ./tests/PalindromeTest.php</kbd>.

Prvým argumentom je `--bootstrap` a jeho hodnota je cesta k súboru, ktorý je zodpovedný za autoload – načítanie potrebných tried. Druhým argumentom je cesta k testovaciemu súboru alebo k celému adresáru. PHPUnit bude hladať všetky súbory končiace „&ast;test.php“ alebo akýmkoľvek iným definovaným reťazcom ( viac o tomto na ich oficialnej stránke ).

Trieda zatiaľ neexistuje a teda test skončí chybou *Class ‘src\Palindrome’ not found*.

#### Writing the code

Text by mal byť rovnaký pri čítaní zľava ako zprava. Na toto sa dá použiť PHP funkcia `strrev` a porovnať pôvodný reťazec s tým obráteným. Vytvor nový php súbor v adresári *src* s názvom *Palindrome.php*.

```
<?php

namespace src;

class Palindrome {
    /**
     * Check if a string is palindrome
     * @param string $str
     * @return bool
     */
    public function check( $str ) {
        return $str == strrev($str);
    }
}
```

Keď sa teraz spustia testy, tak už to neskončí chybou a na výstupe bude niečo takéto :

```
$ phpunit –bootstrap vendor/autoload.php tests/PalindromeTest.php
PHPUnit 4.8.21 by Sebastian Bergmann and contributors.
F.

Time: 263 ms, Memory: 4.00Mb

There was 1 failure:

1) PalindromeTest::testSuitableString
Failed asserting that false is true.

*/ComposerExample/tests/PalindromeTest.php:15

FAILURES!
Tests: 2, Assertions: 3, Failures: 1.
```

Test skončil s informáciou o zlyhaní ( *FAILURES!* ). Jeden test neprešiel a môžeš ho nájsť na riadku 15. Keď sa na ten riadok pozrieš, hneď je jasné, prečo to z opačnej strany nie je ten istý reťazec. Medzi slovami sú medzery, o ktoré je treba sa postarať, inak sa reťazce nebudú rovnať. Kód po úprave :

```
public function check( $str ) {
    $sanitizedString = \str_replace(' ', '', $str);
    return $sanitizedString == strrev($sanitizedString);
}
```

Spusti test znova a už by mal skončiť úspešne pre každý test.

```
OK (2 tests, 4 assertions)
```

Samozrejme, toto by nemal byť koniec. Stále je niekoľko reťazcov, ktoré by mali prejsť, ale neprešli by. Napríklad ak by sa v texte vyskytlo veľké písmeno, alebo nejaký znak ako bodka alebo otáznik. Ak by si strávil viac času premýšlaním o probléme a jeho riešení a spomenutými scenármi pri písaní testov, tak takýmto a ďalším problémom sa vyhneš.

## Záver

Toto je koniec krátkeho intra do Composera. Dúfam, že bolo spomenuté všetko dôležité a v pripade ak niečomu nerozumieš, kľudne mi napíš.
Je ešte veľa vecí, ktoré som nespomenul. Napríklad môžeš definovať extra závislosti pre dev prostredie, alebo vytvárať vlastné ( súkromné ) balíčky. Tak isto aj PHPUnit má kopec ďalších možností. Viac k tejto téme ale až v niektorom z neskorších postov.

#### Odkazy

Oficiálna [Composer](https://getcomposer.org/) stránka, kde nájdeš úvodný tutoriál ( *Getting Started* ), dokumentáciu, balíčky a ďalšie informácie.

Stránka [PHPUnit frameworku na testovanie](https://phpunit.de/) s výbornou dokumentáciou.

[Symfony framework](https://github.com/symfony/symfony) na GitHub-e, kde si môžeš pozrieť celý *composer.json* súbor.
