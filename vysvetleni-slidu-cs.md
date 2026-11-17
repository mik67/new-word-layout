# Vysvětlení slidů (volnější verze prezentace)

Jen pro tebe: co je na každém slidu, proč tam je a co bys k němu měl vědět, kdyby přišla otázka.
Prezentace: https://claude.ai/artifact/4xiZ1bANEUiiQujt7kChia (15 slidů).

---

## 1 · Cover: „Word layouts just got a lot less painful“

**Na slidu:** logo Tres Tria, datum, nadpis a podtitul „BC 2026 wave 2 (v29)“.

**Co vědět:**
- Release wave 2 2026 = BC verze 29 (update 29.0).
- Online zákazníci si update mohou naplánovat nejpozději do února 2027, takže téma se týká všech našich SaaS zákazníků v příštích měsících.

---

## 2 · „Three things that hurt today“

**Na slidu:** tři dlaždice, tři bolesti, které zná každý v týmu.
- **New look?** Logo samotné problém není, standardní layouty ho berou dynamicky z **Company Information** (stejně jako adresu a banku). Problém je **vzhled a obsah** záhlaví a zápatí, který má každý Word layout (faktura, nabídka, objednávka, výpis…) zkopírovaný v sobě: nové firemní barvy a písma, jiné rozvržení, nový řádek v zápatí (registrace, právní doložka, „Copyright…“), přesunutí bankovních údajů. Taková změna = otevřít a upravit každý soubor zvlášť.
- **Hide a zero?** Word dosud neuměl žádnou logiku. „Skryj řádek se slevou, když je nulová“ znamenalo práci vývojáře: spočítat to v AL, přidat pomocný dataitem a vnořené repeatery.
- **Uploaded a layout?** Nahrání layoutu (Replace) ho okamžitě nasadilo všem. Žádný koncept, žádné schválení, žádná stopa „kdo a kdy“.

**Proč tam je:** všichni v místnosti se v tom poznají a máš je na své straně. Každá z bolestí má v dalších slidech řešení.

---

## 3 · „One layout, three layers“

**Na slidu:** vlevo tři vrstvy, vpravo skutečná nabídka vyrenderovaná v našem kontejneru.
- **Theme (.dotx):** písma, barvy, styly odstavců a tabulek. Je to Word šablona.
- **Header/footer (.docx):** logo, adresa, DIČ, banka, „strana x z y“.
- **Body (.docx):** jen to, co je specifické pro daný report (řádky, součty, poznámky).

**Jak to funguje:**
- BC tři části spojí v okamžiku tisku. Tomu se říká *composite layout*.
- Layout typu Body sám o sobě nemá logo ani barvy. Nabídka na obrázku má body úplně šedé a všechno firemní přišlo z dílů.
- Klasické samostatné layouty dál fungují beze změny, jen se teď jmenují subtype **Default**. Skládání je dobrovolné.

**Pointa:** „Change the header once. Every report follows.“ Vzhled záhlaví a zápatí (a přes téma i barvy a písma) se mění na jednom místě, ne ve třiceti souborech. Data v nich (logo, adresa, banka) se dál berou z Company Information, teď přes blok `CompanyMetadata`.

---

## 4 · Docker lab: „Everything today runs in a box on my laptop“

**Na slidu:** tok AL appka → build.ps1 → bc29 → render.ps1 a tři štítky (URL, čísla reportů, složka).

**Co je za tím:**
- **AL appka** `WordToolkitDemo` má dva reporty:
  - **50600** Sales Quote: má všechny čtyři layouty, tj. klasický, body, header/footer a theme.
  - **50601** Customer Overview: má jen body, branding mu přiřadíme v labu 1.
- **build.ps1:**
  1. Zkompiluje appku. Compiler sám vytvoří chybějící prázdné soubory layoutů.
  2. Python skript je naplní obsahem: logem, vazbami na data a tabulkami.
  3. Kompiluje se podruhé, aby se hotové layouty zabalily do appky.
- **bc29:** BC 29.0 v Dockeru (GB sandbox, multitenant, proto `?tenant=default` v adrese).
- **render.ps1:** publikuje appku do kontejneru a pomocnou codeunitou vytiskne PDF do `demo/output`.

**Proč tam je:** publikum vidí, že všechno je skutečné a dá se zopakovat. Celý lab je ve sdílené složce.

---

## 5 · „The closest rule wins“ ⭐ (podrobně)

**Na slidu:** pět kroků zleva doprava: *This layout → This report → This company → Everyone → The app*. Pod tím tip pro support k FactBoxu **Details**.

### Princip
Když se report tiskne, BC pro něj hledá **theme** a **header/footer**. Prochází nastavení od nejkonkrétnějšího k nejobecnějšímu a **použije první, které najde**. Je to jako v CSS nebo jako u výjimek ze směrnice: obecné pravidlo platí, dokud ho nepřebije konkrétnější.

### Přesné pořadí (podle Microsoft Learn)

| Pořadí | Platí pro | Kde se nastavuje |
|---|---|---|
| 1 | tento body layout, **v aktuální firmě** | Report Layouts › Composite layout › **Set report theme and header-footer**, případně řádek s vyplněným **Company Name** |
| 2 | tento body layout, **ve všech firmách** | totéž, **Company Name** prázdné |
| 3 | všechny body layouty reportu, **v aktuální firmě** | **Report defaults for theme and header-footer** › **Set for one report** + **Company Name** |
| 4 | všechny body layouty reportu, **ve všech firmách** | totéž, **Company Name** prázdné |
| 5 | všechny reporty **v aktuální firmě** | **Company Information** › FastTab **Reporting** › **Default Theme**, **Default Header/Footer** |
| 6 | všechny reporty, všechny firmy | **Report defaults…** › **Add global default** |
| 7 | nic z toho není nastavené | díly deklarované v AL: nejdřív u layoutu (`HeaderFooterPart`, `ThemePart`), pak u reportu (`DefaultHeaderFooterPart`, `DefaultThemePart`) |

Na slidu je to zjednodušené do pěti kroků. „This layout“ jsou řádky 1–2, „This report“ řádky 3–4, „This company“ řádek 5, „Everyone“ řádek 6 a „The app“ řádek 7.

### Tři důležitá pravidla
1. **Theme a header/footer se vyhodnocují zvlášť.** Report může mít vlastní záhlaví na úrovni reportu a přitom převzít téma z firemního nastavení.
2. **Prázdné pole = „nevím, zeptej se o úroveň výš“.** Když na úrovni reportu vyplníš jen header/footer a theme necháš prázdné, theme se hledá dál (firma, globální…).
3. **Nastavení admina přebíjí výchozí díly z appky.** To je pro nás klíčové. Naše A365 appka může mít v AL vlastní výchozí branding, ale jakmile si zákazník nastaví globální výchozí díly, jeho branding vyhraje.

### Nuance, kterou můžeš, ale nemusíš zmiňovat
Microsoft Learn píše, že když je díl uložený přímo s vybraným layoutem, použije se ještě před nastavením admina. Na slidu to není, aby nebyl přeplněný. V původní verzi prezentace je to řádek „0“.

### Příklady z praxe
- **Zákazník se třemi firmami (UK, IE, CZ):**
  - **Global default** = skupinové téma (barvy, písma) a obecné záhlaví.
  - Každá firma má na **Company Information › Reporting** vlastní **Default Header/Footer** s vlastním logem, adresou, DIČ a bankou.
  - Téma nechají prázdné, takže ho dědí z globálního nastavení.
  - Výsledek: jedno téma a tři záhlaví místo desítek upravených layoutů.
- **Dodací list bez bankovního spojení:**
  - Firemní záhlaví má v zápatí banku a IBAN, ale na dodacím listu je nechtějí.
  - **Set for one report** pro dodací list, s header/footer „bez banky“ a theme prázdným.
  - Dodací list tak má jiné zápatí a stejné barvy.
- **Jeden speciální layout:**
  - Exportní faktura v angličtině pro jednoho velkého odběratele má jiné záhlaví (např. s EORI).
  - **Set for one layout** jen pro ten jeden body layout. Ostatní layouty faktury zůstávají beze změny.
- **Support: „V druhé firmě je na faktuře staré logo“:**
  1. **Report Layouts** › vyber body layout › FactBox **Details** a podívej se na **Header/Footer source**.
     - **Company** → oprav to na **Company Information** dané firmy.
     - **Global default** → oprav globální řádek.
     - **None** → pozor: může to přicházet z appky (AL fallback), FactBox to nezobrazuje.
  2. Pozor i na stav: když díl přepneš z **Approved** na **Retired**, **existující přiřazení dál platí**. Přestat ho používat znamená přiřazení smazat.

**Proč tam je:** tohle je otázka, kterou dostane support jako první: „proč se tam bere tohle záhlaví?“

---

## 6 · Docker lab 1: „Brand every report. No code.“ (4 min)

**Na slidu:** pět kroků v BC, poslední zlatě (výsledek).

**Co se děje:**
1. Report **Customer Overview (50601)** vyjde šedý, protože má jen body a nic přiřazeno.
2. Na **Manage themes and header-footer layouts** nahraješ `TTTheme.dotx` (**New theme**) a `TTHeaderFooter.docx` (**New header/footer**) ze složky `demo/parts-for-upload`.
3. Nové díly jsou ve stavu Draft. **Part Status › Set as Approved**, jinak nejdou přiřadit.
4. **Report defaults for theme and header-footer › Add global default** a vybrat oba díly. To je úroveň 6 z předchozího slidu.
5. Report znovu: stejné body, teď s brandingem.

**Riziko:** neověřili jsme, jestli záhlaví generované pro report 50600 vyplní údaje o firmě i u 50601. Když ne, záloha je v `demo-navod-cs.md` (PDF v `demo/output`).

---

## 7 · „Word finally learns a bit of logic“

**Na slidu:** šest dlaždic s funkcemi doplňku Dynamics 365 Business Central Word Add-in.
- **Add Data:** panel s poli, popisky a informacemi o reportu. Klikneš a vloží se navázaný content control, bez XML Mapping Pane.
- **Insert table:** celá datová tabulka s opakovaným řádkem z jednoho dialogu.
- **Hide if empty:** čtyři volby: skrýt pole, když je nula; prázdnou tabulku; prázdný řádek; prázdný sloupec.
- **HideIf:** zobrazit nebo skrýt libovolný blok podle ano/ne hodnoty (boolean) z dat. To je ta „logika“.
- **Layout comment:** poznámky a historie verzí, které jsou vidět jen při návrhu a nikdy se netisknou.
- **Parts:** v doplňku se dají navrhovat i header/footer a theme.

**Praktická rada dole:** vždy začínat z **Report Layouts › Layout › Update and export layout**. Jen tak má soubor metadata, bez kterých doplněk neukáže pole.

**Pozor:** přesný název tlačítka HideIf v doplňku nemáme ověřený (je v roadmapě, na stránce Learn zatím ne).

---

## 8 · Docker lab 2: „The quote that tidies itself“ (5 min)

**Na slidu:** pět kroků v BC a ve Wordu.
1. Layout z appky nejde nahradit. Nejdřív kopie: **Edit info › Save Changes to a Copy**.
2. **Layout › Update and export layout** a otevřít ve Wordu.
3. HideIf na třech poznámkách: sleva (`HasAnyDiscount`), cizí měna (`IsForeignCurrency`) a velká nabídka (`IsLargeQuote`).
4. **Hide Field if Zero** na poli slevy v řádku a **Hide Empty Table Column** na záhlaví sloupce „Disc. %“. Sloupec zmizí, když nikdo nemá slevu.
5. **Replace layout**: layout je teď Draft. **Run report** na dvou nabídkách (se slevou a bez) a pak **Set as Approved**.

**Co připravit:** nabídku se slevou (a ideálně v cizí měně, nad 10 000) a nabídku bez slevy (CRONUS 1001).

---

## 9 · „Replace a layout? It’s a Draft now.“ (zlatý slide)

**Na slidu:** jedna zpráva, kterou si mají všichni zapamatovat, a stavy Draft → Pending Approval → Approved → Retired.

**Co vědět:**
- Ve v29 každý nový nebo nahrazený uživatelský layout začíná jako **Draft**. Na request page ho běžní uživatelé nevyberou, ale přes **Run report** ho otestuješ.
- **Approved** = dostupný, **Retired** = už se nenabízí, ale není smazaný.
- Platí pro Word, Excel i RDLC layouty a také pro theme a header/footer díly.
- U layoutů z appky se změna stavu uloží jako přepis (override). Appka zůstane nedotčená.
- Pro support: tohle bude generovat telefonáty typu „nahrál jsem novou fakturu a nikdo ji nevidí“.

---

## 10 · „Four subtypes and a few booleans“

**Na slidu:** vlevo ukázka AL, vpravo čtyři body.
- **Subtype:** Default (klasický), Body, HeaderFooter, Theme. Nastavuje se v sekci `rendering`.
- **Company info is free:** každý Word dataset automaticky obsahuje blok `CompanyMetadata` (název, až 8 řádků adresy, telefon, e-mail, web, logo, DIČ, IČO, banka, IBAN, SWIFT). Vývojář nemusí přidávat sloupce.
- **Booleans:** rozhodnutí se spočítá v AL a dá se do datasetu jako srozumitelně pojmenovaný boolean. Konzultant pak ve Wordu určí, co skrývá.
- **runtime 18.0** je minimum. Compiler vytvoří chybějící `.docx` i `.dotx` s datasetem. Vlastnost `WordAddInCompatibleLayouts` je zastaralá a cesty k layoutům fungují s `/` i `\`.

---

## 11 · Docker lab 3: „Same engine, the parts make the brand“ (3 min)

**Na slidu:** čtyři kroky a dva náhledy PDF (jen body vs. body + díly).
1. VS Code: sekce `rendering` v `SalesQuoteKS.Report.al`, čtyři layouty.
2. Smazat `TTTheme.dotx` a spustit `build.ps1`: soubor je zpět, protože compiler ho vygeneroval.
3. `.docx` je zip. Uvnitř je `customXml` a v něm blok `CompanyMetadata`.
4. `render.ps1` a porovnat PDF.

**Tip:** `render.ps1` trvá 1–2 minuty. Když nebude čas, ukaž hotová PDF v `demo/output`.

---

## 12 · „Stuff that bit me“

**Na slidu:** šest věcí, na které jsi narazil při přípravě.
- **Feature switch:** bez zapnuté funkce „Document Report Experience“ (Feature Management) se nic ze skládání neukáže.
- **App parts stay put:** díl deklarovaný v AL u reportu A nejde přiřadit reportu B. Pro sdílení mezi reporty je potřeba díl nahrát jako uživatelský.
- **Copy, then edit:** layout z appky nejde nahradit, jen jeho kopie.
- **Docker has no fonts:** kontejner měl jen Lucida Console a PDF vypadala hrozně. Pomohl `Add-FontsToBcContainer`.
- **Multitenant login:** bez `?tenant=default` hlásí přihlášení „technical issue“.
- **Hidden menus:** „Update and export layout“ je schované pod rozbalovacím tlačítkem **Layout**.

---

## 13 · „Something for every team“

**Na slidu:** čtyři dlaždice podle rolí.
- **Konzultanti:** při každém go-live jeden „branding pack“ (theme + header/footer) jako globální výchozí. U zákazníků s více firmami výchozí díly pro každou firmu.
- **Sales:** „váš brand na všech dokladech“ jako malý balíček za pevnou cenu. Dá se ukázat živě v pre-sales za dvě minuty.
- **Support:** první otázka „je layout Approved?“, druhá „který díl vyhrává?“ (FactBox **Details**).
- **Dev a A365:** když naše appky budou mít dokladové reporty jako Body layouty, automaticky převezmou branding každého zákazníka. To je silný prodejní argument.

---

## 14 · „Five small steps from here“

**Na slidu:** pět konkrétních kroků s rolí.
1. Z dema udělat náš standardní branding pack (konzultanti).
2. Balíček „brand na dokladech“ za pevnou cenu (sales).
3. A365: dokladové reporty na Body layouty a booleany pro HideIf (dev).
4. Jednostránková poznámka pro support: Draft vs. Approved, který díl platí (support).
5. Vybrat jednoho pilotního zákazníka po přechodu na v29 (všichni).

**Co udělat na místě:** zeptat se, kdo si který krok vezme. Konkrétní jména nejsou na slidu záměrně.

---

## 15 · „Questions? Or your best Word layout horror story.“

**Na slidu:** poděkování, otázky, odkaz na složku s labem a na Microsoft Learn.

**Proč „horror story“:** lidé se rozpovídají snáz, když mají vyprávět vlastní zkušenost, než když mají položit „chytrou“ otázku. Když bude ticho, zeptej se: „Který zákazník by byl nejlepší pilot?“
