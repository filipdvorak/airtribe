# Rozdělování peněz z vystoupení

Spočítá, kolik z výdělku za vystoupení náleží komu — včetně nákladů na dopravu, výdajů,
které někdo zaplatil za ostatní, a odměny pro organizátora. Výsledek je webová stránka
na jedné adrese: ty ji spravuješ, ostatní si ji jen otevřou a přečtou.

Celá aplikace je jeden soubor `index.html`. Žádný server, žádná databáze, žádná registrace.

---

## Jak se to používá

### Ostatní — jen čtení

Otevřou odkaz a vidí u každé akce, kolik komu vychází. Po kliknutí na jméno se rozbalí
rozpad částky krok po kroku, u každého kroku je **i** s vysvětlením a vzorcem. Nic
nemohou změnit — v tomhle zobrazení nejsou žádná editovatelná pole.

Po otevření se ukáže naposledy vytvořená akce. Přepínač akcí je přilepený nahoře; když
se přestane vejít na řádek, sbalí se na tlačítko **⌄ Starší akce (N)**.

Zobrazuje se jen to, co má obsah: kdo na akci nebyl, se v seznamu neobjeví, a nulové
náklady se vynechají i s celou sekcí. Na mobilu se místo široké tabulky použije seznam
funkcí s počty, takže se nikde nic neposouvá do strany.

### Ty — editace

Přidej `?edit` na konec adresy:

```
https://vystoupeni.vercel.app/?edit
```

Můžeš měnit výdělek a náklady, zaškrtávat, kdo se účastnil a kdo koho vezl, přidávat
a mazat lidi i akce a duplikovat akci jako šablonu. Čísla se přepočítají okamžitě.

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
2. **Interní řidič** dostane zpátky své náklady na cestu; při více řidičích se náhrada
   dělí mezi ně. Složí se na ni všichni, kdo autem jeli — **včetně samotného řidiče**,
   ten se do dělení započítá automaticky a nemusí se zaškrtávat jako pasažér.
3. **Pasažéři externího řidiče** se rovným dílem složí na cenu cizího dopravce.
4. **Individuální externí náklad** platí každý označený sám za sebe.
5. **Hromadný externí náklad** si označení rozdělí rovným dílem.
6. **Interní hromadný náklad** se strhne označeným plátcům a v plné výši připadne
   označenému příjemci — typicky když někdo zaplatil něco za skupinu ze svého.
7. **Organizátor** dostane navíc procento z výdělku (výchozí 5 %). Tahle částka se rovným
   dílem strhne ostatním účastníkům, takže celkový součet zůstává stejný.

Body 3 až 5 jsou peníze, které **odcházejí ven ze skupiny** — o ně je k rozdělení méně.
Body 2, 6 a 7 se jen **přesouvají mezi lidmi** a celkovou částku nemění. Sekce
*Kam peníze jdou* to odděluje a sekce *Kontrola* ověřuje, že „výdělek − náklady ven"
přesně sedí na součet všech výplat.

Když se v označení objeví nesmysl — třeba se někdo veze naším autem, ale nikdo není
označený jako řidič, nebo u interního nákladu chybí příjemce — aplikace na to upozorní
a rozdíl v kontrole to ukáže.

Všechny částky jsou počítané a zobrazované přesně na haléře.

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
