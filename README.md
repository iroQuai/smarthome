
# inleiding
de  smart home oplossing van Philips Hue werkt goed maar is wel aan de prijs. Lampen zijn gemakkelijk aan te vullen via IKEA tradfri, die ook lampen van prima kwaliteit bieden (niet zo goed als Hue, maar dat is ook niet op alle plekken noodzakelijk, zoals gang of wc). de slimme schakelaars en bewegingsmelders van Ikea worden dan weer niet herkend in de gateway van Hue. Vreemd eigenlijk, want ze zijn wel te koppelen en werken dan ook samen. (Hue Essentials heeft hier goede uitleg en video's over). Deze oplossing werkte acceptabel, maar niet optimaal. Soms deed een knop plots niets meer en het opnieuw pairen van de knop was behoorlijk gedoe.


Home Assistant lijkt een uitkomst! 


# hardware
## Home Assistant als aanvulling op hue
Ik had nog een Raspberry Pi 4 liggen en dus heb ik daar Home Assistant op geïnstalleerd. Eerst in een losse docker naast andere programma's, maar al snel werd me duidelijk dat ik mezelf veel tijd en gedoe zou besparen als ik overstapte op Home Assistant OS, Incl supervisor.


De "get started" documentatie op de site van Home Assistant heeft me daarbij goed op weg geholpen! 
https://www.home-assistant.io/getting-started/


## Home Assistant ter vervanging van Hue Gateway
In eerste instantie heb ik home Assistant naast de hue gateway gedraaid om eens te experimenteren.


Overstappen van Hue gateway naar Home Assistant via een zigbee stick. Ik heb gekozen voor de Conbee II van Dresden Elektronica.


# Zigbee integratie in Home Assistant


Drie opties: 


* deCONZ: https://www.home-assistant.io/integrations/deconz/
* Zigbee Home Automation (ZHA) - https://www.home-assistant.io/integrations/zha/
* Zigbee 2 MQTT (Z2M) - https://github.com/Koenkk/zigbee2mqtt (gebruik m samen met https://github.com/yllibed/Zigbee2MqttAssistant)
* 



Ik heb zha gekozen ivm grootste integratie/compatibeliteit met Home Assistant


Weg met hue hub


Welke issues loop ik tegenaan?


Hoe lampen resetten?
* lampen
    * Lidl: eerst aanzetten dan 3x uit, direct weer heel kort aan, snel achter elkaar
Ikea: 6x aan/uit achter elkaar
    * Hue: 10 sec de on en off knoppen indrukken van de hue dimmer, vlakbij de lamp. 
* knoppen
    * ikea: 4x snel op link-knopje bij batterij
    * hue dimmer: 15 sec knopje setup ingedrukt houden 


Kan ik m'n oude apps nog gebruiken? De hue app of hue essentials? 
Ja! Zie onderstaande link: https://github.com/marcelveldt/hass_emulated_hue


De software staat nog in de kinderschoenen en er zijn behoorlijk wat losse eindjes, maar in principe werkt het wel! 


Hoe knoppen dezelfde functionaliteit laten houden (of meer)
Twee opties:
* Blueprint / yaml automations voor basale functies. werkte prima, maar traploos dimmen was niet zo soepel
* controllerX https://xaviml.github.io/controllerx/


ControllerX werkt verreweg het best! bovenstaande site geeft goede documentatie


Enige waar ik een beetje mee met worstelde was om een groep lampen te dimmen. Oplossing: een light-group maken (in lights.yaml)


Zigbee bindings: knoppen ook werkend als HA uit staat
dit nog  uitzoeken! Basale functies lijkt te kunnen via bindings. Lijkt te kunnen bij zha onder device properties / clusters. Maar nog niet gelukt https://community.home-assistant.io/t/how-to-bind/171120/15?u=iroquai
