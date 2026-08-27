# Rozdělování peněz z vystoupení

Spočítá, kolik z výdělku za vystoupení náleží komu — včetně nákladů na dopravu, výdajů,
které někdo zaplatil za ostatní, a odměny pro organizátora. Výsledek je webová stránka
na jedné adrese: ty ji spravuješ, ostatní si ji jen otevřou a přečtou.

Celá aplikace je jeden soubor `index.html`. Žádný server, žádná databáze, žádná registrace.

---

## Jak se to používá

### Ostatní — jen čtení

Otevřou odkaz a vidí u každé akce, kolik komu vychází — pod jménem je drobně jeho číslo
účtu, ať se peníze dají hned poslat. Po kliknutí na jméno se rozbalí
rozpad částky krok po kroku, u každého kroku je **i** s vysvětlením a vzorcem. V každém
oddílu je rozbalená vždy jen jedna položka — otevřením další se předchozí zavře.

Jediné, co v tomhle zobrazení jde přepnout, je zaškrtávátko **přišly mi peníze** u jména.
Kdo si ho zaškrtne, uvidí to i ostatní — viz *Potvrzení plateb* níže. Nic dalšího změnit
nejde, žádná jiná editovatelná pole tu nejsou.

Po otevření se ukáže naposledy vytvořená akce. V přilepeném pruhu nahoře je vidět vždy
jen právě vybraná akce s jejím datem a vedle ní vlevo tlačítko ☰. To vysune od levého
okraje panel se seznamem všech akcí i s datem a výdělkem; zavře se křížkem, kliknutím
vedle nebo Escapem.

Zobrazuje se jen to, co má obsah: kdo na akci nebyl, se v seznamu neobjeví, a nulové
náklady se vynechají i s celou sekcí. Na mobilu se místo široké tabulky použije seznam
funkcí s počty, takže se nikde nic neposouvá do strany.

### Ty — editace

Přidej `?edit` na konec adresy:

```
https://vystoupeni.vercel.app/?edit
```

Můžeš měnit název, datum a výdělek, zaškrtávat, kdo se čeho účastnil, a přidávat či mazat
lidi. U každého člověka je vedle jména políčko na číslo účtu — vyplněné se ukáže drobným
písmem pod jménem ve výpisu; při přejmenování jde s ním a při smazání zmizí. Čísla se přepočítají okamžitě. Tlačítka **+ Nová akce**, **⧉ Duplikovat akci**
a **🗑 Smazat akci** jsou hned nad shrnujícími kartami. Přejmenovat akci jde i rovnou
v seznamu pod ☰ — tužka u ní otevře pole pro nový název, takže na ni nemusíš nejdřív
přepínat.

Oddíly jdou v tom pořadí, v jakém se data obvykle zadávají: **Nastavení akce** →
**Kdo se čeho účastní** → **Doprava** → **Náklady** → **Kontrola** (na tu se kouká
naposled — musí v ní vyjít nula). Kliknutím na nadpis sloupce
(*Účastník*, *Organizátor*; na mobilu na štítek pod kartami lidí) se rozbalí vysvětlivka
s vzorcem — a u organizátora spolu s ní i pole na jeho procento z výdělku.

Jízdy a náklady se zadávají po jednotlivých položkách ve dvou sbalovacích oddílech, takže
nezabírají místo, když je zrovna nepotřebuješ:

- **Doprava** — libovolný počet jízd, u každé částka, řidič a osádka. Řidič je u jízdy
  vždycky jeden (vybírá se kolečkem) — když jela dvě auta, přidáš dvě jízdy. Název naší jízdy se odvodí od řidiče a skloní se podle věty: u řidiče se ukáže
  *řídil svoje auto*, u ostatních *vezl se Milošovým autem* (při více řidičích
  *Milošovým a Patrikovým autem*) — a stejně i v rozpadu částky. Když chceš vlastní název,
  prostě ho přepiš; pak se píše s dvojtečkou a neskloňuje.
- **Náklady** — libovolný počet výdajů, každý s vlastním názvem. Vybíráš ze tří typů podle
  toho, kam peníze putují (viz níže).

U každé položky je štítek s typem a vedle něj **i** s vysvětlením a vzorcem.

Změny zatím žijí jen v tvém prohlížeči. Zveřejníš je tlačítkem **💾 Uložit na web** —
aplikace zapíše data přímo do repozitáře na GitHubu a na stejné adrese se do ~20 vteřin
objeví nový obsah. Vyžaduje to jednorázové připojení tokenu, který zůstává jen ve tvém
prohlížeči a nikdy se nedostane do stránky ani do sdíleného odkazu.

Záložní cesty jsou v panelech níže: **Stáhnout soubor webu** k ručnímu nahrání
a **Odkaz s daty** na rychlou jednorázovou ukázku.

---

## Jak se počítá

Pro každou akci, v tomto pořadí:

1. **Základ** — výdělek se rozdělí rovným dílem mezi účastníky.

2. **Jízdy** — každá cesta se počítá zvlášť, takže dvě auta s různými náklady se nemíchají.
   - *Naše auto* — řidič dostane celou částku zpátky a zároveň se na ní podílí; do dělení
     se započítá automaticky. Peníze **zůstávají uvnitř skupiny**.
   - *Cizí doprava* — cena se dělí mezi označené pasažéry a **odchází ven**.

3. **Náklady** — každý výdaj má svůj název, částku a lidi. Typ určuje, co se s penězi stane:
   - *Interní náklad* — částku zaplatil jeden z účastníků ze svého a vybraní se mu na ni
     složí ze svých podílů. Peníze se jen **přesunou uvnitř skupiny**.
   - *Externí individuální náklad* — pevná částka, kterou platí každý označený sám za sebe.
     Nedělí se: kolik je označených, tolikrát částka **odejde ven**.
   - *Externí hromadný náklad* — jedna společná částka placená ven, kterou si označení
     rozdělí rovným dílem. Taky **odchází ven**.

4. **Organizátor** dostane navíc procento z výdělku (výchozí 5 %). Tahle částka se rovným
   dílem strhne ostatním účastníkům, takže celkový součet zůstává stejný.

Co **odchází ven ze skupiny** (cizí doprava, oba externí náklady), o to je k rozdělení méně.
Co se jen **přesouvá mezi lidmi** (naše jízdy, interní náklad, odměna organizátora), celkovou
částku nemění. Sekce *Kam peníze jdou* to odděluje a jmenovitě vypisuje, koho se co týká;
sekce *Kontrola* ověřuje, že „výdělek − náklady ven" přesně sedí na součet všech výplat.

Jména se v textu skloňují podle věty — *pro Miloše*, *pro Jendu*, *Milošovým autem*.
U jmen, která by šlo ohnout špatně (třeba na **-í**), aplikace radši zvolí bezpečný opis.

Když se v označení objeví nesmysl — třeba se někdo veze naším autem, ale nikdo není
označený jako řidič, nebo u interního nákladu chybí ten, kdo ho zaplatil — aplikace na to upozorní
a rozdíl v kontrole to ukáže.

Všechny částky jsou počítané a zobrazované přesně na haléře.

---

## Potvrzení plateb

Zaškrtávátko **přišly mi peníze** u jména je jediná věc, kterou ve čtecím režimu ovládají
i ostatní. Zaškrtnutí uvidí všichni, takže je hned jasné, komu už peníze dorazily a komu
ještě ne.

Má to jeden háček, který stojí za vysvětlení: web je jediný soubor a zapisovat do něj umí
jen ty svým tokenem. Účastník tedy nemá kam uložit něco, co uvidí ostatní. Zaškrtnutí proto
putují **mimo tenhle web**, do malého sdíleného souboru na adrese, kterou nastavíš
v editaci v oddílu **Potvrzení plateb**. Ukládá se do něj jenom to, komu už peníze dorazily
— žádná jména, částky ani nic dalšího tam nejdou.

Nastavení zabere chvilku:

### Založení úložiště (Firebase Realtime Database)

Úložiště musí umět **`GET`** (přečíst) i **`PUT`** (uložit) JSON a — to je ten zádrhel —
musí zápis povolit i stránce běžící na jiné adrese (CORS). Většina „jednorázových" JSON
schránek to neumí; `jsonblob.com` například pustí čtení, ale zápis z prohlížeče odmítne.
Osvědčená volba zdarma je Firebase Realtime Database:

1. Na <https://console.firebase.google.com> dej **Add project**, vymysli název,
   analytiku můžeš vypnout.
2. V levém menu **Build → Realtime Database → Create Database**. Jako umístění zvol
   `europe-west1`, pak **Start in test mode** a **Enable**.
3. Nahoře přepni na záložku **Rules** a přepiš je na tohle (jinak by za měsíc přestaly
   platit a zápis by se zasekl). Otevřená je jen větev `platby`, zbytek zůstává zavřený:

   ```json
   {
     "rules": {
       "platby": { ".read": true, ".write": true }
     }
   }
   ```

   Dej **Publish**.
4. Na záložce **Data** je nahoře adresa databáze, něco jako
   `https://nazev-projektu-default-rtdb.europe-west1.firebasedatabase.app`.
   Do aplikace patří tahle adresa **plus `/platby.json`** na konci:

   ```
   https://nazev-projektu-default-rtdb.europe-west1.firebasedatabase.app/platby.json
   ```

### Zapojení do aplikace

1. Otevři aplikaci s `?edit`, rozbal **Potvrzení plateb** a adresu vlož do políčka
   **Adresa úložiště**. Políčka pro hlavičku nech prázdná. Dej **Uložit nastavení**.
2. Klikni **Otestovat** — aplikace do úložiště zkusmo zapíše a zase po sobě uklidí, takže
   se hned pozná, jestli všechno hraje.
3. Dej **💾 Uložit na web**, aby se adresa dostala i k ostatním.

Dokud adresa není vyplněná, zaškrtávátko se nikomu neukazuje a aplikace se chová jako dřív.
Tlačítkem **Vypnout** ho kdykoli schováš.

### Zámek po minutě

Zaškrtnutí jde vzít zpět jenom **jednu minutu** — to je na opravu překliku dost, na
rozmýšlení ne. Potom se z něj stane obyčejný štítek „peníze dorazily", na který se nedá
kliknout, a přepne se sám, i když stránku nikdo neobnoví. Odškrtnout ho může už jen ten,
kdo otevře web s `?edit` — tam zámek neplatí nikdy. Do úložiště se proto místo `true`
ukládá čas zaškrtnutí; staré záznamy s hodnotou `true` se berou jako už zamčené.

Je to zámek na dveřích, ne trezor: kdo má odkaz, může do úložiště zapsat cokoli i mimo
aplikaci. Proti překliku to funguje spolehlivě, proti záměrnému přepsání ne.

---

## Záloha dat (nemuset po nahrání nové verze nic vyplňovat)

Data žijí v souboru `index.html`. To má jednu nepříjemnost: když nahraješ novou verzi
souboru, přijdeš s ní i o všechno vyplněné. Proto se při každém **💾 Uložit na web**
posílá kopie všech dat i do stejného úložiště, jaké se používá na potvrzení plateb
(uloží se tam pod klíč `__data`, vedle zaškrtnutí, která nepřepíše).

Když se pak otevře stránka, ve které jsou data starší než v úložišti, aplikace si
tu novější verzi sama stáhne a řekne o tom: *„Načtena novější data z úložiště."*
Nahrání nové verze `index.html` tak neznamená nic víc než nahrání souboru.

Které jsou novější, se pozná podle razítka `ulozenoMs` — počtu milisekund, kdy se data
naposledy ukládala. Do souboru i do zálohy jde stejné razítko, takže se nemůže stát,
že by se aplikace přetahovala sama se sebou.

Aby to vyšlo i tehdy, když v čerstvě nahraném souboru ještě není adresa úložiště,
pamatuje si ji tvůj prohlížeč zvlášť (v `localStorage`). Poprvé ji tedy zadáš jednou
a pak už se o ni nemusíš starat.

Obnova se dělá **jen při načtení stránky**, nikdy ne během editace — rozdělanou práci
tedy nemá jak přepsat. A verze načtená z odkazu (`#d=…`) se ze zálohy nepřepisuje nikdy.

Pořád platí, že originál dat je i v souboru na GitHubu, včetně historie commitů. Kdyby
někdo do úložiště zapsal nesmysl, stačí soubor otevřít a znovu uložit na web.

Co je dobré vědět: kdo má odkaz na web, může zaškrtnutí měnit — úložiště nikoho nerozlišuje.
Na „už mi přišly peníze" to stačí, na nic citlivějšího bych se na to nespoléhal. Když dva
lidé zaškrtnou naráz, nic se neztratí — aplikace si před uložením vždy načte aktuální stav
a spojí ho se svým. Když úložiště zrovna nejede, zaškrtnutí se vrátí zpátky a aplikace to
řekne; spočítané částky to nijak neovlivní.

---

## Profilové obrázky

Ve výchozím stavu stojí u výplaty jen jméno. Když k někomu chceš fotku, stačí
v `index.html` doplnit jeho jméno a adresu obrázku do připraveného seznamu:

```js
var FOTKY = {
  "Filip": "fotky/filip.jpg",
  "Jenda": "https://example.com/jenda.png"
};
```

Adresa může být soubor uložený v repozitáři vedle `index.html` i odkaz na internet.
Obrázek se sám vyřízne a vyplní celou výšku řádku; u koho nic nezadáš, stojí prostě
jen jméno.

---

## Nasazení a údržba

Postup, jak dostat aplikaci na trvalou adresu a jak zapnout ukládání z prohlížeče,
je krok za krokem v **[NASAZENI.md](NASAZENI.md)**.

## Struktura

```
index.html   celá aplikace — HTML, CSS, JS, favikon i data v jednom souboru
vercel.json  hlavičky, aby prohlížeč vždy načetl aktuální verzi
NASAZENI.md  nasazení, token a běžná údržba
README.md    tento popis
```

Data jsou uložená přímo v `index.html` v bloku
`<script id="data" type="application/json">`. Když nahráváš novou verzi kódu, přepíšeš
tím i data — v editaci je proto panel **Přenos dat do nové verze** s tlačítky na jejich
zkopírování a opětovné vložení.
