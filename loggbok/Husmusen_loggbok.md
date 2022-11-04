# Projekt: Husmusen --- Utvecklingsloggbok
## 13 juni 2022 -- Vecka 1
Idag läste jag igenom projektbeskrivningen än en gång och strök under saker jag ansåg viktigare.

Jag påbörjade också ett enklare schema för vad som görs när, det lyder:
```
13 juni -- Start av projekt
    Planera, möte med RAÄ, skissa, osv.
17 juni -- Planering klar.

-------------------------------------------------------------------

20 juni -- Påbörja fas 1
    Skapa dokumentation, kravspecifikationer, mm.

29 juni -- Påbörja fas 2
    Skapa exempelimplementering av systemet. Ändra specifikationer
    vid behov, osv.
    
20 juli -- Påbörja fas 3
    Skapa ett system som kan kommunicera data till och från andra
    databassystem. T.ex. Kringla/K-samsök.
    
-------------------------------------------------------------------

17 augusti -- Skolan börjar; projektet slut?
```

Jag kan antagligen behöva ändra datumen lite under tidens gång...

Jag började också skissa och tänka på hur systemet kan vara utformat...



## 14 juni 2022
Jag planerade hur en databas skulle kunna vara strukturerad utefter projektets behov. Jag kom fram till följande specifikation:

```sql
CREATE TABLE husmusen_items (
    item_id       BINARY(16)     PRIMARY KEY,
    name          VARCHAR(128)   NOT NULL,
    description   TEXT           DEFAULT '',
    type          VARCHAR(128)   NOT NULL,
    added_at      TIMESTAMP      DEFAULT current_timestamp,
    updated_at    TIMESTAMP      DEFAULT current_timestamp,
    item_data     JSON           DEFAULT '{}' CHECK (JSON_VALID(item_data))
);
```

Först hade jag tänkt att varje `ItemType` skulle få sin egen tabell, men kom fram till att det då skulle finnas väldigt många tabeller -- att koppla ihop dem skulle vara väldigt jobbigt. Istället bestämde jag mig för att lagra föremålsspecifik data som en JSON-string.



## 15 juni 2022
Jag ritade upp diagram för hur jag tänker mig systemet och skissade lite på UI för sökningen.

![](https://docs.qwik.space/uploads/056d7992350bac0a392515c00.png)

| Sök | Resultat |
| :-: | :-: |
| ![](https://docs.qwik.space/uploads/056d7992350bac0a392515c01.png) | ![](https://docs.qwik.space/uploads/056d7992350bac0a392515c02.png) |

I eftermiddag ska jag ha ett möte med RAÄ (Riksantikvarieämbetet) om hur det skulle gå att samspela med deras system *Kringla*.

---

Mötet med RAÄ gick bra. Det var intressant och givande. Viktiga saker att ta med sig:
- All metadata ska vara licensierad under CC0.
- Varje objekt/sak i databasen måste ha ett unikt ID med unik perma-länk.



## 16 juni 2022
Idag arbetade jag med protokolldefinitionen av Husmusen. Jag skrev ut vilka olika typer av objekt som kan finnas i enlighet med [K-Samsök](https://www.raa.se/hitta-information/k-samsok/att-anvanda-k-samsok/objektstyper/). Jag har också utvecklat min modell på hur ett objekt i databasen ser ut:
```yaml
Name:            String   -- Namnet på ett objektet, t.ex. en boks titel
Description:     String   -- En längre beskrivning av objektet.
Type:            ItemType -- Typen av objekt. (Se nedan.)
ItemID:          ItemID   -- Objektets unika ID i databasen.
InventoryNumber: Integer  -- Inventarienummer för objektet.
AddedAt:         Date     -- När objektet blev tillagt i databasen.
UpdatedAt:       Date     -- När objektet senast blev uppdaterat.
ItemData:        JSON     -- Ett objekts data, t.ex. höjd, antal sidor, etc.
CustomData:      JSON     -- Här går det att lägga övrig data som inte är 
                             med i standarden.
HasThumbnail:    Boolean  -- Sant om objektet har en tumnagel (bild).
ItemFiles:       JSON     -- En JSON-array med filer relaterade till objektet.
```
Jag tror att modellen kommer behövas slipas på lite mer, men det är en bra början i alla fall.

Jag började också tänka på funktioner som borde finnas:
- Sök
- Hämta enskilt objekt

För administratör:
- Ladda upp nya objekt
- Redigera gamla objekt
- Ta bort objekt



## 17 juni 2022
Idag har jag fortsatt att skriva definitioner av protokollet. Jag har mestadels lagt in relevanta fält för olika `ItemTypes`, t.ex. är en bok och ett konstverk olika:

> `Book`
> ```yaml
> Authour:          String
> ISBN:             String
> Language:         String
> OriginalLanguage: String
> OriginalTitle:    String
> PageCount:        Integer
> Publisher:        String
> Title:            String
> Translator:       String
> Year:             Integer
> ```
> `ArtPiece`
> ```yaml
> Artist:   String   -- Konstnärens namn
> Material: String   -- Material i konstverket.
> Style:    String   -- Information om konststilen.
> Weight:   Integer  -- Konstverkets vikt i hela gram.
> Year:     Integer  -- Året som konstverket skapades.
> ```

Det finns fortfarande vissa detaljer kvar på definitionen av olika `ItemTypes`, bl.a. lägga in information om vad de olika fälten betyder och lägga in vissa fält till.



## 20 juni 2022 -- Vecka 2
Idag har jag påbörjat skrivandet av manualen och slutfört lite arbete på kravspefikationen. Det har inte blivit några *enorma framsteg*, men små steg som är viktiga att ta...




## 21 juni 2022
Jag fortsatte att skriva lite på manualen. Har nu skrivit om *Kravspecifikation* och *API-definitioner*. Jag har antagligen mycket mer kvar, men det är skönt att ha fått lite gjort. Jag tror att nästa steg blir att fortsätta lite på exemplena på kravspecifikation och API-definition. De har fortfarande lite kvar på sig...



## 22 juni 2022
Idag har jag skrivit färdigt alla definitioner på de olika `ItemTypes` som finns. (Tror jag i alla fall --- kanske kommer jag på något mer...) Jag tror att nästa steg blir att på något sätt skriva en API-definition för *paths*, *queries*, och så, alternativt skriva kravspecifikation på funktionerna som krävs för att skapa databassystemet.



## 23 juni 2022
Idag var jag på möte med min handledare och vi diskuterade projektet och kollade på Excel-filen med nuvarande data. Vi var eniga om mycket och planen såg vettig ut enligt båda sidor. Nästa steg blir, som sagt igår, att påbörja en API-definition och efter det faktiskt börja programmera.



## 24 juni 2022
**MIDSOMMAR.**
Jag jobbade bara lite på projektet. Jag påbörjade en *outline* av API:t när det kommer till hur man når olika data.



## 27 juni 2022 -- Vecka 3
Idag skrev jag ganska mycket på API-definitionen. Jag skapade också en GitHub-organisation där jag kan haålla koll på all kod och dokumentation när det väl blir relevant. ([github.com/mushuset](https://github.com/mushuset))

Jag slängde också snabbt ihop en temporär logga för systemet:

![](https://docs.qwik.space/uploads/5c881b718cf4d33fccbc6f400.png)

Jag hoppas kunna börja programmera på en exempelimplementering av *Husmusen* här framåt i veckan.



## 28 juni 2022
Idag har jag fortsatt på API-defintionen och kommit en ganska bra bit. Jag har också renskrivit vissa delar av protokolldefintionen. Jag har också föreberett lite repositories på GitHub tills jag behöver dem. Jag tror att jag börjar närma mig det stadium att jag kan börja skriva en exempelimplementering snart.



## 29 juni 2022
Jag tror att jag i princip börjar bli klar med hela protokoll- och API-definitionen. Jag tror jag har mestadels renskrivning och kanske några små ändringar framför mig. Snart borde jag också kunna börja skriva exempelkod. Jag hoppas bli *klar nog* med definitionerna denna vecka, så jag kan köra hårt på exemeplkod/-implementering och manualen från och med nästa vecka.



## 30 juni 2022
Jag fortsatte att skriva på protokollspecifkationen. Idag jag skrev jag till hur det ska gå till att markera objekt som utgångna, t.ex. om de går sönder eller försvinner. Jag skrev också om hur det ska gå till att faktiskt plocka bort ett objekt ur databasen. Jag renskrev också lite till. Jag tror att det kan bli lite planeringstid även nästa vecka. Dock hoppas jag på att kunna börja programmera i alla fall lite...



## 1 juli 2022
Idag har jag renskrivit lite till på protokollspecifikationen samt förnyat exemplet för att skapa en tabell för `Item` i MariaDB. Min  plan just nu är att börja programmera nästa vecka och göra förändringar till specifikationen varteftersom det behövs. Jag behöver också börja skriva lite mer på manualen...



## 4 juli 2022 -- Vecka 4
Idag har jag påbörjat att programmera ytte-pytte-lite och börjat göra lite research om hur en använder MariaDB. Jag har inte gjort några speciellt stora framsteg ännu, men hoppas göra det under veckan.



## 5 juli 2022
Idag har jag fortsatt att programmera. Jag har skrivit funktioner som kan (i större utsträckning) genera `Item`- och `File`-objekt. Jag har också gjort så att min kod kan koppla sig till databasmjukvaran MariaDB. (Och det funkar, men gör inget mer än att bara koppla upp sig mot MariaDB.) Jag har också gjort så att det skrivs lite error-meddelanden till konsolen och lite error-hantering.



## 6 juli 2022
Jag har blivit sjuk och öm i kroppen. Det var jobbigt att jobba och min koncentration är sådär, så gjorde lite bäbissteg idag...



## 7 juli 2022
Jag är fortfarande sjuk och inte helt klar i skallen. Jag gjorde dock början på själva webb-API:t. Nu funkar `/api/db_info`, `/api/db_info/version`, och `/api/db_info/versions`...



## 8 juli 2022
Fortfarande sjuk. Jag har gjort så att `DBInfo` läses från en fil istället för att vara programmerat rakt in i koden. Jag har också öppnat och kikat lite på manualen, men kom inte riktigt på något mer att skriva. Renskrev lite av det dock. Jag insåg också att mer rensrkivning av den krävs, men jag tror att det är bättre att vänta med det tills alla delar är klara så jag inte fastnar i en ändlös loop av att renskriva...



## 11 juli 2022 -- Vecka 5
Idag implementerade jag funktionaliteten som gör att en med hjälp av `Husmusen-Output-Format`-headern kan välja om en vill ha utdatan som JSON eller YAML. Den känns lite halv-stökig, men får jobbet gjort i alla fall. Jag har implementerat början på den funktionalitet som gör att en kan skicka en förfrågan av olika MIME types, t.ex. skicka en JSON- eller YAML-förfrågan och båda förstås av servern.
Jag gjorde också en mini-ändring i protokollet så även `Husmusen-Output-Format` ska vara en *MIME-type*.



## 12 juli 2022
Idag har jag skrivit lite grundläggande kod för att hantera alla *paths* som det ska gå att nå till servern. Jag har också gjort lite *refactors* av koden, bl.a. fixat lite med hur servern läser `DBInfo`. Jag har även börjat grubbla lite över hur autentisiering av bl.a. uppladdning av nya objekt ska ske. Jag tror jag vet ungefär hur det kan vara, det behöver bara implementeras... Just nu ska jag fokusera på att få fram en implementering av Husmusen, sedan efter det ska jag gå över protokollet igen och göra eventuella tillägg/förändringar -- jag vet redan att jag behöver lägga till autensiering...

Imorgon tror jag att jag vill börja göra grunden till gränssnittet. Tror det kan vara bra att ha den redan lite i början så saker är lätta att testa....



## 13 juli 2022
Idag har jag skrivit grunden till det grafiska användargränssnittet. Det är än så länge primitivt, men det estetiska är (temporärt) på hyllan då funktion just nu är lite viktigare. Nästa sak jag vill implementera är att det ska gå att skapa ett föremål i databassystemet. Det ska bara vara ett "generiskt föremål" utan alla speciell `ItemData` till att börja med, sen efter det ska jag lägga till `ItemData` och se till att den verifieras.




## 14 juli 2022
Idag har jag gjort så att API:t kan lägga till och hämta objekt från databasen. Dock finns det ingen autensiering ännu och vem som helst kan lägga till objekt till databasen. (Måste fixas!) Jag har också skannat koden efter säkerhetsbrister och funnit några som jag borde fixa. Jag tänker att det får bli imorgon som jag gör det, så jag avslutar veckan med säkerhetsfixar, och om jag har tid också ett sätt att se till att användare måste vara autensierad för att få lägga till nya objekt i databasen.

## 18 juli 2022 -- Vecka 6
Idag har jag implementerat lösenordsbyte och att admins kan skapa nya användarkonton.
Jag har också implementerat ett sätt att se till att användare är inloggade för att t.ex. skapa ett nytt objekt.
Till slut gjorde jag en liten refactor på delar av koden.


## 19 juli 2022
Idag har jag implementerat borttagning av användare -- det betyder att alla funktioner som behövs för användare nu är implementerade och jag borde kunna fokusera en större del på att lägga till/uppdatera/ta bort föremål och filer ur databasen. Jag har också gjort lite refactoring och skriver nu en server logg till en fil.



## 20 juli 2022
Idag har jag implementerat eett sätt för Administratörer att få tillgång till server loggen, så de kan inspektera den om något konstigt händer. Där ska det loggas t.ex. vem som tar vort vilket objekt, vem som skapar ett objekt, osv. Jag har också implementerat grunden till att söka efter föremål, men den är långt ifrån klar! Det är inte mycket kvar nu:
- Få klart sökning helt.
- Ett sätt att få tesauren.
- Ändra, markera som utgångna, och ta bort objekt.
- Filer.
- Fixa det grafiska användargränssnittet.
- Skriv klart manual och protokolldefinition.
- Och säkert något mer jag glömt.



## 21 juli 2022
Idag har jag implementerat ett sätt att få alla nyckelord som finns i databasen, både generellt för alla föremål, och för föremål av speciella typer. Att få tesauren är helt enkelt avklarat. Jag har också arbetat vidare lite på sökningen, men inte kommit jättelångt. Känner att jag måste göra lite mer research om bra/effektiva sätt att söka i MariaDB, särskilt när det kommer till fritextsökningen.



## 22 juli 2022
Idag har jag implementerat nyckelords-sökning. Det har tagit alldeles för lång tid och jag har gått igenom olika implementeringar, men till slut hittat något som verkar funka -- RegExes (Regular Expressions). Det som återstår när det kommer till sökning är fritext och att sortera -- sortera är ganska lätt tror jag, men fritext kan nog vara lite krångligare...



## 25 juli 2022 -- Vecka 7
Idag har jag implementerat borttagning och "markera som utgånget" av föremål i databasen. Jag har också gjort så att det går att välja fält som en sorterar sökresultaten efter. När det gäller föremål, så finns det bara en sak kvar att implementera för nuläget: redigering/ändring. Jag vill försöka implementera det imorgon. Efter det vill jag fixa så att allt med filerna fungerar. Förhoppningsvis får jag det påbörjat mot slutet av veckan. När det väl är klart så tänker jag fixa gränssnittet, rätta till protokolldefinitionen och se till att allt stämmer, samt skriva klart manualen. Det finns också fritext-sökning kvar att implementera, men jag får se när jag trycker in det.

På eftermiddagen hade jag möte med min handledare. Vi kom fram till:
1. Det måste finnas en *och-sökning* för nyckelord, inte bara *eller-sökning*.
2. Jag måste kolla upp lite mer om att "synka" fält mellan Husmusen och K-Samsök.
3. Nyckelorden ska definieras i en lista av en Admin, och bara de nyckelorden får användas när ett föremål skapas.



## 26 juli 2022
Idag har jag implementerat det nya nyckelord-systemet där en admin kan definiera alla nyckelorden. Än så länge verifieras det inte att endast dessa nyckelord finns med när en skapar ett nytt objekt i databasen, men det bör vara relativt lätt att fixa. Jag implemeterade inte ändring/redigering av objekt idag. Det behövde detta nya system med nyckelorden, annars hade jag behövt göra om den koden lite senare då jag skulle behöva kontrollera nyckelorden...



## 27 juli 2022
Idag har jag gjort så att nyckelorden verifieras när ett nytt föremål skapas. De sorteras också i alfabetisk ordning. Jag har också implementerat "OCH-sökning" för nyckelorden så det går att söka efter objekt som har både nyckelord A och B, innan var det A eller B, eller båda.



## 28 juli 2022
Idag har jag implementerat funktioner för `Files`: skapa, spara, hämta en, osv. Dock har jag inte kopplat ihop dem med webb-API:t ännu. Men det bör inte vara allt för svårt, nu när väl allt annat är fixat. Förutom uppdatering! Jag har inte skrivit funktioner för att uppdatera vare sig filer eller objekt! Jag har också typ fixat fritextsökning. Jag tycker att implementeringen av sökning generellt är väldigt *eeeehhhh*. Jag behöver kolla på den lite. Jag funderar på att köra en relativt stor refactor på all kod snart, kanske redan imon...



## 29 juli 2022
Idag har jag påbörjat lite refactoring av kodbasen, men för det mesta har jag sett till att skriva lite dokumentation om hur en skulle göra för att starta en instans av Husmusen med hjälp av exempelimplementeringen. Den borde täcka allt väsentligt. Jag måste fortsätta lite med att göra en refactor på koden -- gör det på måndag. Efter det tänker jag nog börja på det grafiska användargränssnittet. Att-göra-listan ser just nu ut såhär:

- [ ] Göra så att det går att uppdatera/ändra/redigera objekt/föremål.
- [ ] Göra så att det går att uppdatera/ändra/redigera filer.
- [ ] Lägga till de *routes* som gör så att API:t kan använda alla funktioner som finns för filer, så att en faktiskt kan lägga till filer, inte att det bara finns en oanvänd *spara-fil-funktion*...
- [ ] Skapa ett grafiskt gränssnitt med sökning och kontrollpanel.
- [ ] Skapa en standard för error-koder.
- [ ] Uppdatera protokoll-definitionen så alla nya funktioner finns med i den.
- [ ] Göra så att systemet kan användas med K-Samsök.
- [ ] Skriv klart manualen.

Jag har bara två veckor och två dagar kvar på sommarlovet. Jag vill få så mycket som möjligt klart innan dess, eftersom det kan bli svårt att jonglera skola och *Husmusen*. I värsta fall får jag fortsätta på höstlov eller liknande. Om jag lägger upp en grov plan nu, ser den antagligen ut:

* Nästa vecka
    * Mån 1 aug - tisdag 2 aug: Få klart min refactor där jag städar upp koden och lägger till lite kommentarer där det behövs.
    * Ons 3 aug - tors 4 aug: Påbörja fixandet av standardiserade error-koder.
    * Fre 5 aug: Gör så att det går att uppdatera/redigera/ändra objekt och implementera alla *routes* för filer.
* Näst-nästa vecka
    * Mån 8 aug: Om det finns något kvar från fredagen, gör klart det. Alternativt, hoppa till nästa dag i planeringen.
    * Tis 9 aug - fre 12 aug: Gör klart ett grafiskt gränssnitt som funkar. (Inte mycket fokus på estetik i detta stadie.) Om klar: påbörja estetiken lite.
* Sista två dagarna:
    * Mån 15 aug - tis 16 aug: Uppdatera protokoll-definitionen och skriv klart manualen.

Den här planen lämnar kvar: koppling med K-Samsök, samt det som jag inte hinner fixa med protokoll-definitionen och manualen. Att skriva dokument bör inte vara så svårt att göra samtidigt som jag går i skolan. Så länge inget nytt läggs till i protokollet, vilket det inte gör om jag inte programmerar, så är det bara att skriva och det går att få ut mycket text bara på en timme. Det blir värre med integreringen av K-Samsök. Jag vet inte hur lätt/komplicerad den kommer vara att göra. Jag har försökt läsa lite av deras dokumentation och det verkar lite utmanande, men kanske är det lättare än jag tror. Det kan vara smart att skicka ett mejl till RAÄ och kolla upp vad som behövs osv.



## 1 augusti 2022 -- Vecka 8
Idag har jag enligt planen fortsatt med en refactor på koden. Jag kan eventuellt har blivit helt klar, får se om jag hittar något mer imon, annars kommer jag gå vidare och fixa error-koder och sånt.



## 2 augusti 2022
Idag påbörjade jag error-koderna, och jag blev klar med det jag kan lägga till hittills. Det gick snabbare än jag trodde, så imorgon kan jag påbörja arbetet med de sista API-funktonerna och lägga till error-koder där det behövs.

Jag har märkt att jag har en del *TODOs* och *FIXMEs* i kodbasen. Det kan vara värt att ta en kik på dem...



## 3 augusti 2022
Idag har jag implementerat alla *routes* för fil-funktionerna och lagt till en *route* för uppdatering av objekt/föremål. Jag har dessvärre inte testat dem, men de *borde* funka. Jag kommer se till att testa dem framöver... Jag tror att jag behöver se över koden lite till imorgon, annars kommer jag gå vidare på att implementera det grafiska gränssnittet...



## 4 augusti 2022
Idag har jag påbörjat det grafiska gränssnittet. Har försökt lägga lite grund till att det ska se *helt okej ut*, i all fall...



## 5 augusti 2022
Idag implementerade jag inloggning i det grafiska gränssnittet.



## 8 augusti 2022 -- Vecka 9
Idag har jag implementerat sökning i det grafiska gränssnittet. Jag har också gjort lite små finslipningar på andra håll.



## 9 augusti 2022
Idag har jag finslipat på lite håll och implementerat individuell vy av föremål. I morgon hoppas jag implementera så mycket som möjligt av kontrollpanelen, så det blir gjort.



## 10 augusti 2022
Idag har jag lagt till `DBInfo` i användargränssnittet. Det ser lite *ehhhh* ut, men det får funka för nu. Jag har också lagt till så att det går att ändra sitt lösenord via gränssnittet. Dessutom har jag sett till att fixa några kritiska buggar som kunde krascha servern.



## 11 augusti 2022
Idag har jag gjort så att det går att lägga till nya objekt, permanent ta bort objekt, samt markera dem som utgågna via gränssnittet. Jag har också gjort så att det går att se server-loggen. Dessutom har jag fixat lite smärre syntax- och errorhanteringsproblem lite här och där i koden.



## 12 augusti 2022
Idag har jag implementerat alla funktioner som var kvar att imlementera till kontrollpanelen. Det enda jag kan tänka på som nu behövs i gränssnittet är att filerna ska visas på de individuella vyerna över filerna, samt så kan tumnaglar vara bra att implementera...



## 15 augusti 2022 -- Vecka 10
Idag har jag fixat med de sista små detaljerna till gränssnittet. Såvitt jag vet så finns nu alla funktioner som behöver finnas båda i API:t och i det grafiska gränssnittet. Det betyder att jag från och med nu kan fokusera ganska mycket på dokumentation tills den punkt kommer att jag vet hur jag kopplar Husmusen till K-Samsök.

På eftermiddagen hade jag ett möte med min handledare där vi diskuterade den nuvarande implementeringen. Vi kom fram till vissa mini-fixar som jag ska fixa under morgondagen.



## 16 augusti 2022
Idag har jag gjort de sista småfixarna och själva implementeringen av Husmusen kan nog betraktas som *klar* nu. Det är inte kanske den mest effektiva och användarvänliga implementeringen, men den visar i alla fall hur systemet skulle fungera.

Det som återstår nu är kopplingen till K-Samsök och dokumentationen. Det här var sista dagen på sommarlovet, så jag kommer inte kunna arbeta lika aktivt på Husmusen längre. Dock kommer jag arbeta på dokumentationen lite då och då när jag har tid, och senare kommer jag även jobba på kopplingen mellan Husmusen och K-Samsök.








































