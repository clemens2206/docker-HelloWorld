# docker-HelloWorld

## Java Programm mittels Docker-Container und openJDK kompilieren und ausführen
___

### Einleitung
In diesem Bericht geht es um die Ausführung und Kombilieren eines Java-Programmes, welches innerhalb eines Docker-Containers mit openjdk ausgeführt wird.
Dieses Projekt bietet ein einfaches Beispiel für die Verwendung von Docker-Containern zum Erstellen und Ausführen einer Java-Anwendung, ohne Java zu installieren.
___

### Verwendete Technologien
* Oracle Virutal Box Version: 6.1.18 r142142
* Linux Server Version: Ubuntu 20.04.1 LTS
* Visual Studio Code Version: 1.55.0
* Docker 19.03.13
* openJDK 8u131-jdk-alpine
* openJDK 9-b170-jre
___

### Kobilieren einer Java Klasse
Zum Erstellen einer Java-Anwendung benötigen wir ein Java Development Kit (JDK). Wir können ein vorhandenes Docker-Image mit einem JDK verwenden, um unsere Klasse zu erstellen.

`docker run -it -v $(pwd):/build openjdk:8u131-jdk-alpine javac /build/HelloWorld.java`

#### Erklärung
1. `docker run` = Der Befehl erstellt zuerst eine beschreibbare Containerebene über dem angegebenen Image und startet sie dann mit dem angegebenen Befehl.
2. `-it` = weist Docker an, ein Pseudo-TTY zuzuweisen, das mit dem Standard des Containers verbunden ist. 
3. `-v`= Mappt ein Verzeichnis im Container auf den Host.
4. `$(pwd):/build openjdk:8u131-jdk-alpine`= Pfad zum Image
5. `javac`= Kombiliert die `.java` Datei und erstellt eine `.class` Datei. In diesem Fall von der HelloWorld Datei.
6. `/build/HelloWorld.java`= der Pfad zu der Datei die umgewandelt werden sollte.
___

### Erstellen eines Docker-Container-Images
Wir können dann unsere Java-Klassendatei in ein Container-Image bündeln.

`docker build -t hello-world:8u131 -f Dockerfile-8u131 . `

#### Erklärung
1. `docker build`= buildet ein Image aus dem Dockerfile
2. `-t hello-world`= der Tag des Docker-Images
3. `8u131 -f`= Löscht einen Container.
4. `Dockerfile-8u131 .`= der Pfad zu dem Dockerfile.
___

### Run Container
Sobald wir eine kompilierte Java-Klasse haben, können wir sie einfach mit einer Java Runtime Environment (JRE) ausführen.

`docker run -it --rm=true hello-world:8u131`

#### Erklärung
1. `docker run`= Der Befehl erstellt zuerst eine beschreibbare Containerebene über dem angegebenen Image und startet sie dann mit dem angegebenen Befehl.
2. `it --rm=ture`= wann vorhanden löscht er den Container nach Beendigung.
3. `hello-world: 8u131`= das Image, welches gestartet werden soll.
___

### Dockerfile-8u131
```
FROM openjdk:8u131-jdk-alpine
ENV HW_HOME=/opt/hello-world
ADD HelloWorld.class $HW_HOME/
WORKDIR $HW_HOME
ENTRYPOINT [ "java", "HelloWorld" ]
```
___

### Builden und Run Container mit Java 9
Ich habe auch versucht, ob es funktionieren würde eine Java-Klasse auch in einer Java 9-Umgebung auszuführen, um sie zu testen. Wir können problemlos unsere HelloWorld.class-Datei, das mit Java 8u131 kompiliert wurde, in einem Java 9-Container ausführen. Wir verwenden eine andere Docker-Datei, die die Java 9-JVM als Basisimage angibt. Dann erstellen wir es und markieren es, sodass wir sowohl die Java 8- als auch die Java 9-Images ausführen können.

#### Befehle
(die Befehle funktionieren genau gleich, nur dass man ein anderes Dockerfile benötigt und angeben muss.)
Die einzigen Befehle die wir benötigen:

`docker build -t hello-world:9b170 -f Dockerfile-9b170 .`

`docker run -it --rm=true hello-world:9b170`
___

### Dockerfile-9b170
```
FROM openjdk:9-b170-jre
ENV HW_HOME=/opt/hello-world
ADD HelloWorld.class $HW_HOME/
WORKDIR $HW_HOME
ENTRYPOINT [ "java", "HelloWorld" ]
```
___ 

### HelloWorld Datei
```
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
        
    }
}
```
