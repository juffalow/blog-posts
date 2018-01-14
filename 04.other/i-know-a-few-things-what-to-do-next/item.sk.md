---
title: 'Niečo už viem a čo ďalej?'
media_order: challenge-accepted.png
published: true
date: '28-11-2015 00:23'
slug: nieco-uz-viem-a-co-dalej
header_image_file: challenge-accepted.png
---

Baví ťa programovanie, čítaš knihy, blogy, tutoriály ale nemáš žiadne vlastné nápady, kde svoje vedomosti využiť alebo otestovať? Alebo sa programovaniu venuješ už dlho a chceš sa posunúť niekam ďalej? Tu je taký menší zoznam, ktorý ti môže pomôcť. Ide o niekoľko stránok, kde sú zadania na programovanie súvisiace aj s ďaľšími vedomostami zo sveta IT. V druhej časti je niečo málo k Open Source projektom a návrhy ako sa môžeš zapojiť do vývoja.

===

Kedysi, keď som s programovaním ešte len začínal, tak po prečítaní pár kníh som už nevedel čo ďalej, akú úlohu / zadanie / projekt by som si vymyslel. Skúšal som riešiť rôzne matematické zadania, vytvárať napodobeniny iných programov, atď. Rád by som preto spomenul aspoň pár stránok, kde sú zadania od jednoduchých až po tie zložité.

Takéto stránky zároveň môzu slúžiť aj pre potencionálneho zamestnávatela alebo sú priamo firmou vytvorené za účelom nájdenia talentu. Existujú aj iné možnosti ako sa zlepšiť a aj zviditelniť. Tu by som rád nadviazal na prednášku od [Paweł Jędrzejewski](https://twitter.com/@pjedrzejewski), ktorú mal v Brne – *You should be an Open Source Developer*.

## Stránky s úlohami

#### [www.hacker.org](http://www.hacker.org/)

Jedná sa o hru, kde každé „políčko“ je úloha. Úlohy sú rozdelené do 4roch hlavných kategórii : kryptológia, programovanie, web, rôzne. Zo začiatku sú úlohy skôr jednoduché. Potom ale prídu úlohy, ktoré vyžadujú určité znalosti a schopnosti riešitela.

Popri riešení týchto úloh spoznáš veľa nových vecí, budeš nútený hľadať na internete rôzne informácie a pracovať s nimi.

Príklad z kryptológie :

```
- .... . .- -. ... .-- . .-. .. ... .... --- .- .-. ... .
```

Za každú správne vyriešenú úlohu dostáva riešitel body. Kedže je to hra, tak je tam aj tabulka celkového poriadia, plus pri každej úlohe vidieť počet hráčov, ktorí danú úlohu vyrešili.

#### [www.codehunt.com](https://www.codehunt.com/)

Stránka s úlohami označená ako vzdelávacia programátorská hra. Bola vytvorená skupinou *Research in Software Engineering ( RiSE )* spolu s *Microsoft Research*.

Úlohy sa riešia v jazyku **Java** alebo **C#**. Tie sú rozdelené do niekolkých hlavných kategórii, napríklad aritmetické operácie, cykly, vyhladávacie algoritmy. Výsledný kód je potom testovaný na rôznych vstupoch. Výsledok testov je v tabulke, kde zobrazí aj prípadné zle vyhodnotené vstupy.

Príklad zadania :

<div class="row">
<div class="col-md-12">
<p class="text-left">Zisti aritmetickú operáciu aplikovanú na ‘x’.</p>
</div>
<div class="col-md-6 col-sm-12">
<pre class="prettyprint">
public class Program {
    public static int Puzzle(int x) {
        return 0;
    }
}
</pre>
</div>
<div class="col-md-6 col-sm-12" style="background-color: aliceblue;">
<table class="table">
<thead>
<tr>
<td></td>
<td>X</td>
<td>Očakavaný výsledok</td>
<td>Tvoj výsledok</td>
<td>Popis</td>
</tr>
</thead>
<tbody>
<tr>
<td><i class="fa fa-times" style="color: red;"></i></td>
<td>0</td>
<td>1</td>
<td>0</td>
<td>Nezhoduje sa</td>
</tr>
<tr>
<td><i class="fa fa-check" style="color: #FFD900;"></i></td>
<td>-1</td>
<td>0</td>
<td>0</td>
<td></td>
</tr>
</tbody>
</table></div>
</div>

Stačí navštíviť stránku [www.codehunt.com](https://www.codehunt.com/), prihlásiť sa pomocou *Google+* alebo *Facebooku* a môžeš riešiť.

#### [Google Code Jam](https://code.google.com/codejam/)

Google code jam je medzinárodná programátorská súťaž organizovaná spoločnosťou Google. Súťaž pozostáva z niekolkých algoritmických problémov, ktoré musia byť vyriešené v pevne stanovenom čase. Súťažiaci môžu použiť akýkoľvek programovací jazyk.

Väčšina týchto úloh ( až do roku 2008 ako som pozeral / našiel ) je stále vyvesená na oficiálnej stránke. Sú verejne pristupné a funguje aj overenie výsledného výstupu. Stále je teda možnosť vyskúšať si staré zadania a overiť si správnosť riešenia.

Príklad jedného zo zadaní pre rok 2015 :

```
Je premiera v opere a tvoja kamarátka je primadona ( hlavná speváčka ). Nebudeš v publiku, ale chceš zaistiť aby sa jej dostali búrlivé ovácie - aby každý v publiku vstal a tlieskal jej.

Na začiatku, celé pulikum sedí. Každý v publiku má svoj "level hanblivosti". Člen publika s levelom hanblivosti Si bude čakať, kým sa aspoň Si iných členov publika nezdvihne a nezačne tlieskať a ak ano, hneď sa zdvihne a začne tlieskať. Ak Si = 0, tak sa šlen publika hneď zdvihne a začne tlieskať, bez ohladu na to, čo urobia iní. Napríklad, člen publika s Si = 2, ktorý bude sedieť hneď na začiatku, ale postaví sa a začne tlieskať až potom ako uvidí aspoň dvoch iných ludí stáť a tlieskať.

Level hanblivosti každého z publika poznáš a si pripravený pozvať ďalších kamarátov primadony aby boli v publiku a aby si sa uistil, že každý z davu sa na konci postaví a bude tlieskať. Každý z týchto kamarátov môže mať level hanblivosti aký chceš, nemusí byť nevyhnutne rovnaký. Aký je minimálny počet kamarátov, ktorých musíš pozvať, aby si zaistil búrlivé ovácie?

Vstup

Prvý riadok vstupu je počet testovacích prípadov, T. Nasleduje T testovacích prípadov. Každý pozostáva z jedného riadka s Smax, maximálny level hanblivosti najhanblivejšej osoby v publiku, nasledovaný reťazcom Smax + 1 jednotlivých číslic. K-ta číslica v reťazci ( počíta sa od 0 ) reprezentuje kolko ludí v publiku má level hanblivosti K. Napríklad reťazec "409" by znamenal, že sú tam 4 členovia publika s Si = 0 a 9 čelnov publika s Si = 2 ( a žiadny s Si = 1 alebo inou hodnotou ). Všimni si, že vždy bude 0 až 9 ludí s každým levelom.

Reťazec nikdy nebude končiť 0. To znamená, že vždy bude aspoň jedna osoba v publiku.

Výstup

Pre každý testovací prípad, vypíš jeden riadok obsahujúci "Case #x: y", kde x je číslo testovacieho prípadu ( začínajúce od 1 ) a y je minimálny poťet kamarátov, ktorých musíš pozvať.
```

## Známe úlohy a algoritmy

Je pravda, že na všetko už teraz existuje nejaká knižnica, komponenta, plugin a podobne, čo to rieši, ale… Všetko sú to zaujímavé veci, ktoré sa sem tam môžu hodiť. Treba mať o nich prehlad a aspoň približne vedieť ako fungujú. Sem tam sa tiež môže takéto zadanie vyskytnúť nejakej súťaži alebo na pohovore. A ked nie, tak už len minimálne ako precvičenie a následné odkontrolovanie s hotovým riešením.

#### Zoraďovanie / triedenie

Tu len v krátkosti by som spomenul sortovacie algoritmy. Je dobré si ich pozriet, poznať ich a poznať ich princíp. Každý takýto algoritmus má svoje výhody a nevýhody.

#### Osem dám na šachovnici

Problém osmych dám – teda ako rozmiestniť osem dám na šachovnicu tak, aby sa podla pravidiel šachu navzájom neohrozovali. Všeobecné zadanie je *N* dám na šachovnici *N x N*. Je to staré zadanie, ktoré bolo prvý krát zverejnené v Berlínskom šasopise v roku 1848 Maxom Bezzelom.

Často sa to rieši pri výučbe programovania, kvôli jednoduchosti ukážky **backtracking**ových algoritmov.

Ďalšími podobnými úlohami môže byť **Prechádzka jazdca** alebo **Sudoku**. Pri prechádzke jazdcom ide znova o šachovú figúrku, ktorá sa pohybuje v súlade s pravidlami šachu po šachovnici a jej úlohou je navštíviť každé pole šachovnice iba raz.

Tomuto algoritmu – **backtrackingu** – by som chcel neskôr venovať celý článok, spolu s riešením problému ôsmych dám a sudoku.

## Open Source

Každý sa isto pohrával s myšlienkou, že by prispel niečím do Open Source projektov. Velmi dobrá prednáška ku tejto téme bola na [Brno PHP konferencii](https://www.brnophp.cz/) – Mal by si byť Open Source vývojár.

Nejde však len o prispievanie do cudzích projektov, kludne môže ísť o zdielanie vlastných projektov, knižníc, alebo častí kódu. Ak zverejniš svoj kód, môžu ti ostatní programátori pomôcť riešiť nejaký problém, prípadne poradiť čo sa dá robiť lepšie. Je to tiež cesta ako sa učit a posúvať dalej. Zároveň tým pomáhaš daľším programátorom.

#### „Dlh“

Čítaním blogov, rôznych fórumov, využívaním kódov z open source projektov si vytváraš nejaký „dlh“. Tento dlh je voči komunite, ktorá to všetko pre teba a ďaľších vytvorila. Prispievaním do open source projektov, písaním vlastného blogu alebo všeobecne pomocou ostatným programátorom tento dlh svojim spôsobom splácaš.

Buduješ si tak aj svoje meno, reputáciu, učíš sa spolupráci s ostatnými.

#### GitHub

Velmi oblúbenou službou je GitHub, kde si môžes vytvoriť vlastný priestor pre projekty. Pomôže ti to aj pri správe projektu, kedže sa tu dajú nahlasovať chyby. Čím viac ludí projekt využíva, tým skôr sa odladia nedostatky. Nemusí sa jednať len o nahlásenie chyby. Niekto, kto chybu nájde ju môže hneď aj vyriešiť a ponúknuť ti toto riešenie.

V prípade, že nemáš svoj vlastný projekt, alebo sa chceš podielať na vývoji iného, môžes si otvoriť zoznam problémov ( *issues* ) a skúsiť jednotlivé chyby riešiť. Alebo ak natrafíš na nejakú chybu v balíčku, ktorý využívaš, nahlás ju. Aj to je pomoc!

#### Dokumentácia

Vela projektov môže byť zaujímavých a dobrých, ale ak majú slabú dokumentáciu, ludia ich nebudú využívať. Aj tu sa dá prispieť a pomôcť. Použil si niečo, kde si ešte musel riešiť nejaké problémy, alebo sa ti zdala dokumentácia nedostatočná? Kludne ju môžeš napísať / doplniť.

## Záver

Viem, že spomenuté veci zaberú nejaký ten čas a toho je málo. Nie je to ale zbytočná práca naviac. Zlepšuješ tým svoje schopnosti, získavaš nových kamarátov a tiež to je spôsob ako sa zviditelniť a ukázať čo je v tebe.

Na *WebSupporte* sa objavil prednedávnom článok „[6 užitočných tipov na hladanie IT talentov](http://www.websupport.sk/blog/2015/11/6-uzitocnych-tipov-na-hladanie-it-talentov/)„. Rýd by som spomenul jednu čast z neho :

> Sociálne siete sú novodobé životopisy – o to viac to platí pri špecializovaných sociálnych sieťach, kde môžu ITčkári zdieľať svoje vedomosti a práce.

Tým chcem len potvrdiť to, že sú ludia, ktorí si tvoju aktivitu budú všímať.