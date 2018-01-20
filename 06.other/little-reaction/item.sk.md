---
title: 'Malá reakcia'
media_order: keep-calm-and-love-programming.png
published: true
date: '13-12-2016 00:54'
slug: mala-reakcia
header_image_file: keep-calm-and-love-programming.png
---

Vcera bola jedna z prednasok skupiny [Web Up](https://www.facebook.com/groups/webup/), ktoru som, bohuzial, nestihol. Jeden z prednasajucich zverejnil svoju prednasku a ku nej aj priklady.

===

## Co sa mi (ne)paci

Je super, ze sa rozbiehaju taketo prednasky a „talky“, kde sa sharuju skusenosti programatorov. Treba si navzajom pomahat a tahat sa dopredu + ak sa zucastnia nejaki studenti, o to lepsie. Uvidia na com a s cim sa pracuje v roznych firmach, pripadne sa uz mozu dopredu rozhodovat, kam by asi tak chceli ist.

#### To horsie

Castokrat, ked aj na nete nieco hladam, najdem postup, ktory sice vyriesi moj problem, ale uz na pohlad zlym sposobom. Samozrejme, kazdy sa uci a zdokonaluje. Jedna z moznosti je, pisat si blog alebo zdielat svoj kod ci uz na GitHub-e alebo na StackOverflow a citat reakcie ludi. Ale pokial ignorujeme „feedback“, neposuvame sa dalej a stale opakujeme tie iste chyby, nieco je zle. A zdielanim tychto zlych postupov neposuvame nasich buducich kolegov vpred ani o kusok.

Ak idem o niecom pisat, precitam k danej teme viacero clankov, vyskusam si to a po dopisani urobim „alfa test“ -> text odoslem par znamym, ktori by mi k tomu mohli dat nejake kratke „review“. Ci im text dava zmysel, ci v postupe alebo priamo v kode nevidia nejaky zadrhel a podobne.

#### K veci

Otvoril som si teda [materialy](https://github.com/rostacik/webUP4tsdemo) k teme *TypeScript*. Presiel som do nahodneho subora [mathHelper.ts](https://github.com/rostacik/webUP4tsdemo/blob/master/src/mathHelper.ts) a pozeram na to.

Nasledujuca podmienka je v danej triede 3x, co znamena, ze autor nepozna jeden z hlavnych principov – [DRY ( Don’t Repeat Yourself](https://en.wikipedia.org/wiki/Don't_repeat_yourself) ) :

```
if ((param1) && (is.number(param1) && (param2) && (is.number(param2)))) {
```

Nevadi, kod ma sluzit ako ukazka a vsetko je ok. Vyuzitie kniznice [is.js](http://is.js.org/) mi ale tiez nepride ako najlepsi napad. Preco? Nie je to tak davno, co sa rozhodol jeden z open source programatorov [stiahnut svoje moduly](https://medium.com/@azerbike/i-ve-just-liberated-my-modules-9045c06be67c#.koxs0fbwb) z NPM. Kvoli jednemu z nich, ktory nerobil nic zlozite a mal 11 riadkov padlo hodne vela projektov. Oplatilo sa pre 11 riadkov pridavat takuto zavislost? Tu je jedna z kratkych reakcii ku tejto teme.

[V jednoduchosti je krasa](https://en.wikipedia.org/wiki/KISS_principle) a tu mi hned udrie do oci vsadepritomna podmienka :

```
if ((param1) && (is.number(param1) && (param2) && (is.number(param2)))) {
    return param1 + param2;
} else {
    throw new Error("mathHelper - add - empty or non numbers specified");
}
```

Ak sa mi nieco nepaci, vyhodim chybu, ak je to ok, tak len spravim co mam. Preferoval by som teda maly „code refactoring“ :

```
if (!((param1) && (is.number(param1) && (param2) && (is.number(param2))))) {
    throw new Error("mathHelper - add - empty or non numbers specified");
}
return param1 + param2;
```

Najlepsie by bolo vyhodit podmienku do vlastnej metody. Dovod? Metoda by mala robit jednu vec, tato robi dve ( zistuje ci je vstup cislo a scituje ).

A posledna vec, co sa uz netyka kodu ale temy ako takej. Ked je uz ten TypeScript taky dobry, preco po urceni typu, musim aj tak tento typ overovat? Hmm…

## Ok ok ok *20.12.2016*

Autor prednasky [odpovedal](http://rostacik.net/2016/12/16/re-mala-reakcia-k-mojej-prednaske-o-typescript-v-ziline/) v kratkom blogposte, kde s niektorymi pripomienkami suhlasil a s niektorymi nie. Ano, je fakt, ze z mojej strany ide len o kritiku. Je to hlavne preto, ze ked cokolvek hladam, najdem prevazne zly, zavadzajuci content. Potom tak aj nasa praca, praca programatorov, vyzera.

S tym npm si to zobral uplne zle. Nevravim, ze netreba pouzivat npm, len netreba to tlacit uplne vsade a toto bol priam ukazkovy priklad. Na tuto temu odporucam precitat clanky, na ktore som sa odkazoval, pripadne pogooglit nieco viac. Nepisem to len tak zo srandy, je to aj moja vlastna skusenost. Viacero projektov, na ktorych so mal robit, som si vyklonoval ku sebe a po <kbd>npm install</kbd> to ani zdaleka nebolo funkcne. Zastarale moduly, niektore uz ratali s novsimi verziami, ine uz nerobili to, co mali. Cim viac tychto modulov vyuzivas, tym viac bude projekt nachylny na podobne chyby.

Neposudzujem autora clanku, pozeram sa na pracu, ktoru zverejnil. Ano, je fakt, ze neviem kedy ju pisal, ci bol unaveny, alebo zrovna nemal nejaky dobry den. Ale v tom pripade si tychto veci musel byt vedomy a musel ratat s tym, ze sa to nemusi zrovna vsetkym pacit. Naviac prezentacia akejkolvek temy je ukazkou toho, ako ma autor prezentacie zvladnutu danu temu – teda prezentuje aj sam sema.

Este raz chcem zdoraznit, ze proti autorovi nemam nic. Nechcel som ho tymto urazit ani nic podobne. Jedna sa mi len na poukazanie na veci, ktore sa mi nepacia.

## Záver

Tymto nechcem na autora prezentacie ziadno utocit – samotna prezentacia mohla byt dobra. Jedna sa mi len o to, ze vsade podhadzujeme *priklady*, ktore sice mozu byt k veci, ale zaroven ucia posluchacov / citatelov prehliadat tieto „nedostatky“.