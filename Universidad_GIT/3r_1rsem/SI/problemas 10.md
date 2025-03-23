https://www.funcas.es/odf/la-industria-financiera-ante-los-ciberataques/

### 1. **Injecció SQL (SQL Injection):**

- **Mesura:** Utilitzar consultes parametritzades i instruccions preparades.
- **Exemple:** Implementar `Prepared Statements` en lloc de concatenar cadenes SQL.

---

### 2. **Atacs de denegació de servei (DDoS):**

- **Mesura:** Implementar un firewall d’aplicació web (WAF) i serveis de mitigació DDoS.
- **Exemple:** Utilitzar solucions com Cloudflare o AWS Shield per filtrar i absorbir tràfic maliciós.

---

### 3. **Atacs de força bruta:**

- **Mesura:** Limitar intents d'inici de sessió i afegir autenticació multifactor (MFA).
- **Exemple:** Bloquejar l’IP després de 5 intents fallits i usar Google Authenticator per a MFA.

---

### 4. **Cross-Site Scripting (XSS):**

- **Mesura:** Validar i escapar totes les dades d’entrada i implementar Content Security Policy (CSP).
- **Exemple:** Utilitzar funcions com `htmlspecialchars()` en PHP i definir una política CSP estricta.

---

### 5. **Atacs Man-in-the-Middle (MitM):**

- **Mesura:** Implementar HTTPS/TLS a totes les comunicacions.
- **Exemple:** Utilitzar certificats SSL/TLS vàlids amb autorenovació i forçar HTTPS amb HSTS.