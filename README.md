# Spoločný nákup 🛒

Rodinný nákupný zoznam ako webová appka. Funguje na iPhone aj na Androide, dá sa pridať na plochu a zmeny vidia všetci hneď.

**Súbory:**
- `index.html` je celá appka (dizajn, animácie, logika)
- `config.js` je prepojenie s Firebase (vyplníš ho v kroku 1)
- `firestore.rules` sú pravidlá, kto môže čítať a zapisovať (krok 1)
- `manifest.webmanifest`, `sw.js` a `icons/` slúžia na pridanie na plochu a fungovanie bez signálu

Kým nevyplníš `config.js`, appka beží v **demo režime**: všetko funguje, ale zoznam sa ukladá len v jednom telefóne.

---

## Krok 1: Firebase (databáza), asi 10 minút

1. Choď na **https://console.firebase.google.com** a prihlás sa Google účtom.
2. **Create a project** (Vytvoriť projekt), názov napr. `rodinny-nakup`. Google Analytics môžeš vypnúť, netreba ich.
3. **Zapni anonymné prihlásenie:**
   v ľavom menu **Build → Authentication → Get started → Sign-in method → Anonymous → Enable → Save**.
   (Appka sa tak prihlási sama a nikto nemusí zadávať heslo.)
4. **Vytvor databázu:**
   **Build → Firestore Database → Create database**.
   - Lokalita: **eur3 (europe-west)** alebo `europe-central2` (Varšava)
   - Režim: **Start in production mode**
5. **Nastav pravidlá:**
   vo Firestore otvor kartu **Rules**, zmaž všetko, čo tam je, vlož celý obsah súboru `firestore.rules` a klikni **Publish**.
6. **Zaregistruj webovú appku:**
   ozubené koliesko vľavo hore → **Project settings** → dole **Your apps** → ikona **`</>`** (Web).
   Názov napr. `nakup`, **Firebase Hosting nezaškrtávaj** → **Register app**.
7. Zobrazí sa kód s `const firebaseConfig = { ... }`. Skopíruj hodnoty `apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId` do súboru **`config.js`** namiesto prázdnych úvodzoviek.

   Výsledok vyzerá takto (tvoje hodnoty budú iné):
   ```js
   window.FIREBASE_CONFIG = {
     apiKey: "AIzaSyB...",
     authDomain: "rodinny-nakup.firebaseapp.com",
     projectId: "rodinny-nakup",
     storageBucket: "rodinny-nakup.firebasestorage.app",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abc123"
   };
   ```
   Tieto kľúče **nie sú tajné**, môžu byť verejne na GitHube. Zoznam chránia pravidlá z kroku 5.

Bezplatný plán (Spark) stačí s veľkou rezervou: 50 000 čítaní a 20 000 zápisov denne. Rodina minie zlomok.

---

## Krok 2: GitHub Pages (zverejnenie), asi 5 minút

Rovnako ako pri Impostorovi:

1. Na **github.com** vytvor nový repozitár, napr. `nakup`, nastav **Public**.
2. **Add file → Upload files** a pretiahni tam **všetky súbory aj priečinok `icons`** (nie samotný priečinok `nakup`, ale jeho obsah). Potom **Commit changes**.
3. **Settings → Pages → Branch: `main` / `(root)` → Save**.
4. Po minúte bude appka na adrese `https://TVOJE-MENO.github.io/nakup/`.

---

## Krok 3: Vytvor zoznam a pozvi rodinu

1. Otvor adresu v telefóne, napíš svoje meno a ťukni na **Vytvoriť nový zoznam**.
2. Ťukni na **Pozvať rodinu** a vyber WhatsApp → rodinnú skupinu.
   Odkaz vyzerá ako `https://…/nakup/#k7x9Q2…`. Časť za `#` je tajný kód zoznamu.
3. Každý ťukne na odkaz, napíše svoje meno a je pripojený.
4. **Pridanie na plochu:**
   - **iPhone:** Zdieľať → **Pridať na plochu** (v Safari aj v Chrome)
   - **Android:** Chrome → ⋮ → **Pridať na plochu** / **Inštalovať aplikáciu**

   Na iPhone má appka z plochy vlastné úložisko, takže sa ťa pri prvom otvorení ešte raz spýta na meno. Keby sa namiesto zoznamu ukázala úvodná obrazovka, ťukni na „Mám odkaz od rodiny“ a vlož odkaz z WhatsAppu.

---

## Ako sa to používa

- **Pridať:** napíš dole a stlač Enter. Funguje aj `2 mlieka` alebo `rožky 6` (množstvo sa doplní samo).
- **Kategória** sa odhadne podľa názvu. Ak je zlá, ťukni na inú a appka si to zapamätá.
- **Návrhy:** pri písaní appka ponúka, čo ste už kupovali. Ťuknutím položku hneď pridáš. **Podržaním** ju odstrániš z návrhov.
- **Odškrtnúť:** ťukni na krúžok. Položka sa presunie do „Kúpené“.
- **Upraviť alebo vymazať:** potiahni položku **doľava** (Upraviť / Vymazať), potiahni ju úplne doľava a rovno sa vymaže, alebo na ňu ťukni či ju podrž a otvorí sa úprava (názov, množstvo, kategória).
- **Vymazať kúpené:** tlačidlo pri sekcii „Kúpené“. Ak sa pomýliš, máš 5 sekúnd na „Späť“.
- Keď niekto iný niečo pridá, hore sa ukáže bublina.
- **Bez signálu** appka funguje ďalej a zmeny sa odošlú, keď sa telefón pripojí.

---

## Úpravy

- Farby sú na začiatku `index.html` v `:root` (`--sky`, `--pink`, `--white`).
- Kategórie a slová na ich odhad sú v `index.html` (`CATS` a `GUESS`).
- **Po každej úprave** zvýš v `sw.js` číslo `nakup-v1` napríklad na `nakup-v2`, nech si telefóny stiahnu novú verziu.

## Keď niečo nejde

| Problém | Riešenie |
|---|---|
| Hore svieti „Demo“ | `config.js` nie je vyplnený alebo sa ešte nenahral na GitHub |
| Hore svieti „Nepripojené“ | Skontroluj, či je zapnuté **Anonymous** prihlásenie (krok 1.3) a či sú pravidlá publikované (krok 1.5) |
| Zmeny nevidia ostatní | Každý musí mať **ten istý odkaz** (rovnaký kód za `#`) |
| Na telefóne je stará verzia | Zvýš číslo v `sw.js` a appku dvakrát otvor a zavri |
