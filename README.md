# CANTUS

**Spievajme spolu.**

CANTUS je systém na správu a premietanie hudobnej časti liturgie. Texty piesní sa počas omše zobrazujú na TV alebo projektore a ovládajú sa z tabletu. Vznikol z aplikácie „Farský spevník“.

Beží na čistom Node.js, server nepotrebuje žiadne npm balíčky. Voliteľne sa dá zabaliť do desktopovej aplikácie (Electron).

## Rýchly štart

```bash
node server.js
```

Server beží na `http://localhost:3800` a je dostupný aj z ostatných zariadení v tej istej sieti (tablet, TV). Iný port nastavíte premennou `PORT`. Na Windowse stačí `SPUSTIT.bat`, na Linuxe `./spustit.sh`.

Desktopová verzia (Electron):

```bash
npm install
npm start          # spustenie
npm run dist       # zostavenie inštalátora pre Windows
```

## Stránky

| Adresa | Na čo slúži |
|---|---|
| `/home` | úvodný prehľad: rýchle vyhľadávanie, fronta, obľúbené, história |
| `/tablet` | ovládač počas omše: výber piesne a slohy, fronta, obľúbené |
| `/tv` | premietanie pre TV a projektor (celá obrazovka cez F11) |
| `/admin` | administrácia: piesne, vzhľad TV, obrázky, história, zabezpečenie (vyžaduje prihlásenie) |

Ovládač a TV sa synchronizujú v reálnom čase, bez obnovovania stránky.

## Administrácia a roly

Do administrácie sa vstupuje menom a heslom. Účty sa spravujú v sekcii **Zabezpečenie** (iba Administrátor).

| Rola | Čo smie |
|---|---|
| Administrátor | všetko vrátane účtov a záloh |
| Redaktor | piesne, obrázky a farebný motív |
| Kostolník | iba obrázky a farebný motív |

Pri prvom spustení vznikne účet `admin`. Ak ide o novú inštaláciu, predvolené heslo je `admin`, **po prvom prihlásení ho hneď zmeňte**. Oprávnenia vynucuje server, nielen rozhranie.

## Farebné motívy a vzhľad

Značková paleta je na jednom mieste v `public/cantus.css`: tmavomodrá `#101B35`, zlatá `#D4AF66`, svetlé pozadie `#F7F8FA`, biela `#FFFFFF`, sivá `#64748B`. Motív (napríklad CANTUS, Slávnostná, Cezročné obdobie, Pôst a Advent) sa volí v administrácii a platí pre celú aplikáciu. Logo a ikony sú v `public/brand/`.

## Štruktúra projektu

```
server.js               # HTTP server, API, synchronizácia (SSE)
vespery.js              # načítanie vešpier z breviar.sk
public/                 # stránky (home, tablet, tv, admin), cantus.css, glass.css, brand/
data/                   # dáta aplikácie (JSON): zbierky, fronta, motív, používatelia, obrázky
electron/               # desktopový obal (Electron)
docs/                   # audit, návrh pre viac farností, podrobný návod
zaloha_povodne_stranky/ # staré stránky (už sa nezapínajú)
```

## Dáta a bezpečnosť

- Všetko sa ukladá do súborov v `data/`. Pred nasadením a pri aktualizáciách si ho zálohujte (administrácia → Stiahnuť zálohu).
- `data/users.json` obsahuje účty a zahashované heslá. Nezverejňujte ho a nedávajte do verejného repozitára (je v `.gitignore`).
- Server je určený pre dôveryhodnú sieť (farská WiFi). Ovládač a TV sú bez prihlásenia, zmeny v administrácii vyžadujú prihlásenie a príslušnú rolu.
- Heslá sa ukladajú cez scrypt so soľou.

## Dokumentácia

- [`docs/AUDIT_A_PLAN.md`](docs/AUDIT_A_PLAN.md): audit pôvodnej aplikácie a plán rebrandingu
- [`docs/NAVRH_VIAC_FARNOSTI.md`](docs/NAVRH_VIAC_FARNOSTI.md): návrh pre viac farností
- [`docs/PODROBNY_NAVOD.md`](docs/PODROBNY_NAVOD.md): podrobný návod k ovládaču, fronte, skratkám a obrázkom
