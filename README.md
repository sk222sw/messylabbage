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



## prestanda

### fewer http requests
### external stylesheets
### external scripts
### scripts at the bottom
### styles at the top
### 404 materialize
### no caching?


## egna reflektioner

