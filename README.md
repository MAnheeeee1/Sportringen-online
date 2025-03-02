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
