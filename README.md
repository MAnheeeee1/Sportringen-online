## **Projektstruktur och Konventioner**

Det här projektet använder en **komponentbaserad arkitektur** för att säkerställa modularitet, skalbarhet och underhållbarhet. Nedan finns riktlinjer för filorganisation, namngivningskonventioner och hur man integrerar komponenter i `index.html`.

---

### **Filstruktur**

```
/project
├── index.html                  # Huvud HTML-fil
├── components/                 # Återanvändbara UI-komponenter
│   ├── header.html             # Header-komponent
├── styles/                     # Komponent-specifik CSS
│   ├── header.css              # Stilar för header-komponenten
├── scripts/                    # Komponent-specifik JavaScript
│   ├── header.js               # Script för header-komponenten
├── images/                     # Bilder och tillgångar
└── README.md                   # Projektdokumentation
```

---

### **Namngivningskonventioner**

1. **HTML-filer**:

   - Använd gemener och bindestreck för filnamn (t.ex., `header.html`, `hero-section.html`).
   - Placera alla HTML-filer för komponenter i mappen `components/`.

2. **CSS-filer**:

   - Använd samma namn som motsvarande HTML-fil (t.ex., `header.html` → `header.css`).
   - Placera alla CSS-filer för komponenter i mappen `styles/`.
   - Använd **BEM (Block-Element-Modifier)** för klassnamn för att säkerställa begränsade och återanvändbara stilar:
     Exempel:
     ```css
     .header {
       /* Block */
     }
     .header__logo {
       /* Element */
     }
     .header__button--primary {
       /* Modifier */
     }
     ```

3. **JavaScript-filer**:

   - Använd samma namn som motsvarande HTML-fil (t.ex., `header.html` → `header.js`).
   - Placera alla JavaScript-filer för komponenter i mappen `scripts/`.

4. **Bilder/Tillgångar**:
   - Placera alla bilder och tillgångar i mappen `images/`.
   - Använd beskrivande namn (t.ex., `company-logo.png`, `hero-background.jpg`).

---

### **Hur man integrerar en komponent i `index.html`**

Följ dessa steg för att dynamiskt ladda en komponent i `index.html`:

1. **Skapa komponenten**:

   - Lägg till komponentens HTML-, CSS- och JavaScript-filer i respektive mappar (t.ex., `components/header.html`, `styles/header.css`, `scripts/header.js`).

2. **Länka CSS**:

   - Lägg till en `<link>`-tagg i `<head>` av `index.html` för att ladda komponentens CSS-fil:
     ```html
     <link rel="stylesheet" href="styles/header.css" />
     ```

3. **Infoga HTML**:

   - Använd JavaScripts `fetch` API för att ladda komponentens HTML i en platshållare `<div>`:

     ```html
     <div id="header"></div>

     <script>
       fetch("components/header.html")
         .then((response) => response.text())
         .then((data) => {
           document.getElementById("header").innerHTML = data;
         });
     </script>
     ```

4. **Ladda JavaScript**:
   - Ladda komponentens JavaScript-fil dynamiskt efter att HTML har infogats:
     ```javascript
     const script = document.createElement("script");
     script.src = "scripts/header.js";
     document.body.appendChild(script);
     ```

---

### **Exempel: Integrera Header-komponenten**

#### `index.html`

```html
<!DOCTYPE html>
<html lang="sv">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Landningssida</title>
    <link rel="stylesheet" href="styles/header.css" />
  </head>
  <body>
    <!-- Header-komponent -->
    <div id="header"></div>

    <script>
      // Ladda header-komponenten
      fetch("components/header.html")
        .then((response) => response.text())
        .then((data) => {
          document.getElementById("header").innerHTML = data;

          // Ladda header.js-scriptet
          const script = document.createElement("script");
          script.src = "scripts/header.js";
          document.body.appendChild(script);
        });
    </script>
  </body>
</html>
```

---

### **Regler för CSS och JavaScript**

1. **CSS**:

   - Använd **begränsade klassnamn** (t.ex., `.header`, `.header__button`) för att undvika konflikter.
   - Undvik globala stilar om det inte är nödvändigt (t.ex., reset.css eller global typografi).

2. **JavaScript**:

   - Begränsa dina selektorer till komponentens rotelement:
     ```javascript
     const header = document.querySelector(".header");
     const button = header.querySelector(".header__button");
     ```
   - Undvik globala variabler genom att använda **IIFE** eller **ES6-moduler**.

3. **Testning**:
   - Testa varje komponent isolerat innan du integrerar den i `index.html`.
   - Se till att det inte finns några konflikter mellan komponenter.

Här är en kort beskrivning av grundläggande Git-kommandon för att **lägga till**, **puscha**, och **merga**, samt hur grenar kan användas i projektet. Du kan inkludera detta i din `README.md`-fil.

---

## **Grundläggande Git-kommandon**

### **Vanliga Kommandon**

1. **Lägg till ändringar**:

   - Lägg till alla ändringar
     ```bash
     git add .
     ```
   - Lägg till en specifik fil:
     ```bash
     git add filnamn
     ```

2. **Spara ändringar (Commit)**:

   - Skapa en commit med ett meddelande:
     ```bash
     git commit -m "Beskriv dina ändringar här"
     ```

3. **Pusha till remote repository**:

   - Skicka dina commits till remote repository:
     ```bash
     git push origin grennamn
     ```

4. **Hämta senaste ändringar**:

   - Hämta ändringar från remote repository:
     ```bash
     git pull origin grennamn
     ```

5. **Skapa en ny gren**:

   - Skapa och byt till en ny gren:
     ```bash
     git checkout -b ny-gren
     ```

6. **Växla mellan grenar**:

   - Bytt till en befintlig gren:
     ```bash
     git checkout grennamn
     ```

7. **Merga grenar**:
   - Merga en gren till din nuvarande gren (t.ex., `main`):
     ```bash
     git checkout main
     git merge grennamn
     ```

---

### **Våra Grenar**

Vi använder följande grenstruktur för att hålla projektet organiserat:

1. **`main`**:

   - Huvudgrenen som innehåller den senaste stabila versionen av koden.
   - Endast färdiggranskad och testad kod mergas in här.

2. **`feature/*`**:
   - Grenar för nya funktioner (t.ex., `feature/header`, `feature/footer`).
   - Varje funktion utvecklas i en egen gren och mergas sedan in i `main`.

---

### **Exempel på Arbetsflöde**

1. Skapa en ny gren för en funktion:
   ```bash
   git checkout -b feature/header
   ```
2. Lägg till och committa ändringar:
   ```bash
   git add .
   git commit -m "Lade till header-komponent"
   ```
3. Pusha grenen till remote:
   ```bash
   git push origin feature/header
   ```
4. När funktionen är klar, merga den till `main`:
   ```bash
   git checkout main
   git pull origin main
   git merge feature/header
   git push origin main
   ```

### **Exempel på Arbetsflöde när du ändrar något**

# Gör ändringar i feature/header

git checkout feature/header
git add .
git commit -m "Uppdaterade header-HTML"

# Uppdatera feature/header med senaste ändringar från main

git checkout main
git pull origin main
git checkout feature/header
git merge main

# Lös eventuella konflikter och committa

git add .
git commit -m "Löst konflikter efter att ha uppdaterat från main"

# Merga feature/header till main

git checkout main
git merge --no-ff feature/header

# Pusha ändringarna till remote main

git push origin main

# Ta bort den lokala och remote feature-grenen (valfritt)

git branch -d feature/header
git push origin --delete feature/header

---

### **Tips**

- **Synka ofta**: Hämta ändringar från `main` regelbundet för att undvika konflikter.
- **Beskrivande commit-meddelanden**: Skriv tydliga och beskrivande meddelanden för varje commit.

---
