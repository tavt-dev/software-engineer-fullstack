# Học Git & GitHub

**Trạng thái:** Đang học.

Các lệnh thao tác file trong phần đầu dùng cú pháp **Git Bash** (cũng phù hợp với terminal trên macOS/Linux). Trên Windows, mở Git Bash để chạy đúng ví dụ; PowerShell có một số lệnh và tham số khác.

## 1. Làm quen với file, thư mục và terminal

| Lệnh | Công dụng |
| --- | --- |
| `pwd` | Xem đường dẫn thư mục hiện tại. |
| `cd <thu-muc>` | Đi vào một thư mục. Dùng dấu nháy nếu tên có khoảng trắng. |
| `cd ..` | Quay về thư mục cha. |
| `ls` | Liệt kê file và thư mục; trên Windows CMD/PowerShell có thể dùng `dir`. |
| `mkdir <ten-thu-muc>` | Tạo thư mục mới, ví dụ `mkdir ThuMuc1`. |
| `touch <ten-file>` | Tạo file rỗng hoặc cập nhật thời gian sửa file, ví dụ `touch 'vi du.txt'`. |
| `echo "noi dung" > <ten-file>` | Ghi nội dung vào file, **ghi đè** nội dung cũ. |
| `echo "noi dung" >> <ten-file>` | Ghi thêm nội dung vào cuối file. |
| `cat <ten-file>` | Hiển thị nội dung file. |
| `diff <file-1> <file-2>` | So sánh nội dung hai file. |
| `rm <ten-file>` | Xóa file. |
| `rm -d <thu-muc>` | Xóa thư mục rỗng. |
| `rm -r <thu-muc>` | Xóa thư mục và nội dung bên trong; kiểm tra đường dẫn trước khi chạy. |

> Trong PowerShell, `touch` và `rm -d` không dùng giống Git Bash. Có thể tạo file bằng `New-Item <ten-file> -ItemType File` và xóa thư mục rỗng bằng `Remove-Item <thu-muc>`.

## 2. Hiểu Git và cấu hình ban đầu

**Thuật ngữ:** repository (repo) là kho lưu trữ; commit là một mốc ghi lại thay đổi; branch là nhánh phát triển; `main` hoặc `master` thường là tên nhánh chính. `develop` là tên nhánh do một số nhóm sử dụng. Merge và rebase đều kết hợp lịch sử giữa các nhánh, nhưng theo cách khác nhau.

| Lệnh | Công dụng |
| --- | --- |
| `git --version` | Xem phiên bản Git. |
| `git --help` | Xem trợ giúp và các lệnh Git thường dùng. |
| `git help <lenh>` | Xem hướng dẫn của một lệnh, ví dụ `git help commit`. |
| `git config --list` | Xem cấu hình Git đang áp dụng. |
| `git config --global user.name "Ten cua ban"` | Đặt tên tác giả cho các repo của người dùng hiện tại. |
| `git config --global user.email "email@example.com"` | Đặt email tác giả cho các repo của người dùng hiện tại. |
| `git config --local user.name "Ten cua ban"` | Đặt tên chỉ cho repo hiện tại. |
| `git config --local user.email "email@example.com"` | Đặt email chỉ cho repo hiện tại. |

Phạm vi cấu hình: `--local` cho repo hiện tại, `--global` cho người dùng hiện tại, `--system` cho toàn máy. `git config --list` là lệnh **xem** cấu hình; khi **đặt** một giá trị, dùng `git config --<pham-vi> <ten-cau-hinh> <gia-tri>`.

## 3. Tạo repo và lưu thay đổi ở máy

| Lệnh | Công dụng |
| --- | --- |
| `git init <ten-thu-muc>` | Tạo repo Git mới trong thư mục chỉ định. |
| `git clone <url-hoac-duong-dan> [ten-thu-muc]` | Sao chép repo có sẵn về máy; tên thư mục đích là tùy chọn. |
| `git status` | Xem file đã sửa, file đang stage và nhánh hiện tại. |
| `git diff` | So sánh **working directory** với **staging area**; không hiển thị thay đổi đã stage. |
| `git diff --staged` | So sánh **staging area** với commit hiện tại (`HEAD`). |
| `git diff HEAD` | Xem toàn bộ thay đổi của file đã theo dõi so với `HEAD`. |
| `git add <ten-file>` | Đưa file được chọn vào staging area. |
| `git add .` | Stage các thay đổi bên dưới thư mục hiện tại; xem lại `git status` trước khi commit. |
| `git commit -m "Mo ta thay doi"` | Tạo commit từ những thay đổi đã stage. |
| `git log` | Xem lịch sử commit đầy đủ. |
| `git log --oneline` | Xem lịch sử commit dạng ngắn. |

Luồng cơ bản: **working directory → `git add` → staging area → `git commit` → lịch sử repo**. `HEAD` thường trỏ tới commit hiện tại của nhánh đang đứng.

## 4. Làm việc với remote

| Lệnh | Công dụng |
| --- | --- |
| `git remote -v` | Xem tên và địa chỉ các remote, thường có `origin`. |
| `git fetch origin` | Tải thông tin commit và nhánh mới từ remote; không tự gộp vào nhánh hiện tại. |
| `git pull origin main` | Lấy thay đổi từ `origin/main` và tích hợp vào nhánh hiện tại theo cấu hình Git. |
| `git push -u origin <ten-nhanh>` | Push nhánh lần đầu và đặt nhánh remote làm upstream. |
| `git push` | Push các commit tiếp theo khi nhánh đã có upstream. |

`git init --bare <ten-thu-muc>` tạo repo không có working directory, thường dùng làm remote tự quản lý. Chưa cần lệnh này để học GitHub cơ bản.

## 5. Làm việc với nhánh và merge

| Lệnh | Công dụng |
| --- | --- |
| `git branch` | Liệt kê các nhánh local; dấu `*` chỉ nhánh hiện tại. |
| `git branch <ten-nhanh>` | Tạo nhánh mới nhưng chưa chuyển sang nhánh đó. |
| `git switch -c <ten-nhanh>` | Tạo và chuyển sang nhánh mới. |
| `git switch <ten-nhanh>` | Chuyển sang nhánh đã có. Lệnh cũ tương đương là `git checkout <ten-nhanh>`. |
| `git merge <ten-nhanh>` | Gộp nhánh được chỉ định vào **nhánh hiện tại**. |
| `git branch -d <ten-nhanh>` | Xóa nhánh local đã merge. |
| `git push origin --delete <ten-nhanh>` | Xóa nhánh trên remote. |
| `git switch --detach <commit-hash>` | Xem repo tại một commit cũ mà không chuyển nhánh; `git checkout <commit-hash>` cũng vào trạng thái này. |

Trong repo này, `learning/git-basics` giữ ghi chép; `practice/git-basics` dùng để thử lệnh. Chỉ merge nội dung muốn lưu lâu dài vào `main`. Sau khi xem commit cũ, dùng `git switch <ten-nhanh>` để quay về nhánh làm việc.

## 6. Học sau khi đã vững luồng cơ bản

| Lệnh | Công dụng |
| --- | --- |
| `git rebase <ten-nhanh>` | Đặt lại các commit riêng của nhánh hiện tại lên đầu nhánh được chỉ định; có thể viết lại lịch sử commit. |
| `git rebase --continue` | Tiếp tục sau khi đã xử lý conflict và stage các file đã sửa. |
| `git rebase --skip` | Bỏ qua commit hiện tại trong quá trình rebase; có thể làm mất thay đổi của commit đó. |
| `git reset <commit-hash>` | Mặc định là `--mixed`: chuyển nhánh về commit đó, bỏ stage, giữ thay đổi trong working directory. |
| `git reset --soft <commit-hash>` | Chuyển nhánh về commit đó, giữ nguyên staging area và working directory. |
| `git reset --hard <commit-hash>` | Chuyển nhánh về commit đó, đặt lại cả staging area và file đã theo dõi; các thay đổi chưa commit có thể bị mất. |

Chỉ thử `reset --hard`, rebase và xóa nhánh trên nhánh thực hành khi đã hiểu tác động. Tránh viết lại lịch sử của nhánh mà người khác đang dùng.
