<!--
  Denne fil er README'en til det OFFENTLIGE repo Sliebheinn/KommbineLink (kun MSI-udgivelser).
  Kopiér indholdet til README.md dér. Hovedrepoet er privat og indeholder ingen kundevendt README.
-->

# Kommbine_Link

Kommbine_Link er en lille Windows-app, der viser en popup nederst til højre på skærmen, når der sker noget, du skal reagere på, fx et indgående opkald. Popup'en viser kontakten, relevante sager og aftaler og op til fire knapper med genveje (åbn CRM, kopiér telefonnummer og lignende). Appen kører i systembakken ved uret og starter automatisk, når du logger ind på Windows.

## Download

**[Hent nyeste version (Kommbine_Link.msi)](https://github.com/Sliebheinn/KommbineLink/releases/latest/download/Kommbine_Link.msi)**

Tidligere versioner og ændringer findes under [Releases](https://github.com/Sliebheinn/KommbineLink/releases).

## Krav

- Windows 10 eller 11 (64-bit)
- Ingen administratorrettigheder nødvendige. Appen installeres kun for din egen Windows-bruger.
- .NET behøver ikke at være installeret. Det følger med.
- Internetadgang til din virksomheds Kommbine-server
- En licensnøgle og et bruger-id fra din administrator eller kontaktperson hos Kommbine

## Installation

1. Hent `Kommbine_Link.msi` fra linket ovenfor.
2. Dobbeltklik på filen. Hvis Windows viser en advarsel, se [Windows-advarsel ved første kørsel](#windows-advarsel-ved-første-kørsel) nedenfor.
3. Læs licensaftalen, og sæt flueben i "Jeg accepterer vilkårene".
4. Indtast **serveradresse** (er normalt forudfyldt), **licensnøgle** og **bruger-id**, som du har fået af din administrator, og tryk *Næste*.
5. Når installationen er færdig, ligger Kommbine_Link i systembakken (ikonet ved uret). Klik på pilen ved uret, hvis du ikke kan se det.

Appen starter fremover af sig selv, når du logger ind. Hvis du lukker en popup, kører appen videre i systembakken. Højreklik på ikonet for at åbne den seneste popup igen, åbne indstillinger, forbinde igen, slå notifikationer til eller fra eller afslutte appen.

## Windows-advarsel ved første kørsel

Installeren er endnu ikke digitalt signeret med et kodesigneringscertifikat. Windows viser derfor en advarsel, når du kører en nyudgivet fil, selvom filen er ægte. Det er forventet og betyder ikke, at filen er ødelagt. Sådan kommer du forbi advarslen:

**Hvis du henter filen i Edge eller Chrome**

1. Browseren kan skrive, at filen "ikke bruges ofte" eller "kan være farlig". Åbn menuen (⋯) ud for downloaden i overførselslisten, og vælg *Behold*. Vælg derefter *Vis mere* og *Behold alligevel*.
2. Åbn den hentede fil.

**Hvis Windows viser "Windows har beskyttet din pc" (SmartScreen)**

1. Klik på *Flere oplysninger*.
2. Klik på *Kør alligevel*.

**Hvis du slet ikke får knappen *Kør alligevel* frem**

1. Højreklik på `Kommbine_Link.msi` og vælg *Egenskaber*.
2. Sæt flueben i *Fjern blokering* nederst på fanen *Generelt*, og klik *OK*.
3. Kør filen igen.

Hent kun filen fra linket ovenfor. Hvis du er i tvivl om, at filen kommer fra Kommbine, så kontakt din administrator, før du kører den.

## Opdatering til en ny version

Hent den nyeste MSI, og kør den. Den nye version erstatter den gamle uden afinstallation. Dine indstillinger og din tilmelding bliver stående, og du skal ikke indtaste licensnøgle igen. Hvis appen kører, lukker installeren den og starter den igen bagefter.

## Afinstallation

Åbn *Indstillinger → Apps → Installerede apps*, find **Kommbine_Link**, og vælg *Afinstaller*.

På nogle Azure AD-tilsluttede computere dukker appen ikke op i listen. Åbn i så fald Kommandoprompt eller PowerShell som dig selv (ikke som administrator), og kør kommandoen nedenfor med stien til den MSI-fil, du installerede fra:

```text
msiexec /x C:\sti\til\Kommbine_Link.msi
```

## Til IT-administratorer

Installeren er en pr.-bruger-MSI og kan rulles ud centralt (Intune, GPO, logon-script) uden brugerinteraktion. Den skal køre i brugerens kontekst, ikke som system. Værdierne kan gives på kommandolinjen, så brugeren ikke skal indtaste dem:

```text
msiexec /i Kommbine_Link.msi BACKENDURL=https://overlay.dinvirksomhed.dk LICENSEKEY=<licensnøgle> USERID=<bruger-id> /qn
```

| Parameter | Betydning |
|---|---|
| `BACKENDURL` | Adressen på virksomhedens Kommbine-server (skal starte med `https://`) |
| `LICENSEKEY` | Virksomhedens licensnøgle. Den bruges kun ved første tilmelding og fjernes bagefter fra computeren. |
| `USERID` | Medarbejderens bruger-id, som aftalt med Kommbine (fx lokalnummer eller e-mail) |

- **Windows-advarslen** vises normalt ikke, når MSI'en rulles ud centralt (fx via Intune), da filen ikke hentes via en browser.
- **Installationsmappe:** `%LocalAppData%\Programs\Kommbine_Link`
- **Indstillinger:** `%LocalAppData%\CustomerOverlay\settings.json`
- **Logfiler:** `%LocalAppData%\CustomerOverlay\logs`, én fil pr. dag, som slettes efter 14 dage. Kontaktdata logges ikke.

## Hjælp

Hvis noget ikke virker, så send den seneste logfil fra mappen ovenfor, og skriv, hvilken version du bruger (se filens egenskaber → *Detaljer*), til din kontaktperson hos Kommbine.
