# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421

Lớp: 58KTPM

**Bài tập 02:** 
# SỬ DỤNG DJANGO ĐỂ TẠO WEB QUẢN LÝ TIỆM CẦM ĐỒ
## deadline : 23h59 ngày 09 tháng 5 năm 2026.


1. TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ: viết tay ra giấy, lấy điện thoại chụp lại, upload ảnh lên github (đã nói về các nghiệp vụ trên lớp, ghi bảng)
   
2. SỬ DỤNG DOCKER TRÊN UBUNTU ĐỂ: 
- Mariadb  : chứa csdl của hệ thống này
- Phpmyadmin: để soi được csdl (chỉ để xem, ko cần tạo bảng từ đây, django sẽ làm hết)
- Django: build 1 docker container (dùng Dockerfile): trên nền python, sử dụng django, nhớ mount thư mục để dễ edit, edit dùng: sudo nano ten_file

sau khi có 3 service này trong file docker-compose.yml :
- run nó, cấu hình để Django nhận csdl mariadb (sửa file settings.py), cấu hình user login ban đầu, mô tả các bảng trong models.py, .... (đc phép sử dụng AI để làm) => KQ được trang admin, y/c đăng nhập, vào trang admin: cho phép thêm sửa xoá dữ liệu các bảng. các trường là khoá ngoại chỉ việc chọn text (mặc dù là csdl tại trường FK đó lưu ID của PK mà nó tham chiếu : sử dụng phpmyadmin để kiểm chứng)
- chú ý kết hợp ssh để chạy lệnh tác động vào django và sudo nano để edit file.
- sử dụng template (file html, sử dụng cú pháp jinja2), lấy context từ 1 view home_page, để tạo trang liệt kê các con nợ đến hạn mà chưa trả tiền!
- sử dụng cloudflare tunnel để public kết quả lên 1 sub-domain => chụp kết quả

Hướng dẫn:
- Tạo thư mục để chứa image tự buid cho django
- Vào thư mục đó tạo file tên: Dockerfile (nội dung hỏi AI xem file này cần có nội dung gì, full comment cho từng dòng lệnh trong file này => mục tiêu kép: để hiểu và để hệ thống chạy được)
- AI sẽ nói cần thêm file requirements.txt để cài các thư viện cho python (cài qua lệnh pip) => tạo file requirements.txt với nội dung tưng ứng, trong file này cũng comment được => comment xem thư viện nào dùng để làm gì
- Sau mỗi lần sửa đỏi có thể phải chạy lệnh dạng : **docker compose exec TÊN_SERVICE_DJANGO_CỦA_BẠN python manage.py migrate** để tác động vào django (còn nhiều lệnh khác chứ ko luôn như này), để django thay đổi csdl hoặc thay đổi cấu hình.


## BÀI LÀM
# 1.TỔ CHỨC CSDL CHO HỆ THỐNG QUẢN LÝ TIỆM CẦM ĐỒ: viết tay ra giấy, lấy điện thoại chụp lại, upload ảnh lên github (đã nói về các nghiệp vụ trên lớp, ghi bảng)

# 2. 
- Cài Ubuntu + Docker
Trong Ubuntu chạy:
sudo apt update
sudo apt install docker.io docker-compose -y
<img width="960" height="540" alt="{497B3343-ECD1-4203-A656-B31B03AEB460}" src="https://github.com/user-attachments/assets/a129a35a-aad0-4f9a-b496-a2f41486361f" />

- cài docker compose
<img width="347" height="317" alt="{F689C15A-6817-483D-B231-3B8F65CB5F23}" src="https://github.com/user-attachments/assets/a3347ceb-1349-4d18-9bcf-e98e18048df3" />

- Tạo thư mục chính
Gõ:
mkdir tiemcamdo
cd tiemcamdo
<img width="332" height="84" alt="{E43AE482-92AA-4CDB-ACA9-D59F7F053D97}" src="https://github.com/user-attachments/assets/5671073f-d013-4e9c-873e-bed1418676f7" />

- TẠO THƯ MỤC DJANGO
   > - Tạo thư mục chứa Django
   > - mkdir django_app
<img width="347" height="63" alt="{C6BC4BA8-7352-4E01-9756-B1F0E1B35F0B}" src="https://github.com/user-attachments/assets/1741f61c-2d32-4dcf-8c1f-c16965ede050" />

- Đi vào django_app
cd django_app

-TẠO FILE Dockerfile
   > - Tạo file Dockerfile
nano Dockerfile
<img width="371" height="478" alt="{25FA767B-775F-4124-B82E-A8BFC39D3463}" src="https://github.com/user-attachments/assets/1abb03c1-36af-4b7c-a1ed-0e3db3f061f5" />

- TẠO requirements.txt
   > - Tạo file: nano requirements.txt
<img width="378" height="470" alt="{AB7D99CA-8CBE-4ADC-B08D-F7B262AA3E3D}" src="https://github.com/user-attachments/assets/08c02125-4d00-4160-92e3-4eee580b2694" />

- Kiểm tra thư mục
<img width="353" height="56" alt="{38A7E2FC-4571-4D53-9816-22EF78191492}" src="https://github.com/user-attachments/assets/ad56b3e3-3139-46ce-ba30-06311b684703" />

- TẠO docker-compose.yml
Tạo file
nano docker-compose.yml
<img width="375" height="472" alt="{6C373DE9-C365-49D6-BDF4-A5D555E8D712}" src="https://github.com/user-attachments/assets/0aa2ea80-6f25-4648-95f4-15a2e4e4607e" />


- BUILD DOCKER
Chạy docker compose
docker compose up -d --build
<img width="960" height="540" alt="{7051DF08-8261-419B-8EA1-E065D1E63D64}" src="https://github.com/user-attachments/assets/11c54696-6840-4365-a336-cbed7f6059c7" />

- TẠO DJANGO PROJECT
docker compose exec django django-admin startproject config .
- Tạo app
docker compose exec django python manage.py startapp pawnshop
<img width="960" height="540" alt="{2951DBAE-9B6C-41C2-A0B3-2FBD65AB6875}" src="https://github.com/user-attachments/assets/c5aab516-91a5-4030-b455-7ac1019a6b31" />

- Mở file settings.py
Chạy: nano django_app/config/settings.py
- tìm đoạn:
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }

}sửa thành như trong ảnh

<img width="348" height="149" alt="{B6D92F3F-3A27-47D8-8194-84917D97C333}" src="https://github.com/user-attachments/assets/4bc2156e-f4b8-4563-a8cf-98e6c819b46c" />


- Tạo models
Mở: nano django_app/pawnshop/models.py
<img width="375" height="471" alt="{2B2A00D5-1B03-430B-AC13-5DB3D00FD3B7}" src="https://github.com/user-attachments/assets/e092cc7b-8c0f-4ba5-97cb-335994b7e2a0" />

- Chạy migration
docker compose exec django python manage.py makemigrations
- Migrate database
docker compose exec django python manage.py migrate

<img width="960" height="540" alt="{4F115C5D-41FB-42FD-9EA2-626C1304EFC0}" src="https://github.com/user-attachments/assets/a79b19c5-9ae8-417d-9b0d-c6eb7e22d07a" />


- Tạo tài khoản admin
docker compose exec django python manage.py createsuperuser

<img width="346" height="139" alt="{6FC71963-6FF8-4D70-B0AD-60F012F7FC05}" src="https://github.com/user-attachments/assets/9d2e2afc-3df6-4cc1-a4c2-8205c390d44d" />

- Cho admin quản lý bảng
Mở: nano django_app/pawnshop/admin.py

<img width="472" height="540" alt="{ED443D51-E6E7-46F2-A4C0-E747BAC8A6DD}" src="https://github.com/user-attachments/assets/bdc5f836-1d85-405e-baac-cbd5e8a656cd" />

- Chạy Django
   > - docker compose down
   > - docker compose up -d --build

- Test web
Mở trình duyệt Ubuntu: http://localhost:8000
- hoặc : http://127.0.0.1:8000/admin

<img width="379" height="490" alt="{CF50E381-659E-49C3-8A67-D78F88BD9F1E}" src="https://github.com/user-attachments/assets/62e8c271-b748-4662-9521-23e5f454813a" />

- Tạo thư mục templates
Mở terminal Ubuntu:
mkdir -p django_app/pawnshop/templates
Tạo file home.html
nano django_app/pawnshop/templates/home.html

<img width="465" height="337" alt="{13449C89-82F3-4B84-9CC6-815754081D10}" src="https://github.com/user-attachments/assets/d0c155a8-88b1-4046-9d31-f46e3a330f51" />

- Mở views.py
nano django_app/pawnshop/views.py

<img width="481" height="324" alt="{22DDB5DC-57DC-4EF8-A6C0-74C9A1D622F1}" src="https://github.com/user-attachments/assets/2cb97a27-922b-4540-bd23-cf6b9481d34d" />

- Tạo urls.py cho app
nano django_app/config/urls.py

<img width="339" height="147" alt="{81AA0D4D-5AB6-4144-9B2D-631A1D0A3479}" src="https://github.com/user-attachments/assets/606b60f3-3a0f-4311-af96-9205fa66bd47" />


- Mở trình duyệt:
http://127.0.0.1:8000

<img width="480" height="263" alt="{1F1C2DBA-D577-467F-9F96-64A0C6E4A281}" src="https://github.com/user-attachments/assets/2a93c7bf-87af-454b-8893-7da68de85615" />


- Kiểm tra dữ liệu bằng phpMyAdmin
  
<img width="478" height="383" alt="{796170A5-A911-453D-9B67-4C1BD65A7BF7}" src="https://github.com/user-attachments/assets/2298fe74-40ea-4bc4-84d7-b79bf1ab3d2a" />

<img width="960" height="540" alt="{6F3335C0-2542-4163-98F6-AC7982A5B511}" src="https://github.com/user-attachments/assets/02690c22-6f66-48d2-bcfe-1beb9d0693b1" />

<img width="960" height="540" alt="{09A3D32B-F74F-4112-8D53-8024A8E8C2A6}" src="https://github.com/user-attachments/assets/142550c9-8f43-43de-a10c-f75d043446f3" />


- Public website bằng Cloudflare Tunnel > - đã có container: cloudflared nên giờ chỉ cần cấu hình tunnel. > - Kiểm tra cloudflared đang chạy: docker logs cloudflared

   
