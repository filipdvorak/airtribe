# Rozdělování peněz z vystoupení

Spočítá, kolik z výdělku za vystoupení náleží komu — včetně nákladů na dopravu, výdajů,
které někdo zaplatil za ostatní, a odměny pro organizátora. Výsledek je webová stránka
na jedné adrese: ty ji spravuješ, ostatní si ji jen otevřou a přečtou.

Celá aplikace je jeden soubor `index.html`. Žádný server, žádná databáze, žádná registrace.

---

## Jak se to používá

### Ostatní — jen čtení

Otevřou odkaz a vidí u každé akce, kolik komu vychází. Po kliknutí na jméno se rozbalí
rozpad částky krok po kroku, u každého kroku je **i** s vysvětlením a vzorcem. V každém
oddílu je rozbalená vždy jen jedna položka — otevřením další se předchozí zavře. Nic
nemohou změnit — v tomhle zobrazení nejsou žádná editovatelná pole.

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
lidi. Čísla se přepočítají okamžitě. Tlačítka **+ Nová akce**, **⧉ Duplikovat akci**
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

- **Doprava** — libovolný počet jízd, u každé částka, řidiči a osádka. *Naše auto* i *cizí
  doprava*. Název naší jízdy se odvodí od řidiče (*Milošovo auto*, *Milošovo a Patrikovo
  auto*); když chceš jiný, prostě ho přepiš.
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
   - *Naše auto* — řidič dostane částku zpátky (při více řidičích se dělí mezi ně) a zároveň
     se na ní podílí; do dělení se započítá automaticky. Peníze **zůstávají uvnitř skupiny**.
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

Když se v označení objeví nesmysl — třeba se někdo veze naším autem, ale nikdo není
označený jako řidič, nebo u interního nákladu chybí ten, kdo ho zaplatil — aplikace na to upozorní
a rozdíl v kontrole to ukáže.

Všechny částky jsou počítané a zobrazované přesně na haléře.

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
index.html   celá aplikace — HTML, CSS, JS i data v jednom souboru
vercel.json  hlavičky, aby prohlížeč vždy načetl aktuální verzi
NASAZENI.md  nasazení, token a běžná údržba
README.md    tento popis
```

Data jsou uložená přímo v `index.html` v bloku
`<script id="data" type="application/json">`. Když nahráváš novou verzi kódu, přepíšeš
tím i data — v editaci je proto panel **Přenos dat do nové verze** s tlačítky na jejich
zkopírování a opětovné vložení.
