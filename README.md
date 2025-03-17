# Manlibro de Uzanto: Esperanto-Kanji Konvertilo kaj Rubia Anotacia Ilo

## Enhavo
1. Enkonduko
2. Bazaj Funkcioj
3. Kiel Uzi la Ĉefan Paĝon
4. Specialaj Teknikaj Notoj
5. Agordi Proprajn Anstataŭigajn Regulojn
6. Eligaj Formatoj
7. Konsiloj kaj Solvoj de Problemoj

## 1. Enkonduko

Bonvenon al la Esperanto-Kanji Konvertilo kaj Rubia Anotacia Ilo! Ĉi tiu aplikaĵo permesas al vi:

- **Anstataŭigi** Esperantajn vortojn per ĉinaj ideogramoj (kanji)
- **Aldoni** rubiajn anotaciojn al Esperantaj vortoj
- **Krei** proprajn anstataŭigajn regulojn laŭ viaj preferoj
- **Elekti** inter diversaj eligaj formatoj (HTML, krampoj, ktp)

La aplikaĵo utilas por instruaj celoj, por helpi lernantojn kompreni la strukturon de Esperantaj vortoj, kaj por esplorado de lingvaj interrilatoj. Ĝi ankaŭ estas interesa ilo por vizualigi Esperanton per ĉinaj ideogramoj.

## 2. Bazaj Funkcioj

La aplikaĵo konsistas el du ĉefaj partoj:

### Ĉefa Paĝo (Anstataŭigilo)
Uzata por anstataŭigi Esperantajn tekstojn per ideogramoj aŭ aldoni rubiajn anotaciojn.

### Paĝo por Generi JSON-dosieron
Uzata por krei proprajn anstataŭigajn regulojn laŭ viaj preferoj (por spertaj uzantoj).

## 3. Kiel Uzi la Ĉefan Paĝon

### Paŝo 1: Elekti JSON-dosieron por anstataŭigo

La aplikaĵo bezonas JSON-dosieron, kiu enhavas la regulojn por anstataŭigi Esperantajn vortojn per ideogramoj. Vi povas:

- **Uzi la defaŭltan JSON-dosieron** (rekomendita por komencantoj)
- **Alŝuti propran JSON-dosieron** (se vi jam kreis ĝin)

Se vi volas vidi ekzemplon de la JSON-dosiero, klaku "Elŝuti ekzemplan JSON-dosieron" sub tiu elekto.

### Paŝo 2: Agordi la paralelan pretigon (laŭvole)

Se vi traktas grandajn tekstojn, vi povas uzi la paralelan pretigon por plirapidigi la procezon:

1. Klaku "Malfermi agordojn por paralela pretigo"
2. Marku la skatoleton "Uzi paralelan pretigon"
3. Elektu la kvanton da samtempaj procezoj (2-4)

### Paŝo 3: Elekti la eligan formaton

Elektu kiel vi volas vidi la rezulton:

- **HTML-formato kun rubi-anotacioj kaj grandec-ĝustigo** - Montras Esperantajn vortojn kun ideogramoj kiel rubiaj anotacioj, aŭtomate ĝustigante la grandecon
- **HTML-formato kun rubi-anotacioj, grandec-ĝustigo kaj kanji-anstataŭigo** - Montras ideogramojn kun Esperantaj vortoj kiel rubiaj anotacioj
- **HTML-formato** - Simpla HTML-formato kun rubiaj anotacioj (sen grandec-ĝustigo)
- **HTML-formato kun kanji-anstataŭigo** - Simpla HTML-formato kun inversigitaj rubiaj anotacioj (ideogramoj kiel ĉefteksto)
- **Formato kun krampoj** - Montras vortojn en formo "Esperanto(ideogramo)"
- **Formato kun krampoj kaj kanji-anstataŭigo** - Montras vortojn en formo "ideogramo(Esperanto)"
- **Nur konservi la anstataŭigitan tekston** - Montras nur la ideogramojn sen la originala Esperanta teksto

### Paŝo 4: Enigi la tekston

Vi povas enigi la tekston per du manieroj:

- **Mane tajpi** - Rekte enigi la tekston en la tekstujon
- **Alŝuti dosieron** - Alŝuti TXT, CSV aŭ MD-dosieron kun via teksto (UTF-8 enkodado rekomendita)

### Paŝo 5: Elekti la formon de Esperantaj literoj

Elektu kiel la Esperantaj supersignaj literoj (ĉ, ĝ, ĥ, ĵ, ŝ, ŭ) aperu en la rezulto:

- **Supersigna formo** - Uzas la oficialan formon kun supersignoj (ĉ, ĝ, ĥ...)
- **x-formato** - Uzas la x-sistemon (cx, gx, hx...)
- **^-formato** - Uzas la hatforma sistemo (c^, g^, h^...)

### Paŝo 6: Procezi kaj rigardi la rezulton

1. Klaku la butonon "Sendi" por procezi vian tekston
2. La rezulto aperos sub la formularo
3. Vi povas vidi la rezulton en diversaj formetoj (HTML-antaŭrigardo, HTML-kodo aŭ simpla teksto, depende de via elektita formato)
4. Klaku "Elŝuti la rezulton" por konservi la rezulton kiel dosieron

## 4. Specialaj Teknikaj Notoj

### Specialaj Markadoj por Kontroli Anstataŭigon

Vi povas uzi specialajn markadojn por precize kontroli kiuj partoj de via teksto estos anstataŭigitaj:

- **%teksto%** - Teksto ĉirkaŭita per procentosignoj **NE** estos anstataŭigita. Ekzemple, "%Esperanto%" restos "Esperanto" en la fina rezulto.
- **@teksto@** - Teksto ĉirkaŭita per @-signoj estos anstataŭigita **LOKE** (nur ene de tiu fragmento). Tio estas utila por apliki specifajn anstataŭigojn al specifaj vortoj.

Limigoj:
- Por teksto en %-signoj: maksimume 50 signoj
- Por teksto en @-signoj: maksimume 18 signoj

## 5. Agordi Proprajn Anstataŭigajn Regulojn

Se vi volas krei proprajn anstataŭigajn regulojn, vi povas uzi la duan paĝon de la aplikaĵo "Krei JSON-dosieron por anstataŭigi (ĉinajn ideogramojn) en Esperantaj frazoj".

### Paŝo 1: Prepari CSV-dosieron

Vi bezonas CSV-dosieron, kiu enhavas Esperantajn radikojn kaj iliajn respondajn ideogramojn aŭ tradukojn. Vi povas:

- **Alŝuti propran CSV-dosieron** - Krei vian propran liston de Esperantaj radikoj kaj ideogramoj
- **Uzi la defaŭltan CSV-dosieron** - Uzi la provizitan ekzemplon

La CSV-dosiero devas havi la formaton:
```
Esperanta_radiko,Ideogramo_aŭ_Traduko
am,愛
bird,鳥
...
```

### Paŝo 2: Prepari JSON-dosieron(jn) (laŭvole)

Vi povas alŝuti du specojn de JSON-dosieroj por plidetale agordi la anstataŭigon:

1. **JSON pri segmentado de Esperantaj radikoj** - Indikas kiel dividi Esperantajn vortojn je radikoj
2. **JSON pri la finaj anstataŭigitaj tekstoj** - Permesas pli detalajn agordojn pri la fina aspekto

Vi povas ankaŭ uzi la defaŭltajn dosierojn por ambaŭ.

### Paŝo 3: Agordi la paralelan prilaboradon (laŭvole)

Simile al la ĉefa paĝo, vi povas agordi la paralelan prilaboradon por plirapidigi la procezon.

### Paŝo 4: Krei la JSON-dosieron

1. Klaku "Krei la JSON-dosieron por anstataŭigo"
2. Atendu dum la aplikaĵo prilaboras la datumojn (tio povas daŭri iom da tempo por grandaj dosieroj)
3. Kiam la proceso finiĝas, klaku "Elŝuti la finan anstataŭigan liston" por konservi la JSON-dosieron

### Paŝo 5: Uzi vian kreitan JSON-dosieron

Post kiam vi elŝutis vian JSON-dosieron, vi povas uzi ĝin en la ĉefa paĝo:
1. Iru al la ĉefa paĝo
2. Elektu "Alŝuti dosieron" ĉe la JSON-elektilo
3. Alŝutu vian ĵus kreitan JSON-dosieron

## 6. Eligaj Formatoj

La aplikaĵo ofertas plurajn formatojn por la eliga teksto. Jen pli da detaloj pri ĉiu:

### HTML-formato kun rubi-anotacioj kaj grandec-ĝustigo

- Montras Esperantajn vortojn kiel ĉeftekston
- Ideogramoj aperas kiel rubiaj anotacioj super la vortoj
- La grandeco de la rubiaj anotacioj estas aŭtomate ĝustigita laŭ la longo de la vorto
- Por tre longaj rubiaj anotacioj, aŭtomata linio-rompo aplikatas

Ekzemplo:
```html
<ruby>amiko<rt class="M_M">友</rt></ruby>
```

### HTML-formato kun rubi-anotacioj, grandec-ĝustigo kaj kanji-anstataŭigo

- Inversigita versio de la supre
- Ideogramoj aperas kiel ĉefteksto
- Esperantaj vortoj aperas kiel rubiaj anotacioj super la ideogramoj
- La grandeco estas simile ĝustigita

Ekzemplo:
```html
<ruby>友<rt class="M_M">amiko</rt></ruby>
```

### Formato kun krampoj

- Pli simpla formato sen HTML
- Montras vortojn en formo "Esperanto(ideogramo)"

Ekzemplo:
```
amiko(友)
```

### Formato kun krampoj kaj kanji-anstataŭigo

- Inversigita versio de la supre
- Montras vortojn en formo "ideogramo(Esperanto)"

Ekzemplo:
```
友(amiko)
```

### Nur konservi la anstataŭigitan tekston

- Montras nur la ideogramojn sen la originala Esperanta teksto
- Utila por tujaj komparoj aŭ specifa analizo

Ekzemplo:
```
友
```

## 7. Konsiloj kaj Solvoj de Problemoj

### Optimumaj Praktikoj

- **Por novaj uzantoj**: Komencu per la defaŭltaj JSON kaj CSV-dosieroj por komprenigi kiel la aplikaĵo funkcias.
- **Por grandaj tekstoj**: Uzu la paralelan pretigon por plirapidigi la procezon.
- **Por specifaj vortoj**: Uzu la @-markadon por kontroli kiel specifaj vortoj estas anstataŭigitaj.
- **Por konservi partojn de la teksto**: Uzu la %-markadon por eviti anstataŭigon.

### Oftaj Problemoj kaj Solvoj

1. **Problemo**: La aplikaĵo ne ŝargas la JSON-dosieron.
   **Solvo**: Certigu, ke la JSON-dosiero estas en la ĝusta formato. Vi povas elŝuti ekzemplon kaj kompari.

2. **Problemo**: Kelkaj vortoj ne estas anstataŭigitaj.
   **Solvo**: Verŝajne tiuj vortoj ne estas en la JSON-dosiero. Vi povas krei propran JSON-dosieron aldonante tiujn vortojn.

3. **Problemo**: La aplikaĵo malrapide prilaboras grandajn tekstojn.
   **Solvo**: Aktivigu la paralelan pretigon kaj agordu la kvanton da procezoj pli alten.

4. **Problemo**: La teksto estas tro granda por montri ĝin entute.
   **Solvo**: La aplikaĵo montras nur limigitan antaŭrigardon (la unuaj 247 linioj kaj la lastaj 3 linioj) por tre longaj tekstoj. Vi povas elŝuti la plenan rezulton per la elŝuta butono.

### Notoj pri Specialaĵoj

- La aplikaĵo povas trakti diversajn formojn de Esperantaj supersignaj literoj (ĉ, cx, c^) kaj konvertas ilin al la elektita formato.
- La JSON-dosiero estas kreita kun atento al la longo de vortoj kaj la strukturo de Esperantaj radikoj.
- La aplikaĵo inkluzivas specialan traktadon por verbaj sufiksoj (-as, -is, -os, ktp) kaj aliaj gramatikaj finaĵoj.

---

Ni esperas, ke vi ĝuos uzi ĉi tiun ilan por esplori la interrilaton inter Esperanto kaj ĉinaj ideogramoj! Se vi havas pliajn demandojn, bonvolu konsulti la GitHub-deponejon aŭ kontakti la evoluigantojn.
