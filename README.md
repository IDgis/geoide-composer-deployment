# Geoide Composer

Geoide Composer is gebouwd met meteorjs (Zie [Meteor](https://www.meteor.com/)).   

## Installatie
### Instructie voor inrichten Windows machine voor Geoide Composer 
De ingelogde gebruiker dient administratie rechten te hebben.   
In deze instructie wordt schijf C:\ als voorbeeld genomen om Geoide Composer in te installeren.   
U kunt hier desgewenst een andere schijf gebruiken.   
Voor het openen en uitpakken van zip bestanden wordt het programma [7-zip](http://www.7-zip.org/) aangeraden.
 
#### Installatieset van Geoide Composer (initieel)
Er moeten 3 zip bestanden worden gedownload.   
- [database](https://github.com/IDgis/mongodb/releases)   
``mongodb-[versieNr].zip`` MongoDB configuratie, data lokatie en scripts    
Deze wordt eenmalig gebruikt voor de initeele MongoDB data structuur.
- [configuratie](https://github.com/IDgis/geoide-admin-deployment/releases):   ``geoide-admin-deployment-[versieNr].zip**`` Geoide Composer configuratie, data en scripts      
Deze wordt gebruikt bij de installatie van elke nieuwe Geoide Composer instantie.
- [programmatuur](https://github.com/IDgis/geoide-admin/releases):  
``geoide-admin-[versieNr].zip**`` Geoide Composer programmatuur   
Deze wordt gebruikt bij het installeren van een nieuwe release van Geoide Composer 

Download bij de opgegeven versie van de release, de zip bestanden onder:   
  **Downloads** 
       *Source code (zip)*    

\*\* De naam de van de zip is die van de het github project *geoide-admin*, de naam van het product is *Geoide Composer*.

#### Installeren basis programma's   
Meteor en Mongo worden eenmalig geinstalleerd voor alle instanties van Geoide Composer. 
#####  *Meteor - develop/runtime omgeving*  
  [Meteor Installatie](https://www.meteor.com/install), volg de instructies voor Windows.   
  Start InstallMeteor.exe als administrator.  
  Wordt lokaal geinstalleerd voor de ingelogde gebruiker:  
  ``C:\Users\USER\AppData\Local\.meteor\``   
  Sla tijdens installatie de stap 'Create developer account' over.    
  (Geoide Composer is gebouwd met meteor 1.4)  
   
#####  *mongoDB - NoSQL database systeem*   
  [Mongo](https://www.mongodb.com/download-center#community)       
  Centrale Mongo applicatie waar alle meteor applicaties mee kunnen verbinden   
  Kies *Previous Releases*, versie 2.6.12 en download msi 'versie 2.6.12 for Windows server R2' en volg de instructies:   
  Voer het MSI bestand uit als administrator en kies 'Complete Installation'.  
  ![Mongo 2.6. download](images/mongo26.png)   
  
  Mongo wordt in een van de standaard Windows folders geinstalleerd C:\Program Files of C:\Program Files (x86)   
  
  Aanmaken van folder structuur en starten van MongoDB service.   
   1. Kopieer de folder ``mongodb`` uit ``mongo-[versieNr].zip`` naar een schijf.   
      Het resultaat is dan bijvoorbeeld ``C:\mongodb\``   
      (In de rest van de tekst wordt aangenomen dat de installatie in ``C:\mongodb\`` heeft plaatsgevonden)   
   2. Maak een MongoDB service als volgt:   
    Open een terminal (DOS prompt) en ga naar C:\Program Files\MongoDB 2.6 Standard\bin\   
    Voer uit:   
    ``C:\Program Files\MongoDB 2.6 Standard\bin> mongod --config  C:\mongodb\config\mongo.config --install``   
    Als schijf D: is gekozen als installatie schijf voor de database bestanden :   
    ``C:\Program Files\MongoDB 2.6 Standard\bin> mongod --config  D:\mongodb\config\mongoSchijfD.config --install``   
   3. Open Windows Service beheer en start de service MongoDB.   
    
NB. Als een andere schijf of folder wordt gebruikt, dan moeten de paden in mongo.config worden aangepast.


#### Voorbereiding Geoide Composer
 
*Geoide Composer*   
   
Kopieer de folder geoide-composer uit  ``geoide-admin-deployment-[versieNr].zip`` naar een schijf.   
  Het resultaat is dan bijvoorbeeld ``C:\geoide-composer``   
  Hernoem deze folder als u meerdere instanties naast elkaar wilt gebruiken en kopieer de folder vervolgens nogmaals naar dezelfde schijf en hernoem:    
   
      C:\
      |
      |-- geoide-composer-test\
      |       \ ...
      |
      |-- geoide-composer-live\
      |       \ ...
      |

U kunt desgewenst ook alle instanties groeperen onder een hoofdfolder:   
   
     C:\geoide-composer
     |     \ -- geoide-composer-test\
     |            \ ...
     |     \ -- geoide-composer-live\
     |            \ ...
     |

In de volgende voorbeelden wordt aangenomen dat de installatie in ``C:\geoide-composer-test\`` heeft plaatsgevonden.

#### Installeren van Geoide Composer
1. Open de programmatuur zip:  ``geoide-admin-[versieNr].zip``   
2. ga naar ``C:\geoide-composer-test\``
3. kopieer inhoud van zip (onder ``geoide-admin-[versieNr]``, dus niet deze foldernaam zelf) naar ``C:\geoide-composer-test\meteor\``   
4. Pas de configuratie aan:   

##### Configuratie   
 De configuratie van de geoide-composer staat in het bestand ``C:\geoide-composer-test\conf\settings.json``     
 Dit bestand heeft de volgende structuur:
 
    {
      "viewer": {
        "reloadConfigUrl": "http://localhost:<VIEWER-POORT>/geoide/refresh"
      },
      "legendGraphic": {
        "uploadFolder": "C:/geoide-composer-test/upload/"
      },
      "requestcache": {
        "delay" : 600000 
      }
    }

Dit bestand kan gewijzigd worden met een teksteditor zoals Windows kladblok of NotePad++.   
De volgende onderdelen moeten aangepast worden aan de huidige Composer instantie:
  * reloadConfigUrl - dit is een url van de Geoide Viewer   
    ``<VIEWER-POORT>`` is bijvoorbeeld 9000 en kan worden gevonden in de viewer configuratie   
    Geoide Composer roept deze url aan telkens als er iets wordt opgeslagen.    
    Hierdoor blijft de Viewer up-to-date bij wijzigingen met de Composer.   
  * uploadFolder - dit is de folder waar legendGraphic plaatjes, die met de Geoide Composer zijn ge�pload, worden bewaard.   
    De standaard waarde is: [Installatie folder]/[programma naam]/upload
    bijvoorbeeld ``C:/geoide-composer-test/upload/``   
    NB. gebruik hier "/" in plaats van de in Windows gebruikelijke "\". 
  
NB. het bestand kan gewijzigd worden terwijl de service draait, wijzigingen worden vanzelf overgenomen.


#### Folder structuur na voorbereiding en installatie
  *Geoide-Composer*  
  De geoide-composer applicatie    
   
    C:\geoide-composer-test\
     |    \-- conf\
     |        settings.json # configuratie van het meteor programma
     |    \-- data\         # data voor een initiele vulling van de database
     |    \-- logs\         # logging van het meteor programma
     |        out.log
     |        err.log
     |    \-- meteor\       # meteor programma
     |       (inhoud van folder geoide-admin-[versieNr] uit zip file)
     |    \-- nssm\         # scripts voor het maken en starten van het meteor programma als Windows service  
     |    \-- upload\       # lokatie voor ge�ploade legendGraphic plaatjes en scripts
     |
     |

  *MongoDB*  
  Beheer van gegevens van alle Geoide Composer instanties   
    
    C:\mongodb\
     | README
     |-- backup\                  # mogelijke lokatie voor backups
     |-- config\
     |     mongo.config           # Configuratie om MongoDB als service te starten
     |     mongoSchijfD.config    # idem als folderstructuur op schijf D is geplaatst
     |-- data\                    # centrale locatie voor alle databases van de mongo service
     |-- images\                  # plaatjes voor README
     |-- logs\                    # mongo log files
     |-- scripts\                 # backup en restore scripts
     |     mongo-backup.bat
     |     mongo-restore.bat

#### Geoide-Composer als service starten   
   Open een terminal (als administrator uitvoeren) en ga naar folder ``C:\geoide-composer-test\nssm\`` en 
   start batch bestand nssm-install-meteor-service.bat met de volgende parameters:   
   ``nssm-install-meteor-service.bat [lokatie meteor installaties] [meteor programma naam] [meteor poort]``   
   
##### Toelichting:
Parameters:   
``[lokatie meteor installaties]`` hoofdlokatie van Geoide composer installaties bijv. ``C:``   
``[meteor programma naam]`` een subfolder van de hoofdlokatie bijv. ``geoide-composer-test``   
De *service naam* en *database naam* worden gelijk aan de *programma naam*   
``[meteor poort]``
Kies meteor poorten uit de reeks 3010, 3020, 3030 etc.   
Elk meteor programma moet een uniek poort nummer krijgen.   
*Voorbeelden:*   
1. bij installatie op ``C:\geoide-composer-test\`` :   
      ``nssm-install-meteor-service.bat  C:  geoide-composer-test  3010``   
   de service en database naam worden dan 'geoide-composer-test'   
2. bij installatie op ``C:\geoide-composer\live\`` :   
      ``nssm-install-meteor-service.bat  C:\geoide-composer  live  3020``   
   de service en database naam worden dan 'live'   
    
De service wordt nog niet gestart!      
Eerst moet de volgende aanpassing worden uitgevoerd:   
1. Start Windows Service beheer op.     
2. Ga naar de service ``geoide-composer-test`` en klik op eigenschappen   
3. Ga naar tab Aanmelden en voer bij 'Dit account' de naam en wachtwoord in van de gebruiker die meteor heeft geinstalleerd.   
4. Druk op OK en start de service.   

NB. Het opstarten kan lang duren omdat meteor eerst de applicatie moet bouwen

#### Instructie voor update van Geoide Composer

1. stop de service bijv. ``geoide-composer-test``
2. ga naar ``C:\geoide-composer-test\meteor``
3. delete alles in deze folder
4. Open de programmatuur zip: /tmp/.uploads/``geoide-admin-[versieNr].zip``   
5. kopieer inhoud van zip (onder ``geoide-admin-[versieNr]``, dus niet deze foldernaam zelf) naar ``C:\geoide-composer-test\meteor``   
6. start de service ``geoide-composer-test``   
NB: Het opstarten kan lang duren omdat meteor eerst de applicatie moet bouwen  
 

## Verificatie   
### Geoide Composer programma
  Kijk of de service onder de opgegeven naam is geinstalleerd (Windows beheer, services)   
  Start indien nodig de service en ga met een browser naar http://localhost:METEOR_PORT   
### Log bestanden 
Log bestanden bevinden zich in    
   Geoide Composer: ``C:\geoide-composer-test\logs``    
   Mongo: ``C:\mongodb\logs`` 

### url's
Het onderscheid tussen meteor applicaties zit in het poort nummer van de url.  
Dus bijvoorbeeld ``http://localhost:3010/`` en ``http://localhost:3020/``.   
Externe urls kunnen dan zijn ``http://www.MijnBedrijf.nl:3010/``, ``http://www.MijnBedrijf.nl:3020/``.  
Het gebruik als ``http://www.MijnBedrijf.nl/composer-test/`` en ``http://www.MijnBedrijf.nl/composer-live/``   
blijkt tot problemen te kunnen leiden in de applicatie, in ieder geval bij gebruik van Windows IIS.   
  
## Backup en restore van Geoide Composer gegevens
Zie voor gebruik en toelichting het [mongodb](https://github.com/IDgis/mongodb/tree/master/mongodb/scripts) project in Github.

### Installeren initieele dataset voor CRS in Geoide Composer 
De database van Geoide composer kan voor gebruik worden gevuld. Er is data beschikbaar gemaakt voor CRS2 gebruikers.   
1. stop de service ``geoide-composer-test``   
2. Voer het restore script uit van het [mongodb](https://github.com/IDgis/mongodb/tree/master/mongodb/scripts) project:    
Open een terminal (DOS prompt) en ga naar C:\Program Files\MongoDB 2.6 Standard\bin\   
Voer het restore commando uit:   
``C:\mongodb\scripts\mongo-restore.bat C:\geoide-composer-test\data\geoide-admin-test_crs2 geoide-composer-test``   
Waarbij geoide-composer-test (de database naam) gelijk is aan de programma naam en de service naam.
3. start de service ``geoide-composer-test`` 

### LegendGraphic plaatjes overnemen van een andere Composer instantie
Als een dataset van een andere Geoide Composer instantie wordt gebruikt of een initieele dataset is geinstalleerd (zie hierboven) moeten de urls van ge�ploade legendGraphic plaatjes nog worden aangepast.   
Voorbeeld:   
Er is een Composer instantie 'geoide-composer-test' op poort 3010 geinstalleerd en hiermee zijn legendGraphic plaatjes ge�pload van schijf. De url's van deze plaatjes beginnen dan 'http://localhost:3010'.   
Als de database later wordt gekopieerd naar een nieuwe Composer instantie 'geoide-composer-live' op poort 3020, dan moeten de urls van deze plaatjes, zoals ze in de database staan, worden aangepast ('localhost:3010' wordt dan 'localhost:3020').

Er staan twee scripts in de upload folder:   
``copy-legendgraphic-files.bat`` Kopieer de legendgraphic plaatjes van een andere instantie van Geoide Composer naar de huidige.   
``fix-legendgraphic-url-in-db.bat`` Herstel de url's naar de legendgraphic plaatjes in de database naar de juiste url van de huidige installatie.  
 
#### Kopieren van legendGraphic plaatjes van een andere instantie van Geoide Comkposer
Open een terminal en ga naar de upload folder van de huidige Composer instantie.  
Voer het script ``copy-legendgraphic-files`` uit.   
``copy-legendgraphic-files.bat [folder]``   
 Voorbeeld:   
 ``copy-legendgraphic-files.bat C:\geoide-composer-test\upload`` 

#### Herstellen van url's van legendGraphic plaatjes
Open een terminal en ga naar de upload folder van de huidige Composer instantie.  
Check of het ``mongo`` command beschikbaar is :   
``C:\geoide-composer-live\upload> where mongo``.   
Als er geen lokatie van het command beschikbaar is, start het volgende script dan vanuit de bin folder in de mongo installlatie (C:\Program Files\MongoDB 2.6 Standard\bin\):   
``C:\Program Files\MongoDB 2.6 Standard\bin\> C:\geoide-composer-live\upload\fix-legendgraphic-url-in-db.bat``   
Voer het script ``fix-legendgraphic-url-in-db`` uit.   
``fix-legendgraphic-url-in-db.bat  [database]  [oude url]  [nieuwe url]  ``   
 Voorbeeld:   
 ``fix-legendgraphic-url-in-db.bat  geoide-composer-live  localhost:3010  localhost:3020``   
In dit voorbeeld is 3010 het poortnummer van de Composer instantie (bv geoide-composer-test) waar de database vandaan kwam en 3020 het poort nummer van de huidige Composer instantie (bv geoide-composer-live) .
    
## Known issue's
### Internet Explorer beveiligingswaarschuwing 
Het kan voorkomen dat bij het openen van een kaart configuratie de kaart niet wordt getoond, of zelfs een pop-up met beveiligingswaarschuwing verschijnt.   
Dit gebeurd dan in Internet Explorer, in Chrome of Firefox is het niet gezien.   
Er moet dan een tekst aan de *Trusted Sites* worden toegevoegd. 

Oplossing:   
In Internet Explorer:   
* Menu: Tools/Internet Options, tab *Security*
 * Kies *Local Intranet*
  * klik op *Sites* en voeg *about:blob* toe.
  * probeer opnieuw een kaart te openen via de lijst met geconfigureerde kaarten. 
  * voeg *about:internet* aan *Sites* toe als het probleem nog niet weg is.
  
  