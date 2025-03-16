Analizinte la kodaron, mi vidas ke ĝi estas aplikaĵo por anstataŭigi Esperantajn vortojn per ĉinaj ideogramoj (kanji) kaj aldoni rubi-anotaciojn. Mi kreos ampleksan gvidlibron por Esperantaj uzantoj.

# Gvidlibro por la Kanji-anstataŭigilo kaj Rubi-anotacia ilo por Esperanto

## Enkonduko

Bonvenon al la "Anstataŭigilo por Esperantaj tekstoj per ĉinaj ideogramoj kaj rubi-anotacia ilo"! Ĉi tiu aplikaĵo permesas al vi transformi vian Esperantan tekston per diversaj manieroj:

1. **Anstataŭigi** Esperantajn radikojn per ĉinaj ideogramoj (kanji)
2. **Aldoni rubi-anotaciojn** super aŭ sub la originalaj vortoj
3. **Konservi** specifajn tekstopartojn senŝanĝe
4. **Apliki lokajn anstataŭigojn** nur en certaj partoj de via teksto

La celo estas helpi al vi vizualigi la Esperantan lingvon tra ideograma perspektivo, krei instrumaterialon aŭ simple esplori la lingvon laŭ nova vidpunkto.

## Kio estas "rubi-anotacio"?

Rubi-anotacio estas metodo por aldoni malgrandajn klarigojn (ofte prononcojn) super aŭ sub teksto, uzata en japana, ĉina kaj aliaj lingvoj. En nia aplikaĵo, vi povas uzi ĝin por montri la originalan tekston dum anstataŭigo per ĉinaj ideogramoj, aŭ inverse.

## Komencante

### Paŝo 1: Aliro al la aplikaĵo

Vizitu la aplikaĵon ĉe unu el la disponeblaj ligil-adresoj por via preferata lingvo:
- **Esperanta versio**: https://esperanto-kanji-converter-and-ruby-annotation-tool-esperanto.streamlit.app/

### Paŝo 2: Elekti JSON-dosieron por anstataŭigo

La unua paŝo estas elekti JSON-dosieron, kiu enhavas la regulojn por anstataŭigi Esperantajn vortojn:

- **Uzi la defaŭltan JSON-dosieron**: La plej simpla elekto. La aplikaĵo uzos la jam inkluditajn anstataŭigajn regulojn.
- **Alŝuti dosieron**: Se vi havas proprajn anstataŭigojn, vi povas alŝuti propran JSON-dosieron.

Se vi volas vidi ekzemplon de la JSON-strukturo, klaku sur "Elŝuti ekzemplan JSON-dosieron".

### Paŝo 3: Elekti eligan formaton

Elektu el sep diversaj formatoj por la eligota rezulto:

1. **HTML-formato kun rubi-anotacioj kaj grandec-ĝustigo**: La Esperanta teksto restas ĉefa, kun rubianotacio supre, ĝustigante la grandecon de la anotacioj.
2. **HTML-formato kun rubi-anotacioj, grandec-ĝustigo kaj kanji-anstataŭigo**: La ĉinaj ideogramoj fariĝas la ĉefa teksto, kun Esperanta rubi-anotacio supre.
3. **HTML-formato**: Simpla HTML-formata rubi sen grandec-ĝustigo.
4. **HTML-formato kun kanji-anstataŭigo**: Simpla HTML-formata rubi kun ĉinaj ideogramoj kiel ĉefa teksto.
5. **Formato kun krampoj**: Montras la Esperantan tekston kun tradukoj en krampoj.
6. **Formato kun krampoj kaj kanji-anstataŭigo**: Montras ideogramojn kun Esperanta teksto en krampoj.
7. **Nur konservi la anstataŭigitan tekston (simpla anstataŭigo)**: Montras nur la anstataŭigitan tekston sen la originalo.

### Paŝo 4: Altnivelaj agordoj (laŭvole)

Klaku sur "Malfermi agordojn por paralela pretigo" por agordi la uzadon de pluraj procezoj por plirapidigi la anstataŭigon. Ĉi tio estas utila por tre longaj tekstoj. Vi povas elekti la nombron da samtempaj procezoj (2-4).

### Paŝo 5: Enigi vian tekston

Vi povas enigi vian Esperantan tekston per du manieroj:

1. **Mane tajpi** la tekston en la tekstkampon.
2. **Alŝuti dosieron** (TXT, CSV aŭ MD-formato) kun via Esperanta teksto.

### Paŝo 6: Elekti la prezentan formon por specifaj Esperantaj literoj

Elektu kiel la specifaj Esperantaj literoj (ĉ, ĝ, ĥ, ktp.) aperos en la rezulto:

1. **Supersigna formo** (ĉ, ĝ, ĵ ktp.)
2. **x-formato** (cx, gx, jx ktp.)
3. **^-formato** (c^, g^, j^ ktp.)

### Paŝo 7: Sendi kaj vidi la rezulton

Klaku la butonon "Sendi" por pretigi vian tekston. Post kiam la procezado estas finita, la rezulto estos montrata malsupre. Vi povas vidi la rezultojn per:

- HTML-antaŭrigardo (por HTML-formatoj)
- Rezulta HTML-kodo (por HTML-formatoj)
- Rezulta teksto (por aliaj formatoj)

Fine, vi povas elŝuti la rezulton kiel HTML-dosieron por uzi aliloke.

## Specialaj funkcioj

### Anstataŭigaj kontroloj

La aplikaĵo provizas du specialajn manojn por kontroli kiuj partoj de via teksto estas anstataŭigitaj:

1. **Protektado de teksto kontraŭ anstataŭigo** - Ĉirkaŭu parton de via teksto per **%** simboloj:
   ```
   %Ĉi tiu parto ne estos anstataŭigita%
   ```

2. **Loka anstataŭigo** - Ĉirkaŭu parton de via teksto per **@** simboloj por anstataŭigi nur tiun parton:
   ```
   @Nur ĉi tiu parto estos anstataŭigita@
   ```

## Kreado de propra JSON-dosiero por anstataŭigo

Se vi volas krei propran JSON-dosieron kun viaj preferataj anstataŭigoj, vi povas uzi la duan paĝon de la aplikaĵo:

1. Alklaku la maldekstran menuon kaj elektu "Paĝo por generi JSON-dosieron..."
2. Sekvu la instrukciojn:
   - Elektu CSV-dosieron kun Esperantaj radikoj kaj tradukaj informoj
   - Elektu JSON-dosieron(jn) por reguloj pri segmentado de Esperantaj radikoj kaj anstataŭigoj
   - Agordu paralelan prilaboradon se necesas
   - Klaku "Krei la JSON-dosieron por anstataŭigo"
   - Elŝutu la rezultan JSON-dosieron

### Formo de CSV-dosiero

La CSV-dosiero devus enhavi du kolumnojn:
- **Unua kolumno**: Esperanta radiko
- **Dua kolumno**: Traduko, ĉina ideogramo aŭ alia priskriba teksto

Ekzemplo:
```
esper,希望
hom,人
lern,学ぶ
```

### Formo de JSON-dosieroj por segmentado

La JSON-dosiero por radiksegmentado specifas kiel dividi vortojn en siajn radikojn. Ĝi povas esti kompleksa, sed jen ekzemplo:

```json
[
  ["am", "dflt", ["verbo_s1"]],
  ["bon", "dflt", ["a", "e"]]
]
```

Tio specifas ke:
- "am" estas traktita kiel verbo kun aktivaj verbofinaĵoj ("verbo_s1")
- "bon" ricevas afiksojn "a" kaj "e" por formi "bona" kaj "bone"

## Teĥnikaj detaloj

La aplikaĵo konsistas el pluraj komponentoj:

1. **Ĉefa aplikaĵo** (main.py) - La enirpunkto kaj ĉefa uzantinterfaco
2. **JSON-generilo** (Paĝo por generi...) - Por krei proprajn anstataŭigajn regulojn
3. **Tekst-anstataŭiga modulo** - Pritraktas la anstataŭigan procezon
4. **JSON-kreada modulo** - Helpas en la kreado de aranĝoj por anstataŭigoj

La aplikaĵo uzas kompleksan algoritmon por:
- Identigi radikvortojn en Esperanto-teksto
- Apliki anstataŭigajn regulojn laŭ ilia prioritato
- Generi HTML-kodon kun rubi-anotacioj
- Adapti la grandecon de rubianotacioj laŭ la proporcio de teksta larĝeco

## Ekzemploj de uzado

### Ekzemplo 1: Baza anstataŭigo

**Eniga teksto:**
```
La patro lernas Esperanton.
```

**Post anstataŭigo (HTML-formato kun rubi-anotacioj):**
```html
<ruby>La<rt>定冠詞</rt></ruby> <ruby>patr<rt>父</rt></ruby>o <ruby>lern<rt>学ぶ</rt></ruby>as <ruby>Esperant<rt>エスペラント</rt></ruby>on.
```

### Ekzemplo 2: Uzado de protektado per %

**Eniga teksto:**
```
Mi %parolas% Esperanton.
```

**Post anstataŭigo:**
```html
<ruby>Mi<rt>私</rt></ruby> parolas <ruby>Esperant<rt>エスペラント</rt></ruby>on.
```

### Ekzemplo 3: Uzado de loka anstataŭigo per @

**Eniga teksto:**
```
La hundo @kuras rapide@.
```

**Post anstataŭigo:**
```html
<ruby>La<rt>定冠詞</rt></ruby> <ruby>hund<rt>犬</rt></ruby>o <ruby>kur<rt>走る</rt></ruby>as <ruby>rapid<rt>速い</rt></ruby>e.
```

## Problemsolvado

### La teksto ne estas anstataŭigita kiel atendite

- Kontrolu ĉu viaj Esperantaj vortoj havas la ĝustan bazan formon
- Kontrolu ĉu la JSON-dosiero enhavas la necesajn anstataŭigajn regulojn
- Provu uzi la defaŭltan JSON-dosieron por komparceloj

### La rubi-anotacio estas tro malgranda aŭ tro granda

- Elektu la formaton kun "grandec-ĝustigo" por aŭtomate adapti la grandecon de la anotacioj
- Redaktu la rezultan HTML-kodon por ĝustigi la CSS-stilojn laŭbezone

### La procezado estas tro malrapida

- Aktivigu "paralelan pretigon" en la altnivelaj agordoj
- Pligarandigu la nombron da samtempaj procezoj (ĝis 4)
- Dividu grandan tekston en plurajn malpli grandajn partojn

## Pliaj resursoj

- La aplikaĵo estas disponebla en 14 diversaj lingvoj - elektu la version plej konvenan por vi el la ligil-sekcio sube.
- La fontokodo kaj pliaj informoj troviĝas en la GitHub-deponejo: [https://github.com/TakafumiYamauchi/Esperanto-Kanji-Converter-and-Ruby-Annotation-Tool-Esperanto](https://github.com/TakafumiYamauchi/Esperanto-Kanji-Converter-and-Ruby-Annotation-Tool-Esperanto)

## Konkludo

Ĉi tiu aplikaĵo ofertas unikan manieron esplori la rilaton inter Esperanto kaj ĉinaj ideogramoj. Ĝi povas esti utila por instruado, lingvolernado, aŭ simple por krei interesan vizualan prezenton de via Esperanta teksto.

Eksperimentu kun diversaj formatoj kaj agordoj por trovi la plej taŭgan kombinaĵon por viaj bezonoj!
