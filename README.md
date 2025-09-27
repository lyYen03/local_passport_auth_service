# Project 3: Local Passport Authentication Service

## Cách chạy
Cài đặt dependencies:
```bash
npm install
node app.js
```

Server chạy tại: http://localhost:3000  
Database: passport_local_demo (MongoDB)  

---

## 1. Register
Request
```http
POST /auth/register
Content-Type: application/json
{
  "username": "admin",
  "password": "12345"
}
```

Kết quả test
- Response: "User registered successfully"
- MongoDB có user mới

Ảnh test:  
![Register Success](public/results/register_success.png)  
![Mongo Register](public/results/mongo_register.png)

---

## 2. Login
Request
```http
POST /auth/login
Content-Type: application/json
{
  "username": "admin",
  "password": "12345"
}
```

Kết quả test
- Sai password → 401 Unauthorized  
![Login Fail](public/results/login_fail.png)

- Đúng username/password → "Login successful"  
![Login Success](public/results/login_success.png)

- Tab Cookies trong Postman có connect.sid  
![Login Cookie](public/results/login_cookie.png)

---

## 3. Profile (Protected route)
Request
```http
GET /auth/profile
```

Kết quả test
- Chưa login → 401 Unauthorized  
![Profile No Session](public/results/profile_no_session.png)

- Đã login → trả về thông tin user  
![Profile With Session](public/results/profile_with_session.png)

---

## 4. Logout
Request
```http
GET /auth/logout
```

Kết quả test
- Response: "Logged out"  
![Logout Success](public/results/logout_success.png)

- Cookie connect.sid vẫn còn trong Postman  
![Logout Cookie](public/results/logout_cookie.png)

Giải thích  
Trong code hiện tại, `req.logout()` chỉ xóa user khỏi session. Passport không tự xóa cookie phía client. Vì vậy cookie connect.sid vẫn hiện, nhưng session đã vô hiệu. Khi gọi lại /auth/profile sẽ trả về 401 Unauthorized. Đây là hành vi mặc định và hợp lệ của Passport.

---

## Hoàn thành
- app.js → cấu hình server, session, Passport Local
- routes/auth.js → xử lý register, login, profile, logout
- Test đầy đủ các trường hợp:
  - Register thành công
  - Login sai → Unauthorized
  - Login đúng → success + cookie session
  - Profile: chưa login vs đã login
  - Logout → session invalid (cookie vẫn còn nhưng vô hiệu)

Ảnh minh họa test: nằm trong thư mục public/results/
