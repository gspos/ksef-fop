# Changelog

## Zmiany w fork gspos/ksef-fop

### Obsługa niestandardowego szablonu XSL

**`InvoiceGenerationParams.java`** - dodano pole `customTemplatePath` umożliwiające powiązanie dowolnego szablonu XSL ze schematem faktury bez konieczności rozszerzania enuma `InvoiceSchema`.

**`PdfGenerator.java`** - `resolveXslTemplate()` sprawdza najpierw `customTemplatePath`, a dopiero potem domyślny szablon dla schematu.

### Szablon rozszerzony `ksef-invoice-ext.xsl`

**Zmiana include** (linia 84) - używa `invoice-rows-ext.xsl` zamiast `invoice-rows.xsl`.

**Podmiot3 - nowe role:**
- Rola 11 (`role.employee` - Pracownik)
- `RolaInna` + `OpisRoli` (`role.other` - Inny podmiot z opisem)
- Poprawka XPath: `crd:Rola != 5` → `not(crd:Rola = 5)` - aby uwzględnić podmioty bez elementu `Rola`

**Adnotacje - rozszerzenie sekcji:**
- **P_23** - procedura uproszczona trójstronna
- **Zwolnienie/P_19** - zwolnienie z VAT z podstawami (P_19A/P_19B/P_19C)
- **PMarzy** - procedura marży (turystyka, towary używane, dzieła sztuki, antyki)
- **NoweSrodkiTransportu/P_22** - wewnątrzwspólnotowa dostawa nowych środków transportu (data, marka, model, kolor, rejestracja, przebieg, motogodziny)
- **TP** - powiązania między nabywcą a dostawcą
- **FP** - faktura do paragonu (art. 109 ust. 3d)

### GTIN w `invoice-rows-ext.xsl`

Dodano wyświetlanie numeru GTIN jako drugiej linii pod nazwą towaru/usługi (P_7) - czcionka 6pt, kolor `#555555`. Obsłużone w:
- tabeli pozycji fakturowych (`crd:FaWiersz`)
- tabeli różnic korekty (`differencesTable`)

### Etykiety i18n

**`messages_pl.json`** i **`messages_en.json`** - dodano etykiety:

| Klucz | PL | EN |
|-------|----|----|
| `role.employee` | Pracownik | Employee |
| `role.other` | Inny podmiot | Other entity |
| `simplifiedTriangular` | Procedura uproszczona - Loss triangle | Simplified triangular procedure |
| `taxExemption` | Dostawa towarów/świadczenie usług zwolnionych z VAT | Supply of goods/services exempt from VAT |
| `taxExemption.basis` | Podstawa zwolnienia | Exemption basis |
| `newTransport.*` | 7 etykiet dot. nowych środków transportu | |
| `marginProcedure.*` | 5 etykiet dot. procedury marży | |
| `tp` | Powiązania między nabywcą a dostawcą | Related party transaction |
| `fp` | Faktura do paragonu (art. 109 ust. 3d) | Invoice to receipt (Art. 109(3d)) |

### Test

**`GeneratePdfTest.java`** - dodano test `generateFa3ExtInvoicePdfWithGtin` używający `26_1_Fa.xml` z szablonem `ksef-invoice-ext.xsl` przez `customTemplatePath`.
