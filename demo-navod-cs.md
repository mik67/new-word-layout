# Návod k demům: New Word Toolkit in Wave 2

Postup kliknutí pro všechna tři dema. Akce jsou popsané česky. **Tučně** jsou přesné anglické názvy stránek, akcí, polí a tlačítek. Ty se do vyhledávání (Alt+Q) zadávají tak, jak jsou napsané.

> **Původ názvů:** názvy v BC jsou převzaté ze zdrojových kódů Base Application 29.0.54011 (z kontejneru bc29). Dokumentace Microsoftu se místy liší, např. píše „Set Approved“, ale v systému je **Set as Approved**. Platí to, co je tady.
> Názvy tlačítek v doplňku pro Word jsou z Microsoft Learn. Jediná neověřená věc je tlačítko HideIf podle booleanu (viz demo 2).

---

## 0. Příprava (ráno ve středu, ~15 min)

1. **Docker:**
   ```powershell
   docker stop bcserver
   docker start bc29
   ```
   Počkej, až půjde otevřít `http://bc29/BC/`, a přihlas se svým uživatelem kontejneru.
2. **Funkce zapnutá:** Alt+Q › **Feature Management**. Vyhledej `Document` a zkontroluj, že **Document Report Experience** má **Enabled for** = **All Users**. V kontejneru je už zapnutá, jde jen o kontrolu.
   - Bez ní nejsou na stránce **Report Layouts** vidět sloupec **Subtype**, FactBox **Details** ani skupina **Composite layout**.
3. **Čistý start:**
   - Alt+Q › **Manage themes and header-footer layouts**: v seznamu nesmí být nic s **Publisher** = uživatelský díl, tj. nahraný z minulé zkoušky. Pokud tam něco je, označ to a dej **Delete**.
   - Alt+Q › **Report defaults for theme and header-footer**: seznam musí být prázdný. Pokud není, smaž řádky.
   - Alt+Q › **Company Information** › FastTab **Reporting**: pole **Default Theme** a **Default Header/Footer** musí být prázdná.
4. **Doklady pro demo 2.** Alt+Q › **Sales Quotes**. Připrav si dvě nabídky:
   - **A, „s příplatky“:** řádek se slevou (sloupec **Line Discount %**, např. 10) a vyplněné pole **Your Reference**. Ideálně i zákazník s vyplněným **Currency Code** a celková částka nad 10 000.
   - **B, „čistá“:** bez slevy, v GBP, malá částka. Stačí CRONUS nabídka **1001**.
   - Poznamenej si čísla obou nabídek.
5. **Word:** otevři Word, **Home** › **Add-ins**, vyhledej **Dynamics 365 Business Central Word add-in** a přihlas se v něm. Na pásu karet musí být karta **Business Central**.
6. **Okna připravená předem:**
   - Prohlížeč s BC.
   - Word.
   - VS Code se složkou `demo\WordToolkitDemo`.
   - Průzkumník se složkami `demo\parts-for-upload` a `demo\output`.

---

## Demo 1: Konzultanti, branding bez vývojáře (≈ 4 min)

**Pointa:** report 50601 má jen tělo bez loga a barev. Admin nahraje téma a záhlaví/zápatí, nastaví je jako globální výchozí a report je rázem v brandingu firmy.

### 1.1 Ukaž výchozí stav: šedivý report
1. Alt+Q › **Customer Overview (Knowledge Share Demo)**, tj. report 50601.
2. Na request page nech filtry prázdné (nebo zadej pár zákazníků), pak **Preview** (nebo **Send to…** › PDF).
3. Řekni: *„Tohle je jen body. Žádné logo, žádné firemní barvy, obyčejné šedé písmo.“*

### 1.2 Nahraj téma a záhlaví/zápatí
1. Alt+Q › **Manage themes and header-footer layouts**.
2. Akce **New theme**. Otevře se dialog **New Theme or Header/Footer**:
   - **Name**: `TT Theme`
   - **Description**: `Tres Tria fonts and colours`
   - **OK**, pak vyber soubor `demo\parts-for-upload\TTTheme.dotx`.
3. Akce **New header/footer**, stejný dialog:
   - **Name**: `TT Header Footer`
   - **Description**: `Logo, address, VAT, bank, page numbers`
   - Přepínač **Create a Blank Layout** nech **vypnutý**, protože nahráváš hotový soubor.
   - **OK**, pak vyber `demo\parts-for-upload\TTHeaderFooter.docx`.
4. Ukaž sloupce **Type** (Theme / HeaderFooter) a **Status** = **Draft**.
   Řekni: *„Nový díl začíná jako Draft, nejde přiřadit, dokud ho neschválím.“*
5. Označ oba nové řádky › skupina **Part Status** › **Set as Approved**.
6. Volitelně ukaž další akce na stránce: **Export**, **Show info**, **Edit description**, **Replace**, **Delete**.
   - Díly z extension (**Publisher** = název appky) jdou jen exportovat, nejdou upravit ani nahradit.

### 1.3 Nastav globální výchozí
1. Alt+Q › **Report defaults for theme and header-footer**.
2. Akce **Add global default**. Vznikne řádek, kde **Applies to** = **All reports**.
3. Pole **Header/Footer Part**: tři tečky (assist-edit) › vyber `TT Header Footer`.
4. Pole **Theme Part**: tři tečky › vyber `TT Theme`.
5. Ukaž další akce, ale nespouštěj je:
   - **Set for one report** (výchozí pro jeden report),
   - **Set for one layout** (jeden layout),
   - **Apply to all layouts** (rozšíří řádek z jednoho layoutu na celý report).
   - Řekni: *„Pořadí platnosti: layout v této firmě → layout ve všech firmách → report → firma → globální.“*

### 1.4 Spusť znovu: report je v brandingu
1. Znovu spusť **Customer Overview (Knowledge Share Demo)** › **Preview**.
2. Výsledek: logo, adresa, DIČ/VAT, banka a čísla stránek v záhlaví/zápatí, písmo Segoe UI, modré tabulky.
3. Řekni: *„Body se nezměnilo ani o bajt. Stačí jeden theme a jeden header/footer pro všechny reporty zákazníka.“*

### 1.5 Ukaž, kde se to dá ověřit a přebít
1. Alt+Q › **Report Layouts**. Najdi report **50601**, layout **Customer overview - body (composite)**.
   - Sloupec **Subtype** = **Body**.
2. Vpravo FactBox **Details** › sekce **Theme and Header/Footer**. Pole **Theme**, **Theme source**, **Header/Footer** a **Header/Footer source** ukazují **Global default**.
3. Volitelně akce **Composite layout** › **Set all report theme and header-footer**. Otevře stránku **Theme and header-footer per layout** se stejnými sloupci pro všechny layouty reportu.
4. Alt+Q › **Company Information** › FastTab **Reporting**: pole **Default Theme** a **Default Header/Footer**.
   - Řekni: *„Výchozí díly pro konkrétní firmu, u zákazníka s více firmami má každá svoje logo a banku.“*
   - Nic neměň.

> **Záloha, kdyby záhlaví v 50601 vyšlo prázdné.** Pole adresy/VAT/banky se neplní, protože díl byl generovaný pro report 50600. Pak:
> - ukaž, že logo a téma fungují (logo je obrázek přímo v souboru, nečte se z dat),
> - nebo přepni na report **Sales Quote (Knowledge Share Demo)**,
> - nebo pusť PDF `demo\output\customers-composite.pdf` (vyrenderované s díly z AL).
>
> **Na co upozornit pro support.** U reportu 50600 FactBox **Details** ukazuje **None**, i když report branding má. Díly deklarované v extension (AL fallback) FactBox nezobrazuje, to je zdokumentované chování.

---

## Demo 2: Word add-in, HideIf a Hide if empty (≈ 5 min)

**Pointa:** konzultant upraví layout ve Wordu bez XML. Poznámky se zobrazí jen tehdy, když dávají smysl, prázdný sloupec slevy zmizí, layout má interní komentář a projde schvalovacím stavem.

### 2.1 Vytvoř uživatelskou kopii layoutu z extension
1. Alt+Q › **Report Layouts**. Najdi report **50600**, layout **Sales Quote - body (composite)**.
   - Sloupce **Subtype** = **Body**, **Extension** = název demo appky.
2. Akce **Edit info**. Otevře se dialog **Edit Report Layout**:
   - Zapni **Save Changes to a Copy**.
   - **Layout Name**: `Sales Quote - body (KS demo)`.
   - **OK**.
3. Řekni: *„Layout z extension nejde nahradit, vždy se upravuje kopie.“*
   - Kopie má **Extension** prázdné, tj. je user-defined (pohled **User-Defined** nahoře).

### 2.2 Exportuj layout pro Word
1. Na kopii: skupina **Layout** › **Update and export layout**.
   - Důležité: ne **Export layout**. Jen „Update and…“ vloží aktuální dataset a metadata, která add-in potřebuje.
2. Otevři stažený `.docx` ve Wordu › karta **Business Central**.

### 2.3 Ukaž Add Data (rychle)
1. **Business Central** › **Add Data**. Panel má skupiny **Data**, **Labels** a **Report Information**.
2. Rozbal **Report Information** › **CompanyMetadata**. Řekni: *„Údaje o firmě jsou v datasetu automaticky, vývojář je nemusí přidávat.“*
3. Volitelně ukaž postup na vložení pole: kurzor do dokumentu, vybrat pole v panelu, **Add field**.
4. Volitelně (rychle) **Insert table**. Otevře se dialog **Create Data Table**:
   - **Data source** = `Line`,
   - přidat 2–3 pole,
   - **Include header row**,
   - **Create table**.
   - Pak tabulku smaž (Ctrl+Z), jen ukázka.

### 2.4 HideIf na poznámkách (podmíněný obsah podle booleanu)
V těle layoutu jsou pod tabulkou tři poznámky: o slevě, o měně a o velké nabídce.
1. Označ odstavec s poznámkou o slevě.
2. Na kartě **Business Central** najdi akci pro skrytí podle podmínky („HideIf“). Jako řídicí pole vyber **HasAnyDiscount**.
3. Totéž pro poznámku o měně (**IsForeignCurrency**) a o velké nabídce (**IsLargeQuote**).
4. Další booleany v datasetu: **HasYourReference** (hlavička) a v řádcích **HasLineDiscount** a **IsCommentLine**.

> ⚠️ **Ověř na zkoušce:** přesný název tlačítka HideIf. Microsoft ho ohlásil v roadmapě (RM573329), ale stránka o add-inu ho zatím neuvádí. Může být ve skupině **Hide if empty** nebo jako samostatné tlačítko. Pokud ve tvé verzi add-inu není:
> - ukaž jen **Hide if empty** (2.5),
> - HideIf vysvětli na slidu,
> - a řekni, že booleany jsou v datasetu připravené.

### 2.5 Hide if empty: sloupec slevy zmizí, když nikdo nemá slevu
Skupina **Hide if empty** na kartě **Business Central**:
1. V tabulce řádků klikni do pole **LineDiscountPct** v opakovaném řádku › **Hide Field if Zero**.
2. Klikni do buňky záhlaví sloupce **Disc. %** (řídicí pole v hlavičce) › **Hide Empty Table Column**.
3. Zmiň zbylé dvě volby:
   - **Hide Empty Table**: označ celou tabulku, ne repeater.
   - **Hide Empty Table Row**: označ řídicí pole v opakovaném řádku.

### 2.6 Interní komentář s verzí layoutu
1. Nad tabulku vlož malou tabulku se sloupci *Layout description | Version | Date of change*, např. `KS demo | 1.0 | 18.11.2026`.
2. Označ celou tabulku › **Business Central** › **Insert layout comment**.
   - Při kliknutí dovnitř je vidět rámeček **Hidden Comment**.
3. Řekni: *„Ve Wordu to vidím, v tištěném dokladu to nebude.“*
4. Ulož dokument (Ctrl+S).

### 2.7 Nahraj zpět, otestuj, schval
1. BC › **Report Layouts** › vyber kopii `Sales Quote - body (KS demo)` › skupina **Layout** › **Replace layout**, potvrď a vyber uložený `.docx`.
2. Ukaž sloupec **Layout Status** = **Draft**.
   - Řekni: *„Ve v29 každá nová nebo nahrazená verze začíná jako Draft. Na request page ji běžní uživatelé nevyberou.“*
3. Akce **Run report**. Na request page:
   - filtr **No.** = nabídka **A** (se slevou),
   - záložka **Options** › **Large quote threshold** = 10 000,
   - **Preview**.
   - Výsledek: sloupec **Disc. %** je vidět a poznámky se zobrazují. Komentář s verzí se netiskne.
4. **Run report** znovu, tentokrát filtr na nabídku **B** (bez slevy).
   - Výsledek: sloupec slevy zmizel a poznámky taky.
5. Skupina **Layout Status** › **Set as Approved**.
   - Volitelně ukaž zbylé akce: **Set as Draft**, **Set as Pending Approval**, **Set as Retired**.
   - Nahoře pohledy **Approved Layouts**, **Draft Layouts**, **Pending Approval** a **Retired Layouts**.
6. Volitelně: protože je kopie Body, sloupec **Subtype** = **Body** a FactBox **Details** ukazuje stejné díly (Global default z dema 1). Branding se dědí i do upravené kopie.

---

## Demo 3: Vývojáři, AL a soubory (≈ 3 min)

**Pointa:** v AL se deklaruje body, header/footer a theme. Compiler sám vygeneruje prázdné soubory včetně `.dotx` a výsledek je stejný jako u klasického samostatného layoutu.

### 3.1 AL: sekce rendering
1. VS Code › `src\SalesQuoteKS.Report.al`.
2. Ukaž vlastnosti reportu:
   - `DefaultRenderingLayout = TTQuoteBody;`
   - `DefaultHeaderFooterPart = TTHeaderFooter;`
   - `DefaultThemePart = TTTheme;`
3. Sjeď na sekci `rendering` a čtyři layouty:
   - `TTQuoteClassic`: `LayoutSubtype` Default, klasický samostatný layout,
   - `TTQuoteBody`: Body,
   - `TTHeaderFooter`: HeaderFooter, `.docx`,
   - `TTTheme`: Theme, `.dotx`.
4. Ukaž v datasetu booleovské sloupce: `HasAnyDiscount`, `IsForeignCurrency`, `IsLargeQuote` atd.
   - Řekni: *„Pro HideIf v AL připravíme boolean, layout pak nepotřebuje žádnou logiku.“*
5. `app.json`: `"runtime": "18.0"`, což je minimum pro nové vlastnosti.
6. Zmiň dvě věci:
   - `WordAddInCompatibleLayouts` je deprecated,
   - v cestách k layoutům lze psát `/` i `\`.

### 3.2 Compiler vygeneruje chybějící soubor
1. V průzkumníku smaž (nebo přejmenuj) `layouts\TTTheme.dotx`.
2. V terminálu:
   ```powershell
   cd c:\_xshare\_code\AI\know-share__word-layouts\demo
   .\build.ps1
   ```
3. Soubor je zpět. Compiler vytvořil i `.dotx`.
   - Pozor: `build.ps1` ho pak znovu naplní tématem Tres Tria, takže výsledek je stejný jako předtím.
   - Pokud chceš ukázat čistě vygenerovaný prázdný soubor, spusť místo celého `build.ps1` jen kompilaci F5/Ctrl+Shift+B ve VS Code.

### 3.3 Co je uvnitř souboru layoutu
1. Zkopíruj `layouts\TTQuoteBody.docx` jako `TTQuoteBody.zip` a otevři ho.
2. Složka `customXml`, ukaž dva soubory `item*.xml`:
   - `urn:microsoft-dynamics-nav/reports/...`: klasický dataset pro mapování,
   - `urn:microsoft-dynamics-businesscentral/internal/extended/reports/...`: rozšířená verze pro add-in.
3. V obou najdi `BCReportInformation` › `CompanyMetadata`: jméno firmy, `AddressLines`, telefon, e-mail, logo, VAT, banka, IBAN a SWIFT.

### 3.4 Výsledek: klasický a složený layout jsou stejné
1. Otevři vedle sebe `demo\output\quote-classic.pdf` a `demo\output\quote-composite.pdf`.
2. Řekni: *„Vypadají stejně, ale body vpravo nemá žádné záhlaví, logo ani barvy, ty přišly z dílů.“*
3. Volitelně otevři `layouts\TTQuoteBody.docx` ve Wordu a ukaž, že je opravdu šedivý a bez záhlaví.

> **Živě, jen když je čas.** `.\render.ps1` znovu publikuje obě appky do bc29 a vyrenderuje PDF. Trvá to 1–2 minuty, proto je bezpečnější ukazovat hotová PDF.

---

## Po zkoušce: vrátit do čistého stavu

1. **Report defaults for theme and header-footer**: smaž řádek **All reports**.
2. **Company Information** › **Reporting**: vyprázdni **Default Theme** a **Default Header/Footer**, pokud jsi je zkoušel.
3. **Manage themes and header-footer layouts**: označ `TT Theme` a `TT Header Footer` › **Delete**.
   - Systém hlásí počet přiřazení, potvrď.
4. **Report Layouts**: smaž kopii `Sales Quote - body (KS demo)`.
5. Ve Wordu zavři exportovaný dokument.

---

## Tahák: kde co najdu

| Chci… | Kde (Alt+Q) | Akce / pole |
|---|---|---|
| Zapnout novou funkci | **Feature Management** | **Document Report Experience** › **Enabled for** |
| Nahrát téma / záhlaví-zápatí | **Manage themes and header-footer layouts** | **New theme**, **New header/footer** |
| Schválit díl | totéž | **Part Status** › **Set as Approved** |
| Výchozí pro všechny reporty | **Report defaults for theme and header-footer** | **Add global default** › **Header/Footer Part**, **Theme Part** |
| Výchozí pro jeden report / layout | totéž | **Set for one report**, **Set for one layout**, **Apply to all layouts** |
| Výchozí pro firmu | **Company Information** | FastTab **Reporting** › **Default Theme**, **Default Header/Footer** |
| Díly pro jeden layout | **Report Layouts** | **Composite layout** › **Set report theme and header-footer** |
| Přehled všech layoutů reportu | **Report Layouts** | **Composite layout** › **Set all report theme and header-footer** |
| Co platí pro vybraný layout | **Report Layouts** | FactBox **Details** › **Theme source**, **Header/Footer source** |
| Kopie layoutu z extension | **Report Layouts** | **Edit info** › **Save Changes to a Copy** |
| Export pro Word add-in | **Report Layouts** | **Layout** › **Update and export layout** |
| Nahrát upravený layout | **Report Layouts** | **Layout** › **Replace layout** |
| Schválit layout | **Report Layouts** | **Layout Status** › **Set as Approved** |
| Nový prázdný layout | **Report Layouts** | **New** › **Add New Layout for a Report**: **Format Options** = Word, **Subtype** = Body, **Create a Blank Layout from the report object** |
| Add-in ve Wordu | Word › karta **Business Central** | **Add Data**, **Insert table**, **Hide if empty**, **Insert layout comment** |
