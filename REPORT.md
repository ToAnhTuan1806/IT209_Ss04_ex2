# Báo Cáo Giải Quyết Xung Đột Nhánh (Merge Conflict)

## 1. Các Bước Thực Hiện và Giải Quyết Xung Đột Thủ Công

### Bước 1: Khởi tạo và tạo commit ban đầu trên `main`
```bash
git init -b main
# Tạo README.md ban đầu
git add README.md
git commit -m "feat: initial commit with basic README"
```

### Bước 2: Tạo nhánh `feature-update` và chỉnh sửa
```bash
git checkout -b feature-update
# Chỉnh sửa nội dung tại phần '## Giới thiệu' trong README.md
git add README.md
git commit -m "feat(readme): update system description on feature-update branch"
```

### Bước 3: Chuyển về `main` và sửa cùng vị trí để tạo xung đột
```bash
git checkout main
# Chỉnh sửa cùng phần '## Giới thiệu' trong README.md với nội dung khác
git add README.md
git commit -m "docs(readme): update system overview on main branch"
```

### Bước 4: Thực hiện gộp nhánh và xuất hiện xung đột
```bash
git merge feature-update
```
*Kết quả:* Git báo lỗi xung đột do cả 2 nhánh cùng sửa trên cùng một dòng:
```text
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

### Bước 5: Xử lý xung đột thủ công trong `README.md`
Khi mở `README.md`, Git đánh dấu xung đột dạng:
```markdown
<<<<<<< HEAD
Phiên bản cập nhật chính thức trên main: v1.0.1-stable.
Hệ thống cập nhật tài liệu hướng dẫn triển khai trên Kubernetes cluster.
=======
Phiên bản cập nhật trên nhánh feature-update: v1.1.0-beta.
Tính năng: Tích hợp Docker containerization và CI pipeline.
>>>>>>> feature-update
```
- `<<<<<<< HEAD`: Nội dung trên nhánh hiện tại (`main`).
- `=======`: Đường ranh giới phân tách 2 nhánh.
- `>>>>>>> feature-update`: Nội dung từ nhánh gộp vào (`feature-update`).

**Cách giải quyết:** Xóa bỏ hoàn toàn các ký hiệu đánh dấu (`<<<<<<<`, `=======`, `>>>>>>>`), chọn lọc và dung hòa nội dung của cả 2 nhánh thành:
```markdown
## Giới thiệu
Phiên bản tích hợp hoàn chỉnh (Merge Conflict Resolved):
- Phiên bản: v1.1.0 (kết hợp tính năng từ `feature-update` và tài liệu từ `main`).
- Hệ thống hỗ trợ Docker containerization, CI/CD pipeline và tài liệu hướng dẫn triển khai trên Kubernetes cluster.
```

### Bước 6: Hoàn tất commit gộp nhánh
```bash
git add README.md
git commit -m "Merge branch 'feature-update' into main - resolve conflict in README.md"
```

---

## 2. Kết Quả Lịch Sử Commit Đồ Thị Nhánh (`git log --graph --oneline`)

```text
*   fc13557 Merge branch 'feature-update' into main - resolve conflict in README.md
|\  
| * 7b89807 feat(readme): update system description on feature-update branch
* | 1423c68 docs(readme): update system overview on main branch
|/  
* 3a3ca5a feat: initial commit with basic README
```

- **Giải thích đồ thị (3-Way Merge):**
  - `3a3ca5a`: Commit gốc ban đầu (Base / Common Ancestor).
  - `7b89807`: Commit trên nhánh `feature-update`.
  - `1423c68`: Commit trên nhánh `main`.
  - `fc13557`: Commit gộp (Merge Commit) có 2 nhánh tổ tiên sau khi giải quyết xung đột thủ công thành công.
