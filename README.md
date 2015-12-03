# messylabbage
webbteknik 2 - labb 2 - messy labbage - rapport
 
## säkerhet

### SQL injections

#### Problem
Injections är det mest kritiska säkerhetsproblemet idag. Injection går till på så sätt att användaren skickar med SQL-kod i en HTTP-request till servern, och kan med denna SQL-kod utföra kommandon som inte är tänkta att användaren ska ha tillgång till[1, A1]. I Labby Message går det att göra en injection i inloggningsforumläret.

#### Konsekvens
Med injections kan användaren exempelvis komma åt känslig information, så som inloggningsuppgifter, och man kan även köra kommandon som ändrar eller tar bort information[1, A1]. 

#### Åtgärd
OWASP rekommenderar att använda ett API som tillåter parameteriserade frågor. Man ska även vara noga med hur tecken behandlas när man skickar med dem i HTTP-requesten: 
	“[...] you should carefully escape special characters [...]”. [1, A1]
	
### Säkerhetsbrister i sessionshantering

#### Problem
Genom att inte hantera sessioner på rätt sätt tillåter man användare att få tag på sessions-cookien[1, A2]. I Labby Message finns sessionen kvar även efter att man loggat ut, vilket leder till att man kan lägga till /message i URL:en och fortfarande komma åt meddelandesidan.

#### Konsekvens
Andra användare kan genom samma webbläsare på samma dator ta sig in på en annan persons konto, och på så sätt komma åt eventuell känslig information, eller utföra saker i en annan persons namn(konto)[1, A2].

#### Åtgärd
Nogrann implementering av session/cookie-hantering. OWASP tillhandahåller ett Cheat Sheet för att hjälpa utvecklare med detta [2].

### Cross-Site Scripting

#### Problem
Genom att uttnytja dålig validering och implementering av forumlär kan användare skicka med egna skript[1, A3]. I Labby Messages fall går det att köra script från meddelanderutan.

#### Konsekvens
Genom att köra egna skript kan man till exempel komma åt cookie-information, ta över andra användares webbläsare eller köra annan illasinnad kod [1, A3].

#### Åtgärd
OWASP rekommenderar framförallt att man ser till att inmatad data tas hand om på rätt sätt, och läses som text istället för kod/skript, “The preferred option is to properly escape all untrusted data based on the HTML context (body, attribute, JavaScript, CSS, or URL) that the data will be placed into. “ [1, A3] och tillhandahåller ett Cheat Sheet[3] för att underlätta för utvecklare.

### Osäkra referenser till objekt eller data

#### Problem
Genom att göra en request mot servern kan man komma åt data som inte är tänkt att vanliga användare ska komma åt [1, A4]. I Labby Message leder URL:en /static/message.db direkt till databasen.

#### Konsekvens
En oauktoriserad användare kan komma åt känslig data [1, A4]. 

#### Åtgärd
Någon typ av auktoriseringskontroll på servern som inte tillåter vanliga användare att komma åt all information [1, A4]

### Cross-Site Request Forgery

#### Problem
En användare går in på en webbsida som innehåller falska HTTP-requests, och en illasinnad användare kan på så sätt få tag på till exempel cookie-information från andra användare [1, A8]. 

#### Konsekvens
Med information från andra användares cookies kan man utföra saker i andra användares namn. 

#### Åtgärd
OWASP rekommenderar att man använder unika tokens för att koppla ihop rätt användare med rätt browser [1, A8].

## prestanda

### P1. Onödiga HTTP-requests

#### Konsekvenser
Enligt Souders[4, s10] står ocachade HTTP-requests för den största delen av väntetiden då en användare besöker en webbplats. Med många HTTP-requests ökar alltså väntetiden markant. 

#### Åtgärd
En simpel lösning är att kombinera filer av samma typ [4, s15-16]. I Labby Messages fall kan detta göra genom att slå samman Message.js och MessageBoard.js. 

### P2. Skript- och stilmallar är inbakade i HTML-dokumentet

#### Konsekvenser
Att ha skript och stilmallar inbakade i HTML-dokument leder till färre HTTP-requests, vilket kan vara gynsamt för webbplatser som enskilda användare sällan besöker. Om man istället har dem i externa filer blir det det fler HTTP-requests första gången en avändare besöker webbplatsen, men man ger webbläsaren möjlighet att cachea filerna, vilket gör att kommande besök kommer gå snabbare [4, 55-56]. 

#### Åtgärd
I Labby Message fall är det tänkt att man ska återkomma ofta, och det rekommenderas därför att använda externa filer.

### P3. Resurser cacheas inte

#### Konsekvenser
Som nämnt i punkt ett står HTTP-requests för den största delen av väntetiden. Genom att inte cachea data blir väntetiden således lika lång varje gång man besöker en webbplats.

#### Åtgärd
Kapitel tre i High Performance Websites diskuterar kring hur man på bästa sätt kan använda sig av Cache-Control [s23] för att se till att resurser som inte ändras ofta cacheas.

### P4. Stilmall i slutet av HTML-dokumentet

#### Konsekvenser
Webbläsaren läser in data i den ordning det står i HTML-dokumentet [4, s37-38, Progressive Rendering], vilket leder till att användaren kan få se en ostilad HTML-sida[4, s43], eller i vissa webbläsare en blank sida[4, s39]. Detta gör ingen tidsmässig prestandaskillnad, men gör däremot skillnad på användarupplevelsen [4, s39-42].

#### Åtgärd
Flytta CSS till toppen av dokumentet. Som tidigare diskuterat i P2 bör den även länkas in från en extern fil.

### P5. Skript länkas in i Head-elementet

#### Konsekvenser
När webbläsaren läser in skript så stannar den upp med renderingen av sidan[4, s45]. 

#### Åtgärd
Flytta inläsningen av JavaScript-filer från Head-elementet till sist på Body-elementet[4, s49-50, Best Case: Scripts at the Bottom]

### P6. Requests till resurser som inte finns

#### Konsekvenser
Webbläsaren försöker hämta resurser som inte finns på servern, vilket leder till fler onödiga HTTP-requests. Se P1.

#### Åtgärd
Ta bort länkningen till berörda resurser.

## egna reflektioner

Labby Message är en liten applikation med små filer. Detta betyder att rent prestandamessigt gör det inte lika stor skillnad som på en stor applikation med mycket större filer. Därför har jag valt att inte inkludera minification och gzip, då skillnaden inte är särskilt stor. Souders menar på sidan 30 i High Performance Web Sites att de CPU-resurser som används för detta gör det onödigt med gzip/minification för mindre filer/applikationer. 

Den stora fokusen bör istället ligga på att åtgärda de säkerhetshål som finns. Labby Message är tänkt att användas som en intern todo-lista och det är mycket viktigt att endast inloggade användare kommer åt den. Därför anser jag att säkerhetshålen bör åtgärdas innan applikationen börjar användas, medan prestandaförbättringarna gör så pass liten skillnad i detta fall att man bör överväga om det är slöseri med resurser att förbättra prestandan, eller om det kan vara en bra investering om man tänker sig att applikationen senare ska växa och användas i större sammanhäng. 

