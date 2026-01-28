# PULSARIO Stripe Integration Guide
## Consulting Booking Payment Flow

---

## 🎯 Übersicht

Diese Website integriert **Stripe Hosted Checkout** für One-Click-Booking einer Consulting-Session. 

**Warum dieser Ansatz?**
- ✅ **PCI-konform** – keine Kreditkartendaten auf der Website
- ✅ **Rechtssicher** – vollständig DSGVO-konform
- ✅ **Wartungsarm** – Stripe handhabt alles
- ✅ **Schnell live** – minimal Code-Overhead
- ✅ **Zukunftssicher** – einfach erweiterbar

---

## 🔧 Einrichtung in 3 Schritten

### **Schritt 1: Stripe Account erstellen**
1. Gehe zu [stripe.com](https://stripe.com)
2. Registriere dich kostenlos
3. Verifiziere deine Bank

### **Schritt 2: Payment Link erstellen**
1. Im Stripe Dashboard: `Products` → `Payment Links`
2. Klicke `Create payment link`
3. Fülle folgendes aus:

```
Product Name: Consulting Session
Price: €49.00 (oder dein Preis)
Description: 30-Minute PULSARIO Consultation with Expert
```

4. Optional: Customize
   - Add cover image
   - Set success/cancel URLs
   - Enable tax calculation

5. **Kopiere den Link**, z.B.:
   ```
   https://buy.stripe.com/test_abc123def456
   ```

### **Schritt 3: Link in der Website eintragen**

Öffne `index.html` und suche diese Zeile:

```javascript
const STRIPE_PAYMENT_LINK = {
    consultation: 'https://buy.stripe.com/test_placeholder_link',
};
```

Ersetze `test_placeholder_link` mit deinem echten Link:

```javascript
const STRIPE_PAYMENT_LINK = {
    consultation: 'https://buy.stripe.com/test_abc123def456',
};
```

**Fertig!** 🚀

---

## 📍 Wo der Link verwendet wird

### **1. Header Navigation (Desktop)**
```html
<a href="https://buy.stripe.com/..." target="_blank" class="cta-button">
    BERATUNG BUCHEN
</a>
```

### **2. Mobile Menu**
```html
<a href="https://buy.stripe.com/..." target="_blank" 
   style="background: var(--gold); color: black; ...">
   📞 BERATUNG BUCHEN
</a>
```

### **3. Consulting Page (Dedicated)**
```html
<a href="https://buy.stripe.com/..." target="_blank" 
   class="btn-checkout" style="...">
    💳 KONSULTATION BUCHEN – €49
</a>
```

### **4. JavaScript Function**
```javascript
function bookConsultation() {
    window.open(STRIPE_PAYMENT_LINK.consultation, '_blank');
}
```

---

## 🎨 Design-Varianten

### **Button im Hero (Option A)**
```html
<a href="STRIPE_LINK" class="btn-checkout" style="width:auto; padding: 20px 60px;">
    📞 BERATUNG BUCHEN
</a>
```

### **Gold Button (Header)**
```html
<a href="STRIPE_LINK" class="cta-button">BERATUNG BUCHEN</a>
```

### **Card Format (Consulting Page)**
```html
<a href="STRIPE_LINK" class="btn-checkout" style="width: 100%;">
    💳 KONSULTATION BUCHEN – €49
</a>
```

---

## 📊 Zukunftserweiterungen

Dieses System ist **leicht erweiterbar** für mehrere Services:

```javascript
const STRIPE_PAYMENT_LINK = {
    consultation: 'https://buy.stripe.com/consult123',
    
    // Einfach neue Services hinzufügen:
    workshop: 'https://buy.stripe.com/workshop456',
    premium_package: 'https://buy.stripe.com/premium789',
    customization_service: 'https://buy.stripe.com/custom101',
};
```

### Dann kannst du die Funktion generisch gestalten:
```javascript
function bookService(serviceType) {
    window.open(STRIPE_PAYMENT_LINK[serviceType], '_blank');
}
```

---

## 🔐 Sicherheit & Compliance

### **Was Stripe für dich handhabt:**
✅ **PCI-DSS Compliance** – Level 1 zertifiziert  
✅ **SSL/TLS Encryption** – 256-bit  
✅ **Fraud Detection** – Real-time  
✅ **DSGVO-konform** – Zero data on your server  

### **Was du beachten musst:**
- ✓ Link über HTTPS verwenden
- ✓ Terms & Conditions verlinken
- ✓ Widerrufsrecht erwähnen (14 Tage)
- ✓ Datenschutzerklärung updaten

---

## 💳 Payment Methods (Automatisch)

Stripe aktiviert diese automatisch:
- Kreditkarten (Visa, Mastercard, Amex)
- PayPal
- Google Pay
- Apple Pay
- iDEAL (NL)
- SEPA (EU)
- Und viele mehr...

---

## 📈 Tracking & Analytics

### **Option 1: Google Analytics (einfach)**
```javascript
function bookConsultation() {
    // Track event
    gtag('event', 'start_consulting_checkout', {
        value: 49,
        currency: 'EUR'
    });
    
    // Redirect
    window.open(STRIPE_PAYMENT_LINK.consultation, '_blank');
}
```

### **Option 2: Stripe Webhooks (fortgeschritten)**
Wenn ein Payment erfolgreich ist, schickt Stripe einen Webhook:

```json
{
  "type": "checkout.session.completed",
  "data": {
    "object": {
      "customer_email": "kunde@example.com",
      "amount_total": 4900,
      "currency": "eur",
      "payment_status": "paid"
    }
  }
}
```

Du kannst dann z.B. automatisch eine Email senden, den Termin buchen, etc.

---

## 🎯 Best Practices

### **✅ Empfohlen:**
1. **Button prominent platzieren** – im Header, Hero, Consulting-Page
2. **Klare Preisstaffelung** – was kostet die Session?
3. **Was ist enthalten?** – Was der Kunde bekommt
4. **FAQ beantworten** – Stornierung, Rescheduling, etc.
5. **Follow-up automation** – Webhook für Bestätigungsmail

### **❌ Vermeiden:**
- Mehrfach-Redirect (Stripe → deine Seite → Stripe)
- Zu technische Erklärungen
- Versteckte Gebühren
- Fehlende Rückgabegarantie

---

## 🆘 Troubleshooting

### **Problem: Link funktioniert nicht**
→ Überprüfe, dass du den **echten Link** (nicht `test_placeholder`) einsetzt

### **Problem: Stripe-Seite zeigt 404**
→ Copy-Paste-Fehler? Kontrolliere den Link genau

### **Problem: Button funktioniert auf Mobile nicht**
→ Überprüfe `target="_blank"` – nicht alle Browser mögen das
→ Alternative: `target="_self"` zum Ersetzen des Tabs

### **Problem: Zahlungen landen nicht im Stripe-Konto**
→ Hast du Test-Modus ausgeschaltet?
→ Live-Keys verwenden statt Test-Keys?

---

## 📞 Support

**Stripe Support:** support@stripe.com  
**Pulsario Support:** pulsario.official@gmail.com

---

## 📋 Checklist vor Launch

- [ ] Stripe Payment Link erstellt
- [ ] Link in `index.html` eintragen
- [ ] Desktop-Buttons testen
- [ ] Mobile-Buttons testen
- [ ] Bezahlung abschließen (mit Testmode-Card: `4242 4242 4242 4242`)
- [ ] AGB aktualisiert (Widerruf, Konsultation)
- [ ] Datenschutzerklärung aktualisiert (Stripe-Daten)
- [ ] Success/Cancel URLs in Stripe gesetzt
- [ ] Webhook konfigurieren (optional, aber empfohlen)
- [ ] Live-Mode aktivieren
- [ ] Go live! 🚀

---

**Letzte Aktualisierung:** Januar 2026  
**Version:** 1.0 (Minimal Viable Product)
