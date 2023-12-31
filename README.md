# ESPHome-CoffeMaker-Philips5400
Компонент для управления кофеваркой Philips 5400  из Home Assistants на базе ESPHome

Написан на основе исследований, в качестве помощи для Divan (https://github.com/DivanX10).
Модуль на базе  ESP32 перехватывает протокол обмена основной платы и дисплея кофмашины. 
![image](https://github.com/Brokly/ESPHome-CoffeMaker-Philips5400/assets/11642286/15a2a912-50da-4a9e-9f31-052b7b99fb70)
Позволяет полноценное удаленное управление кофеваркой. Это стало возможно благодаря частичной расшифровке протокола.

Настройки доступные в YAML

<b>philips_series_5400:</b><br>
  <b>display_uart:</b> - uart подключенный к плате дисплея кофемашины<br>
  <b>mainboard_uart:</b> - uart подключенный к основной плате кофемашины<br>
  <b>restore_value:</b> - логический параметр (true/false), восстанавливать ли настройки кофе при перезагрузке<br>
  <b>debug:</b> - логический параметр (true/false), режим отладки, позволяет изучить синтез пакета рецепта кофе<br>
  <b>B0_msg:</b> - текстовый сенсор, показывает содержимое перехваченного пакета с заголовком 0xB0<br>
  <b>B5_msg:</b> - текстовый сенсор, показывает содержимое перехваченного пакета с заголовком 0xB5<br>
  <b>BA_msg:</b> - текстовый сенсор, показывает содержимое перехваченного пакета с заголовком 0xBA<br>
  <b>90_msg:</b> - текстовый сенсор, показывает содержимое перехваченного пакета с заголовком 0x90 (пакет рецепта)<br>
  <b>91_msg:</b> - текстовый сенсор, показывает содержимое перехваченного пакета с заголовком 0x90<br>
  <b>93_msg:</b> - текстовый сенсор, показывает содержимое перехваченного пакета с заголовком 0x90<br>
  <b>Coffee_volume:</b> - number, объем кофе, (поддерживает все стандартные параметры mode, min_value, max_value, step и т.д.)<br>
  <b>Milk_volume:</b> - number, объем молока, (поддерживает все стандартные параметры mode, min_value, max_value, step и т.д.)<br>
  <b>Strength_select:</b> - select, помол кофе, (поддерживает стандартные свойства, option - позволяет заменить английские названия вашими, всего 4 строки)<br>
  <b>Cups_select:</b> - select, количество чашек или ExtraShot, (поддерживает стандартные свойства, option - позволяет заменить английские названия вашими, всего 3 строки)<br>
  <b>Product_select:</b> - select, выбор рецепта кофе, (поддерживает стандартные свойства, option - позволяет заменить английские названия вашими, всего 14 строк, ПРИМЕР:<br>
    <b>options:<br>
      - "Эспрессо"<br>
      - "Кофе с собой"<br>
      - "Черный кофе"<br>
      - "Эспрессо лунго"<br>
      - "Кафе крема"<br>
      - "Ристретто"<br>
      - "Американо"<br>
      - "Кофе с молоком"<br>
      - "Кофе латте"<br>
      - "Кофе с молоком с собой"<br>
      - "Латте макиато"<br>
      - "Капучино"<br>
      - "Молочная пена"<br>
      - "Горячая вода"</b>     
  )<br>
  <b>Display_product:</b> текстовый сенсор, напиток который в данный момент готовит или будет готовить кофевака, текстовая хьюман расшифровка.<br>
  <b>Start_key:</b> - кнопка запуска приготовления напитка в соответствии с установленным рецептом<br>
  <b>state_sensor:</b> - сенсор, в который компонент публикует статус в цифровом виде:<br><br>
   <i>1 - Не расшифрован (07 01 00)<br>
   2 - Выключена<br>
   3 - Подача молока<br>
   4 - Подача кофе<br>
   5 - Дозирование<br>
   6 - Создание пара для молока<br>
   7 - Заварочный узел в положение заваривания<br>
   8 - Напиток готов<br>
   9 - Нагрев воды<br>
   10 - Перемалывание зерна<br>
   11 - Промывка<br>
   12 - Закончилось зерно<br>
   13 - Не расшифрован (07 08 14)<br>
   14 - Нагрев<br>
   15 - Готово<br>
   16 - Не расшифрован (07 0C 02)<br>
   17 - Вода в норме<br>
   18 - Контейнер вставлен<br>
   20 - Опорожните контейнер для гущи<br>
   21 - Готова к работе                   
   22 - Долейте воду<br>
   23 - Контейнер извлечен<br>
   24 - Удаление накипи стадия 1<br>
   25 - Удаление накипи стадия 2<br>
   30...37 - Коды ошибок (не расшифрованы)<br><br></i>

    Пример использования:
    on_value:
      then:
      - lambda: |-
         if(x==2){
            id(idPowerStatus).publish_state(false);
         } else if(id(idPowerStatus).state==false){
            id(idPowerStatus).publish_state(true);
         }
         if(x==3){id(idStatus).publish_state("Наслаждайтесь");}
         else if(x==4){id(idStatus).publish_state("Наливаю кофе");}
         else if(x==5){id(idStatus).publish_state("Нагрев воды");}
         // И ТАК ДАЛЕЕ

Больше информации : https://github.com/DivanX10/ESP-Philips-5400-Coffee-Machine
