# Gvidlibro por Esperanto-Teksta Anstataŭiga kaj Ruby-Anotacia Ilo

## Enhavo
1. [Enkonduko](#enkonduko)
2. [Bazaj Funkcioj](#bazaj-funkcioj)
3. [Ĉefa Paĝo: Teksta Anstataŭigo](#ĉefa-paĝo-teksta-anstataŭigo)
4. [Dua Paĝo: Krei JSON-Dosierojn](#dua-paĝo-krei-json-dosierojn)
5. [Formataj Opcioj Detaligitaj](#formataj-opcioj-detaligitaj)
6. [Specialaj Tekstaj Markadoj](#specialaj-tekstaj-markadoj)
7. [Altnivelaj Agordoj](#altnivelaj-agordoj)
8. [Ekzemploj de Uzado](#ekzemploj-de-uzado)
9. [Oftaj Demandoj](#oftaj-demandoj)

## Enkonduko

Bonvenon al la Esperanto-Teksta Anstataŭiga kaj Ruby-Anotacia Ilo! Ĉi tiu programo permesas al vi konverti Esperantan tekston al diversaj formatoj, inkluzive anstataŭigon de Esperantaj vortoj per ĉinaj ideogramoj (kanji) kaj aldonon de Ruby-anotacioj, kiuj montras la originalan Esperantan tekston super la ideogramoj. La programo estas aparte utila por:

- Instruistoj de Esperanto, kiuj deziras montri rilaton inter Esperanto kaj aliaj lingvoj
- Lingvistoj, kiuj studas morfologian strukturon de Esperantaj vortoj
- Kreantoj de instrumaterialoj pri la Esperanta vortfarado
- Esperantistoj, kiuj volas krei vizualan reprezentadon de la lingvo per ideogramoj

La programo estas dividita en du ĉefajn partojn: la ĉefa paĝo por anstataŭigo de teksto, kaj dua paĝo por krei proprajn JSON-dosierojn por personigitaj anstataŭigoj.

## Bazaj Funkcioj

- **Teksta anstataŭigo**: Konverti Esperantajn vortojn al ĉinaj ideogramoj aŭ aliaj signaranĝoj
- **Ruby-anotacioj**: Aldoni malgrandajn tekstojn super la anstataŭigitaj signoj
- **Pluraj elirformatoj**: HTML kun Ruby-tekstoj, HTML kun grandecaj adaptaĵoj, krampformato, ktp.
- **Persona adapteblo**: Krei proprajn JSON-dosierojn kun viaj anstataŭigo-reguloj
- **Protektitaj tekstpartoj**: Marki specifajn tekstpartojn por eviti anstataŭigon
- **Elŝutebla rezulto**: Konservi la rezultan tekston en HTML-aŭ teksta dosiero

## Ĉefa Paĝo: Teksta Anstataŭigo

### Paŝoj por Uzi la Ĉefan Paĝon

1. **Elekti JSON-dosieron por anstataŭigoj**
   - Elektu "Uzi la defaŭltan JSON-dosieron" aŭ "Alŝuti dosieron"
   - Se vi elektas alŝuti, la dosiero devas sekvi specifan formaton (vidu la sekcio "Dua Paĝo")
   - Vi povas elŝuti ekzemplan JSON-dosieron por vidi la formaton

2. **Elekti elirantan formaton**
   - **HTML-formato kun rubi-anotacioj kaj grandec-ĝustigo**: Montras Esperantajn vortojn kun ĉinaj ideogramoj kiel Ruby-superskribaĵoj, adaptante la grandecon laŭ la teksta longeco
   - **HTML-formato kun rubi-anotacioj, grandec-ĝustigo kaj kanji-anstataŭigo**: Simile, sed kun la kanji kiel ĉefteksto kaj Esperanto kiel Ruby-teksto
   - **HTML-formato**: Simpla HTML kun Ruby-anotacioj sen grandec-ĝustigo
   - **HTML-formato kun kanji-anstataŭigo**: Simile, sed kun la kanji kiel ĉefteksto
   - **Formato kun krampoj**: Montras tekston en formo "Esperanto(Kanji)"
   - **Formato kun krampoj kaj kanji-anstataŭigo**: Montras tekston en formo "Kanji(Esperanto)"
   - **Nur konservi la anstataŭigitan tekston**: Simpla anstataŭigo, montrante nur la kanji-rezulton

3. **Provizi enigan tekston**
   - Elektu "Mane tajpi" por enigi tekston rekte en la tekstujon
   - Elektu "Alŝuti dosieron" por uzi tekstan dosieron (UTF-8 enkodita)

4. **Elekti la prezentan formon de Esperantaj literoj**
   - **Supersigna formo**: Uzas la normalajn Esperantajn literojn kun supersignoj (ĉ, ĝ, ĥ, ĵ, ŝ, ŭ)
   - **x-formato**: Konvertas al cx, gx, hx, jx, sx, ux
   - **^-formato**: Konvertas al c^, g^, h^, j^, s^, u^

5. **Klaki "Sendi"**
   - La programo procezos vian tekston laŭ la elektitaj agordoj
   - Se la rezulto estas longa, nur parto estos montrata

6. **Elŝuti la rezulton**
   - Klaku "Elŝuti la rezulton" por konservi la tutan rezultan tekston

### Specialaj Tekstaj Markadoj

Vi povas uzi specialajn markadojn en via teksto por kontroli kiel specifaj partoj estas anstataŭigitaj:

- **%teksto%**: Teksto ĉirkaŭita per procentaj signoj NE estos anstataŭigita. Ekzemple, en "La %hundo% kuras", nur "kuras" estos anstataŭigita.
- **@teksto@**: Teksto ĉirkaŭita per @-signoj estos anstataŭigita LOKE (nur ene de la markita parto, sen influi al aliaj partoj de la teksto). Tio permesas izolitan anstataŭigon por specifaj vortoj.

### Altnivelaj Agordoj

- **Paralela pretigo**: Ebligas uzi plurajn procezojn samtempe por plirapidigi la anstataŭigon
- **Kvanto da samtempaj procezoj**: Difini kiom da procezoj estas uzataj (inter 2 kaj 4)

## Dua Paĝo: Krei JSON-Dosierojn

Tiu ĉi paĝo permesas al vi krei propran JSON-dosieron por personigi kiel Esperantaj vortoj estas anstataŭigitaj.

### Paŝoj por Krei JSON-Dosieron

1. **Pretigi la CSV-dosieron**
   - Elektu "Alŝuti" por alŝuti propran CSV-dosieron, aŭ "Uzi la defaŭlton"
   - La CSV devas enhavi du kolumnojn: Esperanta radiko kaj traduko (ĉinaj ideogramoj aŭ alia teksto)
   - Vi povas elŝuti ekzemplajn CSV-dosierojn por vidi la formaton

2. **Pretigi la(jn) JSON-dosieron(jn)**
   - **Segmentado de Esperantaj radikoj**: Specifas kiel dispartigi Esperantajn vortojn en radikoj
   - **Anstataŭigitaj tekstoj**: Specifas la finan anstataŭigan tekston por certaj vortoj

3. **Altnivelaj agordoj (paralela prilaborado)**
   - Simile al la ĉefa paĝo, vi povas agordi la uzon de paralela prilaborado

4. **Klaki "Krei la JSON-dosieron por anstataŭigo"**
   - La programo kreos JSON-dosieron kun tri ĉefaj listoj:
     - **Globala anstataŭigo-listo**: Por ĝenerala anstataŭigo en la tuta teksto
     - **Du-litera radika anstataŭigo-listo**: Por speciala traktado de du-literaj radikoj
     - **Loka anstataŭigo-listo**: Por anstataŭigo nur en specifaj partoj de la teksto

5. **Elŝuti la finan anstataŭigan liston**
   - La programo provizos butonon por elŝuti la kreitan JSON-dosieron

### Formatoj de Personigitaj JSON-Dosieroj

#### Segmentado de Esperantaj Radikoj

La JSON-dosiero pri segmentado de radikoj uzas la jenan formaton:

```json
[
  ["am", "dflt", ["verbo_s1"]],
  ["sci", "40000", ["verbo_s1", "o", "a"]],
  ["esper", "60000", ["ne", "verbo_s1", "o", "a"]]
]
```

Kie:
- La unua elemento estas la Esperanta radiko
- La dua elemento estas la prioritato (numerado) aŭ "dflt" (defaŭlto)
- La tria elemento estas listo de kategorioj kaj sufiksoj, ekzemple:
  - "verbo_s1": Aldonu ĉiujn verbajn finaĵojn (as, is, os, us, ktp.)
  - "verbo_s2": Aldonu bazajn verbajn finaĵojn (i, u)
  - Aliaj specifaj sufiksoj kiel "o", "a", "e"
  - "ne": Konservi la vorton sen anstataŭigi

#### Anstataŭigitaj Tekstoj

La JSON-dosiero pri anstataŭigitaj tekstoj uzas la jenan formaton:

```json
[
  ["am/ik", "50000", ["o", "a"], "愛/友"],
  ["esper/ant", "60000", ["ne"], "希望/人"]
]
```

Kie:
- La unua elemento estas la Esperanta vorto kun radika segmentado ("/")
- La dua elemento estas la prioritato
- La tria elemento estas listo de sufiksoj aŭ "ne"
- La kvara elemento estas la anstataŭiga teksto kun segmentado ("/")

## Formataj Opcioj Detaligitaj

### HTML-formato kun rubi-anotacioj kaj grandec-ĝustigo

Tiu formato montras Esperantan tekston kiel ĉeftekston, kun ĉinaj ideogramoj en malgrandaj Ruby-anotacioj supre. La grandeco de la Ruby-teksto estas aŭtomate ĝustigita laŭ la teksta longeco. Tio estas utila kiam la traduka teksto estas pli longa ol la originala Esperanta vorto.

Ekzemplo: "amiko" kun Ruby-anotacio de "朋友" (peng you)

### HTML-formato kun rubi-anotacioj, grandec-ĝustigo kaj kanji-anstataŭigo

Tiu formato estas inversa al la antaŭa - ĝi montras la ĉinajn ideogramojn kiel ĉeftekston, kun Esperantaj vortoj en Ruby-anotacioj supre. La grandeco estas ankaŭ aŭtomate ĝustigita.

Ekzemplo: "朋友" kun Ruby-anotacio de "amiko"

### Formato kun krampoj

Tiu formato estas pli simpla, montrante la Esperantan tekston kun traduko en krampoj. Tio estas utila por simpla legado sen specialaj HTML-elementoj.

Ekzemplo: "amiko(朋友)"

## Specialaj Tekstaj Markadoj

### Protektitaj Tekstpartoj (%...%)

Ĉiu tekstparto ĉirkaŭita per procentaj signoj restos netuŝita dum la anstataŭiga procezo. Tio estas utila por:

- Nomoj, kiujn vi ne volas anstataŭigi
- Specialaj terminoj aŭ vortoj
- Ĉiuj tekstoj, kiuj devas resti en originala formo

Ekzemplo: "Mi estas %John% kaj mi amas Esperanton" → Nur "Mi", "estas", "kaj", "mi", "amas", "Esperanton" estos anstataŭigitaj.

**Notu**: La maksimuma longeco por protektita teksto estas 50 signoj.

### Loke Anstataŭigitaj Partoj (@...@)

Teksto ĉirkaŭita per @-signoj estas anstataŭigita LOKE, kio signifas ke nur tiu parto estas anstataŭigita sen influi al la cetera teksto. Tio permesas pli grandan regadon super kiel specifaj vortoj estas anstataŭigitaj.

Ekzemplo: "La @hundo@ kuras rapide" → Nur "hundo" estos anstataŭigita, kaj la rezulto povus esti "La 犬 kuras rapide"

**Notu**: La maksimuma longeco por loke anstataŭigita teksto estas 18 signoj.

## Altnivelaj Agordoj

### Paralela Pretigo

Por plirapidigi la anstataŭigon de longa teksto, la programo povas uzi plurajn procezojn samtempe:

1. Elektu "Uzi paralelan pretigon" ĉe la altnivelaj agordoj
2. Difinu "Kvanto da samtempaj procezoj" (inter 2 kaj 4)

Ĉi tiu opcio estas aparte utila por longaj tekstoj aŭ kiam vi havas multe da anstataŭigaj reguloj. La procesado estos dividita al pluraj procezoj kaj funkcios pli rapide sur komputiloj kun pluraj CPU-kernoj.

## Ekzemploj de Uzado

### Ekzemplo 1: Simpla Teksta Anstataŭigo

1. Elektu "Uzi la defaŭltan JSON-dosieron"
2. Elektu "HTML-formato kun rubi-anotacioj kaj grandec-ĝustigo"
3. Enigi la tekston: "Mi amas Esperanton"
4. Elektu "Supersigna formo" por Esperantaj literoj
5. Klaku "Sendi"

Rezulto: La teksto estos montrata kun ĉiu vorto anstataŭigita laŭ la defaŭlta dosiero, kaj kun Ruby-anotacioj super ĉiu anstataŭigita vorto.

### Ekzemplo 2: Uzi Protektitajn Partojn

1. Elektu "Uzi la defaŭltan JSON-dosieron"
2. Elektu "Formato kun krampoj"
3. Enigi la tekston: "Mi estas %John Smith% kaj mi amas Esperanton"
4. Elektu "Supersigna formo" por Esperantaj literoj
5. Klaku "Sendi"

Rezulto: La teksto estos montrata kun ĉiu vorto anstataŭigita, krom "John Smith", kiu restos neŝanĝita.

### Ekzemplo 3: Krei Propran JSON-Dosieron

1. Iru al la dua paĝo ("Paĝo por generi JSON-dosieron")
2. Elŝutu ekzemplan CSV kaj redaktu ĝin por aldoni viajn proprajn Esperanto-Kanji parojn
3. Alŝutu vian modifitan CSV
4. Uzu la defaŭltajn agordojn por la aliaj opcioj
5. Klaku "Krei la JSON-dosieron por anstataŭigo"
6. Elŝutu la kreitan JSON-dosieron
7. Reiru al la ĉefa paĝo kaj alŝutu vian personigitan JSON-dosieron por uzi ĝin

## Oftaj Demandoj

### Kiel mi povas anstataŭigi nur specifajn vortojn?

Vi povas uzi la @-markadojn. Ekzemple, en "La @hundo@ kuras", nur "hundo" estos anstataŭigita.

### Ĉu mi povas ŝanĝi la koloron de la Ruby-anotacioj?

La defaŭlta koloro por Ruby-anotacioj estas blua. Se vi volas ŝanĝi ĝin, vi bezonus elŝuti la rezultan HTML-dosieron kaj redakti la CSS-stilon.

### Kial miaj vortoj ne estas anstataŭigitaj?

Asekuriĝu, ke:
- Viaj vortoj estas en la JSON-dosiero, kiun vi uzas
- La vortoj ne estas ĉirkaŭitaj per %-signoj
- La JSON-dosiero estas ĝuste formatita
- Via teksto uzas la saman literan formaton kiel la JSON-dosiero (ekz. supersignojn)

### Ĉu mi povas uzi aliajn lingvojn krom ĉinaj ideogramoj?

Jes! Vi povas krei propran CSV/JSON-dosieron kun tradukoj al ajna lingvo. La Ruby-anotacioj povas enhavi ajnajn signojn, ne nur ĉinajn ideogramojn.

### Kiel mi povas kopii la rezulton al alia dokumento?

La plej bona maniero estas elŝuti la rezulton per la "Elŝuti la rezulton" butono, kaj poste enigi la HTML-kodon en vian dokumenton. Se vi bezonas nur la tekston, vi povas kopii ĝin rekte el la teksta kampo.

---

Ĉi tiu programo estas kreita por faciligi la uzadon de Esperanta teksto kun diversaj vizualaj reprezentadoj, precipe kun ĉinaj ideogramoj. Ĝi estas potenca ilo por instruistoj, lingvistoj, kaj ĉiuj, kiuj interesiĝas pri la strukturo kaj reprezentado de Esperanto.

Por pliaj informoj, bonvolu viziti la GitHub-deponejon, kie vi povas trovi pliajn ekzemplojn, dokumentojn, kaj la fontokodon de la programo.