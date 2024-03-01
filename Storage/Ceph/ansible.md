**Defaults/Vars**       


Defaults/Vars là nơi để chứa các biến cần thiết cho role. Lưu ý là các biến trong vars sẽ override các biến trong defaults
**Templates**       


Templates là nơi bạn chứa các file config cần điểu chỉnh biến, Ansible sẽ lấy các biến có trong defaults/vars để điền vào file template của các bạn. Dùng folder template bằng module template  


**Handlers** dùng để trigger một số thao tác như reload/restart/start stop service khi thực hiện một task nào đó trong playbook bằng lệnh notify                


**Tasks** là nơi ta viết các bước setup cụ thể cho role của chúng ta. Viết như 1 playbook bình thường.