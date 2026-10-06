# Báo cáo Bài 5: Khôi phục trạng thái và Đảo ngược commit (Reset vs Revert)

## 1. Mục tiêu
- Ứng dụng thành thạo hai cơ chế reset và revert để sửa chữa sai sót trong lịch sử dự án.
- Phân biệt sự khác nhau giữa khôi phục lịch sử cục bộ và đảo ngược lịch sử an toàn khi làm việc nhóm.

## 2. Các bước thực hành

Giả sử trong quá trình làm việc, em đã tạo một commit chứa mã nguồn bị lỗi (ví dụ: commit có message `"Update code (loi)"`). Dưới đây là 2 cách em xử lý:

### Trường hợp 1 (Sửa đổi cục bộ với `git reset`)
Khi đoạn code lỗi mới chỉ nằm ở local (chưa đẩy lên GitHub), em có thể dùng `git reset` để lùi lại 1 commit trước đó nhưng vẫn giữ lại các thay đổi trong file để tiến hành sửa lại code.

```bash
git reset HEAD~1
```
Lệnh trên là dạng mixed reset (mặc định), nó sẽ xóa commit lỗi khỏi lịch sử nhưng giữ nguyên các file vật lý ở dạng Modified (Unstaged). Kiểm tra bằng lệnh `git status` sẽ thấy:
```
Unstaged changes after reset:
M	file.txt
```

### Trường hợp 2 (Sửa đổi công cộng với `git revert`)
Nếu commit lỗi đã được đẩy lên nhánh chung trên remote (GitHub), việc sử dụng reset sẽ làm sai lệch lịch sử của những người khác trong nhóm. Thay vào đó, em dùng `git revert` để tạo ra một commit mới, trong đó chứa nội dung đảo ngược hoàn toàn (undo) các thay đổi của commit lỗi đó.

```bash
git revert HEAD --no-edit
```
Kết quả là một commit mới có tiêu đề bắt đầu bằng `Revert "Update code (loi)"` được tạo ra một cách an toàn mà không làm mất đi các commit cũ trong lịch sử.

## 3. Sự khác biệt giữa Reset và Revert
- **Reset (Dành cho Local):** Sửa đổi trực tiếp và "xóa sổ" các commit khỏi lịch sử. Rất hữu ích khi dọn dẹp các commit rác trước khi push, nhưng **nghiêm cấm** sử dụng trên các nhánh đã push lên remote (public branch) vì nó sẽ gây xung đột lịch sử cho các thành viên khác.
- **Revert (Dành cho Teamwork):** Không xóa commit cũ mà tạo ra một commit mới để "bù trừ" lại lỗi. Đây là cách an toàn và chuẩn mực nhất để sửa lỗi trên nhánh chung vì nó bảo toàn toàn bộ lịch sử dự án để ai cũng có thể theo dõi.

## 4. Kiểm tra lịch sử (Ảnh chụp chứng minh)
**Hướng dẫn chụp ảnh:**
1. Mở terminal tại thư mục bài tập này.
2. Chạy lệnh: `git log --oneline -n 5`
3. Chụp ảnh toàn bộ màn hình hoặc khu vực terminal hiển thị có chứa dòng chữ `Revert "..."`.
4. Kéo thả hoặc dán ảnh vào ngay dưới dòng này, thay thế cho chữ `ImgEx` ở bên trong ngoặc:

![ImgEx]()
