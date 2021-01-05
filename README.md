# inleiding

Ik heb nu een paar maanden slimme lampen van [Philips Hue](https://www.philips-hue.com/nl-nl). Werkt heel goed, maar alle onderdelen zijn behoorlijk prijzig. Gelukkig zijn er diverse andere merken die lampen bieden die ook werken op de Hue gateway. Een bekende daarvan is [Ikea Tradfri](https://www.ikea.com/nl/nl/cat/slimme-verlichting-36812/). Deze lampen zijn van prima kwaliteit (niet zo goed als Hue, maar prima als aanvulling op minder essentiele plekken zoals wc of gang) en bovendien een stuk goedkoper.  
De [slimme schakelaars](https://www.ikea.com/nl/nl/p/tradfri-afstandsbediening-30443124/) en [bewegingsmelders](https://www.ikea.com/nl/nl/p/tradfri-draadloze-bewegingssensor-wit-70429913/) van Ikea worden helaas niet ondersteund door de hue gateway. Vreemd eigenlijk, want in principe ze zijn wel te koppelen en met een omweg werkt het nog ook ([Hue Essentials](https://www.hueessentials.com) heeft hier goede [uitleg en video's](https://community.hueessentials.com/t/how-to-connect-ikea-tradfri-smart-controls-to-a-philips-hue-bridge/69) over).  Deze oplossing werkte acceptabel, maar niet optimaal. Soms deed een ikea aan/uitschakelaar het weken perfect, maar soms deed 'ie plotsling niets meer. Het opnieuw pairen van de knop was behoorlijk gedoe en  daarnaast was de knop ook niet zichtbaar in de Hue app. 

# Hardware 

Ik ga er voor het gemak even vanuit dat je Home Assistant al opgezet hebt en daarnaast al enigszins wegwijs bent in Home Assistant als het gaat om installeren van nieuwe software. Ik maak zelf gebruik Hass.io en dus ook de supervisor. Dat is verreweg de makkelijkste manier om onderstaande stappen door te voeren, maar je kan ook zelf losse docker images installeren of handmatig zaken aanpassen.

Om zigbee apparaten rechtstreeks aan Home Assistant te koppelen, heb je een zigbee adapter nodig. Er zijn allerlei soorten, de goedkoopste al voor een paar euro via AliExpress. Voor een overzicht, kijk hier: https://www.zigbee2mqtt.io/information/supported_adapters.html

 Ik heb gekozen voor de Conbee II van Dresden elektronik. Wat prijziger dan sommige andere opties, maar wel verreweg de best ondersteunde optie èn het biedt de meeste mogelijkheden (oa gebruik van deCONZ). Meer info hierover oa hier: https://gathering.tweakers.net/forum/list_messages/1901854


# Software: De Zigbee integratie in Home Assistant

Om gebruik te maken van de gateway binnen Home Assistant heb je ook passende software nodig. In het geval van de ConBee II zijn dit de drie meest gebruikte protocollen: 
- [deCONZ](https://www.home-assistant.io/integrations/deconz/)
- [Zigbee Home Automation (ZHA)](https://www.home-assistant.io/integrations/zha/)
- [Zigbee 2 MQTT (Z2M)](https://github.com/Koenkk/zigbee2mqtt)

Iedere variant heeft voor en nadelen. DeCONZ is software die speciaal voor de ConBee II gemaakt is, zit goed in elkaar en heeft goede ondersteuning. ZHA is volledig 'geintegreerd' binnen Home Assistant en daarmee ben je niet afhankelijk van software van derden. Zigbee 2 MQTT is voor als je graag alles via MQTT aanstuurt. Ik zag daar het nut niet zo van in dus heb die direct links laten liggen. 

Het aantal ondersteunde devices is nagenoeg gelijk bij de verschillende opties. Ik heb dus voor ZHA gekozen omdat het me prettig leek om alles binnen dezelfde interface aan te sturen (en dus geen gebruik te hoeven maken van DeCONZ). AChteraf was het misschien toch handig geweest om wèl deconz te gebruiken zodat ik een app zoals Hue Essentials kon blijven gebruiken, maar toen ik dat besefte had ik alles al ingesteld via ZHA. 

# De instellingen

## Hoe laat ik mijn schakelaars dezelfde functionaliteit houden (of meer)

Nadat ik de hard- en software had ingesteld en zowel een Hue lamp als een Tradfri Dimmer had gepaird., werd me direct weer duidelijk dat Home Assistant is geen product voor eindconsumenten. Alle functies die ik voor lief had genomen bij de Hue gateway, moest ik hier zelf instellen!  

De drie opties die ik gevonden heb om je knoppen te laten doen wat jij wil:
1. Handmatig alle functies instellen via YAML.
2. Blueprint gebruikenwerkte prima, maar traploos dimmen was niet zo soepel
3. AppDeamon4 / controllerX

### YAML 

Via YAML kun je zelf automations  maken die er voor zorgen dat een knop indrukken ook daadwerkelijk voor een effect zorgt op een ander device. Je hebt ongekende flexibiliteit,  maar als je niet gewend bent te programmeren is het wel heel complex om alle functies die Hue biedt na te maken. Het duizelde mij in ieder geval al snel en zocht dus naar een makkelijkere optie: 

### Blueprints

Via de Blueprints functie kun je een vooropgezette automation importeren in je eigen setup, om vervolgens alleen wat details aan te passen. Voor de ronde tradfri dimmer die je via zha aanstuurt kun je dus via deze blueprint (https://community.home-assistant.io/t/zha-ikea-five-button-remote-for-lights/253804) nog instellen welke lamp je wil dat 'ie aanstuurt. (en als je een groep lampen wil aansturen kun je een lightgroup maken in home assistant) 

Werkt op zich prima, maar de opties zijn toch nèt wat beperkt. Wat nu als je een bepaalde scene wil oproepen bij een bepaalde knop, geen ZHA maar deCONZ gebruikt, of de knop voor iets anders wil gebruiken dan lichten?  Voor iedere aanpassing is weer een andere blueprint nodig, en als je die zelf niet kan maken is het maar hopen dat iemand anders in de community dat gedaan heeft. 

### ControllerX

Toen ontdekte ik ControllerX! Het installeren kostte iets meer moeite (eerst AppDeamon4 installeren, daarna ControllerX), maar na de documentatie even goed doorgelezen te hebben bleek het instellen van allerlei schakelaars voor allerlei soorten doeleinden echt héél makkelijk te zijn.  Het voert te ver om het hier allemaal uit te leggen, want de documentatie is behoorlijk duidelijk.  Niet alleen voor het bedienen van lampen, maar ook het aanroepen van scenes, het besturen van mediaplayers en rolluiken, en het instellen van 'multi-click' functies (bijv: 1x klik op 'aan' is 50% brightness, 2x klik is 100% brightness). 
.
Nu ik ControllerX gebruik, is een nieuwe schakelaar instellen echt een fluitje van een cent.  Check dus snel  https://xaviml.github.io/controllerx/

## Kan ik m'n ingestelde scenes en routines kopieren naar Home Assistant?

In de HUE app had ik een aantal scenes ingesteld. Deze zijn helaas niet direct te kopieren naar Home Assistant. Wèl zijn ze makkelijk opnieuw te maken. Daarvoor verbind je de Hue gateway eerst met Home Assistant via de integratie. Maak in home assistant een nieuwe scene aan via de gui (instellingen --> scenes --> nieuwe scene) en voeg de lampen toe die je in de scene wil opnemen. Druk dan in de hue app op de scene die je wil kopieren, zodat alle lampen in de goede stand komen te staan. Druk dan op "opslaan" in Home Assistant. Zet de lampen even in een andere stand (of uit) en activeer daarna de scene om te checken of het gelukt is.  Later, als je hue gateway afgekoppeld is en je alles via home assistant doet, kan je die scene dan aanpassen en hoef je alleen de light.identities aan te passen in de scene (of je zorgt dat de light.identities van de lampen bij hue  dezelfde zijn als de direct gekoppelde lampen later)

Wat betreft routines heb ik niet zo'n shortcut gevonden, maar heb ik handmatig nieuwe routines gemaakt, die in home assistant "automations" heten.  Eventueel zou je nog gebruik kunnen maken van de scheduler integration en lovelace card (hier te vinden: https://community.home-assistant.io/t/scheduler-card-custom-component/217458 ) om een iets vertrouwdere interface te hebben bij het maken van de routines.  

## Kan ik m'n oude apps nog gebruiken? 

Wat nu als je verknocht bent aan de officiele Hue app om je lampen te bedienen? Of je maakte altijd gebruik van Hue Essentials om allerlei heftige lichteffecten aan te zetten? Dan heb je in principe pech, want Home Assistant wordt niet ondersteund door die apps. (Er zijn uitzonderingen; Hue Essentials ondersteunt wel DeCONZ, dus als je van dat protocol gebruik maakt, kan je die app nog wel gebruiken)

Gelukkig is daar wederom een oplossing: Home Assistant kan de Hue Gateway emuleren via "Hass Emulated Hue" ( https://github.com/marcelveldt/hass_emulated_hue). Niet alle functies van de HUE app werken, maar je kan al je lampen aansturen, zelfs als het geen zigbee lampen zijn. Ook groepen en area's worden ondersteund. 

De software staat nog in de kinderschoenen en er zijn behoorlijk wat losse eindjes, maar in principe werkt het wel en de ontwikkelingen gaan vlot. 

# Conclusie

Nu mijn scenes en automations allemaal in home asssitant zijn aangemaakt, alle lampen en knoppen rechtstreeks aangesloten zijn op Home Assistant via de ConBee II en alle knoppen op de manier zijn ingesteld zoals ik graag wil, ben ik een tevreden man en heb ik mijn Hue Gateway afgekoppeld. Weer een device minder in het netwerk, en bovendien meer functionaliteit!  Het nadeel is dat het allemaal wel meer knutselen is voordat het werkt, maar aan de andere kant is dat ook wel weer mooi om te doen! Al met al raad ik dit zeker aan :)

