# Teknika Gvidlibro: Interna Funkciado de la Esperanto-Kanji Konvertilo

## Enhavtabelo

1. Arkitektura Superrigardo
2. Baza Fluo de Datumoj
3. Ĉefaj Moduloj kaj iliaj Funkcioj
4. Gravaj Algoritmoj
5. Datumstrukturoj
6. Paralela Prilaborado
7. Specialaj Teĥnikoj kaj Trukoj
8. Ebloj por Etendo kaj Personigo

## 1. Arkitektura Superrigardo

La Esperanto-Kanji Konvertilo estas konstruita kiel Streamlit-aplikaĵo konsistanta el kvar ĉefaj Python-dosieroj:

- **main.py**: La ĉefa programo, kiu difinas la uzantinterfacon kaj la ĉefan funkciadon
- **Paĝo por generi JSON-dosieron...**: Aparta Streamlit-paĝo por krei la anstataŭigajn JSON-dosierojn
- **esp_text_replacement_module.py**: Modulo kiu enhavas funkciojn por teksta anstataŭigo kaj prilaborado
- **esp_replacement_json_make_module.py**: Modulo kiu enhavas funkciojn por krei JSON-dosierojn por anstataŭigo

La aplikaĵo uzas plurajn teĥnikajn principojn:

- **Vica anstataŭigo kun placeholders**: Uzo de unikaj provizore rezervitaj tekstoĉenoj por eviti problemon de interfero dum anstataŭigaj procezoj
- **Paralela prilaborado**: Uzo de Python `multiprocessing` por rapide pritrakti grandajn kvantojn da teksto
- **Regulesprimoj**: Por analizi kaj pritrakti specifajn tekstopartojn
- **Dinamika HTML-generado**: Por krei rubiajn anotaciojn kaj ĝustigi ilian grandecon

## 2. Baza Fluo de Datumoj

La baza fluo de datumoj en la aplikaĵo estas:

1. **Eniga fazo**:
   - Uzanto enigas tekston aŭ alŝutas tekstan dosieron
   - Uzanto elektas JSON-dosieron kun anstataŭigaj reguloj aŭ uzas la defaŭltan
   - Uzanto elektas preferiton formaton kaj literformojn

2. **Prilabora fazo**:
   - La JSON-dosiero estas ŝargita kaj analizita en tri listojn:
     * `replacements_final_list` (por ĝenerala anstataŭigo)
     * `replacements_list_for_localized_string` (por loka anstataŭigo)
     * `replacements_list_for_2char` (por dvuliteraj radikoj)
   - Teĥnikaj placeholders estas importitaj por eviti interferon
   - La teksto estas prilaborita laŭ kvar paŝoj:
     * Partoj markitaj per `%...%` estas konservitaj intaktaj
     * Partoj markitaj per `@...@` estas anstataŭigitaj nur loke
     * La restanta teksto estas anstataŭigita per la ĝeneralaj reguloj
     * La anstataŭigitaj placeholders estas restarigitaj al iliaj finaj valoroj

3. **Eliga fazo**:
   - La rezulto estas montrata en la interfaco
   - HTML-kodoj kaj stiloj estas aplikitaj laŭ la elektita formato
   - La uzanto povas elŝuti la rezulton

## 3. Ĉefaj Moduloj kaj iliaj Funkcioj

### 3.1 main.py

La ĉefa programo havas plurajn gravajn partojn:

- **Inicialigo kaj paĝaranĝo**:
  ```python
  st.set_page_config(
      page_title="Ilo por Anstataŭigi (Kanji) Karakterojn en Esperanto-teksto",
      layout="wide"
  )
  ```

- **JSON-ŝargado kun memorkaŝado**:
  ```python
  @st.cache_data  # Memorkaŝado por rapida ripetata ŝargado
  def load_replacements_lists(json_path: str) -> Tuple[List, List, List]:
      # Ŝargas la JSON kaj disigas ĝin en tri listojn...
  ```

- **Kontrolado de enigo kaj formato**:
  ```python
  # Pluraj elektopcionoj por eniga teksto kaj eligo-formatoj
  ```

- **Ĉefa anstataŭiga funkcio**:
  ```python
  # Uzante aŭ paralel_process aŭ orchestrate_comprehensive_esperanto_text_replacement
  processed_text = orchestrate_comprehensive_esperanto_text_replacement(
      text=text0,
      placeholders_for_skipping_replacements=placeholders_for_skipping_replacements,
      # ... aliaj argumentoj ...
  )
  ```

### 3.2 esp_text_replacement_module.py

Ĉi tiu modulo enhavas la kernajn tekst-anstataŭigajn funkciojn:

- **Ĉefaj supersignaj transformoj**:
  ```python
  # Konvertaj vortaroj por diversaj literformoj (ĉ, cx, c^)
  x_to_circumflex = {'cx': 'ĉ', 'gx': 'ĝ', ...}
  # Funkcioj por konverti inter formatoj
  def convert_to_circumflex(text: str) -> str: ...
  ```

- **La bazo de ĉiu anstataŭigo: safe_replace**:
  ```python
  def safe_replace(text: str, replacements: List[Tuple[str, str, str]]) -> str:
      """
      Tiu ĉi funkcio estas la kerno de la anstataŭiga algoritmo.
      Ĝi transformas 'old' al 'placeholder', poste al 'new' por eviti
      interferon inter anstataŭigoj.
      """
  ```

- **Orkestra funkcio por kompleta prilaborado**:
  ```python
  def orchestrate_comprehensive_esperanto_text_replacement(...):
      """
      Ĉi tie okazas la kompleta anstataŭiga procezo kun ĉiuj paŝoj:
      1) Normaligo de spacoj kaj Esperantaj literoj
      2) Konservado de tekstoj markitaj per %...%
      3) Loka anstataŭigo de tekstoj markitaj per @...@
      4) Ĝenerala anstataŭigo
      5) Speciala prilaborado de dvuliteraj radikoj
      6) Restarigo de placeholders al finaj valoroj
      """
  ```

- **Paralelaj prilaboradaj funkcioj**:
  ```python
  def parallel_process(...):
      """
      Dividas la tekston en partojn kaj prilaboras ilin paralele
      per multiprocessing por plirapidigi la procezon.
      """
  ```

### 3.3 esp_replacement_json_make_module.py

Ĉi tiu modulo specialiĝas pri la kreado de JSON-dosieroj por anstataŭigo:

- **Rubia formato kaj grandeco**:
  ```python
  def output_format(main_text, ruby_content, format_type, char_widths_dict):
      """
      Generas la finan formaton (HTML rubia, krampoj, ktp) laŭ 
      elektita formato kaj adaptas la grandecon por rubio.
      """
  ```

- **Paralelaj funkcioj por krei anstataŭigajn vortarojn**:
  ```python
  def parallel_build_pre_replacements_dict(...):
      """
      Uzas plurajn procezilajn kernojn por rapide krei grandajn
      anstataŭigajn vortarojn.
      """
  ```

- **Specialaj prilaboradoj de rubio**:
  ```python
  def capitalize_ruby_and_rt(text: str) -> str:
      """
      Majuskligas nur la unuan literon de rubiaj tekstoj, 
      konservante HTML-strukturon.
      """
  ```

### 3.4 Paĝo por generi JSON-dosieron...

Ĉi tiu dosiero kreas apartan Streamlit-paĝon por generi JSON-dosierojn:

- **Filtrado kaj prilaborado de verbaj sufiksoj kaj specialaj radikoj**:
  ```python
  # Gravaj listoj por specialaj vortoj kaj sufiksoj
  verb_suffix_2l = {'as':'as', 'is':'is', 'os':'os', ...}
  suffix_2char_roots=['ad', 'ag', 'am', 'ar', ...]
  prefix_2char_roots=['al', 'am', 'av', 'bo', ...]
  ```

- **Konstruado de anstataŭigaj listoj en ĝusta ordo**:
  ```python
  # Kreado de la finaj anstataŭigaj listoj kun prioritato 
  # laŭ longeco kaj gramatika funkcio
  ```

- **Kunigo de la tri tipoj de anstataŭigaj listoj**:
  ```python
  combined_data = {}
  combined_data["全域替换用のリスト(列表)型配列(replacements_final_list)"] = replacements_final_list
  combined_data["二文字词根替换用のリスト(列表)型配列(replacements_list_for_2char)"] = replacements_list_for_2char
  combined_data["局部文字替换用のリスト(列表)型配列(replacements_list_for_localized_string)"] = replacements_list_for_localized_string
  ```

## 4. Gravaj Algoritmoj

### 4.1 safe_replace: La Kerno de Anstataŭigo

La funkcio `safe_replace` estas la plej grava algoritmo en la aplikaĵo, ĝi solvas la problemon de interfero inter anstataŭigoj:

```python
def safe_replace(text: str, replacements: List[Tuple[str, str, str]]) -> str:
    """
    (old, new, placeholder) の List を受け取り
    text中の old → placeholder → new の段階置換を行う
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

Ĉi tiu funkcio akceptas liston de (old, new, placeholder) kaj faras du-fazan anstataŭigon:
1. Unue, ĝi anstataŭigas ĉiun 'old' per ĝia 'placeholder' 
2. Due, ĝi anstataŭigas ĉiun 'placeholder' per ĝia 'new'

La avantaĝo de ĉi tiu metodo estas, ke ĝi evitas interferon inter anstataŭigoj. Ekzemple, se vi anstataŭigus "am" per "愛" kaj poste "amas" per "愛as", la unua anstataŭigo povus ŝanĝi parton de la dua vorto, rezultante en "愛as" anstataŭ "愛as".

### 4.2 Orkestrado de Kompleksa Anstataŭigo

La kompleksa anstataŭiga procezo estas orkestrita de la funkcio `orchestrate_comprehensive_esperanto_text_replacement`:

```python
def orchestrate_comprehensive_esperanto_text_replacement(
    text, 
    placeholders_for_skipping_replacements,
    replacements_list_for_localized_string,
    placeholders_for_localized_replacement,
    replacements_final_list,
    replacements_list_for_2char,
    format_type
) -> str:
    # 1. Unuecigi spacetojn kaj konverti al supersigna formato
    # 2. Konservi %...% fragmentojn
    # 3. Loke anstataŭigi @...@ fragmentojn
    # 4. Fari ĝeneralan anstataŭigon
    # 5. Fari dvuliternajn anstataŭigojn (dufoje)
    # 6. Restarigi placeholders
    # 7. Apliki HTML-formatadon se necesas
```

Ĉi tiu funkcio enhavas la kompletan proceson de teksta prilaborado kaj sekvas precize difinitan ordon de operacioj por certigi la ĝustan rezulton.

### 4.3 Paralela Prilaborado

Por grandaj tekstoj, la aplikaĵo uzas paralelan prilaboradon:

```python
def parallel_process(
    text: str,
    num_processes: int,
    # aliaj argumentoj...
) -> str:
    # Dividi la tekston en liniojn
    lines = re.findall(r'.*?\n|.+$', text)
    # Dividi la liniojn inter procezoj
    # Starigi multiprocessing.Pool
    # Kombini la rezultojn
```

Ĉi tiu metodo konsiderinde plirapigas la prilaboradon de grandaj tekstoj dividante la laboron inter pluraj procezilaj kernoj.

## 5. Datumstrukturoj

### 5.1 La Tri Anstataŭigaj Listoj

La aplikaĵo uzas tri ĉefajn anstataŭigajn listojn:

1. **replacements_final_list**: Por ĝenerala anstataŭigo de vortoj kaj frazoj
   ```python
   [(old1, new1, placeholder1), (old2, new2, placeholder2), ...]
   ```

2. **replacements_list_for_localized_string**: Por loka anstataŭigo (@...@)
   ```python
   [(old1, new1, placeholder1), (old2, new2, placeholder2), ...]
   ```

3. **replacements_list_for_2char**: Por anstataŭigo de dvuliteraj radikoj
   ```python
   [(old1, new1, placeholder1), (old2, new2, placeholder2), ...]
   ```

### 5.2 Placeholders

La aplikaĵo uzas tri tipojn de placeholders, kiuj estas importitaj el apartaj tekstaj dosieroj:

1. **placeholders_for_skipping_replacements**: Por konservi %...% tekstojn
2. **placeholders_for_localized_replacement**: Por lokaj @...@ anstataŭigoj
3. **imported_placeholders_for_global_replacement**: Por ĝeneralaj anstataŭigoj

Ĉi tiuj placeholders estas unikaj tekstoĉenoj (ekz. "%1854%", "@5134@", "$20987$") kiuj estas uzataj kiel provizora deponejo dum la anstataŭiga procezo.

### 5.3 La Finaj JSON-Strukturoj

La fina JSON-dosiero havas la sekvan strukturon:

```json
{
  "全域替换用のリスト(列表)型配列(replacements_final_list)": [
    ["old1", "new1", "placeholder1"],
    ["old2", "new2", "placeholder2"],
    ...
  ],
  "二文字词根替换用のリスト(列表)型配列(replacements_list_for_2char)": [
    ["old1", "new1", "placeholder1"],
    ...
  ],
  "局部文字替换用のリスト(列表)型配列(replacements_list_for_localized_string)": [
    ["old1", "new1", "placeholder1"],
    ...
  ]
}
```

## 6. Paralela Prilaborado

La aplikaĵo uzas la Python `multiprocessing` modulon por paralela prilaborado en du ĉefaj lokoj:

### 6.1 En la Ĉefa Anstataŭiga Procezo

```python
def parallel_process(text, num_processes, ...):
    # Dividi la tekston en partojn
    lines = re.findall(r'.*?\n|.+$', text)
    # Krei taskojn por ĉiu procezo
    with multiprocessing.Pool(processes=num_processes) as pool:
        results = pool.starmap(process_segment, [...])
    # Kunigi la rezultojn
    return ''.join(results)
```

### 6.2 Dum Kreado de JSON-Dosieroj

```python
def parallel_build_pre_replacements_dict(E_stem_with_Part_Of_Speech_list, replacements, num_processes=4):
    # Dividi la prilaborendan liston en partojn
    # Starigi paralelajn procezojn
    with multiprocessing.Pool(num_processes) as pool:
        partial_dicts = pool.starmap(process_chunk_for_pre_replacements, [...])
    # Kunigi la rezultojn
    merged_dict = {}
    for partial_d in partial_dicts:
        # Kunfandi la partajn vortarojn...
```

Notu la gravecon de la `multiprocessing.set_start_method("spawn")` linio en la ĉefa kodo, kiu estas necesa por eviti problemojn kun Streamlit.

## 7. Specialaj Teĥnikoj kaj Trukoj

### 7.1 Adaptado de Rubia Grandeco

Por HTML-rubiaj formatoj, la aplikaĵo ĝustigas la grandecon de la rubiaj anotacioj laŭ la proporcio inter la originala kaj la anstataŭiga tekstoj:

```python
def output_format(main_text, ruby_content, format_type, char_widths_dict):
    if format_type == 'HTML格式_Ruby文字_大小调整':
        width_ruby = measure_text_width_Arial16(ruby_content, char_widths_dict)
        width_main = measure_text_width_Arial16(main_text, char_widths_dict)
        ratio_1 = width_ruby / width_main
        if ratio_1 > 6:
            return f'<ruby>{main_text}<rt class="XXXS_S">{insert_br_at_third_width(ruby_content, char_widths_dict)}</rt></ruby>'
        elif ratio_1 > (9/3):
            return f'<ruby>{main_text}<rt class="XXS_S">{insert_br_at_half_width(ruby_content, char_widths_dict)}</rt></ruby>'
        # ... pli da kondiĉoj ...
```

Ĉi tiu kodo kalkulas la proporcion inter la larĝecoj de la originala kaj anstataŭiga tekstoj, kaj elektas taŭgan grandecon por la rubia anotacio. Por tre longaj anotacioj, ĝi eĉ enmetigas aŭtomatajn lini-rompojn.

### 7.2 Majuskligado de Rubiaj Tekstoj

La aplikaĵo havas specialan funkcion por majuskligi nur la unuan literon de rubiaj tekstoj, konservante la HTML-strukturon:

```python
def capitalize_ruby_and_rt(text: str) -> str:
    def replacer(match):
        # Eltrovi grupojn el la regulesprima kongruo
        # Majuskligi nur la unuan literon de la ĉefa teksto kaj rubio
    replaced_text = RUBY_PATTERN.sub(replacer, text)
    return replaced_text
```

### 7.3 Forigado de Redundantaj Rubiaĵoj

La aplikaĵo havas funkcion por forigi redundantajn rubiajn anotaciojn, kie la originala kaj anstataŭiga tekstoj estas identaj:

```python
def remove_redundant_ruby_if_identical(text: str) -> str:
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

## 8. Ebloj por Etendo kaj Personigo

### 8.1 Aldoni Novajn Anstataŭigojn

Por aldoni novajn anstataŭigajn regulojn, vi povas:

1. Modifi la CSV-dosieron kun Esperantaj radikoj kaj respondaj tradukoj
2. Agordi specialajn radikajn segmentadojn en la JSON-dosiero por radika segmentado
3. Difini specialajn substituajn tekstojn en la alia JSON-dosiero

### 8.2 Modifi la Eligan Formaton

La eligaj formatoj estas difinitaj en la funkcio `output_format` en `esp_replacement_json_make_module.py`. Vi povas modifi ĝin por krei novajn formatojn aŭ modifi ekzistantajn.

### 8.3 Etendi la Algoritmon

La ĉefa anstataŭiga algoritmo estas en `orchestrate_comprehensive_esperanto_text_replacement`. Vi povas modifi ĉi tiun funkcion por aldoni novajn paŝojn aŭ modifi la ekzistantajn paŝojn de la anstataŭiga procezo.

Vi ankaŭ povas aldoni novajn funkciaĵojn al la Streamlit-interfaco por doni al uzantoj pli da kontrolo super la anstataŭiga procezo.

---

Ĉi tiu gvido provizas profundan komprenigon pri la interna funkciado de la Esperanto-Kanji Konvertilo kaj Rubia Anotacia Ilo. Kiel meza-nivela programisto, vi nun havas la necesan scion por kompreni, modifi kaj etendi la aplikaĵon laŭ viaj bezonoj. La kombinado de inteligenta teksta prilaborado, paralela optimumigo kaj adaptema HTML-generado faras ĉi tiun aplikaĵon potenca ilo por esplori la rilatojn inter Esperanto kaj ideogramoj.