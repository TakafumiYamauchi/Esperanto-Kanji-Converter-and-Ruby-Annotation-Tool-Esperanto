# Teĥnika Analizo kaj Dokumentado de la Esperanto-Kanji-Konvertilo

## Enkonduko por Programistoj

Saluton, mez-nivela programisto! Vi jam konas la uzantflankan funkciadon de la aplikaĵo, sed nun ni esploros la internan strukturon kaj la teĥnikajn meĥanismojn. Ĉi tiu dokumentado celas provizi profundan komprenon pri la arkitekturo, algoritmoj kaj procezfluo de la "Anstataŭigilo por Esperantaj tekstoj per ĉinaj ideogramoj kaj rubi-anotacia ilo".

La aplikaĵo estas konstruita per Python, uzante la Streamlit-framon por la retinterfaco. Ĝi konsistas el kvar ĉefaj Python-dosieroj:

1. **main.py** - La ĉefa aplikaĵo kun la retinterfaco
2. **Paĝo por generi JSON-dosieron...** - Speciala paĝo por krei personigitajn anstataŭigajn JSON-regulojn
3. **esp_text_replacement_module.py** - Modulo por tekst-anstataŭigo kaj transformoj
4. **esp_replacement_json_make_module.py** - Modulo por konstrui anstataŭigajn JSON-dosierojn

Ni analizos ĉiun dosieron detale, klarigante la kodan strukturon, la algoritmojn kaj la interagojn inter diversaj komponantoj. Ni ankau provizos diagramojn kaj ekzemplojn por faciligi vian komprenon.

## 1. Arkitekturo de la Sistemo

La aplikaĵo sekvas modulan arkitekturon, kie ĉiu komponanto havas specifan respondecon:

![Arkitektura Superrigardo]

### 1.1 Moduloj kaj iliaj interrilatoj

- **main.py** estas la enirpunkto kiu organizas la fluecon de la aplikaĵo, importas la funkcionalecojn el aliaj moduloj, kaj prezentas la uzantinterfacon per Streamlit.
- **esp_text_replacement_module.py** enhavas la kernfunkciojn por transformi Esperantajn tekstojn.
- **esp_replacement_json_make_module.py** fokusiĝas pri la kreado kaj pritraktado de anstataŭigaj JSON-strukturoj.
- **Paĝo por generi JSON...** estas speciala Streamlit-paĝo kiu permesas krei kaj adapti anstataŭigajn regulojn.

### 1.2 Datenfluoj

La datenfluoj en la aplikaĵo sekvas ĉi tiujn paŝojn:

1. La uzanto enigas tekston aŭ alŝutas dosieron
2. La teksto trapasas serion da transformaj funkcioj
3. Anstataŭigaj reguloj estas aplikataj laŭ prioritato
4. La transformita teksto estas montrata al la uzanto en la elektita formato

## 2. Detala Analizo de `main.py`

La `main.py` dosiero servas kiel la kerno de la aplikaĵo. Ni analizu ĝiajn ĉefajn elementojn:

### 2.1 Strukturo kaj Importoj

```python
import streamlit as st
import re
import io
import json
import pandas as pd
from typing import List, Dict, Tuple, Optional
import streamlit.components.v1 as components
import multiprocessing
```

La importoj montras ke la aplikaĵo uzas:
- **Streamlit** por la retinterfaco
- **Regulesprimo (re)** por tekst-traktado
- **JSON** por datumtraktado
- **Pandas** por tabelaj datumstruktoj
- **Multiprocessing** por plirapidigi anstataŭigon per paralela procezado

### 2.2 Multiprocessing-Agordoj

```python
try:
    multiprocessing.set_start_method("spawn")
except RuntimeError:
    pass
```

Ĉi tiu kodo agordigas multiprocessing por uzi la 'spawn' metodon, kiu evitas problemojn kun 'PicklingError'. La spawn-metodo kreas novan Python-interpretilon por ĉiu procezilo, malsame al 'fork' kiu kopias la tutan staton de la ĉefa procezo.

### 2.3 Importoj el Personaj Moduloj

```python
from esp_text_replacement_module import (
    x_to_circumflex,
    x_to_hat,
    hat_to_circumflex,
    circumflex_to_hat,
    replace_esperanto_chars,
    import_placeholders,
    orchestrate_comprehensive_esperanto_text_replacement,
    parallel_process,
    apply_ruby_html_header_and_footer
)
```

Ĉi tio importas specifajn funkciojn el `esp_text_replacement_module.py` por:
- Konverti Esperantajn literojn inter diversaj formatoj (ĉ/cx/c^)
- Importi rezervitajn placotenilojn
- Direkti la ĉefan anstataŭigan procezon
- Apliki paralelajn procezojn
- Aldoni HTML-ĉapon/piedojn al la rezultaj tekstoj

### 2.4 Kaŝmemora Funkcio por JSON-Ŝargado

```python
@st.cache_data
def load_replacements_lists(json_path: str) -> Tuple[List, List, List]:
    """
    JSONファイルをロードし、以下の3つのリストをタプルとして返す:
    1) replacements_final_list
    2) replacements_list_for_localized_string
    3) replacements_list_for_2char
    """
```

La `@st.cache_data` dekoratoro estas kritika por rendimento: ĝi konservas la rezulton de la funkcio por eviti ripetitan ŝargadon de grandaj JSON-dosieroj. La funkcio mem legas JSON-dosieron kaj redonas tri listojn uzatajn por diversaj anstataŭigtipoj:

1. **replacements_final_list** - Anstataŭigaj reguloj por tutaj vortoj
2. **replacements_list_for_localized_string** - Anstataŭigaj reguloj por lokoj (markataj per @)
3. **replacements_list_for_2char** - Anstataŭigaj reguloj por dulitera radikaj afiksoj

### 2.5 Retinterfaco per Streamlit

La plimulto de `main.py` estas dediĉita al la Streamlit-retinterfaco, kiu enhavas:

- Paĝotitolo kaj agordoj (`st.set_page_config`)
- JSON-elektilo (defaŭlta aŭ alŝutita)
- Placotenilaj datumoj (por markataj tekstpartoj)
- Anstataŭiga formato (HTML, krampo-formato, ktp.)
- Tekst-enigo (mana aŭ alŝutita)
- Preferencoj pri Esperantaj literformoj
- Rezulto-prezentado kaj elŝut-ebloj

### 2.6 Ĉefa Procezfluo

La ĉefa procezfluo komenciĝas kiam la uzanto alklakadas la "Sendi" butonon:

```python
if submit_btn:
    st.session_state["text0_value"] = text0
    if use_parallel:
        processed_text = parallel_process(
            text=text0,
            num_processes=num_processes,
            placeholders_for_skipping_replacements=placeholders_for_skipping_replacements,
            replacements_list_for_localized_string=replacements_list_for_localized_string,
            placeholders_for_localized_replacement=placeholders_for_localized_replacement,
            replacements_final_list=replacements_final_list,
            replacements_list_for_2char=replacements_list_for_2char,
            format_type=format_type
        )
    else:
        processed_text = orchestrate_comprehensive_esperanto_text_replacement(
            text=text0,
            placeholders_for_skipping_replacements=placeholders_for_skipping_replacements,
            replacements_list_for_localized_string=replacements_list_for_localized_string,
            placeholders_for_localized_replacement=placeholders_for_localized_replacement,
            replacements_final_list=replacements_final_list,
            replacements_list_for_2char=replacements_list_for_2char,
            format_type=format_type
        )
```

Rimarku kiel la aplikaĵo disponigas du vojojn por procezi la tekston:
1. **Serieca procezado** per `orchestrate_comprehensive_esperanto_text_replacement`
2. **Paralela procezado** per `parallel_process`

La elekto dependas de la uzanta agordo "Uzi paralelan pretigon".

### 2.7 Rezulta Prezentado

Post procezado, la rezulto estas prezentata en diversaj formatoj:

```python
if processed_text:
    # Montri antaŭrigardon (limigita al MAX_PREVIEW_LINES)

    if "HTML" in format_type:
        tab1, tab2 = st.tabs(["HTML-antaŭrigardo", "Rezulto (HTML-kodo)"])
        with tab1:
            components.html(preview_text, height=500, scrolling=True)
        with tab2:
            st.text_area("Generita HTML-kodo:", preview_text, height=300)
    else:
        tab3_list = st.tabs(["Rezulta teksto"])
        with tab3_list[0]:
            st.text_area("Rezulto:", preview_text, height=300)
```

Tiu kodo montras la rezulton en taŭga formato, limitigante la antaŭrigardon por longaj tekstoj kaj provizante HTML-antaŭrigardon kiam necesas.

## 3. Analizo de `esp_text_replacement_module.py`

Ĉi tiu modulo enhavas la kernfunkciojn por transformi Esperantajn tekstojn. Ĝiaj ĉefaj kapabloj inkluzivas:

### 3.1 Esperantaj Literkonvertiĝoj

```python
x_to_circumflex = {
    'cx': 'ĉ', 'gx': 'ĝ', 'hx': 'ĥ', 'jx': 'ĵ', 'sx': 'ŝ', 'ux': 'ŭ',
    'Cx': 'Ĉ', 'Gx': 'Ĝ', 'Hx': 'Ĥ', 'Jx': 'Ĵ', 'Sx': 'Ŝ', 'Ux': 'Ŭ'
}
```

La modulo difinas plurajn vortarojn kiuj ligas diversajn Esperantajn literformatojn:
- `x_to_circumflex`: Konvertas x-formaton (cx) al ĉapelitaj literoj (ĉ)
- `circumflex_to_x`: Inverse, konvertas ĉapelitajn literojn al x-formato
- `x_to_hat`: Konvertas x-formaton al suprenstreko-formato (c^)
- ... ktp

Tiuj mapoj estas uzataj de funkcioj kiel `replace_esperanto_chars` kaj `convert_to_circumflex` por unuformigi la Esperantajn literojn en la teksto.

### 3.2 Spacetado kaj Normigo

```python
def unify_halfwidth_spaces(text: str) -> str:
    """
    全角スペース(U+3000)は変更せず、半角スペースと視覚的に区別がつきにくい空白文字を
    ASCII半角スペース(U+0020)に統一する。
    """
    pattern = r"[\u00A0\u2002\u2003\u2004\u2005\u2006\u2007\u2008\u2009\u200A]"
    return re.sub(pattern, " ", text)
```

Ĉi tiu funkcio enordigigas spacetojn por konsekvenco, transformante diversajn Unicode-spacetajn signojn al regula ASCII-spaceto.

### 3.3 Placotenila Traktado

```python
def safe_replace(text: str, replacements: List[Tuple[str, str, str]]) -> str:
    """
    (old, new, placeholder) のリストを受け取り、
    text中の old → placeholder → new の段階置換を行う。
    """
    valid_replacements = {}
    # まず old→placeholder
    for old, new, placeholder in replacements:
        if old in text:
            text = text.replace(old, placeholder)
            valid_replacements[placeholder] = new
    # 次に placeholder→new
    for placeholder, new in valid_replacements.items():
        text = text.replace(placeholder, new)
    return text
```

La `safe_replace` funkcio estas grava por eviti anstataŭigajn koliziaĵojn. Ĝi:

1. Unue anstataŭigas ĉiun trovan ŝablonon per unika **placotenilo**
2. Poste anstataŭigas la placotenilojn per la finalaj valoroj

Ĉi tiu du-paŝa procedo certigas ke la anstataŭigaj ŝablonoj ne interakciu neantaŭvideble, precipe kiam unu anstataŭigo povus krei novan trafon por alia anstataŭigo.

### 3.4 Specialaj Tekstpartoj kun '%' kaj '@'

```python
# '%' で囲まれた箇所をスキップするための正規表現
PERCENT_PATTERN = re.compile(r'%(.{1,50}?)%')

def find_percent_enclosed_strings_for_skipping_replacement(text: str) -> List[str]:
    """'%foo%' の形を全て抽出。50文字以内に限定。"""
    matches = []
    used_indices = set()
    for match in PERCENT_PATTERN.finditer(text):
        start, end = match.span()
        if start not in used_indices and end-2 not in used_indices:
            matches.append(match.group(1))
            used_indices.update(range(start, end))
    return matches
```

La modulo havas specialajn funkciojn por trovi kaj trakti tekstojn ĉirkaŭigitajn per:
- **%...%** - Teksto kiu ne estos anstataŭigita
- **@...@** - Teksto kiu estos anstataŭigita loke (nur ene de tiuj signoj)

Ĉi tiuj funkcioj uzas regulespriman kongruan (`PERCENT_PATTERN`, `AT_PATTERN`) por identigi tiujn specialajn regionojn kaj speciale trakti ilin.

### 3.5 Ĉefa Anstataŭiga Funkcio

```python
def orchestrate_comprehensive_esperanto_text_replacement(
    text,
    placeholders_for_skipping_replacements: List[str],
    replacements_list_for_localized_string: List[Tuple[str, str, str]],
    placeholders_for_localized_replacement: List[str],
    replacements_final_list: List[Tuple[str, str, str]],
    replacements_list_for_2char: List[Tuple[str, str, str]],
    format_type: str
) -> str:
```

Tio estas la kernfunkcio kiu direktas la tutan anstataŭigan procezon. Ĝi aplikas plurajn transformojn en specifika ordo:

1. Unuformigas spacetojn kaj Esperantajn literojn
2. Traktas `%`-ĉirkaŭigitajn regionojn por protekti ilin
3. Traktas `@`-ĉirkaŭigitajn regionojn por loka anstataŭigo
4. Aplikas ĉeftekstajn anstataŭigojn
5. Aplikas dukterradikajn anstataŭigojn (dufoje, por trakti potencialajn truojn)
6. Restaŭrigas placotenilojn
7. Adaptigas por HTML-prezentado, se bezonata

### 3.6 Paralela Procesado

```python
def parallel_process(
    text: str,
    num_processes: int,
    placeholders_for_skipping_replacements: List[str],
    replacements_list_for_localized_string: List[Tuple[str, str, str]],
    placeholders_for_localized_replacement: List[str],
    replacements_final_list: List[Tuple[str, str, str]],
    replacements_list_for_2char: List[Tuple[str, str, str]],
    format_type: str
) -> str:
```

Ĉi tiu funkcio implementas paraleligitan version de la anstataŭiga procezado. Ĝi:

1. Dividas la tekston laŭ linioj
2. Distribuas apartajn linio-grupojn al malsamaj procezoj
3. Rekunigas la rezultojn en la ĝusta ordo

Tio signife plibonigas la rendimenton kun grandaj tekstoj, precipe sur multkerniĝaj sistemoj.

## 4. Analizo de `esp_replacement_json_make_module.py`

Ĉi tiu modulo enhavas funkciojn por krei personigitajn anstataŭigajn JSON-dosierojn. Ĝi ĉefe estas uzata per la dua paĝo de la aplikaĵo.

### 4.1 Tekst-Formata Eligo

```python
def output_format(main_text, ruby_content, format_type, char_widths_dict):
    """
    エスペラント語根(main_text) と それに対応する訳/漢字(ruby_content) を
    指定の format_type で繋ぎ合わせる
    """
    if format_type == 'HTML格式_Ruby文字_大小调整':
        width_ruby = measure_text_width_Arial16(ruby_content, char_widths_dict)
        width_main = measure_text_width_Arial16(main_text, char_widths_dict)
        ratio_1 = width_ruby / width_main
        if ratio_1 > 6:
            return f'<ruby>{main_text}<rt class="XXXS_S">{insert_br_at_third_width(ruby_content, char_widths_dict)}</rt></ruby>'
        elif ratio_1 > (9/3):
            return f'<ruby>{main_text}<rt class="XXS_S">{insert_br_at_half_width(ruby_content, char_widths_dict)}</rt></ruby>'
```

Tiu funkcio generas diversajn eligo-formatojn bazitajn sur la uzanta elekto. La plej interesa aspekto estas la inteligenteco aplikita al la HTML-formatoj kun grandecaj ĝustigoj:

1. Ĝi mezuras la larĝecon de la ĉefteksto kaj la rubiteksto
2. Kalkulas la larĝecan proporcion inter ili
3. Aŭtomate ĝustigas la rubitekstan grandecon surbaze de tiu proporcio
4. Aldonas lini-rompojn kiam la rubiteksto estas tro longa

Tio ĉi certigas ke la rubianotacioj ĉiam vidiĝas bone, eĉ kiam la proporcio de Esperanto/Kanji tekstolarĝo varias.

### 4.2 Rubistila Agordiĝo

```python
def capitalize_ruby_and_rt(text: str) -> str:
    """
    <ruby>〜</ruby> の親文字列 / ルビ文字列を大文字化する例。
    """
    def replacer(match):
        g1 = match.group(1)
        g2 = match.group(2)
        g3 = match.group(3)
        g4 = match.group(4)
        g5 = match.group(5)
        g6 = match.group(6)
        g7 = match.group(7)
        g8 = match.group(8)
        if g1.strip():
            return g1.capitalize() + g2 + g3 + g4 + g5 + g6 + (g7 if g7 else '') + g8
        else:
            parent_text = g3.capitalize()
            rt_text = g5.capitalize()
            return g1 + g2 + parent_text + g4 + rt_text + g6 + (g7 if g7 else '') + g8
```

Ĉi tiu funkcio estas specife uzata por majuskligi tekstojn en rubi-markoj. Ĝi uzas kompleksan regulespriman grupiĝon por identigi la diversajn partojn de la HTML-kodero kaj apliki majuskligon al specifaj regionoj.

### 4.3 Paralela JSON-Konstruado

```python
def parallel_build_pre_replacements_dict(
    E_stem_with_Part_Of_Speech_list: List[List[str]],
    replacements: List[Tuple[str, str, str]],
    num_processes: int = 4
) -> Dict[str, List[str]]:
```

Simile al la paralela procezado en la ĉefmodulo, ĉi tiu funkcio paralelumas la konstruadon de anstataŭiga vortaro, dividas la datumojn inter pluraj procezoj kaj rekunigas la rezultojn.

### 4.4 Redundanca Rubi-Forigado

```python
def remove_redundant_ruby_if_identical(text: str) -> str:
    """
    <ruby>xxx<rt class="XXL_L">xxx</rt></ruby> のように、
    親文字列とルビ文字列が完全に同一の場合に <ruby> を取り除く
    """
    def replacer(match: re.Match) -> str:
        group1 = match.group(1)
        group2 = match.group(2)
        if group1 == group2:
            return group1
        else:
            return match.group(0)
    replaced_text = IDENTICAL_RUBY_PATTERN.sub(replacer, text)
    return replaced_text
```

Tiu funkcio forigas nenecesajn rubimarkojn kiam la ĉefteksto kaj la rubiteksto estas identikaj, pligajnante la prezentadon kaj malgrandigante la HTML-grandecon.

## 5. Analizo de la Paĝo por Generi JSON-dosieron

La dosiero "Paĝo por generi JSON-dosieron por anstataŭigi Esperantajn frazojn per signĉenoj (ĉinaj skribsignoj).py" enhavas specialan Streamlit-paĝon por kreado de personigitaj anstataŭigaj JSON-dosieroj.

### 5.1 Koncerna Strukturo

La dosiero havas kvin ĉefpartojn:

1. **Prepara kodo**: importoj, konstantaj listoj kaj placoteniloj
2. **Streamlit-retinterfaco**: por uzi la ilojn
3. **CSV-procezado**: legas informojn pri Esperanto-radikoj kaj tradukaj rubi-anotacioj
4. **JSON-procezado**: legas regulojn pri radikaj segmentadoj kaj anstataŭigoj
5. **JSON-generada algoritmo**: konstruas la finajn anstataŭigajn listojn

### 5.2 Gramatikaj Listoj

```python
verb_suffix_2l = {
    'as':'as', 'is':'is', 'os':'os', 'us':'us','at':'at','it':'it','ot':'ot',
    'ad':'ad','iĝ':'iĝ','ig':'ig','ant':'ant','int':'int','ont':'ont'
}

suffix_2char_roots=['ad', 'ag', 'am', 'ar', 'as', 'at', 'av', ...]
prefix_2char_roots=['al', 'am', 'av', 'bo', 'di', 'du', ...]
standalone_2char_roots=['al', 'ci', 'da', 'de', 'di', 'do', ...]
```

La algoritmo uzas kompleksajn listojn por identigi kaj trakti Esperantajn gramatikajn elementojn:

- Verbaj sufiksoj (as, is, os, us, ktp.)
- Dukteraj radiksufiksoj (ad, ag, am, ktp.)
- Dukteraj radikafiksoj (al, am, av, ktp.)
- Dependaj dukteraj radikoj (al, ci, da, ktp.)

Plie, specialaj listoj `AN` kaj `ON` enhavas Esperantajn vortojn kun specifaj afikssekvaĵoj, kiuj bezonas aparte atenti ĉe la anstataŭigado.

### 5.3 Alta Komplekseco en la Anstataŭigo-Algoritmo

Granda parto de la kodo estas dediĉita al multobla pasado tra la Esperantaj vortoj, konstruante kompleksan anstataŭigan retan sistemon. Ĝi havas aparte sofistikan pritraktadon de:

- Verbaj formoj (inkluzivante ĉiujn tempojn kaj modojn)
- Afiksitaj vortoj (kun prefiksoj kaj sufiksoj)
- Prioritigiĝo inter konkursantaj anstataŭigoj (ekz. anstataŭigi laŭ longeco)

Rimarku tiun kodan pecon kiu ilustras la kompleksecon:

```python
for i,j in pre_replacements_dict_2.items():
    if j[2]==20000:
        if "名词" in j[1]:
            for k in ["o","on",'oj']:
                if not i+k in pre_replacements_dict_2:
                    pre_replacements_dict_3[' '+i+k] = [' '+j[0]+k, j[2] + (len(k)+1)*10000 - 5000]
        if "形容词" in j[1]:
            for k in ["a","aj",'an']:
                if not i+k in pre_replacements_dict_2:
                    pre_replacements_dict_3[' '+i+k] = [' '+j[0]+k, j[2] + (len(k)+1)*10000 - 5000]
                else:
                    pre_replacements_dict_3[i+k] = [j[0]+k, j[2] + len(k)*10000 - 5000]
                    unchangeable_after_creation_list.append(i+k)
```

Tiu kodo konstruas diversajn vortformojn bazitajn sur la vortspeco kaj aldonas ilin al la anstataŭiga vortaro kun taŭgaj prioritatoj.

## 6. Procezfluo: Paŝo post Paŝo

Por helpi al vi plene kompreni la aplikaĵon, jen detala paŝo-post-paŝa pritakto de la procezfluo:

### 6.1 Teksta Procezado en `main.py`

1. **Uzanto enigas tekston** en la Streamlit-interfaco
2. **Aplikaĵo ŝargas anstataŭigajn regulojn** el JSON-dosiero
3. **Submit-butono aktivigas** la anstataŭigan procezon
4. **Aplikaĵo prokuras placotenilojn** el eksternaj dosieroj
5. **Uzante paralelajn procezojn** (laŭvole) aŭ seriecan procezadon, la teksto estas transformata
6. **Esperantaj literoj estas konvertataj** al la elektita formato
7. **HTML-kodigoj estas aldonataj** por rubianotacioj
8. **Rezulta teksto estas montrata** al la uzanto

### 6.2 Anstataŭiga Procezado en `orchestrate_comprehensive_esperanto_text_replacement`

1. **Unuformigas spacetojn**
2. **Konvertas Esperantajn literojn** al ĉapelitaj formoj
3. **Trovas kaj procezas %...% sekvaĵojn** (protektataj regionoj)
4. **Trovas kaj procezas @...@ sekvaĵojn** (lokaj anstataŭigoj)
5. **Apliko de tutglobaj anstataŭigaj reguloj**
6. **Apliko de dukteraj radikaj anstataŭigoj** (dufoje)
7. **Restaŭrigo de placoteniloj**
8. **Transformado al la fina formato** (HTML, krampoj, ktp.)

### 6.3 JSON-Kreado en la Speciala Paĝo

1. **Uzanto alŝutas CSV-dosieron** kun Esperantaj radikoj kaj tradukoj
2. **Aplikaĵo legas JSONajn dosierojn** kun reguloj pri radikaj segmentadoj kaj anstataŭigoj
3. **Aplikaĵo ŝargas Esperantajn specialajn listojn** (verbaj sufiksoj, afiksoj, ktp.)
4. **Granda traktado de la radikaj datumoj** okazas, konstruante la vortformojn
5. **Prioritato-sistemo estas aplikata** al diversaj anstataŭigaj tipoj
6. **Tri anstataŭigaj listoj estas konstruataj**:
   - Tutglobaj anstataŭigoj
   - Lokaj anstataŭigoj
   - Dukteraj radikaj anstataŭigoj
7. **Rezulta JSON-dosiero estas generata** por elŝuto


## 7. Gravaj Teĥnikaj Analizaĵoj

### 7.1 Placotenila Sistemo

Unu el la plej kritikaj teknikaj eltrovoj en la aplikaĵo estas la placotenila sistemo. Kial ĝi estas tiom grava? Ĉar aŭtonoma tekst-anstataŭigo prezentas kialimojn de ambigueco kaj koliziaĵoj.

Ekzemple, imagu ke vi havas du anstataŭigajn regulojn:
- "est" → "存在"
- "lernest" → "学校"

Se vi simple aplikus ilin laŭ sinsekve, kaj komencus per anstataŭigo de "est" → "存在", kio okazus kun la vorto "lernestoj"?

Ĝi fariĝus "lern存在oj", kaj la sekva anstataŭigo ne plu rekonus "lernest". Por eviti tiun problemon, la aplikaĵo:

1. Unue anstataŭigas ĈIUJN tekstojn per unikaj placoteniloj
2. Poste transformas la placotenilojn al finalaj valoroj

```python
def safe_replace(text: str, replacements: List[Tuple[str, str, str]]) -> str:
    valid_replacements = {}
    # まず old→placeholder
    for old, new, placeholder in replacements:
        if old in text:
            text = text.replace(old, placeholder)
            valid_replacements[placeholder] = new
    # 次に placeholder→new
    for placeholder, new in valid_replacements.items():
        text = text.replace(placeholder, new)
    return text
```

La placoteniloj estas antaŭŝargitaj el eksternaj tekstdosieroj, kiuj enhavas milojn da unikaj ŝablonoj kiel "$20987$", "$499999$", "@20374@", ktp. Tiuj estas garantie unikaj kaj ne troveblas en la originalaj Esperantaj tekstoj.

### 7.2 Prioritata Sistemo

Alia sofistika elemento estas la prioritata sistemo por anstataŭigoj. La aplikaĵo ne simple anstataŭigas vortojn laŭ la ordo en kiu ili aperas en la listo, sed asignas komplikan prioritataĵon:

```python
pre_replacements_dict_3[i+k] = [j[0]+k, j[2] + len(k)*10000 - 3000]
```

La prioritataj kalkuloj enhavas plurajn elementojn:
- Baza prioritato laŭ vorta longeco (j[2])
- Prioritata bono por ĉiu aldonita litero (len(k)*10000)
- Prioritata malbono por nematchita vorto (-3000)

Tiu sistemo certigas ke:
1. Pli longaj vortoj estas anstataŭigitaj unue (evitante partajn vortajn anstataŭigojn)
2. Kompletaj gramatikaj formoj (kiel "lerni" kun la verbfino "i") estas pripreferataj kompare kun nur radikaj formoj ("lern")
3. Specialaj kazoj (kiel la dulitera radiko "al" kiel prefikso aŭ memstara vorto) estas traktataj ĝuste

### 7.3 Rubia Grandeca Adaptado

La aplikaĵo enhavas sofistikan sistemon por adapti la grandecon de rubi-anotacioj laŭ la proporcio de la tekstoj:

```python
def output_format(main_text, ruby_content, format_type, char_widths_dict):
    if format_type == 'HTML格式_Ruby文字_大小调整':
        width_ruby = measure_text_width_Arial16(ruby_content, char_widths_dict)
        width_main = measure_text_width_Arial16(main_text, char_widths_dict)
        ratio_1 = width_ruby / width_main
        if ratio_1 > 6:
            return f'<ruby>{main_text}<rt class="XXXS_S">{insert_br_at_third_width(ruby_content, char_widths_dict)}</rt></ruby>'
        # ...ktp
```

Rimarku kiel la funkcio:
1. Mezuras la ekzaktan larĝecon de ĉiu teksto uzante antaŭdefinitan karakterlarĝecan tabelon
2. Kalkulas la proporcion inter la du tekstoj
3. Aplikas taŭgan CSS-klason por ĝustigi la grandecon
4. Aldonas lini-rompojn kiam necesas por longaj anotacioj

La sistemo uzas specialan JSON-dosieron kiu enhavas mezurojn de preskaŭ ĉiu signo en la Unicode Basic Multilingual Plane, kio permesas precize kalkuli tekstlarĝecojn sen efektive bildigi la tekston.

### 7.4 Paralela Procezado

La paralela procezado en la aplikaĵo estas notinda ekzemplo de rendimenta optimumigo:

```python
def parallel_process(text: str, num_processes: int, ...):
    lines = re.findall(r'.*?\n|.+$', text)
    num_lines = len(lines)

    lines_per_process = max(num_lines // num_processes, 1)
    ranges = [(i * lines_per_process, (i + 1) * lines_per_process) for i in range(num_processes)]
    ranges[-1] = (ranges[-1][0], num_lines)

    with multiprocessing.Pool(processes=num_processes) as pool:
        results = pool.starmap(
            process_segment,
            [(lines[start:end], ...) for (start, end) in ranges]
        )
    return ''.join(results)
```

La implemento:
1. Dividas la tekston laŭ linioj
2. Kalkulas optimuman nombron da linioj por ĉiu procezo
3. Kreas listo de interspacoj por distribui al malsamaj procezoj
4. Uzas Python `multiprocessing.Pool` por ruli la anstataŭigadojn paralele
5. Rekunigas la rezultojn en la sama originala ordo

Tio povas signife akceli la procezon, precipe sur modernaj plurkernaj procesoroj kaj kun longaj tekstoj.

## 8. Internaj Algoritmetoj kaj Ilia Interrilatado

La aplikaĵo enhavas multajn malsamajn algoritmojn kiuj interrilatas komplekse. Ni priskribu kelkajn kernajn interrilatojn:

### 8.1 Anstataŭigaj Listoj kaj Ilia Uzado

Tri anstataŭigaj listoj estas kritikaj por la aplikaĵo:

1. **replacements_final_list** - Uzata por tutglobalaj anstataŭigoj
2. **replacements_list_for_2char** - Uzata por dulitera radikaj anstataŭigoj
3. **replacements_list_for_localized_string** - Uzata por lokaj (@-ĉirkaŭitaj) anstataŭigoj

Tiuj listoj estas uzataj en specifa ordo en la `orchestrate_comprehensive_esperanto_text_replacement` funkcio por certigi konvenan procezadon:

```python
# 5) 大域置換 (old, new, placeholder)
valid_replacements = {}
for old, new, placeholder in replacements_final_list:
    if old in text:
        text = text.replace(old, placeholder)
        valid_replacements[placeholder] = new

# 6) 2文字語根置換(2回)
valid_replacements_for_2char_roots = {}
for old, new, placeholder in replacements_list_for_2char:
    # ...
```

La dulitera radika anstataŭigo estas farata dufoje, kio estas neordinara sed intenca: la unua paso povas krei novajn okazojn de anstataŭendaj dukteraj radikoj, kiujn la dua paso kaptas.

### 8.2 Generado de Anstataŭigaj Listoj

La algoritmo kiu generas ĉi tiujn anstataŭigajn listojn estas unu el la plej kompleksaj partoj. Ĝi enhavas plurajn paŝojn:

1. **Kolekti bazan vortaron** el la radika CSV-dosiero
2. **Generi vortvariojn** laŭ gramatikaj reguloj (verbaj finaĵoj, afiksoj, ktp.)
3. **Apliki personigajn regulojn** el la uzantaj JSON-dokumentoj
4. **Prioritatigi anstataŭigojn** laŭ longeco kaj specialaj reguloj
5. **Generi variojn de ĉiu anstataŭigo** por majusklaj, minusklaj kaj kapitaligitaj formoj
6. **Konstrui tri separatajn listojn** por malsamaj anstataŭigtipoj

Tiu ĉi proceso estas tre kompleksa kaj bone projekcigita por trakti la lingvistikaĵojn de Esperanto.

### 8.3 Diagramo de la Anstataŭiga Procezo

Por pli bone kompreni la tutan procezon, jen koncepta diagramo de la anstataŭiga fluo:

```
Eniga Teksto
   ↓
1. Spacoj kaj Esperanta Litera Normigo
   ↓
2. Identigo kaj Protektado de %...% Region
   ↓
3. Identigo kaj Aparta Procezado de @...@ Regionoj
   ↓
4. Tutglobala Anstataŭigado (replacements_final_list)
   ↓
5. Unua Dulitera Radika Anstataŭigado
   ↓
6. Dua Dulitera Radika Anstataŭigado
   ↓
7. Restaŭrigo de Lokalaj kaj Protektataj Regionoj
   ↓
8. Fina Formatado (HTML/krampa)
   ↓
Eliga Teksto
```

## 9. Esceptotraktado kaj Sekureco

La aplikaĵo inkluzivas plurajn mekanismojn por sekureco kaj esceptotraktado:

### 9.1 Sekuraj Konvertaĵoj

La `safe_replace` funkcio estas desegnita por eviti anstataŭigajn koliziaĵojn kiel jam diskutite. Tio estas esenca por la sekura teksta procezado.

### 9.2 Esceptotraktado ĉe Dosierŝargado

La aplikaĵo uzas teknikaĵojn kiel `try/except` por trakti erarojn kiam ŝargas JSON kaj aliajn dosierojn:

```python
try:
    with open(default_json_path, 'r', encoding="utf-8") as file:
        # ...
except FileNotFoundError:
    st.error("Ne eblas trovi la defaŭltan JSON-dosieron. Procezo haltigita.")
    st.stop()
```

Tio certigas ke la aplikaĵo ne kraŝas kiam okazas problemo kun laŭvice:
- Mankantaj dosieroj
- Malĝustformitaj JSON-dosieroj
- Nevalida UTF-8 kodigo

### 9.3 Manipulado de Grandaj Datumoj

La aplikaĵo manipulas grandajn datumojn per pluraj teĥnikoj:

1. **Kaŝmemorigo** (per `@st.cache_data`) por eviti ripetan ŝargadon de grandaj JSON-dosieroj
2. **Paralela procezado** por dividi la laboron inter kernojn
3. **Limigita antaŭrigardo** por grandaj eligaj tekstoj:

```python
MAX_PREVIEW_LINES = 250
lines = processed_text.splitlines()
if len(lines) > MAX_PREVIEW_LINES:
    first_part = lines[:247]
    last_part = lines[-3:]
    preview_text = "\n".join(first_part) + "\n...\n" + "\n".join(last_part)
```

## 10. Potencialaj Plibonigoj kaj Etendeblecoj

Kiel mez-nivela programisto, vi eble pripensos kiel plibonigi aŭ personaligi la aplikaĵon. Jen kelkaj ideoj:

### 10.1 Plibonigoj en la Algoritmo

1. **Optimumigo de la anstataŭiga algoritmo** per pli efikaj datumstrukturoj (ekz. Trie-arbo por la vortaro)
2. **Plibonigita gramatika analizo** kiu pli profunde analizas Esperantajn vortojn en ties gramatikajn partojn
3. **Plibonigita rubigrandeca algoritmo** kiu konsideras ĉinajn ideogramojn kaj Esperantajn vortojn malsame

### 10.2 Interfacaj Plibonigoj

1. **Realtempa antaŭrigardo** kiu montras anstataŭigojn dum la uzanto tajpas
2. **Pli integritan regulespriman interfacon** por personigi la anstataŭigajn regulojn
3. **Pli da formataj opcioj** por la eligoj (ekz. PDF, interacia JS)

### 10.3 Etendeblecoj

1. **API-interfaco** por integri la anstataŭigan funkciaron en aliajn aplikaĵojn
2. **Pliaj lingvoj** por rubi-anotacioj (nuntempe ĉefe angla kaj ĉina/japana)
3. **Inversaj konvertiĝoj** por traduki ideogramojn reen al Esperanto

## 11. Disvastigaj Konsideroj

Por kompreni la aplikaĵon komplete, konsideru:

### 11.1 Efikoj de Vortaj Elektoj

La originaj vortaj elektoj por la anstataŭigaj reguloj povas havi grandan efikon. Se 'lern' estas anstataŭigita per '学ぶ' (japana por "lerni"), ĉiuj vortoj devenantaj de "lern" estos anstataŭigitaj per tiu ideogramo, kio eble ne ĉiam estas dezirinda.

### 11.2 Teknikaj Defioj de Rubi en Retumiloj

Retumiloj malsame subtenas HTML `<ruby>` markojn, kaj la aplikaĵo enhavas specialajn CSS-stilojn por pliigi konvenan renderon:

```css
rt.M_M {
  --ruby-font-size: 0.5em;
  margin-top: -4.00em !important;
  transform: translateY(-0.0em) !important;
}
```

Tiuj stiloj estas esencaj por certigi ke la rubi-anotacioj vidiĝas bone en diversaj retumiloj.

## 12. Konkludo

La "Anstataŭigilo por Esperantaj tekstoj per ĉinaj ideogramoj kaj rubi-anotacia ilo" estas rimarkinde kompleksa aplikaĵo kiu kunplektas lingvistikajn kaj komputikajn principojn por krei unikan tekst-transforman ilon.

La plej impresaj aspektoj de la aplikaĵo inkluzivas:

1. **Sofistikitaj placoteniloj kaj anstataŭigaj algoritmoj** por precize trakti kompleksaĵojn de Esperanta vortfarado
2. **Inteligentaj rubi-anotacioj** kiuj adaptiĝas al la ekzakta teksta proporcio
3. **Paralela procezado** por efikeca rendimento eĉ kun grandaj tekstoj
4. **Fleksebla personigado** per JSON-konfigurado kaj CSV-dosieroj

Kiel mez-nivela programisto, vi nun havas profundan komprenon pri la ĉefaj teĥnikaj ideoj kaj procezoj en la aplikaĵo, ebligante al vi ĝin modifi, plibonigi aŭ inspiriĝi por viaj propraj projektoj.

La programista arto videbla en ĉi tiu apliaĵo demonstras kiel kompleksaj lingvistikaj transformadoj povas esti implementitaj per bone strukturitaj algoritmoj kaj datumstrukturoj.
