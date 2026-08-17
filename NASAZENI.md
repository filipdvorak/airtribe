# Jak získat jeden trvalý odkaz

Cíl: **jedna adresa, kterou pošleš jednou a už ji nikdy neměníš.** Když do aplikace
přidáš novou akci, obsah na té adrese se změní sám — ostatní jen otevřou tentýž odkaz.

Klíč je nasadit web přes **GitHub**. Repozitář je zdroj pravdy: vyměníš v něm soubor
`index.html` a hosting to během chvilky sám zveřejní na stejné adrese.

> **Pozor na slepou uličku:** vercel.com/drop (přetažení složky do prohlížeče) vypadá
> lákavě, ale **každé přetažení založí nový projekt, tedy i novou adresu**. Na trvalý
> odkaz se nehodí. Stejně tak *Odkaz s daty* (`#d=...`) v editaci — ten je pokaždé jiný
> a slouží jen na rychlé jednorázové ukázky.

---

## Krok 1 — založ repozitář (jednorázově, ~3 minuty)

1. Přihlas se na <https://github.com> (účet zdarma).
2. Vpravo nahoře **+** → **New repository**.
3. **Repository name:** `vystoupeni` · **Public** · ostatní nech být → **Create repository**.
4. Na stránce prázdného repozitáře klikni **uploading an existing file**.
5. Přetáhni do okna soubory `index.html`, `vercel.json` a `README.md` → dole **Commit changes**.

## Krok 2 — zapni hosting (jednorázově, ~2 minuty)

Vyber si jednu z variant. Obě jsou zdarma a obě drží adresu napořád.

### A) Vercel — doporučeno

1. Jdi na <https://vercel.com/new> a přihlas se přes GitHub.
2. V seznamu najdi repozitář `vystoupeni` → **Import**.
3. Nic nenastavuj (je to statický web) → **Deploy**.
4. Za pár vteřin dostaneš adresu, např. `https://vystoupeni.vercel.app`.
   V *Settings → Domains* si ji můžeš přejmenovat, ať se dobře posílá.

### B) GitHub Pages — bez druhé služby

1. V repozitáři **Settings** → vlevo **Pages**.
2. **Source:** *Deploy from a branch* → **Branch:** `main` a `/ (root)` → **Save**.
3. Za minutu naběhne adresa `https://<tvoje-jmeno>.github.io/vystoupeni/`.

**Tuhle adresu pošli lidem. Víc už s odkazem nikdy nic dělat nemusíš.**

---

## Krok 3 — zapni ukládání přímo z prohlížeče (jednorázově, ~3 minuty)

Díky tomu už nikdy nebudeš nic stahovat ani přetahovat. Otevřeš odkaz, naklikáš změny,
dáš **Uložit na web** a hotovo.

1. Na GitHubu jdi do **Settings → Developer settings → Personal access tokens →
   Fine-grained tokens** → **Generate new token**.
2. **Repository access:** *Only select repositories* → vyber `vystoupeni`.
3. **Permissions → Repository permissions:** nastav **Contents** na **Read and write**.
   Nic dalšího token nepotřebuje.
4. Zvol expiraci, vygeneruj token a zkopíruj ho — GitHub ho ukáže jen jednou.
5. Otevři svůj web s `?edit`, v sekci **Uložení na web** vyplň repozitář (`jmeno/vystoupeni`),
   větev `main`, cestu `index.html` a vlož token → **Připojit a ověřit**.

Token se uloží **jen do tvého prohlížeče** (localStorage). Nikdy se nezapisuje do stránky
ani do sdíleného odkazu, takže se k němu nikdo přes web nedostane. Na cizím počítači ho
nepoužívej a po práci dej **Odpojit**. Kdyby přece jen unikl, na GitHubu ho jedním
kliknutím zneplatníš — a protože je omezený na jeden repozitář a jen na obsah souborů,
víc než tenhle web s ním nikdo neudělá.

## Krok 4 — běžná práce

1. Otevři `https://vystoupeni.vercel.app/?edit`.
2. Naklikej změny — nová akce, částky, zaškrtnutí. Čísla se přepočítávají hned.
3. Klikni **💾 Uložit na web**.
4. Za zhruba 20 vteřin vidí všichni na původním odkazu nová čísla.

Tlačítko **Otestovat spojení** kdykoli ověří, že token pořád platí. Když ti expiruje,
vygeneruješ nový a znovu ho vložíš — nic jiného se nemění.

Kdyby GitHub nefungoval nebo token nechtěl použít, zůstává i původní cesta:
**⬇ Stáhnout aktualizovaný web** a soubor nahrát ručně přes *Add file → Upload files*.

## Když chceš upravit kód aplikace

Data žijí **uvnitř** `index.html`, takže nahrání nové verze kódu přepíše i je. Aby ses o ně
nepřipravil:

1. Otevři web s `?edit` → sekce **Přenos dat do nové verze aplikace** → **Zkopírovat data**
   a text si někam ulož.
2. Nahraj na GitHub nový `index.html`.
3. Znovu otevři `?edit` → **Vložit data z jiné verze** → vlož text → **Načíst data**.
4. Zkontroluj čísla a klikni **💾 Uložit na web**.

Pokud novou verzi kódu připravuje někdo jiný (třeba Claude), jde to i obráceně: pošli mu
aktuální `index.html` z repozitáře a dostaneš zpátky verzi, která už tvoje data obsahuje —
pak jen nahraješ soubor a nic nepřenášíš.

## Časté otázky

**Uvidí ostatní editaci?** `?edit` musí do adresy někdo napsat ručně — a i kdyby to udělal,
uvidí jen prázdný formulář pro připojení k GitHubu. Bez tvého tokenu nemá jak cokoli uložit;
jeho změny zůstanou v jeho prohlížeči a po zavření stránky zmizí.

**Co když někdo uhodne, že je to GitHub repozitář?** Nic z toho neplyne. Zápis do repozitáře
vyžaduje token, který má jen tvůj prohlížeč. Veřejný je jen výsledný web.

**Neuvidí někdo starou verzi z cache?** Ne. `vercel.json` a hlavička v souboru říkají
prohlížeči, aby si stránku vždy ověřil. Na GitHub Pages může výjimečně chvilku trvat, než
se změna projeví — pomůže obnovení stránky.

**Je repozitář veřejný — nevadí to?** Data jsou jména a částky za vystoupení. Pokud je
nechceš mít veřejně, dej repozitář jako **Private**; Vercel i GitHub Pages (na placeném
tarifu) z něj umí nasazovat dál, samotný web zůstane veřejně dostupný přes odkaz.

**Chci hezčí adresu.** Ve Vercelu *Settings → Domains* můžeš přidat vlastní doménu, nebo
si stačí přejmenovat projekt — adresa `nazev.vercel.app` se řídí jeho jménem.
