# Teknika Dokumentado: Interna Funkciado de la Esperanto-Kanji Konvertilo

## Enhavo
1. Arkitektura Superrigardo
2. Dosiera Strukturo kaj Modula Organizo
3. Ĉefaj Datumstrukturoj
4. Detala Fluo de Teksta Traktado
5. Anstataŭigaj Mekanismoj
6. Paralela Prilaborado
7. JSON-Generado
8. Rubia Anotaciado kaj HTML-Generado
9. Plibonigaj Ebloj kaj Teknikaj Konsideroj

## 1. Arkitektura Superrigardo

Tiu ĉi aplikaĵo estas konstruita uzante **Streamlit** kadron kaj konsistas el du ĉefaj paĝoj:

1. **Ĉefa paĝo** (`main.py`) - Pritraktas Esperantan tekston, anstataŭigas vortojn per ĉinaj ideogramoj (kanji) kaj aldonas rubiajn anotaciojn
2. **JSON-generada paĝo** (`Paĝo por generi...`) - Kreas personigitajn JSON-dosierojn por la anstataŭigaj reguloj

La fluo de la kodo sekvas la jenan modelon:

```
[Enigo] → [Ŝargado de reguloj] → [Teksta transformado] → [Eligo]
```

La aplikaĵo uzas du helpajn modulojn:
- `esp_text_replacement_module.py` - Enhavas la funkciarojn por teksta transformado
- `esp_replacement_json_make_module.py` - Enhavas funkciarojn por krei JSON-anstataŭigajn regulojn

## 2. Dosiera Strukturo kaj Modula Organizo

### `main.py` (Ĉefa aplikaĵo)

La ĉefa aplikaĵo estas strukturita jene:

```python
# Importoj de bibliotekoj kaj moduloj
# Difino de kaŝmemorigaj funkcioj (@st.cache_data)
# Agordo de la paĝo
# Uzanta interfaco por elekti JSON-dosieron
# Uzanta interfaco por altnivelaj agordoj
# Uzanta interfaco por elekti eligan formaton
# Uzanta interfaco por eniga teksto
# Formularo por teksta prilaborado
# Montrilo de rezulto kaj elŝuta butono
```

### `Paĝo por generi JSON-dosieron...` (JSON generado)

La JSON-generada paĝo sekvas similan strukturon:

```python
# Importoj de bibliotekoj kaj moduloj
# Difino de konstantoj kaj datumoj (sufiksoj, prefiksoj, ktp)
# Ŝargado de placeholders (indiklokoj)
# Paĝaj agordoj
# Uzanta interfaco por CSV-elektado
# Uzanta interfaco por JSON-elektado
# Uzanta interfaco por paralela prilaborado
# JSON-generada proceso
# Elŝuta butono por la finita JSON
```

### Helpaj Moduloj

#### `esp_text_replacement_module.py`

Enhavas ĉefe:
- Vortarojn por Esperantaj literoj (ĉ → cx, ktp)
- Funkciojn por konverti inter diversaj Esperantaj literformoj
- Funkciojn por sekura teksta anstataŭigo
- Funkciojn por pritrakti specialajn markojn (%...%, @...@)
- Ĉefan anstataŭigan funkcion `orchestrate_comprehensive_esperanto_text_replacement`
- Funkciojn por paralela prilaborado

#### `esp_replacement_json_make_module.py`

Enhavas:
- Vortarojn por Esperantaj literoj (similar al la supre)
- Funkciojn por mezuri tekstan larĝon
- Funkciojn por krei eligajn formatojn (HTML, krampoj, ktp)
- Utilajn funkciojn por placeholders (indiklokoj)
- Funkciojn por paralela prilaborado dum JSON-kreado

## 3. Ĉefaj Datumstrukturoj

La aplikaĵo uzas tri ĉefajn listojn por anstataŭigo:

### 1. `replacements_final_list`

La ĉefa anstataŭiga listo, kiu estas uzata por ĝenerala teksta anstataŭigo. Ĉiu ero en la listo estas tuplo kun tri elementoj:

```python
(originala_vorto, anstataŭigita_vorto, placeholder)
```

Ekzemplo:
```
("amiko", "<ruby>amiko<rt class=\"M_M\">友</rt></ruby>", "$12345$")
```

### 2. `replacements_list_for_localized_string`

Uzata por lokaj anstataŭigoj (ĉirkaŭitaj per @...@). Same strukturita:

```python
(originala_vorto, anstataŭigita_vorto, placeholder)
```

### 3. `replacements_list_for_2char`

Speciala listo por pritrakti du-literajn vorterojn (prefiksojn, sufiksojn, ktp):

```python
(originala_vortero, anstataŭigita_vortero, placeholder)
```

Ekzemploj: "al$", "$as", "re$"

## 4. Detala Fluo de Teksta Traktado

En `main.py`, la prilaborada fluo okazas jene:

1. **Ŝargado de anstataŭigaj listoj**:
   ```python
   replacements_final_list, replacements_list_for_localized_string, replacements_list_for_2char = load_replacements_lists(json_path)
   ```

2. **Ŝargado de indiklokaj tekstoj**:
   ```python
   placeholders_for_skipping_replacements = import_placeholders('...')
   placeholders_for_localized_replacement = import_placeholders('...')
   ```

3. **Teksta prilaborado** (depende de la elekto de paralela prilaborado):
   ```python
   if use_parallel:
       processed_text = parallel_process(...)
   else:
       processed_text = orchestrate_comprehensive_esperanto_text_replacement(...)
   ```

4. **Apliko de literformo**:
   ```python
   if letter_type == '上付き文字':  # Supersigna
       processed_text = replace_esperanto_chars(processed_text, x_to_circumflex)
       processed_text = replace_esperanto_chars(processed_text, hat_to_circumflex)
   # ... (aliaj literformoj) ...
   ```

5. **Apliko de HTML-kapo kaj -piedo**:
   ```python
   processed_text = apply_ruby_html_header_and_footer(processed_text, format_type)
   ```

## 5. Anstataŭigaj Mekanismoj

La kerno de la aplikaĵo estas la `safe_replace` funkcio, kiu uzas tri-paŝan procezon por eviti nedeziratajn anstataŭigojn:

```python
def safe_replace(text: str, replacements: List[Tuple[str, str, str]]) -> str:
    valid_replacements = {}
    # 1-a paŝo: Anstataŭigu originalan tekston per placeholders
    for old, new, placeholder in replacements:
        if old in text:
            text = text.replace(old, placeholder)
            valid_replacements[placeholder] = new
    # 2-a paŝo: Anstataŭigu placeholders per la finaj tekstoj
    for placeholder, new in valid_replacements.items():
        text = text.replace(placeholder, new)
    return text
```

La anstataŭiga proceso estas organizita en pli kompleksa fluo en la ĉefa funkcio `orchestrate_comprehensive_esperanto_text_replacement`:

1. **Unuecigo kaj konvertado**:
   - Unuecigi spacetojn
   - Konverti al supersigna formo (ĉ, ĝ, ktp.)

2. **Pritraktado de specialaj markoj**:
   - Trovi tekstojn ĉirkaŭitajn per `%...%` kaj konservi ilin por eviti anstataŭigon
   - Trovi tekstojn ĉirkaŭitajn per `@...@` kaj apliki lokajn anstataŭigojn

3. **Ĉefaj anstataŭigoj**:
   - Apliki ĝeneralajn anstataŭigojn el `replacements_final_list`
   - Apliki du-literajn anstataŭigojn el `replacements_list_for_2char` (dufoje)

4. **Restaŭrado kaj fina formatado**:
   - Remeti indiklokojn per finaj tekstoj
   - Restaŭri lokajn kaj saltitajn partojn
   - Alĝustigi HTML-formatadon se necese

## 6. Paralela Prilaborado

Por grandaj tekstoj, la aplikaĵo povas uzi paralelan prilaboradon per la `multiprocessing` biblioteko:

```python
def parallel_process(text, num_processes, ...):
    # Dividi la tekston en liniojn
    lines = re.findall(r'.*?\n|.+$', text)

    # Kalkuli kiom da linioj iros al ĉiu procezo
    lines_per_process = max(num_lines // num_processes, 1)

    # Krei limojn por ĉiu procezo
    ranges = [(i * lines_per_process, (i + 1) * lines_per_process) for i in range(num_processes)]

    # Lanĉi paralelajn procezojn
    with multiprocessing.Pool(processes=num_processes) as pool:
        results = pool.starmap(process_segment, [...])

    # Kunigi la rezultojn
    return ''.join(results)
```

La `process_segment` funkcio prilaboras apartan parton de la teksto, kaj la rezultoj estas kunigitaj poste.

## 7. JSON-Generado

La dua paĝo de la aplikaĵo (`Paĝo por generi...`) estas speciale dezajnita por krei personigitajn JSON-anstataŭigajn dosierojn.

La ĉefa proceso estas:

1. **Ŝargi radikajn datumojn** el CSV-dosiero kaj aliaj fontoj
2. **Krei vortaron** kun Esperantaj radikoj kaj iliaj tradukoj/kanji
3. **Prioritatigi anstataŭigojn** surbaze de vortlongeco (pli longaj vortoj havas pli altan prioritaton)
4. **Apliki modifojn** por verbaj sufiksoj, prefiksoj, kaj aliaj specialaj kazoj
5. **Krei tutajn listojn** por globala anstataŭigo, loka anstataŭigo, kaj du-litera anstataŭigo
6. **Kombini ĉion** en unu granda JSON-dosiero

La procedo enhavas multajn detalajn paŝojn por trakti la ĝustan prioritaton kaj eviti nedeziratajn anstataŭigojn.

## 8. Rubia Anotaciado kaj HTML-Generado

La Rubia anotaciado estas realigita per HTML-strukturo kun `<ruby>` kaj `<rt>` etikedoj:

```html
<ruby>amiko<rt class="M_M">友</rt></ruby>
```

La apliko havas plurajn eligajn formatojn:

- **HTML kun rubio kaj grandeca ĝustigo**: Uzas CSS-klasojn por alĝustigi la grandecon de rubiaj anotacioj
- **HTML kun rubio kaj ĉina anstataŭigo**: Inversigas la rolojn de Esperantaj vortoj kaj ĉinaj ideogramoj
- **HTML simpla**: Baza HTML-strukturo sen pliaj ĝustigoj
- **Krampoj**: Simpla teksta formato kun krampoj
- **Simpla anstataŭigo**: Nur la anstataŭigitaj ideogramoj

La plej kompleksa formato uzas CSS-klasojn por alĝustigi la grandecon de rubiaj anotacioj laŭ la proporcio inter la longo de la Esperanta vorto kaj la ĉina ideogramo:

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
        # ... (aliaj kazoj) ...
```

## 9. Plibonigaj Ebloj kaj Teknikaj Konsideroj

### Efika Prilaborado de Grandaj Tekstoj

La aplikaĵo jam havas paralelan prilaboradon, sed por vere grandaj tekstoj, vi eble volos:

- Plifortigi la kaŝmemorigon (`@st.cache_data`)
- Dividi la enigan tekston en eĉ pli malgrandajn pecojn
- Konsideri asinkonan prilaboradon (eble per `asyncio`)

### Personigitaj Anstataŭigoj

Se vi volas aldoni pliajn personigitajn anstataŭigojn:

1. Kreu vian propran CSV-dosieron kun Esperanta radiko kaj ideogramo/traduko
2. Uzu la JSON-generadan paĝon por krei novan JSON-dosieron
3. Alŝutu vian novan JSON-dosieron en la ĉefa paĝo

### Modifado de Eligaj Stiloj

La CSS-stiloj por la rubiaj anotacioj estas difinitaj en la `apply_ruby_html_header_and_footer` funkcio. Se vi volas modifi la stilon:

```python
def apply_ruby_html_header_and_footer(processed_text: str, format_type: str) -> str:
    if format_type in ('HTML格式_Ruby文字_大小调整','HTML格式_Ruby文字_大小调整_汉字替换'):
        ruby_style_head="""<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8">
    <style>
    /* Modifi la CSS-stilojn ĉi tie */
    </style>
  </head>
  <body>
  <p class="text-M_M">
"""
```

### Lerni-Referencoj

La kodo enhavas multajn bonajn ekzemplojn de:

- Teksta prilaborado en Python
- Funkcia programado kaj kompozicio
- Paralela prilaborado kun `multiprocessing`
- Uzo de Streamlit por konstrui interagajn TTT-aplikaĵojn
- Regula esprimiĝo por kompleksaj tekstvizitoj

## Konklude

Ĉi tiu detala teknika dokumentado celas helpi meznivelaj Esperanto-programistojn kompreni la internajn mekanismojn de la Esperanto-Kanji konvertila aplikaĵo. La kodo estas bone strukturita kaj dokumentita, kio faciligas ĝian komprenadon kaj, se necese, etendadon aŭ personigadon.

La aplikaĵo demonstras multajn bonajn programadpraktikojn kaj ofertas bonan ekzemplon de kiel efike prilabori tekstojn por lingvaj transformoj. La uzado de placeholders (indiklokoj) por eviti nedeziratajn anstataŭigojn estas aparte eleganta solvo al la problemo de tekstaj anstataŭigoj.
