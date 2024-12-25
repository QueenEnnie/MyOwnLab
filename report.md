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

    THRESHOLD=10
    LOGFILE="/var/log/disk_check.log"
    WARNING_FILE="/var/log/low_space_warning"
    DISK_USAGE=$(df -h / | grep '/' | awk '{print $5}' | sed 's/%//')
    DATE=$(date '+%Y-%m-%d %H:%M:%S')
    
    echo "$DATE Checking disk space..." >> $LOGFILE
    
    if [ "$DISK_USAGE" -ge "$((100 - THRESHOLD))" ]; then
        echo "$DATE WARNING: Free space is below ${DISK_USAGE}% (used: ${DISK_USAGE}%)" >> $LOGFILE
        echo "Free space is critically low: ${DISK_USAGE}% used." > $WARNING_FILE
    else
        echo "$DATE INFO: Disk space is sufficient (used: ${DISK_USAGE}%)" >> $LOGFILE
        [ -f $WARNING_FILE ] && rm -f $WARNING_FILE
    fi
    ```
   ![2024-12-25_23-09-45](https://github.com/user-attachments/assets/0f54033b-9e08-4b20-a8dd-00f5e656b67d)
   
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

