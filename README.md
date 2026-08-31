# Michalina Bielawska — michalinabielawska.pl

Strona osobista / landing produktowy zbudowany w React + Vite. Zawiera pełną stronę
główną (hero, statystyki, produkty, FAQ), Regulamin, Politykę prywatności oraz
interaktywny test „Czy Twoja marka jest AI Ready?".

## Struktura projektu

```
.
├── index.html          ← punkt wejścia HTML (meta tagi, miejsce na Meta Pixel)
├── package.json        ← zależności: react, react-dom, lucide-react
├── vite.config.js       ← konfiguracja Vite
├── netlify.toml         ← konfiguracja builda dla Netlify
├── src/
│   ├── main.jsx          ← montuje aplikację w #root
│   └── App.jsx            ← cała strona (komponenty, dane produktów, style)
└── .gitignore
```

Design, treści, układ, funkcjonalność i logika testu są dokładnie takie same jak w
przekazanym wcześniej pliku JSX — nic nie zostało zmienione, tylko przeniesione do
struktury gotowej pod deploy.

## Uruchomienie lokalnie

Wymaga [Node.js](https://nodejs.org) w wersji 18 lub nowszej.

```bash
npm install
npm run dev
```

Strona wystartuje pod adresem, który pokaże terminal (zwykle `http://localhost:5173`).

## Build produkcyjny

```bash
npm run build
```

Gotowe pliki trafiają do folderu `dist/`. Podgląd builda lokalnie:

```bash
npm run preview
```

## Wdrożenie na Netlify

### Opcja A — przez GitHub (zalecana, z automatycznym redeployem)

1. Załóż repozytorium na GitHub i wypchnij do niego zawartość tego folderu:
   ```bash
   git init
   git add .
   git commit -m "Pierwsza wersja strony"
   git branch -M main
   git remote add origin <adres_twojego_repo>
   git push -u origin main
   ```
2. Wejdź na [app.netlify.com](https://app.netlify.com) → **Add new site** → **Import an existing project**.
3. Wybierz GitHub i swoje repozytorium.
4. Netlify sam wykryje ustawienia z `netlify.toml` (build command: `npm run build`, publish directory: `dist`) — nie musisz nic zmieniać.
5. Kliknij **Deploy site**.

Od tej pory każdy `git push` na branch `main` automatycznie aktualizuje stronę na Netlify.

### Opcja B — bez GitHuba, przez Netlify CLI

```bash
npm install -g netlify-cli
npm run build
netlify deploy --prod --dir=dist
```

### Opcja C — najszybsza, „przeciągnij i upuść"

1. Uruchom `npm run build` lokalnie.
2. Wejdź na [app.netlify.com/drop](https://app.netlify.com/drop).
3. Przeciągnij folder `dist/` na stronę.

Ta opcja nie daje automatycznego redeployu przy kolejnych zmianach — do tego potrzebna
jest Opcja A lub B.

## Domena własna (michalinabielawska.pl)

Po wdrożeniu: w panelu Netlify → **Domain settings** → **Add a domain**, wpisz
`michalinabielawska.pl` i podłącz zgodnie ze wskazówkami Netlify (zmiana rekordów DNS
u dostawcy domeny — w Twoim przypadku home.pl).

## Rzeczy do uzupełnienia przed publikacją

Poszukaj w kodzie frazy `[DO UZUPEŁNIENIA]` — to miejsca, które celowo zostały opisane
jako otwarte zamiast wymyślone:

- **Meta Pixel** — bazowy skrypt z Twoim Pixel ID wklej do `index.html` w sekcji
  `<head>` (dokładne miejsce i gotowy fragment kodu są tam opisane w komentarzu).
  Po jego dodaniu automatycznie zaczną działać zdarzenia `AIReadyQuizStart`,
  `AIReadyQuizComplete` i `AIReadyQuizProductClick` zaszyte w teście.
- **Regulamin / Polityka prywatności** (`src/App.jsx`, komponenty `TermsPage` i
  `PrivacyPage`) — kilka pozycji czeka na Twoje potwierdzenie (m.in. przekazywanie
  danych poza EOG przez ewentualne przyszłe narzędzia analityczne).
- **Zdjęcie w hero** — obecnie placeholder pod `/images/michalina-hero.jpg`; podmień
  na plik w `public/images/` i zaktualizuj ścieżkę w `src/App.jsx` (szukaj
  `hero__photo`).

## Uwaga o rozmiarze pliku

`src/App.jsx` zawiera osadzone bezpośrednio w kodzie (jako base64) wszystkie okładki
produktów oraz logo — stąd jego duży rozmiar. To celowe: gwarantuje, że obrazy zawsze
się wyświetlą, niezależnie od hostingu. Jeśli w przyszłości strona urośnie o kolejne
produkty, wartościową optymalizacją byłoby przeniesienie obrazów do `public/images/` i
odwoływanie się do nich przez zwykłe ścieżki — to zmniejszy rozmiar paczki i przyspieszy
ładowanie. To nie jest pilne, ale warto o tym pamiętać przy kolejnej większej
aktualizacji.
