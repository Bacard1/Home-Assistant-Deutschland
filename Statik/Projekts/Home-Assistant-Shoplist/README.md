<h1 align="center">Bildeinkaufsliste</h1>

<br>

##  Vorbereitung:
- Installieren Sie die folgenden Pakete von HACs oder besuchen Sie die Repositorys der folgenden Links 🔽:
  - [Local Conditional card](https://github.com/PiotrMachowski/Home-Assistant-Lovelace-Local-Conditional-card)
  - [Vertical Stack In Card](https://github.com/ofekashery/vertical-stack-in-card)
  - [mini-graph-card](https://github.com/kalkih/mini-graph-card)

  Starten Sie nach der Installation der Pakete Ihren Heimassistenten und wechseln Sie mit den nächsten Schritten 🔽:

## Schaffung:
- **Erstellen Sie eine neue Liste: ** Erstellen Sie eine neue Liste, an die Sie die Namen der von Ihnen gedrückten Elemente weiterleiten.🔽::
  
  <img align="center" src="https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/create_list.gif?raw=true">      

- **Shoplist Dashboard: ** Erstellen Sie ein neues Panel mit dem Namen "ShopList". Dort erstellen wir alle erforderlichen Listen.🔽::


  <img align="center" src="https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/create_shoplist_dashboard.gif?raw=true">  
  
  - Fügen Sie als neue Karte die Liste hinzu, die Sie verwenden werden.🔽::
    
    <img align="center" src="https://github.com/user-attachments/assets/62fe909f-7019-4958-8b1c-4187b441959a">

    Handbuch hinzufügen Karte 🔽:
    
    🛠️
    
        type: todo-list
        entity: todo.list

  - **Elemente:** Es ist eine einköpfige Erstellung einer "Assistenz -Schaltfläche", mit der wir die Elemente hinzufügen werden.Es ist nicht notwendig, für jeden zu erstellenArtikel, da es nicht möglich ist, mehrere Elemente gleichzeitig zu drücken.Die Taste ermöglicht jedes Hinzufügen individueller Änderungen.🔽::

    ![image](https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/create_button_helper.gif?raw=true)
    
   
  - **Kategorien:** Für jede Kategorie ist es erforderlich, einen Assistenten zu erstellen.Wenn Sie beim Öffnen einer Kategorie den gleichen Helfer für alle Kategorien verwenden,Jeder wird sich öffnen.In Komfort werden wir auch Automatisierung hinzufügen, die beim Öffnen einer Kategorie alle anderen Kategorien schließt.🔽::
    
    ![image](https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/create_Namenskategorie_helpers.gif?raw=true)

    Fügen Sie ihn nach dem Erstellen des Helfers für die Kategorie als Karte im Dashboard hinzu.🔽::

    ![image](https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/shoplist_und_kategodie.gif?raw=true)

    Handbuch hinzufügen Karte 🔽:

    🛠️
    
        type: vertical-stack
        cards:
          - type: entities
            entities:
              - entity: input_boolean.namenskategorie
                tap_action:
                  action: toggle
                name: Namenskategorie
                image: https://github.com/Bacard1/Home-Assistant-Shoplist/blob/main/IMG/shoplist/Alkoholische-Getr%C3%A4nke/%D0%91%D0%B8%D1%80%D0%B0.png?raw=true
          - type: conditional
            conditions:
              - condition: state
                entity: input_boolean.namenskategorie
                state: "on"
              - condition: state
                entity: input_boolean.namenskategorie
                state_not: "off"
            card:
              type: grid
              cards:
                - show_state: false
                  show_name: true
                  camera_view: auto
                  type: picture-entity
                  entity: input_button.artikul
                  image: https://github.com/Bacard1/Home-Assistant-Shoplist/blob/main/IMG/shoplist/Alkoholische-Getr%C3%A4nke/%D0%91%D0%B8%D1%80%D0%B0.png?raw=true
                  name: Arikul 1
                  theme: yourname
                  tap_action:
                    action: call-service
                    service: shopping_list.add_item
                    service_data:
                      name: Arikul 1
                  card_mod:
                    style: |
                      ha-card {                
                        border: 1;
                        width: 90%;
                        height: 90%;    
                      }              
        card_mod:
          style: |
            ha-card {
              --ha-card-background: none;
              border: 2; 
            }
    
      ⚠️ Fügen Sie die Kartenkarte genau so ein, wie sie angegeben wird, und ersetzen Sie nur "- Entity: input_boolean.namenskategorie" durch den Namen der Hilfe (Kategorie).Sobald Sie es gespeichert haben, können Sie den Rest der Kartengrafiken problemlos ändern oder ein neues Element hinzufügen. 🔽:
  
  
      ![image](https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/kategorie_card_edit.gif?raw=true)
    
  - [Hier](https://github.com/Bacard1/HomeAssistant-Bulgaria/tree/main/Statik/Projekts/Home-Assistant-Shoplist/IMG) Sie finden eine umfassende Sammlung von Bildern für Ihre Einkaufsliste, geteilt nach Kategorie.
<br>

##  Automatisierung:

  ⚠️ Die folgende Automatisierung ist optional, aber empfohlen.Ihr Mangel wird die Arbeit der Einkaufsliste nicht beeinträchtigen, aber es wird es einfacher machen.

- **1. Es überwacht die bereits entfernten Produkte und aktiviert den Reiniger.🔽:**

   🛠️
  ```html  
  alias: ""
  description: ""
  triggers:
    - event_type: shopping_list_updated
      trigger: event
  conditions: []
  actions:
    - delay: "00:01:00"
      enabled: false
    - data: {}
      enabled: false
      action: python_script.clear_completed_items
    - metadata: {}
      data:
        skip_condition: true
      target:
        entity_id: automation.izchisti_spiska_za_pazaruvane_sled_20_minuti
      action: automation.trigger
  ```

- **2. Reiniger einer Liste entfernter Produkte -** Diese Automatisierung reinigt alle Produkte, die Sie nach einem 10 -minütigen Aufenthalt entsorgt haben.Dies engagiert Sie nicht und verwirrt Sie mit dem täglichen Gebrauch. 🔽:

   🛠️
  ```html
  alias: ""
  description: ""
  triggers:
    - event_type: shopping_list_updated
      trigger: event
  conditions:
    - condition: template
      value_template: |
        {% if state_attr('sensor.shopping_list', 'items') is defined %}
          {% for item in state_attr('sensor.shopping_list', 'items') %}
            {% if item['complete'] %}
              {% set completed = true %}
            {% endif %}
          {% endfor %}
          {{ completed | default(false) }}
        {% else %}
          false
        {% endif %}
  actions:
    - delay:
        hours: 0
        minutes: 10
        seconds: 0
        milliseconds: 0
    - data: {}
      action: shopping_list.clear_completed_items
  ```

  ⚠️ Automatisierung 1 und 2 werden beiseite gelegt, weil einer die andere aktiviert!

- **3. Nur eine offene Kategorie:** Diese Automatisierung erlaubt nicht mehr als eine Kategorie.Beim Öffnen einer Kategorie schließen sich alle anderen. 🔽:

   🛠️
  ```html
  alias: ""
  description: ""
  triggers:
    - entity_id:
        - input_boolean.kategorie1
      to: "on"
      trigger: state
  conditions: []
  actions:
    - target:
        entity_id:
          - input_boolean.input_boolean.kategorie2
          - input_boolean.input_boolean.kategorie3
          - input_boolean.input_boolean.kategorie4
          - input_boolean.input_boolean.kategorie5
          - input_boolean.input_boolean.kategorie6
      data: {}
      action: input_boolean.turn_off
  mode: single
  ```
  
- **4. Ein neuer Artikel auf der Einkaufsliste:** Sendet eine Benachrichtigung an alle mobilen Geräte, um einer Liste ein neues Element hinzuzufügenт.🔽:

  🛠️
  ```html
  alias: ""
  triggers:
    - event_type: shopping_list_updated
      event_data:
        action: add
      trigger: event
  actions:
    - data:
        message: "{{ trigger.event.data.item.name }} has been added to the shopping list"
        data:
          clickAction: /shopping-list
          url: /shopping-list
      action: notify.notify
    ```  
