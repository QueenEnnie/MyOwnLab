# Лабораторная работа: планировщик задач cron

#### **Шаги для выполнения**

1. **Создание скрипта**
   
   Создаем файл `disk_check.sh`, в который будем записывать скрипт:
   
   ```bash
   nano ~/disk_check.sh
   ```
   
   Записываем следующий код:
   
    ```bash
   #!/bin/bash
   
   # Пороговое значение для свободного места на диске (в процентах)
   THRESHOLD=10
   
   # Путь к файлу логирования
   LOGFILE="/var/log/disk_check.log"
   
   # Путь к файлу предупреждения о низком свободном месте на диске
   WARNING_FILE="/var/log/low_space_warning"
   
   # Получаем процент использования диска для корневого раздела (/), используя команду df
   # Команда `df -h` выводит информацию о доступном месте на диске, `grep '/'` фильтрует строку с корневым разделом,
   # `awk '{print $5}'` извлекает процентное значение использования, а `sed 's/%//'` удаляет символ процента.
   DISK_USAGE=$(df -h / | grep '/' | awk '{print $5}' | sed 's/%//')
   
   # Получаем текущую дату и время в формате YYYY-MM-DD HH:MM:SS
   DATE=$(date '+%Y-%m-%d %H:%M:%S')
   
   # Записываем начало проверки в лог-файл
   echo "$DATE Checking disk space..." >> $LOGFILE
   
   # Проверяем, превышает ли процент использования диска пороговое значение
   # 100 - $THRESHOLD вычисляет пороговое значение для использования диска (например, если THRESHOLD=10, то это 90% использования)
   if [ "$DISK_USAGE" -ge "$((100 - THRESHOLD))" ]; then
       # Если процент использования диска больше или равен пороговому значению (диск заполнен на 90% или больше),
       # записываем предупреждение в лог и создаём файл с предупреждением.
       echo "$DATE WARNING: Free space is below ${DISK_USAGE}% (used: ${DISK_USAGE}%)" >> $LOGFILE
       echo "Free space is critically low: ${DISK_USAGE}% used." > $WARNING_FILE
   else
       # Если место на диске выше порогового значения, записываем информацию в лог.
       echo "$DATE INFO: Disk space is sufficient (used: ${DISK_USAGE}%)" >> $LOGFILE
       # Если файл предупреждения существует, удаляем его
       [ -f $WARNING_FILE ] && rm -f $WARNING_FILE
   fi

    ```

   ![2024-12-25_23-40-48](https://github.com/user-attachments/assets/c6906fa2-a449-4d7d-a36e-b81c5055d58c)

   Сделаем скрипт исполняемым:
   
   ```bash
   chmod +x ~/disk_check.sh
   ```

2. **Добавление задачи в `crontab`**
   
   Открываем редактор crontab:
   
   ```bash
   crontab -e
   ```

   Дописываем строку для запуска скрипта каждые 2 минуты:
   
   ```bash
   */2 * * * * ~/disk_check.sh
   ```
   
   ![2024-12-25_23-15-19](https://github.com/user-attachments/assets/85a0a50d-88ad-404d-823a-82b859f900a0)

3. **Проверка работы**
   
   Ждем несколько минут и проверяем содержимое файла лога:
   
   ```bash
   cat /var/log/disk_check.log
   ```

  ![2024-12-25_23-17-18](https://github.com/user-attachments/assets/49fe8e9c-273b-4957-86a7-fd7024f32da9)

