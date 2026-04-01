# Franklin — Osobní finanční pomocník
**Datum:** 2026-04-01

## Přehled

Franklin je komplexní osobní finanční pomocník — jeden soubor `index.html` s Firebase backendem. Určen pro správu příjmů, nákladů a investic s přístupem z libovolného zařízení přes Google přihlášení.

---

## Technologie

- **Frontend:** Jeden soubor `index.html` — CSS a JS inline, žádné build nástroje
- **Backend:** Firebase Firestore (cloud databáze) + Firebase Auth (Google přihlášení)
- **SDK:** Firebase přes CDN (importováno v `<script>` tagu)
- **Offline fallback:** Ne — aplikace vyžaduje připojení a přihlášení
- **Export:** Tlačítko "Export do JSON" v Nastavení
- **Jazyk:** Čeština
- **Motiv:** Světlý

---

## Struktura — záložky

### 1. Přehled (Dashboard)
Hlavní záložka, zobrazí se po přihlášení.

**Aktuální měsíc:**
- Příjmy tento měsíc
- Náklady tento měsíc
- Rozdíl (co zbývá)

**Aktuální rok:**
- Příjmy celkem
- Náklady celkem
- Rozpad nákladů po kategoriích (kolik jde do zábavy, auta, bytu...)
- Aktuální hodnota investičního portfolia celkem

---

### 2. Příjmy
- Přepínač roku (2024, 2025, 2026...) — nový rok přidáš jedním kliknutím
- Data z minulých let zůstanou jako historie
- Každý měsíc obsahuje položky: název zdroje, částka, volitelná poznámka
- Příklady zdrojů: Plat PČR, Příjem byt, Přivýdělek
- Přehled za vybraný rok: celkový příjem + průměr za měsíc

---

### 3. Náklady
- Editovatelné podkategorie (skupiny): přejmenovat, přidat, smazat
- Výchozí skupiny: Zábava, Internet & služby, Auto, Byt, Ostatní
- Každá položka: název, cena, frekvence (měsíční / roční), měna (Kč / EUR / USD), volitelně: splatnost (den v měsíci), platforma, poznámka
- Kurzy měn (EUR, USD → Kč) se nastaví v Nastavení
- Vše se automaticky přepočítá na Kč/měsíc a Kč/rok
- Nahoře vždy viditelný součet: X Kč/měsíc | Y Kč/rok (přes všechny skupiny)

---

### 4. Investice
- **Historie transakcí:** kdy, kolik, z jakého účtu, na jakou platformu
- **Aktuální hodnoty portfolia:** zůstatky na každé platformě (XTB, DEGIRO, Portu, Investown, Coinbase, spořicí účty)
- **Kalkulačka složeného úročení:** zadáš roční úrok (%) a časový horizont (roky) → výpočet budoucí hodnoty portfolia

---

### 5. Nastavení
- Přihlášení / odhlášení (Google účet)
- Kurzy měn (EUR a USD vůči Kč) — ruční zadání
- Export dat do JSON

---

## Datový model (Firestore)

Data jsou uložena pod `users/{uid}/` — každý uživatel vidí jen svá data.

```
users/{uid}/
  income/{year}/{month}/items[]     # položky příjmů
  costs/categories[]                # podkategorie nákladů
  costs/items[]                     # jednotlivé nákladové položky
  investments/transactions[]        # historie transakcí
  investments/platforms[]           # aktuální hodnoty na platformách
  settings/                         # kurzy měn, nastavení
```

---

## Firebase nastavení

1. Uživatel si vytvoří projekt na [console.firebase.google.com](https://console.firebase.google.com)
2. Povolí Firestore a Authentication (Google provider)
3. Zkopíruje Firebase config (apiKey, projectId atd.) do `index.html`
4. Nastaví Firestore Security Rules (data přístupná jen přihlášenému vlastníkovi)

---

## Co není součástí

- Žádost byt Praha (není potřeba)
- Tmavý motiv
- Mobilní aplikace
- Sdílení dat mezi uživateli
