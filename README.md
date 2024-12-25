# Лабораторная работа: планировщик задач cron

---

#### **Описание работы**
В этой работе мы будем учиться использовать планировщик задач `cron` для автоматизации выполнения скриптов и команд, разбираться с синтаксисом crontab и настраивать задачи на регулярное выполнение, предварительно познакомившись с основными командами.

---

Перед выполнеием лабораторной работы следует установить cron на свою виртуальную машину и подключить его.

### **Разминка (в отчет не включается)**

Прежде, чем выполнять задание, познакомимся с базовыми принципами работы с cron.

#### **Шаги для выполнения**

1. **Создание скрипта для логирования**
   
   Создайте файл `log_system_info.sh`, в который будет записываться скрипт:
   
   ```bash
   nano ~/log_system_info.sh
   ```
   
   Добавьте следующий код:
   
   ```bash
   #!/bin/bash
   echo "System Information as of $(date):" >> ~/system_info.log
   echo "-----------------------------" >> ~/system_info.log
   uptime >> ~/system_info.log
   df -h >> ~/system_info.log
   echo "" >> ~/system_info.log
   ```
   
   Сделайте скрипт исполняемым:
   
   ```bash
   chmod +x ~/log_system_info.sh
   ```

2. **Добавление задания в `crontab`**
   
   Откройте редактор crontab:
   
   ```bash
   crontab -e
   ```
   
   Добавьте строку для запуска скрипта каждые 2 минуты:
   
   ```bash
   */2 * * * * ~/log_system_info.sh
   ```

3. **Проверка работы**
   
   Подождите несколько минут и проверьте содержимое файла лога:
   
   ```bash
   cat ~/system_info.log
   ```

---

#### **Задание**

Настроить автоматизированную задачу с помощью `cron`, которая:  

1. Каждые две минуты:  
   - Проверяет, осталось ли свободное место на диске менее 10%.  
   - Если места меньше 10%, создаёт предупреждающий файл `/var/log/low_space_warning` и логирует проблему.  
   - Если места достаточно (больше 10%), логирует, что всё в порядке.  
2. Все результаты проверки записываются в файл `/var/log/disk_check.log`.  
---

## Ресурсы

1. [Crontab](https://man7.org/linux/man-pages/man5/crontab.5.html)
2. [Cron Examples](https://rakeshjain-devops.medium.com/cron-examples-4233b873e5f9)
3. [What Is Crontab Syntax: Understanding Crontab on Linux + Helpful Examples](https://www.hostinger.com/tutorials/crontab-syntax)
4. [‘crontab’ in Linux with Examples](https://www.geeksforgeeks.org/crontab-in-linux-with-examples/#what-is-linux-crontab)
