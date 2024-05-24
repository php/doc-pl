# Polskie tłumaczenie dokumentacji PHP

To repozytorium zawiera pliki polskiego tłumaczenia dokumentacji na [php.net](https://php.net),
a ten dokument opisuje, w jaki sposób można uczestniczyć w procesie tłumaczenia.

## Spis treści

1. [Instalacja](#instalacja)
2. [Zbuduj dokumentację](#zbuduj-dokumentację)
3. [Śledzenie wersji](#śledzenie-wersji)
4. [Tłumaczenie](#tłumaczenie)

## Instalacja

Aby wziąć udział w tworzeniu tłumaczenia dokumentacji PHP na język polski, musisz zacząć od zrobienia
[forka][gh-forking] tego repozytorium, tj. `doc-pl`.

> **Uwaga:** Jeśli posiadasz konto na php.net (przydzielane ręcznie za długotrwały wkład) i uprawnienia
> do tego repozytorium, to możesz pracować bez użycia forka, ale jest to rzadka sytuacja, więc
> skupmy się na tym, co dotyczy większości ludzi.

Poza `doc-pl` potrzebujesz jeszcze dwóch dodatkowych repozytoriów. W zdecydowanej większości sytuacji
nie ma potrzeby dokonywać w nich żadnych zmian, aby pracować nad polską dokumentacją, tak więc nie ma
potrzeby ich forkować. Możesz sklonować je prosto z oryginalnych repozytoriów organizacji PHP na GitHubie.

Najlepiej jest stworzyć jeden katalog, do którego zostaną sklonowane wszystkie trzy repozytoria, na przykład
`phpdoc`.

- `php/doc-base`: to repozytorium zawiera narzędzia do tworzenia dokumentacji. Repozytorium znajduje się
 pod adresem https://github.com/php/doc-base
- `php/doc-en`: angielska wersja dokumentacji, która jest używana m.in. wtedy, gdy dana podstrona nie
  została przetłumaczona na j. polski. Repozytorium znajduje się pod adresem https://github.com/php/doc-en
- `php/doc-pl`: polskie tłumaczenie dokumentacji PHP. Jak wspomniane wyżej, prawdopodobnie musisz stworzyć
  fork tego repozytorium i sklonować repozytorium znajdujące się na Twoim własnym koncie GitHub

> **Uwaga:** upewnij się (przy klonowaniu lub zmieniając nazwę bezpośrednio po nim), że folder z angielską
> dokumentacją nazywa się `en`, a polską `pl`. Innymi słowy, upewnij się, że w sklonowanych repozytoriach
> katalogi z wersjami językowymi (poza `doc-base`) **nie** zaczynają się od `doc-`!

## Zbuduj dokumentację

Po dokonaniu jakichkolwiek zmian w tłumaczeniu powinieneś skorzystać ze skryptu, który buduje dokumentację,
aby upewnić się, że kod XML nie zawiera żadnych błędów. W tym momencie powinieneś mieć następującą strukturę
katalogów:

```
|- phpdoc
 |- doc-base
 |- en
 |- pl
  |- ...
```

Jeśli znajdujesz się w katalogu `pl`, to musisz uruchomić po prostu następujące polecenie:

```bash
php ../doc-base/configure.php --with-lang=pl
```

Jeżeli wszystko poszło okej, to zobaczysz komunikat podobny do tego:

```
All good. Saving .manual.xml... done.
All you have to do now is run 'phd -d /home/user/Dev/php-docs/base/.manual.xml'
If the script hangs here, you can abort with ^C.
         _ _..._ __
        \)`    (` /
         /      `\
        |  d  b   |
        =\  Y    =/--..-="````"-.
          '.=__.-'               `\
             o/                 /\ \
              |                 | \ \   / )
               \    .--""`\    <   \ '-' /
              //   |      ||    \   '---'
         jgs ((,,_/      ((,,___/

 (Run `nice php configure.php` next time!)
```

W przeciwnym razie zobaczysz informację o błędach, które wystąpiły w plikach XML.

> **Uwaga:** mimo iż proces nazywa się "budowanie", to ten krok nie tworzy jeszcze plików dokumentacji,
> które można wygodnie czytać (np. plików HTML). Komenda powyżej tworzy jedynie jeden gigantyczny plik
> XML i sprawdza jego poprawność, a _renderowaniem_ dokumentacji do czytelnej formy zajmuje się [phd][phd]

## Śledzenie wersji

Kluczową sprawą przy tłumaczeniu dokumentacji jest jej zgodność z angielskim pierwowzorem. W tym celu
powstał system `revcheck`, który jest dostępny m.in. na [doc.php.net](http://doc.php.net), który śledzi
zmiany, jakie wystąpiły w angielskiej wersji manuala, a więc to co musimy zmienić w polskim tłumaczeniu,
aby było ono aktualne.

Cały system opiera się o hashe commitów gita i komentarze zawarte na górze każdego przetłumaczonego pliku
XML:

```xml
<!-- EN-Revision: git-hash Maintainer: XXXX Status: ready -->
```

Kiedy tłumaczymy jakiś plik, to `git-hash` w komentarzu powyżej musi być ustawiony na hash _angielskiej_
wersji pliku, na którym opieraliśmy tłumaczenie. Pomaga w tym wspomniana witryna doc.php.net. Na przykład
zakładka "Outdated files" http://doc.php.net/revcheck.php?p=files&lang=pl pokazuje, które z plików
przetłumaczonych już na język polski, wymagają aktualizacji. Podaje ona wtedy, o jaki hash jest oparte
obecne tłumaczenie, hash najnowszej angielskiej wersji, a także diff (wykaz zmian) między nimi. Hash
najnowszej angielskiej wersji musimy umieścić w polu `EN-Revision` wyżej wspomnianego komentarza.

Podobnie ma się sprawa przy tłumaczeniu nowych stron, przechodzimy do sekcji "Untranslated files"
http://doc.php.net/revcheck.php?p=missfiles&lang=pl, wybieramy katalog na górze, po czym widzimy,
jakie pliki nie zostały przetłumaczone i jaki jest ich obecny hash commita w angielskiej wersji
manuala. Wtedy do naszego nowego tłumaczenia dodajemy komentarz jak powyżej, a podany na stronie
hash umieszczamy jako wartość `EN-Revision`.

> **Uwaga:** informacje na stronie doc.php.net są aktualizowane co cztery godziny. Czasy ostatniej
> aktualizacji, podane tam w stopce, są w UTC.

Jeżeli chodzi o pole `Maintainer` to przy aktualizacji tłumaczeń zasadniczo nie zmieniamy go. W wypadku
nowych tłumaczeń możemy tam podać którąś z istniejących już wartości lub, jeśli bierzemy już częstszy udział
w tłumaczeniu na j. polski, pokusić się o "stworzenie" własnego nicku. Nie wymaga to żadnej rejestracji,
a jedynie dodania wpisu do pliku `translators.xml`, znajdującego się w głównym katalogu tego repozytorium.

## Tłumaczenie

Pliki XML powinny używać wcięć o szerokości jednej spacji. W tym repozytorium dołączono plik `.editorconfig`,
więc jeśli używasz edytora, który wspiera ten standard (bezpośrednio lub przez wtyczki), to powinno to zostać
wykryte automatycznie.

Tłumacząc pliki, warto zadbać o to, aby tekst był podzielony na nowe linie w tych samych miejscach, co angielski
pierwowzór. Bardzo ułatwia to późniejszą analizę diffów na doc.php.net, a więc i aktualizację tłumaczeń. Wiadomo,
że nie zawsze jest to możliwe, bo często szyk zdania w języku polskim jest zupełnie inny niż w angielskim, ale warto
dbać o to tam, gdzie to możliwe.

Poniżej zamieszczono listę kilku z częściej występujących i mniej oczywistych tłumaczeń, na które łatwo się natknąć
w dokumentacji PHP.

> **Uwaga:** żadna z osób biorących udział w pracach nad polską wersją podręcznika PHP nie jest zawodowym tłumaczem.
> Pewne frazy można zapewne przetłumaczyć lepiej, więc sugestie są mile widziane. Dobrze byłoby jednak zadbać o spójność,
> więc do momentu osiągnięcia ewentualnego konsensusu w sprawie nowego tłumaczenia, stosujmy się do tych, które już są
> używane w polskiej wersji lub wymienione poniżej.

| Angielski wyraz lub wyrażenie      | Polskie tłumaczenie                  | Komentarz                              |
|------------------------------------|--------------------------------------|----------------------------------------|
| Locale settings                    | Ustawienia regionalne (locale)       | Jako iż nie ma powszechnie używanego tłumaczenia na j. polski to zawieramy też oryginalne słówko, aby ułatwić np. Googlowanie |
| locale-dependent / locale-specific | z uwzględnieniem ustawień regionalnych (locale) | |
| locale-independent                 | bez uwzględnienia ustawień lokalnych (locale) | |
| string                             | ciąg znaków (ew. łańcuch znaków, jeśli gdzieś bardzo chcemy uniknąć powtórzeń | |
| `<parameter>foo</parameter>`       | Parametr `<parameter>foo</parameter>` | Poprzedzić słowem "parametr" przynajmniej przy pierwszym opisywaniu danego parametru na danej stronie. W przeciwnym razie dostajemy mieszkankę polskiego i angielskiego, która nie zawsze jest zrozumiała
| `&resource;`                       | `<link linkend="language.types.resource">Zasób</link>` | Analogicznie dla innych encji typów danych, jeśli uznamy że warto utrzymać linkowanie jak w angielskiej wersji


[gh-forking]: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo
[phd]: https://github.com/php/phd
