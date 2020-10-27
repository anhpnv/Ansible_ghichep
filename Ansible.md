1. Thứ tự ưu tiên của các variable
2. roles trong playbook
3. Tham số cấu hình trong ansible.cfg

## 1. Thứ tự ưu tiên của các variable

1. Variable truyền trực tiếp trong command line
Ví dụ : 
```sh
ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"
```
2. Variable được định nghĩa trong file role/defaults/main.yml
3. inventory file or script group vars
4. inventory group_vars/all 
5. playbook group_vars/all 
6. inventory group_vars/* 
7. playbook group_vars/* 
8. inventory file or script host vars
9.  inventory host_vars/*
10. playbook host_vars/*
11. host facts / cached set_facts
12. play vars
13. play vars_prompt
14. play vars_files
15. role vars (defined in role/vars/main.yml)
16. block vars (only for tasks in block)
17. task vars (only for the task)
18. include_vars
19. set_facts / registered vars	
20. role (and include_role) params
21. include params
22. extra vars (for example, -e "user=my_user")(always win precedence)

## 2. Roles trong playbook

Trong Ansible, Role là một cơ chế để tách 1 playbook ra thành nhiều file. Việc này nhằm đơn giản hoá việc viết các playbook phức tạp và có thể tái sử dụng lại nhiều lần. Mỗi role là một thành phần độc lập, bao gồm nhiều variables, tasks, files, templates, và modules bên dưới

Một role sẽ có 7 folder với các chức năng khác nhau gồm: vars, templates, handlers, files, meta, tasks và defaults. Mỗi một thư mục cần phải chứa 1 file main.yml. Trong đó thì tasks thường là folder quan trọng nhất, thường dùng để chứa những playbook

Trong đó:

1. tasks – chứa danh sách các task chính được thực thi trong role này.
2. handlers – chứa các handler, có thể được dùng trong role này hoặc các role khác.
3. defaults – chứa các biến được dùng default cho role này
4. vars – chứa thông tin các biến dùng trong role, biến trong vars sẽ override biến trong default
5. files – chứa các file cần dùng để deploy trong role này, cụ thể như file binary, file cài đặt…
6. templates – chứa các file template theo jinja format đuôi *.j2 (có thể là file config, file systemd…).
7. meta – định nghĩa 1 số metadata của role này, như là dependencies

Một role cần phải có ít nhất 1 trong 7 thư mục này

## 3. Tham số cấu hình trong ansible.cfg

Ansible hỗ trợ một số nguồn để định cấu hình hành vi của nó, bao gồm một tệp ini có tên ansible.cfg, các biến môi trường, command-line options,playbook keywords và các biến. Xem Kiểm soát cách Ansible hoạt động: quy tắc ưu tiên để biết chi tiết về mức độ ưu tiên tương đối của từng nguồn.

