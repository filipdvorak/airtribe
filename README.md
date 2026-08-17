# Rozdělování peněz z vystoupení

Webová verze původního Excelu. Jeden soubor `index.html`, žádný server, žádná databáze.
Kdo dostane odkaz, uvidí **jen čtení**. Editaci otevřeš pouze ty přidáním `?edit` do adresy.

---

## Nasazení — jeden trvalý odkaz

Podrobný postup krok za krokem je v **[NASAZENI.md](NASAZENI.md)**. Ve zkratce:

1. Nahraj `index.html`, `vercel.json` a `README.md` do nového repozitáře na GitHubu.
2. Naimportuj repozitář na <https://vercel.com/new> (nebo zapni *Settings → Pages*).
3. Vzniklou adresu pošli lidem — už ji nikdy neměníš.
4. Jednou připoj GitHub token v sekci *Uložení na web* (postup v NASAZENI.md).

Aktualizace pak vypadá takhle: otevřeš svůj odkaz s `?edit`, naklikáš změny a dáš
**💾 Uložit na web**. Aplikace zapíše nová data přímo do repozitáře a na stejné adrese
se do ~20 vteřin objeví nový obsah. Nic se nestahuje ani nepřetahuje.

> `vercel.com/drop` (přetažení složky) se pro tenhle účel nehodí — každé přetažení
> vytvoří nový projekt, tedy i novou adresu.

## Jak to používat

### Ostatní (jen čtení)

Otevřou odkaz a vidí přehled akcí, kolik komu vychází a rozpad každé částky.
Nemají jak cokoli změnit — v běžném zobrazení nejsou žádná editovatelná pole.

### Ty (editace)

Na konec adresy přidej `?edit`:

```
https://vystoupeni.vercel.app/?edit
```

Tam můžeš:

- měnit výdělek a všechny náklady,
- zaškrtávat, kdo se účastnil, kdo řídil, kdo se s kým vezl,
- přidávat a mazat lidi i akce, duplikovat akci jako šablonu.

Změny se nejdřív jen počítají v tvém prohlížeči. Zveřejníš je jedním ze dvou způsobů:

1. **💾 Uložit na web** — hlavní způsob. Zapíše data přímo do repozitáře přes GitHub API,
   hosting je sám vyzvedne. Vyžaduje jednorázové připojení tokenu (viz NASAZENI.md).
   Token žije jen v tvém prohlížeči, nikdy se nedostane do stránky ani do odkazu.
2. **⬇ Stáhnout aktualizovaný web** — záložní cesta. Stáhne nový `index.html`, který
   nahraješ ručně (nebo `git add . && git commit -m "..." && git push`).
3. **🔗 Kopírovat odkaz s daty** — data zakódovaná v odkazu (`#d=...`). Pokaždé jiný odkaz,
   takže jen na rychlé "podívej, takhle by to vyšlo".

---

## Jak se počítá

Pro každou akci, v tomto pořadí:

1. **Základ** = výdělek ÷ počet účastníků.
2. **Interní řidič** dostane zpět své náklady (při více řidičích se náhrada dělí mezi ně).
   Složí se na ně **všichni, kdo autem jeli — včetně samotného řidiče**, ten se do dělení
   započítá automaticky a nemusí se zaškrtávat jako pasažér. Peníze zůstávají uvnitř
   skupiny, jen se přesouvají.
3. **Pasažéři externího řidiče** se rovným dílem složí na jeho cenu — ta odchází ven.
4. **Individuální externí náklad** platí každý označený sám za sebe.
5. **Hromadný externí náklad** se dělí mezi označené — odchází ven.
6. **Interní hromadný náklad** *(nové)* — zadaná částka se rovným dílem strhne označeným
   **plátcům** a v plné výši připadne označenému **příjemci**. Typicky když někdo něco
   zaplatil za skupinu ze svého. Peníze zůstávají uvnitř skupiny.
7. **Organizátor** dostane navíc 5 % z výdělku (podíl je nastavitelný); tahle částka se
   rovným dílem strhne ostatním účastníkům, takže součet zůstává stejný.

**Vysvětlivky jsou přímo u funkcí**, ne v samostatném oddílu — klikni na název sloupce
v tabulce *Přehled zaškrtnutí*, na **i** u kroku v rozpadu částky nebo u položky v sekci
*Kam peníze jdou* a rozbalí se popis i vzorec. Vše se rozbaluje plynule.

Sekce **Kam peníze jdou** odděluje náklady odcházející ven od interních přesunů a sekce
**Kontrola** ověřuje, že „výdělek − náklady ven“ přesně sedí na součet všech výplat.

### Co je oproti Excelu opravené

- Vzorce v sešitu měly na několika místech nekonzistentní rozsahy (`F2:F7` vs. `F2:F8`,
  kontrolní součet `T2:T5` místo všech řádků). Tady se vždy počítá se všemi lidmi.
- Jmenovatele (počty pasažérů, organizátorů) se počítají **jen mezi účastníky akce**.
  V Excelu mohl někdo, kdo na akci nebyl, ale měl zaškrtnuté políčko, rozředit podíl
  a peníze by se „ztratily“. Pokud takový stav nastane, aplikace na to v editaci upozorní.
- Náklady externího řidiče se do celkových nákladů započítají jen tehdy, když se
  opravdu někdo vezl.
- Aplikace hlídá i to, že u interních přesunů existují obě strany — když jsou označeni
  plátci, ale chybí příjemce (nebo naopak), upozorní na to a kontrolní součet to ukáže.

---

## Struktura

```
index.html   celá aplikace — HTML, CSS, JS i data v jednom souboru
vercel.json  hlavičky, aby prohlížeč vždy načetl aktuální verzi
NASAZENI.md  postup, jak získat a udržovat jeden trvalý odkaz
README.md    tento popis
```

Data jsou uložená v `index.html` v bloku `<script id="data" type="application/json">`.
Když ti to bude pohodlnější, můžeš je upravit i ručně v textovém editoru.
