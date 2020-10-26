Tìm hiểu về Ansible:
## 1. Playbook
Playbook là 1 file được định dạng YAML format

Một playbook bao gồm 1 hoặc nhiều plays trong 1 list. Trong mỗi 'play' các task sẽ chạy từ trên xuống dưới.Các playbooks với nhiều 'plays' có thể triển khai điều phối trên nhiều máy chủ. Mỗi play chứa các thành phần:
* Các host quản lý
* Các task thực thi trên các host nói trên

Mặc định, Ansible sẽ thực thi các task cho các host tương ứng được khai báo trong playbook. 

Mục đích của play là map host hoặc 1 nhóm các host với 1 số roles xác định, mỗi role sẽ thực hiện các task nào đó. Ở mức độ cơ bản, 1 task đơn giản chỉ là 1 lời gọi tới 1 ansible module. Modules (còn được gọi là 'task plugins' hay 'library plugins') là nơi thực hiện các công việc thực sự trong ansible, chúng là những gì được thực hiện trong mỗi playbook task.

Mỗi module sẽ có các tham số truyền vào là khác nhau, và hầu như tất cả các module đều có tham số theo dạng key=value, các tham số cách nhau bởi dấu cách. Ngoài ra cũng có 1 số module không có tham số như ping...

Trong playbook, các Ansible module được sử dụng như sau:

```yaml
- name: show return value of command module
  hosts: vietanh
  tasks:
    - name: capture output of whoami command
      command: whoami
      register: login
    - debug: msg="{{ login.stdout }} is result of command whoami"
```

Một cách khác để truyền tham số vào 1 module, đó là sử dụng 'complex args':

```yaml
- name: restart webserver
  service:
    name: httpd
    state: restarted
```
Tất cả các output đều trả về dưới dạng json format
- **apt**: quản lý gói apt package
- **service**: Start, stop, restart service
- **command**: Lệnh thực thi
- **copy**: Thực hiện copy file từ máy local tới các máy hosts
...

Demo tìm hiểu về Ansible playbook
Tạo một thư mục có cấu trúc sau:

```
/playbooks
     whoami.yml
     hosts
```
Trong đó **whoami.yml** là file playbook, **hosts** là file chứa thông tin các host

File whoami.yml:
```yaml
- name: show return value of command module
  hosts: vietanh
  tasks:
    - name: capture output of whoami command
      command: whoami
      register: login
    - debug: msg="{{ login.stdout }} is result of command whoami"
```

File hosts:
```
vietanh ansible_host=172.17.0.2 ansible_ssh_user=vietanh ansible_ssh_pass=1
```
Trong ví dụ này, ta định nghĩa thông tin máy host trong file **hosts** và định nghĩa task cần thực thi trong file **whoami.yml** 

Để chạy file playbook, ta sử dụng lệnh sau:
```
onepiece@onepiece:~/Documents/Ansible-Tutorial/playbook$ ansible-playbook -i hosts whoami.yml 

PLAY [show return value of command module] *******************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************
ok: [vietanh]

TASK [capture output of whoami command] **********************************************************************************************************
changed: [vietanh]

TASK [debug] *************************************************************************************************************************************
ok: [vietanh] => {
    "msg": "vietanh is result of command whoami"
}

PLAY RECAP ***************************************************************************************************************************************
vietanh                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
Trong phần out, như đã thấy, 2 task [capture output of whoami command] và [debug] đã thực hiện thành công, tuy nhiên có 1 task được thực thi trước đó, đấy là [Gathering Facts]

Trước khi thực hiện các task trong playbook, ansible sẽ thực hiện task Gathering Facts sử dụng để thu thập thông tin về máy nó connect tới.

