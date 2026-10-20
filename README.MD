# OpenVPN в Yandex cloud

Ansible playbook for deploying OpenVPN with a single click in Yandex Cloud.

## Для чего?

Бывают ситуации, когда нужен доступ к российскому веб-сайту, заблокированному для посещения из-за рубежа. При этом держать созданную виртуальную машину круглосуточно не нужно.

Yandex.Cloud позволяет создать недорогую виртуальную машину preemptive (прерываемую). Этот скрипт поможет вам создать такую виртуальную машину за один клик.

Используется образ [dockovpn](https://github.com/dockovpn/dockovpn) для OpenVPN и плагин для Ansible - [ansible-module-yandex-cloud](https://github.com/patsevanton/ansible-module-yandex-cloud.git).

## Настройка

1. Получите OAuth токен по инструкции [здесь](https://yandex.cloud/ru/docs/iam/concepts/authorization/oauth-token).
2. Найдите в Yandex.Cloud панеле следующие параметры:
   - `folder_id` (идентификатор каталога)
   - `subnet_id` (идентификатор подсети)
   Подробнее о нахождении этих параметров можно узнать [здесь](https://yandex.cloud/ru/docs/resource-manager/operations/folder/get-id#console_1).
3. Сгенерируйте SSH ключ, если его нету:
   ```shell
   ssh-keygen -t ed25519
   ```
4. Создать файл `vars.yml`
```yaml
yc_oauth_token: "y0_AgA .... token"
ssh_private_key: "/root/.ssh/id_ed25519"
folder_id: "b1gr... c"
subnet_id: "e9bi... c"
```
Примечание: В примере путь до ключа указывать относительно контейнера. 

## Запуск

### Docker compose

```commandline
docker compose up
```

### Linux/Mac

_TBD_

### Docker

_TBD_


После успешного выполнения playbook (обычно это занимает около 5 минут) файл `client.ovpn` будет автоматически скачан в текущую директорию.

### Пересоздать конфиг на уже существующей машине

Если конфиг потерялся или нужен новый, можно пересоздать контейнер (и получить новый `client.ovpn`) без создания новой виртуалки:

```commandline
VM_IP=<IP_вашей_ВМ> docker compose up
```

Установка docker и создание ВМ при этом пропускаются.

## Alternatives:
- https://github.com/nikitamishagin/ownvpn
- https://github.com/ptr-dorjin/ansible-vpn-server
- https://github.com/andrey-yantsen/yandex-cloud-vpn
- https://github.com/trailofbits/algo
