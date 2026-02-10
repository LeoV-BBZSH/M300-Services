Zuerst muss Git/Bash installiert werden. Dies ist notwerdig, da die Befehle eigentlich für Linux gemacht sind, und Windows gewisse Befehle nicht kennt. Dies kann von https://git-scm.com/install/ heruntergeladen werden. Bei der Installation kann alles auf Standart belassen werden ![alt text](image.png)

Wenn nachfolgend von Terminal/CMD gesprochen wird, ist Bash gemeint. 

Nun muss als erstes Git konfiguriert werden. Dazu werden der Githb Username und die Email adresse benötigt. ![alt text](image-1.png)

Anschliessen kann als Test das folgende Repository geklont werden. ( https://gitlab.com/ch-tbz-it/Stud/m300/M300) Der Command dazu lautet "git clone [https://xxxxx.com]"![alt text](image-2.png)


Anschliessend kann man mit den Commands, welche im Bild zu sehen sind weiter testen, ob alles funktioniert. ![alt text](image-3.png)

Damit man nun über Bash testen kann, ob auch der Upload korrekt funktioniert, erstellt man am besten im Ordner des Repos ein neues textdoument. Anschliessend kann mit den befehlen 
"  $ git add -A ."
"  $ git commit -m "Mein Kommentar""
"  $ git push"
die Änderungen zu Github hochladen. 
Anschliessen sollte der Inhat vom Lokalen Repo auch auf Github zu sehen sein. ![alt text](image-4.png)![alt text](image-5.png)

Folgende Befehle mit Git sollte man sich merken, da diese immer wieder benötigt werden. 
$  git status                      # Geänderte Datei(en) werden rot aufgelistet
$  git add -A                      # Fügt alle Dateien zum "Upload" hinzu
$  git status                      # Der Status ist nun grün > Dateien sind Upload-bereit (Optional) 
$  git commit -m "Mein Kommentar"  # Upload wird "commited" > Kommentar zu Dokumentationszwecken ist dafür notwendig
$  git status                      # Dateien werden nun als "zum Pushen bereit" angezeigt
$  git push                        #Upload bzw. Push wird durchgeführt

# VirtualBox
VirtualBox ist ein Tool zum erstellen und verwalten von Virtuellen Maschinen, ähnlich wie VMware Workstation. 
Zuerst muss VirtualBox von der offiziellen Webseite heruntergeladen werden. https://www.virtualbox.org/ 
Anschliessend folgt die Installation mit den Standartwerten. Ich werde deshalb nicht weiter darauf eingehen. ![](Pasted-image-20260210081320.png)

Anschliessend kann man über den Button "Neu" eine VM erstellen. Es empfiehlt sich den Haken bei "Unbeaufsichtigter Installation zu setzen, da das Aufsetzen somit nichtmehr ganz so lang geht. Zudem kann angegeben werden, wie die VM heissen soll,  wo sie gespeichert ist, und welche ISo verwendet werden soll.  ![alt text](image-6.png)
Nachdem man anschliessend auf Vorwärts geklickt hat, kommt ein neues Fenster, bei welchem man Benutzername und Passwort eingeben muss. Auch der Host und Domainnamen können hier eigegeben werden. (Kommt nicht wenn man keine Unbeaufsichtigte installation macht. )![](Pastedimage20260210083020.png)

![](Pastedimage20260210083632.png)

![](Pastedimage20260210083644.png)


![](Pastedimage20260210083654.png)


 ![](Pastedimage20260210083706.png)
![](Pastedimage20260210083715.png)

![](Pastedimage20260210083725.png)

![](Pastedimage20260210083737.png)

![](Pastedimage20260210083750.png)

![](Pastedimage20260210083757.png)

![](Pastedimage20260210083804.png)


![](Pastedimage20260210083813.png)



# Tag 2

Cloud Computing ist, wenn man Programme nicht auf dem Lokelen rechner ausführt, sondern auf einem Rechner oder Server in der Ferne. (zb. über das Internet.)

**Saas** = Infrastructure as a Service 
![](Pastedimage20260210084906.png)

**PaaS** = Platform as a Service
![](Pastedimage20260210085100.png)
**IaaS** = Infrastructure as a Service ![](Pastedimage20260210085225.png)

Zudem gibt es seit dem Aufkommen von Docker/ Container eine neue ebene. Diese heisst **CaaS** (Container as a service).
Diese Ebene ist dafür zuständig, containerisierten Workload auf den Ressourcen auszuführen, die eine IaaS-Cloud zur Verfügung stellt. Die Technologien dieser Ebene wie Docker, Kubernetes oder Mesos sind allesamt quelloffen verfügbar. Somit kann man sich seine private Cloud ohne Gefahr eines Vendor Lock-ins aufbauen.

# Microsoft Azure
Azure verwendet Virtualisierung, damit alle Produkte laufen können. Die Server auf denen die Virtuellen Maschienen laufen nennt man Hypervisor. Ein solcher Server ist meist sehr stark, und ist die Heimat von Mehreren Virtuellen Maschinen. Die Virtuellen Maschienen sind dabei Unabhängig von den anderen, und können Alle möglichen Betriebssysteme haben. 

Die Server sind in soganannten Racks zusammengefasst. Wenn jemand eine VM erstellt, wird die Anfrage zuerst über die API an den sogenannten "Orchestrator" geleitet. Dieser sammelt alle Informationen und sucht sich dann das beste Rack aus, und leitet die Informationen dorthin weiter. 

Weitere Anbieter für Cloudcomputig sind AWS, Google Cloud, Oracle, ...


# Infrastructure as  Code
Früher war IT an physische Hardware gebunden und wurde manuell betrieben – Änderungen waren langsam und teuer.  
Heute sind Systeme virtualisiert und automatisiert, Änderungen sind schnell umsetzbar.

**Infrastructure as Code** bedeutet, IT-Infrastruktur wie Software zu behandeln: Sie wird in Dateien definiert, versioniert, getestet und automatisch bereitgestellt.

**Ziele von IaC** sind schnelle Änderungen, weniger manuelle Arbeit, stabile und reproduzierbare Systeme sowie kontinuierliche Verbesserungen statt grosser Umbauprojekte.

# Vagrant

Wichtige Commands:


| Befehl          | Beschreibung                                                                                                      |
| --------------- | ----------------------------------------------------------------------------------------------------------------- |
| vagrant init    | Initialisiert im aktuellen Verzeichnis eine Vagrant-Umgebung und erstellt, falls nicht vorhanden, ein Vagrantfile |
| vagrant up      | Erzeugt und Konfiguriert eine neue Virtuelle Maschine, basierend auf dem Vagrantfile                              |
| vagrant ssh     | Baut eine SSH-Verbindung zur gewünschten VM auf                                                                   |
| vagrant status  | Zeigt den aktuellen Status der VM an                                                                              |
| vagrant port    | Zeigt die Weitergeleiteten Ports der VM an                                                                        |
| vagrant halt    | Stoppt die laufende Virtuelle Maschine                                                                            |
| vagrant destroy | Stoppt die Virtuelle Maschine und zerstört sie.                                                                   |
|                 |                                                                                                                   |
|                 |                                                                                                                   |
Diverses: Mit vagrant ssh kann man sich in die VM verbinden. Dort kann man dan wie auf einer normalen VM arbeiten. 

Achtung, die dinge die man in der VM macht, werden nach dem neu starten nicht übernommen. Dazu müsste man die Commands im Vagrant file hinzufügen. Allternativ kann man sich auch eine externe Provisionierungsdatei bauen. 

**Vagrant Boxen**  
Boxen sind vorkonfigurierte VM-Vorlagen, die die Softwareverteilung und Entwicklung beschleunigen. Einmal heruntergeladen, werden sie lokal gespeichert. Boxen lassen sich mit `vagrant box add` hinzufügen und mit `vagrant box remove` löschen. Sie folgen meist dem Schema _Entwickler/Box_ (z. B. `ubuntu/xenial64`) und werden über die Vagrant-Boxen-Plattform bereitgestellt und geteilt.

**Konfiguration**  
Die gesamte VM-Konfiguration erfolgt im **Vagrantfile** (Ruby-ähnliche Syntax). Dort werden Box, Netzwerk, Hostname und Provisioning definiert.
Beispiel:

```
    Vagrant.configure("2") do |config|
        config.vm.define :apache do |web|
            web.vm.box = "bento/ubuntu-16.04"
            web.vm.provision :shell, path: "config_web.sh"
            web.vm.hostname = "srv-web"
            web.vm.network :forwarded_port, guest: 80, host: 4567
            web.vm.network "public_network", bridge: "en0: WLAN (AirPort)"
    end
```

**Provisioning**  
Provisioning führt automatisch Befehle aus (meist Shell/Bash), z. B. um Software wie Apache zu installieren.
Beispiel:
```
    config.vm.provision :shell, inline: <<-SHELL 
        sudo apt-get update
        sudo apt-get -y install apache2
     SHELL
```

**Provider**  
Der Provider legt fest, welche Virtualisierungsplattform genutzt wird (z. B. VirtualBox) und erlaubt Einstellungen wie Arbeitsspeicher.

Beispiel:
```
config.vm.provider "virtualbox" do |vb|
        vb.memory = "512"  
    end
```


### **Reihenfolge für den Betrieb, das Updaten und das Zerstören einer VM.** 
**Box hinzufügen**  
Eine neue Box wird zur lokalen Registry hinzugefügt mit:

```
`vagrant box add [box-name]`
```

Alle lokal verfügbaren Boxen lassen sich anzeigen mit:

```
`vagrant box list`
```

**VM erstellen**  
Zum Erstellen einer virtuellen Maschine wird ein Vagrantfile angelegt und die Provisionierung gestartet:

```
`mkdir myserver cd myserver vagrant init ubuntu/xenial64 vagrant up`
```

Der aktuelle Status der VM kann jederzeit abgefragt werden mit:

```
`vagrant status`
```

**VM aktualisieren**  
Nach Anpassungen am Vagrantfile kann die VM neu provisioniert werden:

```
vagrant provision
```

Alternativ kann die VM komplett neu erstellt werden:

```
`vagrant destroy -f vagrant up`
```

**VM löschen**  
Zum vollständigen Entfernen der virtuellen Maschine dient folgender Befehl:

```
vagrant destroy -f
```



### Synchronisierte Ordner
Mithilfe von synchronisierten Ordnern kann die VM auf ein Verzeichnis auf dem Host System zugreiffen. Dies ist vorallem bei Webservern, docker, usw. sinvoll. 

```
    Vagrant.configure(2) do |config|
        config.vm.synced_folder ".", "/var/www/html"  
    end
```
**Wichtig:** Standardmässig wird das aktuelle Vagrantfile-Verzeichnis in der VM unter /vagrant gemountet.

# Packer
Packer ist ein Tool zur Erstellung von Images bzw. Boxen für eine Vielzahl von Dynamic Infrastructure Platforms mittels einer Konfigurationsdatei.
Packer wird ähnlich wie Vagrant über die CLI bedient.
Der wichtigste Befehl ist:
```
packer build
```
Dieser wird benötigt, um aus der Konfiguration, welche im JSON Format erfolgt (Beispiel siehe unten) ein Image zu erstellen. 
Beispiel: 
```
    {
      "provisioners": [
        {
          "type": "shell",
          "execute_command": "echo 'vagrant'|sudo -S sh '{{.Path}}'",
          "override": {
            "virtualbox-iso": {
              "scripts": [
                "scripts/server/base.sh",
              ]
            }
          }
        }
      ],
      "builders": [
        {
          "type": "virtualbox-iso",
      "boot_command": [
        " preseed/url=http://{{ .HTTPIP }}:{{ .HTTPPort }}/ubuntu-preseed.cfg<wait>",
      ],
        }
      ],
      "post-processors": [
        {
          "type": "vagrant",
          "override": {
            "virtualbox": {
              "output": "ubuntu-server-amd64-virtualbox.box"
            }
          }
        }
      ]      
    }
```

**Provisioning**  
Auch bei Packer steht Provisioning für Anweisungen an ein anderes Programm (z.B. eine Shell wie Bash).

**Builder**  
Die Builder erstellen ein Image für eine bestimmte dynamische Infrastruktur-Plattform (wie z.B. VirtualBox).

**Post-processors**  
Sind Bestandteile von Packer, die das Ergebnis eines Builders oder eines anderen Post-Prozessor übernehmen, um damit ein neues Artefakt zu erstellen.



# LB2

Zuerst erstellt man eine VM. Dies wurde Bereits Beschrieben, daher ist hier nur ein Foto zu sehen. ![](Pastedimag20260210112819.png)

Anschliessend die VM starten. (Dies kann ein paar Minuten dauern)![](Pastedimage20260210124748.png)


Anschliessend verbindet man sich mit vagrant ssh mit der VM. 

Achtung: Teilweise muss man 2-3 Minuten warten, bis die VM wirklich korrekt gestartet ist. 

![](Pastedimage20260210125552.png)



Nun sollten als erstes die Packetquellen Aktualisiert werden. 
```
sudo apt-get update
```
(eventuelle Fragen mit "Y" bestätigen )

Danach können apache und Webanalyser installiert werden. dazu die folgenden Commands ausführen. 
```
sudo apt-get install -y apache2
```

```
sudo apt-get install -y webalizer 
```

Damit man diese Commands nun nicht immer ausführen muss, kann man sich mit "history" die gemachten befehle anzeigen lassen. Die relevanten befehle können nun in das Vagrant file kopiert werden. ![](Pastedimage20260210130521.png)

(Vagrantfile mit "nano Vagrantfile" öffnen und die Befehle unter "config.vm.provision.shell" einfügen)
![](Pastedimage20260210132011.png)



Wenn man die VM nun zerstört und anschliessend wieder startet, werden die 3 Commands automatisch ausgeführt. Dies lässt sich auch auf der Shell anzeigen.![](Pastedimage20260210131233.png)

Die Verbindung mit dem Webserver wird allerdings mit diesen anpassungen noch nicht möglich sein, da noch weitere probleme bestehen.
- Dateien sind nach dem Zerstören der VM nicht mehr vorhanden
- Port vom Webserver, in der VM, wird nicht weitergeleitet an Host.
- Es existiert keine index.html unter `/var/www/webalizer/index.html`. Siehe **Hinweis** bei [Verwendung](https://wiki.ubuntuusers.de/Webalizer/#Verwendung)
- Der URL (z.B. via `curl http://localhost/webalizer`) von Webanalyzer kann nicht abgerufen werden

Die Probleme lassen sich folgendermaasen lösen:

1. Erstellen eines Synchronisierten Ordners. Die Dateien werden damit vom Host auf die VM Synchronisiert. 
   
```
config.vm.synced_folder ".", "/var/www/html"
```


2. Der Port 80 auf der VM muss für den Host freigegeben werden. Im hier gezeigten Beispiel wird Port 80 auf der VM als Port 8080 auf dem Host freigegeben. 
   
```
config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
```


3. Für die restlichen beiden Probleme sind einige befehle mehr nötig, da der Webserver ja nicht Produktiv genutzt wird.

Traffic erzeugen, damit es etwas zum Analysieren gibt	   
```
curl http://localhost/ >/dev/null 2>&1
curl http://localhost/ >/dev/null 2>&1
curl http://localhost/ >/dev/null 2>&1
curl http://localhost/ >/dev/null 2>&1
curl http://localhost/bad >/dev/null 2>&1    
```

Da der Webanalyser immer nur die Vorletzten Logdatan analysiert, müssen die Acces Logs rotiert werden. 

```
sudo logrotate -f /etc/logrotate.d/apache2    
```

Zudem muss das Outputverzeichniss korrigiert werden. 

```
sudo sed -i -e"s:/var/www/webalizer:/var/www/html/webalizer:" /etc/webalizer/webalizer.conf 
```

Zum schluss muss der Job für 


#  25 Infrastruktur-Sicherheit

Aktuell sind alle Services frei zugreifbar, was ein Sicherheitsrisiko darstellt, wenn eine VM direkt im Internet oder in einer DMZ betrieben wird. Zur Absicherung werden nicht-öffentliche Ports mit einer Firewall gesperrt und der verbleibende Datenverkehr über einen Reverse Proxy verschlüsselt.

Eine **Firewall** kontrolliert und beschränkt den Netzwerkverkehr anhand definierter Regeln wie Absender, Ziel und Dienst, um unerwünschte Zugriffe zu verhindern.

Ein **Reverse Proxy** steht zwischen Client und Server, leitet Anfragen weiter und verbirgt dabei die interne Serveradresse, wodurch zusätzliche Sicherheit entsteht.










