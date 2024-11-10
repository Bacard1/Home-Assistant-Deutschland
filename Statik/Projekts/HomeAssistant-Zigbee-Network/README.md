<h1 align="center">Erstellen/Integration des Zigbee -Netzwerks im Heimassistenten</h1>

<br>

##  Hardware:

- Home Assistant OS für Hardware oder als virtueller Maschine ist installiert und konfiguriert.Sie sind mit diesem Schritt nicht bereit [Hier](https://www.home-assistant.io/installation/)


    - Dieses Projekt wurde verwendet "RaspberryPi 4B 8GB" 🔽:
    <img align="center" src="../../IMG/Devices/RASP PI 4B.png" width="50%" height="50%">


- "Sonoff Zigbee 3.0 USB Dongle Plus" oder andere, um das Zigbee -Netzwerk zu erstellen.Wenn Sie immer noch keine haben, sehen Sie die beiden Links unten 🔽:
    - [Amazon](https://www.amazon.de/dp/B09KZX4WSB?ref=ppx_yo2ov_dt_b_fed_asin_title)
    - [Aliexpress](https://de.aliexpress.com/item/1005004266559661.html?spm=a2g0o.productlist.main.1.29cfYELkYELkj7&algo_pvid=d6c4c86f-f945-433c-addd-962a0da0c955&algo_exp_id=d6c4c86f-f945-433c-addd-962a0da0c955-0&pdp_npi=4%40dis%21EUR%2138.16%2120.99%21%21%2140.55%2122.30%21%402103890117306177577828936efd34%2112000028571354347%21sea%21DE%21749630241%21X&curPageLogUid=DHGOVitBimE5&utparam-url=scene%3Asearch%7Cquery_from%3A) 
    - Sonoff Zigbee 3.0 USB Dongle Plus wurde in diesem Projekt verwendet 🔽:

    <img align="center" src="../../IMG/Devices/Sonoff zigbee3.0 Dongel.png" width="50%" height="50%">


**⚠️ Empfohlen:** Verwenden Sie "Sonoff Zigbee 3.0 USB Dongle Plus" mit einer USB -Erweiterung.Der Grund dafür ist, dass der gesamte Zigbee 3.0 -USB -Dongle vom Hardware -Betrieb beeinflusst wird und Netzwerkprobleme erzeugt!Wenn Sie zögern, was Sie auswählen möchten, sehen Sie sich den folgenden Link an.🔽:
    - [Aliexpress](https://de.aliexpress.com/item/1005007442670601.html?spm=a2g0o.order_list.order_list_main.75.6e4f5c5f9wWYJ0&gatewayAdapt=glo2deu)

 <br>

##  Softwarevorbereitung:

- **Firmware -Update bei "Sonoff Zigbee 3.0 USB Dongle Plus":** Obwohl ein ganz neues Firmware -Update ein Muss ist.Dies vermeidet nicht entschlossene Probleme mit der Kompatibilität zwischen Zusatzstoffen oder Geräten.In den folgenden Links finden Sie alles, was Sie tun müssen. 🔽:
    - [Treiber:](https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads) Laden und installieren Sie zunächst VCP -Treiber auf einem Windows- oder Mac -Gerät und starten Sie das Betriebssystem neu.
    - [Flash -Software:](https://zig-star.com/radio-docs/quick-start/#5have-fun) Laden Sie Zigstar herunter und verbinden Sie "Sonoff Zigbee 3.0 USB Dongle Plus" an einen der USB -Anschlüsse.
    - [Firmware cordinator:](https://github.com/Koenkk/Z-Stack-firmware/tree/master/coordinator/Z-Stack_3.x.0/bin) Laden Sie die neueste Version herunter und fügen Sie sie zu Zigstar hinzu.Siehe unten das Bild 🔽:

        ![image](https://github.com/user-attachments/assets/340206c9-767e-4a19-881d-207f9c098dc4)

    - [Dokumentation:](https://sonoff.tech/wp-content/uploads/2022/11/SONOFF-Zigbee-3.0-USB-dongle-plus-firmware-flashing-.pdf) Sonoffs offizielle Dokumentation

<p></p><br>

- **Installieren Sie den MQTT -Broker in Home Assistant:** Wenn Sie den MQTT -Broker immer noch nicht haben, klicken Sie auf die Schaltfläche unten. 🔽:

<br>

<a href="https://my.home-assistant.io/redirect/supervisor_addon/?addon=core_mosquitto">
    <img align="center" src="../../IMG/Andere/button ADD-ON ON.svg" >
</a>

<br>

- **Schalten Sie nach der Installation die Funktion "Start -up -System" ein und starten Sie den Home -Assistenten neu. 🔽:**

    ![image](https://github.com/user-attachments/assets/f950e020-0fc3-42c4-8ab6-977cc5536a72)


    - Öffnen Sie nach dem Start des Systems die Konfiguration "Mosquitto Broker" und wechseln Sie in den YAML -Bearbeitungsmodus.Ersetzen Sie alles durch diesen Code 🔽:


    🛠️
    ```html
    logins:
      - username: "!secret mqtt_user"
        password: "!secret mqtt_pass"
    require_certificate: false
    certfile: cer.pem
    keyfile: key.pem
    customize:
    active: false
    folder: mosquitto
    anonymous: false
    server: mqtt://_________________:1883
    base_topic: zigbee2mqtt
    debug: true
    ```

    ⚠️ Füllen Sie auf "Server" die IP -Adresse des Geräts aus, in dem der Home -Assistent installiert ist 🔼.



    - Fügen Sie die folgenden Zeilen zu "Secrets.yaml" hinzu.Wenn dies nicht erfolgt, kann die Konfiguration nicht gültig sein 🔽.

    🛠️
    ```html
    # MQTT login daten
    mqtt_user: _____________
    mqtt_pass: _____________
    ```

    ⚠️ Füllen Sie Ihren bevorzugten Benutzernamen und Ihr Kennwort mit ihnen aus. Sie werden sich an MQTT wenden.Speichern Sie die Änderungen und starten Sie "Mosquitto Broker".Stellen Sie sicher, dass "Mosquitto Broker" erfolgreich begonnen hat und mit der Installation von "Zigbee2MQT" fortgesetzt wird

<br>

- **Installation von Zigbee2MQTT im Heimassistenten:**
    - Drücken Sie die Taste unten, um den Zigbee2MQT -Speicher zu Ihren Nahrungsergänzungsmitteln hinzuzufügen 🔽:

    <br>
    <div style="margin-bottom: 50px;">
        <a href="https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fzigbee2mqtt%2Fhassio-zigbee2mqtt">
            <img align="center" src="../../IMG/Andere/button ADD ADD-ON REPOSITORY TO MY.svg" >
        </a>
    </div>
    <br>

    - Aktualisieren Sie nach dem Hinzufügen des Repositorys die Seite und Sie finden Folgendes 🔽:

    ![image](https://github.com/user-attachments/assets/5655390c-9c13-473c-b6b6-6993191648dc)

    Öffnen und installieren Sie Zigbee2MQTT und starten Sie das System neu.

    - Öffnen Sie nach dem Start des Systems die Konfiguration in Zigbee2MQTT und wechseln Sie zum YAML -Bearbeitungsmodus.Ersetzen Sie alles durch den folgenden Code 🔽:

    <br>

    🛠️
    ```html
    data_path: /config/zigbee2mqtt
    socat:
    enabled: false
    master: pty,raw,echo=0,link=/tmp/ttyZ2M,mode=777
    slave: tcp-listen:8485,keepalive,nodelay,reuseaddr,keepidle=1,keepintvl=1,keepcnt=5
    options: "-d -d"
    log: true
    mqtt:
    server: mqtt://_____________:1883  
    user: "!secret mqtt_user"
    password: "!secret mqtt_pass"
    serial:
    port: ______________________________________
    ``` 

    ⚠️ Zu "Server:" Sie müssen dieselbe IP -Adresse hinzufügen, die Home Assistant hat 🔼. Von "Port:" Befolgen Sie die Schritte im Bild unten  🔽:

    <img align="center" src="../../IMG/GIF/patch_usb_port002.gif">

    Speichern Sie die Änderungen!Überprüfen Sie den Start automatisch mit dem System und starten Sie das Hinzufügen -on 🔽:

    <img align="center" src="../../IMG/GIF/Zegbee_save_and_start.gif">    


### Starten Sie das gesamte System neu, wenn das Add -on nicht sofort beginnen möchte.Nach dem Neustart beginnt es automatisch.
### Herzlichen Glückwunsch haben bereits ein funktionierendes Zibee -Netzwerk!
