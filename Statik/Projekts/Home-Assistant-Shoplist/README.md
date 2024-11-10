<h1 align="center">Списък за пазаруване с изображения</h1>

<br>

##  Подготовка:
- Инсталирайте следните пакети от HACS или посетете хранилищата на следните линкове 🔽:
  - [Local Conditional card](https://github.com/PiotrMachowski/Home-Assistant-Lovelace-Local-Conditional-card)
  - [Vertical Stack In Card](https://github.com/ofekashery/vertical-stack-in-card)
  - [mini-graph-card](https://github.com/kalkih/mini-graph-card)

  След инсталацията на пакетите рестартирайте своят Home Assistant и преминете към следващите стъпки 🔽:

## Създаване:
- **Създаване на нов списък:** създайте нов списък към, който ще препращате имената на натиснатите от Вас артикули. 🔽:
  
  <img align="center" src="https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/create_list.gif?raw=true">      

- **Табло Shoplist:** Създайте ново Табло с името "Shoplist" и там ще създадем всички необходими списъци. 🔽:


  <img align="center" src="https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/create_shoplist_dashboard.gif?raw=true">  
  
  - Добавете, като нова карта списъкът, който ще използвате. 🔽:
    
    ![image](https://github.com/user-attachments/assets/62fe909f-7019-4958-8b1c-4187b441959a)

    Ръчно добавяне на карта 🔽:
    
    🛠️
    
        type: todo-list
        entity: todo.list

  - **Артикули:** неоходимо е еднократно създаване на "Помощник Бутон", който ще използваме за добавянето на артикулите. Не е необходимо създаването за всеки един артикул, понеже така или иначе не е възможно натискането на няколко артикула едновременно. Бутонът позволява при всяко добавяне да се правят индивидуални промени. 🔽:

    ![image](https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/create_button_helper.gif?raw=true)
    
   
  - **Категории:** за всяка една категоря е необходимо да се създаде помощник. Ако използвате един и същ помощник за всички категории, при отваряне на една категория, ще се отварят и всички други. Като удобство, ще добавим и автоматизация, която ще затваря всички останали категории при отваряне на категория. 🔽:
    
    ![image](https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/create_Namenskategorie_helpers.gif?raw=true)

    След създаването на помощникът за категорията, я добавете като карта в таблото. 🔽:

    ![image](https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/shoplist_und_kategodie.gif?raw=true)

    Ръчно добавяне на карта 🔽:

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
    
      ⚠️ Вкарайте картата с кодът точно така както е даден, като замените само "- entity: input_boolean.namenskategorie" с името на помошникът (категория). След като го запазите, можете лесно да промените останалите работи през графиката на картата или да добавите нов артикул. 🔽:
  
  
      ![image](https://github.com/Bacard1/HomeAssistant-Bulgaria/blob/main/Statik/IMG/GIF/kategorie_card_edit.gif?raw=true)
    
  - [ТУК](https://github.com/Bacard1/HomeAssistant-Bulgaria/tree/main/Statik/Projekts/Home-Assistant-Shoplist/IMG) ще намерите изчерпателна колекция от изображения за вашият списък за пазаруване, разделени по категории.
<br>

##  Автоматизации:

  ⚠️ Автоматизациите по долу не са задължителни, а препоръчителни. Липсата им няма да попречи на работата на списъкът за пазаруване, но ще я улесни.

- **1. Следи за вече отметнати продукти и активира чистача. 🔽:**

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

- **2. Чистач на списък с отметнати продукти -** тази автоматизация ще изчиства всички отметнати продукти след престой от 10 минути. Това не ви ангажира и обърква при ежедневна употреба. 🔽:

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

  ⚠️ Автоматизации 1 и 2 вървят комлект понеже едната активира другата!

- **3. Само една отворена категория:** тази автоматизация не позволява отварянето на повече от една категория. При отваряне на категория, всички останали се затварят. 🔽:

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
  
- **4. Нов артикул в списъкът за пазаруване:** изпраща известие до всички мобилни устройства за добавянето на нов артикул към списъкът.🔽:

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
