### [codeaday.pro.vn] Tối Ưu Hóa Mã Nguồn: Sử Dụng Code Linter, Formatter và Husky trong Dự Án React. 

**1. Code Linter:**

Code linter là công cụ giúp kiểm tra mã nguồn của bạn để tìm lỗi cú pháp, quy tắc lập trình không tốt và tiềm năng gây lỗi. Một số code linter phổ biến trong cộng đồng React bao gồm ESLint và TSLint (dành cho TypeScript).

**Cài Đặt và Cấu Hình ESLint:**

1. Bắt đầu bằng việc cài đặt ESLint thông qua npm hoặc yarn:

   ```
   npm install eslint --save-dev
   ```

2. Tạo tập tin cấu hình `.eslintrc.js` trong thư mục gốc của dự án:

    Dưới đây là một file ví dụ cụ thể:
```json
{
  "env": {
    "browser": true, // Cho phép sử dụng các biến trình duyệt như document, window
    "node": true, // Cho phép sử dụng các biến của Node.js như require
    "es6": true // Cho phép sử dụng các tính năng của ECMAScript 6
  },
  "extends": [
    "eslint:recommended", // Kế thừa cấu hình mặc định từ ESLint
    "prettier" // Sử dụng cấu hình prettier để đảm bảo mã luôn đúng định dạng
  ],
  "plugins": ["react", "prettier"], // Sử dụng các plugin react và prettier
  "parserOptions": {
    "ecmaVersion": 2018, // Sử dụng phiên bản ECMAScript 2018
    "sourceType": "module", // Sử dụng kiểu module để sử dụng các tính năng ES6 module
    "ecmaFeatures": {
      "jsx": true // Cho phép sử dụng JSX trong React
    }
  },
  "rules": {
    "semi": ["error", "always"], // Yêu cầu sử dụng dấu chấm phẩy sau mỗi câu lệnh
    "quotes": ["warn", "single"], // Yêu cầu sử dụng dấu ngoặc đơn cho chuỗi
    "indent": ["error", 2], // Yêu cầu thụt đầu dòng 2 khoảng trắng
    "no-unused-vars": "off", // Tắt cảnh báo về biến không sử dụng mặc định của ESLint
    "no-console": "off" // Tắt cảnh báo về việc sử dụng console.log()
  },
  "ignorePatterns": [
    "webpack.config.js", // Bỏ qua tệp webpack.config.js
    "*.test.js" // Bỏ qua tất cả các tệp test.js
  ]
}
```

**2. Code Formatter (Prettier):**

Prettier là một công cụ giúp định dạng mã nguồn một cách tự động theo các quy tắc định sẵn. Điều này giúp đảm bảo rằng mã luôn được định dạng thống nhất.

**Cài Đặt và Cấu Hình Prettier:**

1. Cài đặt Prettier thông qua npm hoặc yarn:

   ```
   npm install prettier --save-dev
   ```

2. Tạo tập tin cấu hình `.prettierrc` hoặc thêm khai báo trong `package.json`:

   ```json
   {
   "semi": true,
    "singleQuote": true,
    "tabWidth": 2,
    "printWidth": 80,
    "trailingComma": "es5",
    "arrowParens": "always"
   }
   ```
Lúc này sau khi đã cài đặt xong và việc code sẽ được giám sát bởi eslint. Nhưng vẫn còn thiếu thiếu vì khi vẫn còn chốt chặn cuối cùng để code được sạch đẹp trước khi được push lên repos. Tiếp tục nào...

**3. Husky và lint-staged:**
1. **Cài Đặt Husky và lint-staged:**
 ```json
   npm install lint-staged husky --save-dev
   ```
2. **Cấu Hình Husky và Lệnh Hook:**

Sử dụng Husky CLI để thêm hook pre-commit vào dự án và chỉ định lệnh bạn muốn chạy:
```
npx husky add .husky/pre-commit "npx lint-staged"
```

3. **Cấu Hình lint-staged:**

Thêm cấu hình lint-staged vào tệp package.json để chỉ định các tệp bạn muốn kiểm tra và các lệnh bạn muốn chạy cho chúng.

```
{
  "name": "your-project-name",
  "version": "1.0.0",
  "scripts": {
    // ... các scripts khác
    "lint": "eslint",
    "format": "prettier --write \"src/**/*.js\" \"src/**/*.jsx\"",
  },
  "husky": {
    "hooks": {
      "pre-commit": "npx lint-staged"
    }
  },
  "lint-staged": {
    "src/**/*.{js,jsx}": ["npm run lint", "npm run format"]
  },
  // ...
}
```
Với cấu hình lint-staged như trên khi bạn commit nếu phạm rule thì sẽ bị báo lỗi và không cho commit.

***Bonus***: Nếu bạn muốn tất cả tự động khi commit thì lệnh sau sẽ chạy và tự động tìm lỗi và fix lỗi eslint luôn. Ảo diệu chưa 😎
```json
 "lint-staged": {
    "src/**/*.{js,jsx}": [ 
    "eslint --fix",
    "prettier --write",
    "git add"]
  }
```

3. **Chạy Lại Cấu Hình Husky:**

Chạy lại cấu hình Husky để đảm bảo tệp .husky/pre-commit đã được tạo:

```
npx husky install
```

Bây giờ, khi bạn commit mã, Husky sẽ tự động chạy lint-staged, và lint-staged sẽ chỉ chạy kiểm tra và định dạng cho những tệp đã thay đổi, giúp đảm bảo mã của bạn tuân theo các tiêu chuẩn và quy tắc kiểm tra được xác định.

 ---

***Kết Luận***

Sử dụng code linter và formatter là một phần quan trọng của quy trình phát triển để đảm bảo mã nguồn luôn đáp ứng các quy tắc lập trình và dễ đọc.
