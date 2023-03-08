LVM (Logical Volume Manager) là một công cụ quản lý không gian đĩa linh hoạt trong hệ thống Linux. LVM cho phép tạo ra các phân vùng ảo (logical volumes) và quản lý chúng như các phân vùng thông thường. Trong phần này, chúng ta sẽ tìm hiểu cách tạo, thay đổi kích thước và xóa các phân vùng ảo bằng LVM.

Lưu ý: Trước khi thực hiện bất kỳ thao tác nào trên LVM, hãy đảm bảo sao lưu toàn bộ dữ liệu trên các phân vùng liên quan để tránh mất dữ liệu.

**Tạo Physical Volume (PV)** 
   
Physical Volume là một phân vùng trên một hoặc nhiều ổ đĩa được sử dụng bởi LVM. Để tạo một Physical Volume, ta sẽ sử dụng lệnh pvcreate với đối số là đường dẫn tới ổ đĩa cần tạo.

Ví dụ, để tạo một Physical Volume trên ổ đĩa /dev/sdb, ta sử dụng lệnh sau:


```
sudo pvcreate /dev/sdb
```
**Tạo Volume Group (VG)   **
Volume Group là một nhóm các Physical Volume được sử dụng bởi LVM. Một khi đã tạo Physical Volume, ta sẽ sử dụng lệnh vgcreate để tạo một Volume Group. Ví dụ, để tạo một Volume Group có tên là myvg với Physical Volume /dev/sdb, ta sử dụng lệnh sau:


```
sudo vgcreate myvg /dev/sdb
```
**Tạo Logical Volume (LV)**    
Logical Volume là một phân vùng ảo được tạo từ một Volume Group. Ta sử dụng lệnh lvcreate để tạo một Logical Volume mới. Ví dụ, để tạo một Logical Volume có tên là mylv với kích thước 10GB từ Volume Group myvg, ta sử dụng lệnh sau:

```
sudo lvcreate -L 10G -n mylv myvg       
```
**Thay đổi kích thước Logical Volume**      
Để thay đổi kích thước của một Logical Volume đã có, ta sử dụng lệnh lvresize. Ví dụ, để tăng kích thước của Logical Volume mylv lên 20GB, ta sử dụng lệnh sau:


```
sudo lvresize -L +20G /dev/myvg/mylv
```
Lệnh này sẽ tăng kích thước của Logical Volume mylv lên 20GB.

**Xóa Logical Volume**
Để xóa một Logical Volume, ta sử dụng lệnh lvremove. Ví dụ, để xóa Logical Volume mylv, ta sử dụng lệnh sau:


```
sudo lvremove /dev/myvg/mylv
```
**Xóa Volume Group**    
Để xóa một Volume Group, ta sử dụng lệnh vgremove. Ví dụ, để xóa Volume Group myvg, ta sử dụng lệnh sau:

```
sudo vgremove myvg
```
**Xóa Physical Volume**     
Để xóa một Physical Volume, ta sử dụng lệnh pvremove. Ví dụ, để xóa Physical Volume /dev/sdb, ta sử dụng lệnh sau:


```
sudo pvremove /dev/sdb
```