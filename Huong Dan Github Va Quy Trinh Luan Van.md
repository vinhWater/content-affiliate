# CẨM NANG LÀM VIỆC NHÓM TRÊN GITHUB & QUY TRÌNH NGHIỆM THU DỰ ÁN LUẬN VĂN (ĐH TIỀN GIANG)

Tài liệu này hướng dẫn chi tiết cách tổ chức mã nguồn, phân nhánh Git, phối hợp giữa các thành viên và quy trình tự tích hợp code cho 8 bạn sinh viên tham gia 2 dự án luận văn tốt nghiệp:

- **Dự án 1:** [AI Copilot Dự Toán Xây Dựng](https://github.com/vinhWater/ai-copilot-du-toan-xay-dung) (4 sinh viên – chia làm Module 1 và Module 2\)  
- **Dự án 2:** [Content Affiliate Platform](https://github.com/vinhWater/content-affiliate) (4 sinh viên – chia làm Module 1 và Module 2\)

## 1\. Nguyên tắc làm việc cốt lõi (Tự chủ Code – Nghiệm thu bằng Demo)

1. **Một Repository duy nhất cho mỗi Dự án:** Không sử dụng tính năng Fork. Cả 4 bạn trong cùng một dự án làm việc chung trên chính kho mã nguồn (repository) của dự án đó.  
2. **Nhánh `main` chứa trọn vẹn cả 2 Module:** Nhánh `main` là nơi chứa sản phẩm hoàn chỉnh cuối cùng (kết hợp cả Module 1 và Module 2 chạy liên thông với nhau).  
3. **Sinh viên tự Review và tự Merge code:** Mentor **không** can thiệp duyệt từng dòng code hay duyệt Pull Request hộ các bạn. Các bạn trong nhóm tự kiểm tra chéo (peer-review), tự merge code vào nhánh nhóm và nhánh `main`, đồng thời **tự chịu trách nhiệm 100% để code trên nhánh `main` luôn chạy được ổn định**.  
4. **Mentor chỉ nghiệm thu kết quả Demo thực tế:** Trong các buổi review định kỳ, nhóm sẽ chạy trực tiếp sản phẩm từ nhánh `main` (Working Software) để demo kết quả đầu ra.  
5. **Đánh giá đóng góp cá nhân minh bạch qua lịch sử Git:** Mọi đóng góp của từng bạn được ghi nhận tự động trên bảng thống kê `Insights -> Contributors` của GitHub dựa trên tài khoản cá nhân đã commit và mở Pull Request.

## 2\. Cấu trúc thư mục chuẩn trên nhánh `main`

Để nhóm làm **Module 1** và nhóm làm **Module 2** code song song mà không bao giờ dẫm chân lên file của nhau, nhánh `main` của mỗi dự án được chia sẵn thành các thư mục độc lập:

```
ten-du-an-repository/
├── README.md                     # Tài liệu tổng quan kiến trúc dự án
├── CONTRIBUTING.md               # Tài liệu hướng dẫn quy trình Git này
├── docker-compose.yml            # Cấu hình chạy Database (PostgreSQL/pgvector, Neo4j, n8n)
├── .env.example                  # Mẫu biến môi trường (tuyệt đối không đẩy file .env thật lên Git)
├── shared/                       # THƯ MỤC DÙNG CHUNG GIỮA 2 MODULE
│   ├── schemas/                  # Chứa JSON Schema (brief.v1, video_script.v1) & Pydantic Models
│   ├── database/                 # Script khởi tạo bảng DB dùng chung
│   └── mock_data/                # Dữ liệu mẫu để 2 Module tự test khi chưa ghép nối
├── module_1/                     # THƯ MỤC RIÊNG CỦA NHÓM MODULE 1 (2 sinh viên)
│   ├── agents/                   # Code các AI Agent thuộc Module 1
│   ├── services/                 # Xử lý nghiệp vụ, parser, crawler
│   ├── tests/                    # Kịch bản kiểm thử riêng của Module 1
│   └── requirements.txt
└── module_2/                     # THƯ MỤC RIÊNG CỦA NHÓM MODULE 2 (2 sinh viên)
    ├── agents/                   # Code các AI Agent thuộc Module 2
    ├── ui_or_workflow/           # Giao diện Dashboard (Next.js/Streamlit) hoặc workflow n8n
    ├── tests/                    # Kịch bản kiểm thử riêng của Module 2
    └── requirements.txt
```

**Quy tắc vàng về thư mục:**

- Nhóm Module 1 chỉ chỉnh sửa code bên trong thư mục `module_1/`.  
- Nhóm Module 2 chỉ chỉnh sửa code bên trong thư mục `module_2/`.  
- Khi cần cập nhật cấu trúc dữ liệu giao tiếp trong thư mục `shared/`, đại diện Module 1 và Module 2 phải nhắn tin thống nhất với nhau trước khi sửa.

## 3\. Quy ước đặt tên Nhánh (Branch) đơn giản, không nhầm lẫn

Để tránh nhầm lẫn giữa **Thư mục** và **Nhánh (Branch)**, toàn bộ tên nhánh dùng dấu gạch ngang (`-`), chia làm 3 cấp rõ ràng:

| Cấp độ nhánh | Tên nhánh quy ước | Ý nghĩa & Ai là người sử dụng |
| :---- | :---- | :---- |
| **Cấp 1: Nhánh Sản phẩm chính** | `main` | Chứa code hoàn chỉnh của **cả Module 1 và Module 2** đã ghép nối chạy ổn định để phục vụ **Demo với Mentor**. |
| **Cấp 2: Nhánh Chung của từng Module** | `dev-module-1`<br>`dev-module-2` | Nhánh dùng để 2 bạn trong cùng một Module ghép code với nhau và chạy thử nghiệm nội bộ Module đó. |
| **Cấp 3: Nhánh Cá nhân (Mỗi bạn 1 nhánh)** | **Dự án 1:** `sv-truc`, `sv-thinh` (M1) \| `sv-phong`, `sv-ngoc` (M2)<br>**Dự án 2:** `sv-bao`, `sv-duy` (M1) \| `sv-hoanganh`, `sv-giahan` (M2) | Nhánh làm việc hàng ngày của riêng từng bạn. **Mỗi sinh viên chỉ viết code và push trên nhánh tên mình**, tuyệt đối không code trực tiếp trên nhánh của bạn cùng nhóm hay trên `main`. |

## 4\. Quy trình 4 bước thao tác Git hàng tuần cho Sinh viên

### Bước 1: Tải dự án về máy và chuyển sang nhánh cá nhân (Chỉ làm lần đầu)

Mở Terminal trên VS Code hoặc Google Antigravity IDE và chạy:

```sh
# 1. Tải kho mã nguồn về máy (thay link bằng repo Dự án 1 hoặc Dự án 2)
git clone https://github.com/vinhWater/ai-copilot-du-toan-xay-dung.git
cd ai-copilot-du-toan-xay-dung

# 2. Tạo và chuyển sang nhánh cá nhân của bạn (ví dụ bạn Trực ở Module 1)
git checkout -b sv-truc

# 3. Đẩy nhánh cá nhân lên GitHub lần đầu
git push -u origin sv-truc
```

### Bước 2: Viết code hàng ngày trên nhánh cá nhân (`sv-<ten-ban>`)

Trước khi bắt đầu buổi làm việc, luôn kéo code mới nhất từ nhánh Module của nhóm về nhánh cá nhân để không bị lệch phiên bản:

```sh
# Đảm bảo bạn đang đứng ở nhánh cá nhân của mình
git checkout sv-truc

# Kéo code mới nhất từ nhánh chung của Module (dev-module-1 hoặc dev-module-2) về máy
git pull origin dev-module-1

# Sau khi viết code xong và chạy thử không lỗi trên máy cá nhân:
git add .
git commit -m "feat(module-1): hoan thien parser agent boc tach dieu khoan"
git push origin sv-truc
```

### Bước 3: Hai bạn cùng Module tự ghép code vào nhánh `dev-module-1` (hoặc `dev-module-2`)

Khi một bạn hoàn thành xong một phần việc (ví dụ mỗi 2–3 ngày):

1. Truy cập trang GitHub của dự án, bấm nút **Compare & pull request**.  
2. Chọn chiều ghép nhánh:  
   - **base:** `dev-module-1` *(nhánh chung của nhóm Module 1\)*  
   - **compare:** `sv-truc` *(nhánh cá nhân của bạn)*  
3. Bấm **Create pull request** và nhắn cho người bạn cùng nhóm (ví dụ bạn Thịnh).  
4. Người bạn cùng nhóm vào đọc qua các file thay đổi, kiểm tra xem có xung đột gì không rồi bấm **Merge pull request** để gộp code vào `dev-module-1`.  
5. Sau khi gộp xong, cả 2 bạn đều chạy lệnh `git pull origin dev-module-1` về máy để có chung bộ code mới nhất của nhóm.

### Bước 4: Tự ghép code từ `dev-module-1` và `dev-module-2` lên `main` trước buổi Demo

Muộn nhất vào **tối Thứ Sáu trước ngày họp Demo**, đại diện của 2 nhóm trong cùng dự án phối hợp thực hiện:

1. Nhóm Module 1 mở Pull Request từ `dev-module-1` vào `main` và bấm **Merge**.  
2. Nhóm Module 2 mở Pull Request từ `dev-module-2` vào `main` và bấm **Merge** (do 2 nhóm làm trên 2 thư mục `module_1/` và `module_2/` riêng biệt nên quá trình merge vào `main` sẽ hoàn toàn tự động, không bị xung đột).  
3. Một bạn đại diện kéo nhánh `main` mới nhất về máy (`git checkout main && git pull origin main`), bật chạy thử toàn bộ luồng từ đầu đến cuối để đảm bảo buổi Demo ngày Thứ Bảy diễn ra trơn tru.

## 5\. Những điều cấm kỵ cần tránh để không hỏng Git của nhóm

- **Không đẩy các thư mục cài đặt thư viện nặng lên Git:** Kiểm tra file `.gitignore` đã chặn sẵn các thư mục `venv/`, `.venv/`, `__pycache__/`, `node_modules/`, và các file dữ liệu thô dung lượng lớn (`.mp4`, `.zip`).  
- **Không dùng lệnh `git push --force` lên nhánh `main` hoặc `dev-module`:** Lệnh này sẽ xóa sạch lịch sử commit của các bạn khác trong nhóm.  
- **Nếu gặp thông báo Conflict (Xung đột code):** Khi GitHub báo `Can't automatically merge`, không tự ý xóa code của bạn cùng nhóm. Hai bạn trong nhóm bật Google Meet chia sẻ màn hình 5 phút để cùng chọn giữ lại đoạn code đúng rồi mới commit.

## 6\. Checklist chuẩn bị trước mỗi buổi Demo với Mentor

Trước giờ họp Demo (09h30 Thứ Bảy cho Dự án 1; 15h30 Thứ Bảy cho Dự án 2), các nhóm tự kiểm tra đủ 4 mục sau:

- [ ] Code mới nhất của cả 2 thành viên đã được merge đầy đủ vào nhánh `dev-module` và nhánh `main`.  
- [ ] Chương trình trên nhánh `main` đang bật sẵn và chạy mượt trên máy của người chia sẻ màn hình (không đợi đến lúc vào họp mới bắt đầu cài thư viện hay khởi động server).  
- [ ] Đã chuẩn bị sẵn dữ liệu đầu vào thực tế (bản vẽ/câu hỏi định mức cho Dự án 1; yêu cầu nghiên cứu sản phẩm affiliate cho Dự án 2\) để bấm chạy trực tiếp trong buổi họp.  
- [ ] Ghi chú ngắn gọn các vướng mắc kỹ thuật hoặc câu hỏi về nghiệp vụ/MVP để trao đổi trực tiếp với Mentor trong buổi họp.