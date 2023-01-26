Для автоматизации используется Ansible

Установка:

sudo apt update && sudo apt -y install software-properties-common && sudo apt-add-repository ppa:ansible/ansible && sudo apt install -y ansible

Во время установки в какой-то момент потребуется нажать Enter на клавиатуре

Включение измерения времени:

 sudo vim /etc/ansible/ansible.cfg        (или воспользоваться другим редактором)

В под [defaults] прописать callback_whitelist = profile_tasks

Подготовка к запуску:

1) Сформировать файл hosts.txt по примеру из репозитория (указать свои ip адреса и пароли)
2) Сформировать ssh-ключи командой 

   ssh-keygen

4) Прокинуть ssh-ключи на сервера любым удобным способом. Я использую ssh-copy-id 

Запуск

 ansible-playbook -i hosts.txt playbook.yaml --extra-vars "user=$(whoami) ip_c=<Укажите ваш ip адрес сервера С>"
 
 Далее требуется зайти на сервер В и прописать следующие команды
 
sudo su - postgres
pg_ctlcluster 10 main stop
pg_ctlcluster 10 main start

Далее можно зайти на сервер С и подключиться к базам данных с помощью psql

P.S Чрезмерного употребления run_once и delegate_to можно избежать разделив playbook на две части (очень хотелось сделать это всё одним файлом). Большую часть проблем доставило конфигурирование БД, так как не имел опыта в этом, а нюансов оказалось достаточно (очень долго разбирался почему нельзя перезагружать службу через service/systemd)
