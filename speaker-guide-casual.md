# Mini speaker guide: what to say (casual deck)

Deck: https://claude.ai/artifact/4xiZ1bANEUiiQujt7kChia · 15 slides · about 30 minutes.
Each slide: **EN** = what to say (natural, keep it loose). **CS** = the same in Czech, so you know what it means.

| Block | Slides | Time |
|---|---|---|
| Intro and the big idea | 1–4 | 4 min |
| Who decides + lab 1 | 5–6 | 6 min |
| Word + lab 2 + Draft | 7–9 | 8 min |
| Developers + lab 3 | 10–11 | 5 min |
| Lessons, us, next steps | 12–14 | 4 min |
| Questions | 15 | 3+ min |

---

### 1 · Cover
**EN:** “Hi everyone. Today is about Word layouts, and the good news is they just got a lot less painful. Twenty minutes of story, three live labs in Docker, then your questions. This isn’t a developer-only session. Everyone should leave with something useful.”
**CS:** „Ahoj všem. Dnes o Word layoutech. Dobrá zpráva je, že teď bolí mnohem míň. Dvacet minut povídání, tři živé laby v Dockeru, pak otázky. Není to jen pro vývojáře, každý si má odnést něco užitečného.“

### 2 · Three things that hurt today
**EN:** “Quick show of hands: the logo already comes from Company Information, fine. But who has changed the same footer line, or new brand colours, in thirty layouts? Who has had to ask a developer just to hide a zero? And who has uploaded a layout and broken it for everyone? Right. Wave 2 takes care of all three.”
**CS:** „Rychlé hlasování: logo se už bere z Company Information, to je v pořádku. Ale kdo měnil stejný řádek v zápatí nebo nové firemní barvy ve třiceti layoutech? Kdo musel volat vývojáře kvůli skrytí nuly? A kdo nahrál layout a rozbil ho všem? Přesně. Wave 2 řeší všechny tři.“

### 3 · One layout, three layers
**EN:** “Here’s the big idea. A layout is now three pieces: a theme for fonts and colours, a header/footer for logo and address, and a body with just this report’s content. BC glues them together when it prints. This quote is real output from our container, and the body file has no logo and no colours at all. So: change the header once, every report follows. And the old one-file layouts still work.”
**CS:** „Hlavní myšlenka: layout jsou teď tři kusy. Téma pro písma a barvy, záhlaví/zápatí pro logo a adresu, a body jen s obsahem reportu. BC je slepí při tisku. Tahle nabídka je skutečný výstup z kontejneru a soubor body nemá vůbec žádné logo ani barvy. Takže záhlaví změníš jednou a všechny reporty ho převezmou. Staré jednosouborové layouty dál fungují.“

### 4 · Docker lab
**EN:** “Everything you’ll see today runs in a box on my laptop: a small AL app with two reports, a build script, BC 29 in Docker, and a render script that prints PDFs. No slides for the demos, just the real thing. It’s all in our share, so you can rebuild it yourself.”
**CS:** „Všechno, co dnes uvidíte, běží v krabičce na mém notebooku: malá AL appka se dvěma reporty, build skript, BC 29 v Dockeru a render skript, který tiskne PDF. Žádné slidy místo dema, ale skutečný systém. Celé je to ve sdílené složce, takže si to můžete postavit sami.“

### 5 · The closest rule wins
**EN:** “So who decides which header a report gets? The closest rule wins. A setting for this exact layout beats one for the whole report, which beats the company default, which beats the global default. If nobody set anything, the app’s own default applies. Theme and header are picked separately. Example: a group with three companies. One global theme, and each company gets its own header with its own logo and bank details. And for support: if the Details FactBox says None, the header can still come from the app.”
**CS:** „Kdo rozhoduje, jaké záhlaví report dostane? Vyhrává nejbližší pravidlo. Nastavení pro konkrétní layout přebije nastavení pro celý report, to přebije firemní výchozí a to přebije globální výchozí. Když nikdo nic nenastavil, platí výchozí díl z appky. Téma a záhlaví se vybírají zvlášť. Příklad: skupina se třemi firmami. Jedno globální téma a každá firma má vlastní záhlaví s vlastním logem a bankou. A pro support: když FactBox Details ukazuje None, záhlaví může pořád přicházet z appky.“

### 6 · Docker lab 1 (4 min, in BC)
**EN, while clicking:**
- “Here’s the customer overview. Plain grey, just the body.”
- “Now I upload a theme and a header/footer. Notice they start as Draft, so I approve them.”
- “One global default, both parts.”
- “Run it again… same body, now it’s ours. No code, about two minutes.”

**CS:**
- „Tady je přehled zákazníků. Šedý, jen body.“
- „Teď nahraju téma a záhlaví/zápatí. Všimněte si, že začínají jako Draft, tak je schválím.“
- „Jedno globální výchozí nastavení s oběma díly.“
- „Spustím znovu… stejné body, teď v našem brandingu. Bez kódu, asi za dvě minuty.“

### 7 · Word finally learns a bit of logic
**EN:** “Now the designer side. The Business Central add-in for Word got a big upgrade. Add Data instead of the XML pane, a whole table from one dialog, Hide if empty for zeros and empty rows or columns, and HideIf, which shows or hides a block based on a yes/no from the data. Plus comments that never print. One tip: always start from Update and export layout, otherwise the add-in shows no fields.”
**CS:** „Teď strana návrháře. Doplněk Business Central pro Word se výrazně vylepšil. Add Data místo XML panelu, celá tabulka z jednoho dialogu, Hide if empty pro nuly a prázdné řádky či sloupce, a HideIf, který zobrazí nebo skryje blok podle ano/ne z dat. Plus komentáře, které se netisknou. Tip: vždy začínejte z Update and export layout, jinak doplněk neukáže pole.“

### 8 · Docker lab 2 (5 min, in BC and Word)
**EN, while clicking:**
- “Layouts from an app can’t be replaced, so first a copy.”
- “Export for Word… and here’s the Business Central tab.”
- “These three notes print on every quote today. With HideIf, each one appears only when it applies.”
- “The discount column hides itself when nobody has a discount.”
- “Replace… and look, it’s a Draft. I test it, then approve it.”

**CS:**
- „Layout z appky nejde nahradit, takže nejdřív kopie.“
- „Export pro Word… a tady je karta Business Central.“
- „Tyhle tři poznámky se dnes tisknou na každé nabídce. S HideIf se každá ukáže jen tehdy, když dává smysl.“
- „Sloupec se slevou se sám skryje, když nikdo slevu nemá.“
- „Nahradím… a vidíte, je to Draft. Otestuju a pak schválím.“

### 9 · Replace a layout? It’s a Draft now.
**EN:** “If you remember one thing today, make it this. Replace a layout and it’s a Draft. Nobody can pick it until someone approves it. Great for safety, but it will cause a few ‘I uploaded the new invoice and nobody sees it’ calls. Same for Excel and RDLC layouts, and for themes and headers.”
**CS:** „Jestli si dnes zapamatujete jednu věc, ať je to tahle. Nahradíte layout a je z něj Draft. Nikdo ho nevybere, dokud ho někdo neschválí. Skvělé pro bezpečnost, ale přinese to pár telefonátů typu ‚nahrál jsem novou fakturu a nikdo ji nevidí‘. Platí to i pro Excel a RDLC layouty a pro témata a záhlaví.“

### 10 · Four subtypes and a few booleans
**EN:** “Developers, your part is small. Four subtypes: Default, Body, HeaderFooter and Theme. Company info is now in every Word dataset for free, so stop adding those columns. And give designers booleans like HasAnyDiscount. You decide the logic in AL, they decide what it hides. Runtime 18 is the minimum.”
**CS:** „Vývojáři, vaše část je malá. Čtyři subtypy: Default, Body, HeaderFooter a Theme. Údaje o firmě jsou teď v každém Word datasetu zdarma, takže je přestaňte přidávat. A dejte návrhářům booleany jako HasAnyDiscount. Logiku rozhodnete v AL, oni rozhodnou, co se skryje. Minimum je runtime 18.“

### 11 · Docker lab 3 (3 min, in VS Code)
**EN, while clicking:**
- “Here’s the rendering section: four layouts.”
- “I delete the theme file, build… and it’s back. The compiler generates it.”
- “A .docx is just a zip, and here’s CompanyMetadata inside.”
- “Body only on the left, body plus parts on the right. Same engine, the parts make the brand.”

**CS:**
- „Tady je sekce rendering: čtyři layouty.“
- „Smažu soubor tématu, spustím build… a je zpátky. Compiler ho vygeneruje.“
- „Docx je jen zip a uvnitř je CompanyMetadata.“
- „Vlevo jen body, vpravo body s díly. Stejný engine, branding dělají díly.“

### 12 · Stuff that bit me
**EN:** “A few things that cost me time, so they don’t cost you. The feature switch is off by default. Parts from an app stay with their report. Copy before you edit. Our Docker image has no fonts. The container needs the tenant in the URL. And the export action hides under the Layout menu. Pick the two that matter for your role.”
**CS:** „Pár věcí, které mě stály čas, aby nestály vás. Přepínač funkce je ve výchozím stavu vypnutý. Díly z appky zůstávají u svého reportu. Před úpravou udělat kopii. Náš Docker image nemá písma. Kontejner potřebuje tenant v adrese. A export je schovaný pod nabídkou Layout. Vyberte si ty dvě, které se týkají vaší role.“

### 13 · Something for every team
**EN:** “So what’s in it for us? Consultants: a branding pack at every go-live. Sales: ‘your brand on every document’, and it demos in two minutes. Support: two new first questions, is it Approved and which part wins. And for our A365 apps, this is the big one: ship Body layouts and every customer’s branding just applies.”
**CS:** „Co z toho máme my? Konzultanti: branding pack při každém go-live. Sales: ‚váš brand na každém dokladu‘ a ukážete to za dvě minuty. Support: dvě nové první otázky, je to Approved a který díl vyhrává. A pro naše A365 appky je to největší přínos: dodáme Body layouty a branding každého zákazníka se prostě použije.“

### 14 · Five small steps from here
**EN:** “Five small steps. Nothing huge, but each needs an owner. Online customers move to v29 by February 2027 anyway, so the timing is right. Who wants which one?”
**CS:** „Pět malých kroků. Nic velkého, ale každý potřebuje vlastníka. Online zákazníci přejdou na v29 nejpozději v únoru 2027, takže načasování sedí. Kdo si vezme který?“

### 15 · Questions?
**EN:** “Thank you. Questions? Or your best Word layout horror story, I know you have one.”
If it’s quiet: “Which customer would be our best pilot?”
**CS:** „Děkuju. Otázky? Nebo váš nejlepší horor s Word layoutem, vím, že nějaký máte.“
Když bude ticho: „Který zákazník by byl nejlepší pilot?“
