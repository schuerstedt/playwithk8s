Hier mal ein paar Docker Spielereien :)



# docker interactiv mit busybox benutzen
docker run --name mybusybox -it busybox 

# mit CTRL+p und CTRL+q beenden ohne die laufende busybox 
# das geht aber nicht in Code!!!
docker run --name mybusybox -d -it busybox # busybox detached starten
# zu benden und dann wieder mit
docker exec -ti mybupssybox sh

# mit exit den docker run --namecontainer stoppen
exit

docker kill mybusybox   # container killen
docker rm  mybusybox    # conatiner namen freigeben

docker ps -a   # alle Prozesse auch terminierte anzeigen
docker ps -a | grep mybusybox

# mal als Beispiel einen Apache container starten und diesen dann manipulieren
docker run -p 8080:80 -d --name mywww httpd
docker exec -it mywww sh
# ändern der index.html
echo HI > ./htdocs/index.html

# da die obige Änderung nicht dauerhaft ist, das Volume auf mein lokales Dateisystem mounten
docker kill mywww
docker rm mywww

# needs a sample with volume!
docker run -p 8080:80 -d --name mywww httpd

# get to know more about your container :)
docker inspect mywww
docker inspect --format='{{.State.Pid}}' mywww
docker inspect --format='{{.ID}}' mywww
docker ps -a | grep mywww


docker images       # infos über das image

docker logs mywww   # zeigt die logs an

docker image ls
docker history httpd
# interessantes tool zum spielen https://github.com/wagoodman/dive

# eigenes Image erstellen
docker commit # Image aus einem laufenden Container erzeugen, der vorher verändert wurde
mit einem Dockerfile (bessere Option, da Pipeline fähig)

* Image auf einem bestehenden Image aufbauen
* jedes Image hat sein eigenes Verzeichnis
* Ein Dockerfile
* FROM welchem Image als Basis
* jedes folgende Kommando erzeugt einen neuen Layer
* LABEL key/value
* MAINTAINER wer
* RUN Kommandos ausführen
* EXPOSE Ports
* ENV Environment Variablen
* ADD Dateien kopieren
* (alternative) COPY
* USER gibt einen Kontextuser an
* ENTRYPOINT Start Kommando (sh -c als Standard)
** CMD für Argumente - aber auch mittlerweile als Default benutzt
** CMD kann über die Kommandozeile überschrieben/ergänzt werden
# Beispiel
ENTRYPOINT["/usr/sbin/httpd"]   # httpd aufrufen
CMD["-D","FOREGROUND"]          # Parameter zum Entrypoint - Vorsicht wenn kein Entrypoint spezifiziert wird, weil CMD eigentlich Argument dann zum sh -c sind
würde daher übersetzt werden als
/usr/sbin/httpd -D FOREGROUND

da jedes RUN einen eigenen Layer erzeugt, am Besten RUN mit && als Einzeiler schreiben
RUN commando1 && commando2
oder
RUN commando1 &&\
    commando2

Build des Images mit
docker build im Verzeichnis

docker build -t imagename:tag (latest als Standard)

bauen eines Dockerimages aus Basis buxybox der ein Hello World ausgibt
Dockerfile
FROM busybox
CMD ["echo Hello World"]

docker build -t myownbusybox:1 .

docker run myownbusybox:1

am Besten mit --rm starten, da ja ein Onetime Aufruf
docker run --rm myownbusybox:1
docker run --rm myownbusybox:1 /bin/echo HI MAC ' CMD kann ersetzt werden















