# Uppstart
Hej och välkommen till detta Hackathon! I denna readme har vi samlat den information som ni ska behöva för att tackla det praktiskt runt uppgiften.
Agendan för dessa dagar finns i ett dokument som heter Agenda.pptx.
Ni som team jobbar i detta repo under denna tid. Vi i PL gruppen kommer att titta i repot under dagarnas gång och även kolla i det när vi ska utvärdera vinnarna det är därför viktigt att allt arbete sker här så att allt är på samma plats.

Ett tips till teamet är att gå varvet runt och låta alla presentera sig, ni kan utgå från detta ifall ni vill:
* Vem är du?
* Vad heter du?
* Vart jobbar du och vad jobbar du med?
* Vad har du gjort innan din nuvarande anställning?
* Vilka tekniker jobbar du mest med idag?
* Är det något under dessa dagar som du redan nu vet att du skulle vilja undersöka mera?

# För att klona repo
Github har tagit bort stödet för användarnamn och lösenord över HTTPS så ni behöver skapa ett PersonalAccessToken(PAT).
Gå in under Settings i rullisten uppe i högra hörnet på eran profil. Längst ner i Settings listan finns Developer Settings, klicka på den. Klicka sedan på Personal acces tokens. Klicka sedan på Generate new token. Ge ditt token ett namn och bocka för repo kryssrutan.
Detta skapar ett token med korrekt OAuth scope och du kan nu klona repot genom att tex köra: git clone https://< token >@github.com/Addnode-Hackaton-2022/Team-3.git

### Första uppgifterna till teamet, deadline dag 1 kl 11:00. Svara direkt i denna readme i punkterna under.
* Gå igenom pp Agenda.pptx
* Gå varvet runt och presentera era enligt frågorna ovan.
* Välj en teamleader för erat team.
  - Teamleader - Joackim Pennerup
* Välj ett namn för erat team. Detta skall även skrivas på dörren.
  - Namn - 2 Stable
* Vilken utmaning jobbar ni med?
  - Namn - Videostabilisering
 
 
 ### Andra uppgiften, deadline dag 2 kl 12:30
 * Ett inskick där ni ska fylla på denna readme med information under avsnittet Inskick, deadline dag två kl 13:00
 * En muntlig presentation där ni får presentera eran uppgift.
 * Lösningen som ni jobbat på under dessa dagar.

Det är på dessa moment som eran uppgift blir bedömd.

### Vad händer efter hackatonet?
En vecka efter hackatonet kommer alla repon att göras publika.

# Viktiga tider
### Dag 1 kl 11:00 - Deadline uppstart
### Dag 1 kl 12:00 - Lunch
### Dag 1 kl 18:30 - Middag
### Dag 2 kl 08:00 - Uppsamling H4, dagen startar
### Dag 2 kl 11:00 - Lunch
### Dag 2 kl 12:30 - Deadline för uppgiften. 
Här skall sista commiten till repot vara gjord och den muntliga presentationen vara klar för redovisning.
### Dag 2 kl 15:00 - Hackaton avslutas med mingel för de som har möjlighet

# Inskick, deadline dag två kl 12:30

### Övergripande beskrivning och val av utmaning
Vår utmaning var att stabilisera videoströmmen som kommer från drönaren via RTSP. Vi valde att undersöka flera möjliga lösningar och gick sedan vidare med två, strömma via Gyroflow och Gyroflow CLI.

### Team

#### Namn på medlemmar 
* Joackim Pennerup, Ida Infront AB (Teamleader)
* Rune Lien, Symetri Europe
* Aigeth Magendran, Canella IT Products
* Krister Wicksell, Sokigo AB
* Amir Sada, S-Group Solutions
* Fredrik Åslin, Decerno AB

#### Hur har ni jobbat inom teamet? Har alla gjort samma eller har ni haft olika roller?
Vi började med att förutsättningslöst söka efter information kring videostabilisering och spånade kring tänkta lösningar. Efter ett tag bestämde vi oss för två spår, strömmande stabilisering via Gyroflow samt dela upp strömmen i filer och använda Gyroflow CLI.

Vi delade då upp arbetet i följande delar och fortsatte arbetet med täta avstämningar och diskussioner.
* Strömmande stabilisering via Gyroflow
* Läsning av telemetridata
* Dela upp videoströmmen i filer
* Använda Gyroflow CLI

### Teknik. Beskrivningen på eran teknikstack, språk och APIer ni har använt.
Arbetet har inneburit bekantskap med många nya och intressanta tekniker.

Nya versionen av Gyroflow är till största delen implementerat i Rust och QML men använder även en hel del C++. Den äldre versionen är implementerad i Python.

FFmpeg har använts för att analysera och konvertera video.

Vi har läst på en hel del kring videoströmar och telemetridata, t.ex. om GPMF, GStreamer och Rpanion.

Vi har även kodat en del .NET för att koppla ihop hela processen.

### Lösning, dessa frågor ska minst besvaras
 * Hur har ni löst utmaningen?
 * Hur långt har ni kommit?
 * Vad är nästa steg?
 * Några rekommendationer för framtiden?
 * Några insikter, begränsningar eller utmaningar ni stött på som är intressanta att dela med der av?

Vi har inte kommit till en färdig lösning då utmaningen är väldigt komplex. Vi kan däremot presentera vägen till en möjlig lösning och vilka utmaningar som finns.

Vi kan öppna en videoström i [Gyroflow](https://github.com/gyroflow/gyroflow) efter att ha gått igenom koden och tagit bort alla antaganden om att den öppnade URL:en är en lokal sökväg, även i [qml-video-rs](https://github.com/AdrianEddy/qml-video-rs) som används för att integrera [MDK-SDK](https://github.com/wang-bin/mdk-sdk). MDK-SDK används för att öppna videofiler och MDK-SDK använder i sin tur [FFmpeg](https://ffmpeg.org/) vilken har bra stöd för strömmande video. MDK-SDK kan dock inte spela RTSP strömmen från drönaren men vi har fått [andra](https://www.wowza.com/developer/rtsp-stream-test) RTSP strömmar att fungera. Problemet kan ligga i att strömmen [saknar ljud](https://github.com/wang-bin/mdk-sdk/issues/24). Dock hjälper det inte att inaktivera läsning av ljudspåret.

För att kunna använda Gyroflow CLI har vi även kollat på dev branchen av den [gamla versionen](https://github.com/ElvinC/gyroflow) som innehåller CLI stöd. För att kunna låsa mot horisonten måste smoothing algoritmen bytas mot HorizonLock i cli.py. Gyroflow CLI är dock väldigt långsam på att processa data.

En av utmaningarna blir att få med telemetridata i videoströmmen. Här rekommenderar vi att kolla på [GPMF](https://github.com/gopro/gpmf-write) som används av GoPro för att skicka telemetridata som ett separat spår i videoströmmen. Gyroflow kan via [telemetry-parser](https://github.com/AdrianEddy/telemetry-parser) läsa GPMF direkt från videofiler.

Det är också av stor vikt att telemetridata är tidssynkroniserat med video, eftersom synkronisering av strömmarna är det i särklass långsammaste momentet i Gyroflow. Detta moment kräver dessutom random access läsning av både video och telemetridata. 

För att komma vidare är vår rekommendation att kontakta utvecklarna bakom Gyroflow och tillsammans med dem arbeta fram den bästa lösningen. Troligen går det att få Gyroflow att arbeta helt strömmande. Det är mera en fråga om att lägga ner den tid som krävs.

Tänkt flöde:  
<img src="https://github.com/Addnode-Hackaton-2022/Team-2/blob/main/data-flow.svg?raw=true" width="500">

# Mall för muntlig presentation, deadline dag två kl 12:30
Den totala tiden av presentation får ni distribuera som ni vill men den måste hållas. Presentation i form av text skall vara i en powerpoint medans demo visar ni som ni vill. Tänk bara på att ni ska hinna på utsatt tid.
* Överblick och utmaning - 1min
  - En mening ang vad lösningen gör
  - Vilken utmaning har ni tacklat?
* Team - 1min
  - Vilka är ni i erat team?
  - Vilka roller har ni haft? Hur har ni jobbat tillsammans?
* Teknik - 1min
  - Vilka tekniker har ni använt?
* Lösning och Demo - 2min 30s
  - Demo
  - Hur löser ni denna utmaning?
  - Vad är nästa steg, rekommendationer för framtiden?
