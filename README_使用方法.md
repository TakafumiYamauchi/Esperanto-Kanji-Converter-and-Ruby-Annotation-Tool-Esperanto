# Uzinstrukcio por Esperanto-Teksta Konvertilo kaj Rubea Anotacio

## Enkonduko

Bonvenon al la Esperanto-Teksta Konvertilo kaj Rubea Anotacio! Ĉi tiu aplikaĵo estas potenca ilo por transformi Esperantan tekston diversmaniere, precipe por aldoni ĉinajn ideogramojn aŭ aliajn tradukojn kiel rubeon (superskribitaj notoj super vortoj) al via Esperanta teksto.

La aplikaĵo havas du ĉefajn funkciojn:

1. **Teksta Konvertado**: Transformi Esperantajn vortojn al formato kun rubeoj, kiuj montras ĉinajn ideogramojn aŭ alilingvajn tradukojn super la originalaj vortoj
2. **JSON-Dosiera Kreado**: Krei proprajn JSON-dosierojn por personecigi la konverton laŭ viaj preferoj

## Enhavo

1. [Baza Uzado](#baza-uzado)
2. [Detalaj Funkcioj](#detalaj-funkcioj)
   - [Elekto de JSON-Dosiero](#elekto-de-json-dosiero)
   - [Eniga Fonto](#eniga-fonto)
   - [Elira Formato](#elira-formato)
   - [Paralelaj Procezoj](#paralelaj-procezoj)
3. [Specialaj Signoj kaj Formataĵoj](#specialaj-signoj-kaj-formataĵoj)
4. [Krei Propran JSON-Dosieron](#krei-propran-json-dosieron)
   - [Paŝo 1: Pretigi CSV-Dosieron](#paŝo-1-pretigi-csv-dosieron)
   - [Paŝo 2: Pretigi JSON-Dosierojn](#paŝo-2-pretigi-json-dosierojn)
   - [Paŝo 3: Kreado de la Fina Anstataŭiga JSON-Dosiero](#paŝo-3-kreado-de-la-fina-anstataŭiga-json-dosiero)
5. [Alilingvaj Versioj](#alilingvaj-versioj)

## Baza Uzado

Por bazaj uzantoj, la aplikaĵo estas simpla:

1. Navigi al la ĉefa paĝo de la aplikaĵo
2. Elekti "Uzi la defaŭlton" por la JSON-dosiero
3. Tajpi aŭ alglui Esperantan tekston en la tekstokampon
4. Elekti preferatan eliran formaton kaj literformon
5. Alklaki la butonon "Sendi"
6. Vidi la konvertitan tekston kaj elŝuti ĝin se necese

## Detalaj Funkcioj

### Elekto de JSON-Dosiero

La aplikaĵo bezonas JSON-dosieron, kiu enhavas la anstataŭigajn regulojn. Vi povas:

- **Uzi la defaŭltan** JSON-dosieron, kiu jam havas multajn predifinitajn Esperanto-radikoj
- **Alŝuti propran** JSON-dosieron, se vi kreis unu per la dua paĝo de la aplikaĵo

La JSON-dosiero enhavas:
- Informojn pri kiel dividi Esperantajn vortojn en radikojn
- Tradukojn por ĉiu radiko
- Informojn pri kiel montri la rubeojn

### Eniga Fonto

Vi povas enigi Esperantan tekston per du manieroj:

- **Mana enigado**: Tajpi aŭ alglui rekte en la tekstokampon
- **Dosiera alŝuto**: Alŝuti teksto-dosieron (UTF-8 formato) kun Esperanta enhavo

La aplikaĵo subtenas Esperantajn signojn en diversaj formatoj:
- Supersignojn (ĉ, ĝ, ĥ, ĵ, ŝ, ŭ)
- X-sistemon (cx, gx, hx, jx, sx, ux)
- Ĉapelformon (c^, g^, h^, j^, s^, u^)

### Elira Formato

Vi povas elekti inter diversaj elirformatoj:

1. **HTML-formato kun adaptita grandeco de la rubia teksto**: La rubeoj aperas super Esperantaj vortoj, kaj la grandeco aŭtomate ĝustiĝas laŭ la longeco de la teksto
2. **HTML-formato kun adaptita rubia teksto (ĉinaj ideogramoj anstataŭigitaj)**: Simile al la unua opcio, sed la ĉinaj ideogramoj estas la ĉefa teksto kaj la Esperantaj vortoj aperas kiel rubeo
3. **HTML-formato**: Simpla HTML-rubea formato sen grandeca adaptiĝo
4. **HTML-formato (kun anstataŭigo de ĉinaj ideogramoj)**: Simila al opcio 3, sed kun ĉinaj ideogramoj kiel ĉefa teksto
5. **Formo per krampoj**: Anstataŭ HTML-rubeoj, ĉi tiu formato uzas Esperantan vorton kun traduko en krampoj
6. **Formo per krampoj (kun anstataŭigo de ĉinaj ideogramoj)**: Simila al opcio 5, sed la ĉinaj ideogramoj estas ekstera kaj la Esperantaj vortoj estas en krampoj
7. **Simpla anstataŭigo**: Nur montri la tradukitan tekston, sen la originala Esperanto

### Paralelaj Procezoj

Por grandaj tekstoj, vi povas aktivigi la paralelan procesadon:

1. Malfermi la "Altnivelaj agordoj" sekcion
2. Marki la skatoleton "Uzi paralelan prilaboradon"
3. Agordi la nombron de samtempaj procezoj (kutime inter 2 kaj 4)

Tio plibonigos la rendimenton kiam vi traktas grandajn tekstojn.

## Specialaj Signoj kaj Formataĵoj

La aplikaĵo subtenas specialajn signojn por kontroli kio anstataŭiĝas:

- **%teksto%** - Protektas tekston de anstataŭigo. Teksto inter % signoj (maksimume 50 signoj) ne estos konvertita.
  Ekzemple: "Mi lernas %Esperanto% ĉiutage" → La vorto "Esperanto" restos netuŝita.

- **@teksto@** - Indikas lokalajn anstataŭigojn. Teksto inter @ signoj (maksimume 18 signoj) estos konvertita eĉ se tio ne okazas aliloke.
  Ekzemple: "Mi parolas pri la lingvo @Esperanto@" → Nur tiu specifa okazo de "Esperanto" estos konvertita.

## Krei Propran JSON-Dosieron

Por pli spertaj uzantoj, la dua paĝo de la aplikaĵo permesas krei propran anstataŭigan JSON-dosieron. Jen kiel:

### Paŝo 1: Pretigi CSV-Dosieron

Vi bezonas CSV-dosieron kun almenaŭ du kolumnoj:
1. Esperantaj radikoj
2. Tradukaj enhavoj (ĉinaj ideogramoj, anglaj vortoj, ktp.)

Vi povas:
- Alŝuti ekzistan CSV-dosieron
- Uzi la defaŭltan CSV-dosieron kiel ekzemplon

Ekzemplo de CSV-enhavo:
```
domo,家
lerni,学ぶ
libro,本
```

### Paŝo 2: Pretigi JSON-Dosierojn

Du JSON-dosieroj estas uzataj por kontroli la konvertadon:

1. **Dosiero pri segmentado de Esperantaj radikoj**:
   - Difinas kiel dividi vortojn en radikojn
   - Specifikas kiel trakti verbajn finaĵojn (-as, -is, -os, ktp.)
   - Ekzemple: `["am", "dflt", ["verbo_s1"]]` signifas ke "am" estas verba radiko kaj ĝiaj konjugacioj (amas, amis, amos, ktp.) devus esti traktitaj speciale

2. **Dosiero pri la finaj anstataŭigitaj tekstoj**:
   - Permesas aldoni specialajn anstataŭigojn
   - Malofte necesa por bazaj uzantoj

Ambaŭ JSON-dosieroj povas esti alŝutitaj aŭ vi povas uzi la defaŭltajn dosierojn.

### Paŝo 3: Kreado de la Fina Anstataŭiga JSON-Dosiero

Post kiam vi elektis viajn CSV- kaj JSON-dosierojn:

1. Elektu vian preferatan eliran formaton
2. Se necese, agordu la parametrojn por paralela procesado
3. Alklaku la butonon "Krei la JSON-dosieron por anstataŭigo"
4. Atendu dum la aplikaĵo konstruas la JSON-dosieron (tio povas daŭri kelkajn minutojn)
5. Kiam finita, elŝutu la kreitan JSON-dosieron

Tiam vi povas uzi tiun personigitan JSON-dosieron kun la ĉefa aplikaĵo por viaj propraj anstataŭigoj.

## Alilingvaj Versioj

La aplikaĵo estas disponebla en 14 lingvoj, kiujn vi povas trovi en la ligilo-sekcio ĉe la fino de la ĉefa paĝo. Ĉiu lingva versio havas propran ligilon al la aplikaĵo kaj al la GitHub-deponejo kun detalaj instrukcioj.

Disponebla en: Esperanto, Angla, Japana, Ĉina, Korea, Rusa, Hispana, Itala, Franca, Germana, Araba, Hinda, Pola, Vjetnama, kaj Indonezia.

---

Ni esperas, ke vi ĝuos uzi la Esperanto-Tekstan Konvertilon kaj Rubean Anotacion! Se vi havas demandojn aŭ sugestojn, bonvolu viziti la GitHub-deponejon por pliaj informoj.
