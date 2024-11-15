اگر می‌خواهید پروژه Vue.js خود را بدون نصب Node.js و Yarn به صورت محلی و فقط با استفاده از Docker ایجاد کنید، می‌توانید از Docker برای اجرای دستورات مورد نیاز در یک کانتینر موقت استفاده کنید. در اینجا نحوه انجام این کار توضیح داده شده است:

### مراحل ایجاد پروژه Vue.js با استفاده از Docker

1. **ایجاد یک کانتینر موقت برای نصب Vite:**

   ابتدا یک کانتینر موقت ایجاد کنید که Node.js و Yarn را اجرا می‌کند و پروژه جدیدی با Vite ایجاد می‌کند:

   ```bash
   docker run --rm -v ${PWD}:/app -w /app node:lts-alpine sh -c "yarn global add create-vite && create-vite my-vue-app --template vue"
   ```

    - `--rm`: کانتینر را پس از اتمام کار حذف می‌کند.
    - `-v ${PWD}:/app`: دایرکتوری فعلی را به دایرکتوری `/app` در کانتینر متصل می‌کند.
    - `-w /app`: دایرکتوری کاری کانتینر را به `/app` تنظیم می‌کند.
    - `node:lts-alpine`: از نسخه سبک Node.js استفاده می‌کند.
    - `yarn global add create-vite`: نصب Vite به صورت جهانی.
    - `create-vite my-vue-app --template vue`: ایجاد پروژه جدید با استفاده از Vite و قالب Vue.

2. **نصب وابستگی‌ها:**

   پس از ایجاد پروژه، می‌توانید وابستگی‌ها را با استفاده از Docker نصب کنید:

   ```bash
   docker run --rm -v ${PWD}/my-vue-app:/app -w /app node:lts-alpine sh -c "yarn install"
   ```

   این دستور وابستگی‌های پروژه را در دایرکتوری `my-vue-app` نصب می‌کند.

3. **ایجاد Dockerfile برای توسعه:**

   حالا که پروژه ایجاد شده است، می‌توانید یک Dockerfile برای توسعه برنامه خود ایجاد کنید:

   ```dockerfile
   # Use the official Node.js image as a base
   FROM node:lts-alpine

   # Set the working directory
   WORKDIR /app

   # Copy package.json and yarn.lock
   COPY package.json yarn.lock ./

   # Install dependencies using Yarn
   RUN yarn install

   # Copy the rest of the application code
   COPY . .

   # Expose port 5173 (default port for Vite)
   EXPOSE 5173

   # Command to run the app using Vite
   CMD ["yarn", "dev"]
   ```

4. **ساخت Docker Image:**

   به دایرکتوری `my-vue-app` بروید و Docker Image را بسازید:

   ```bash
   cd my-vue-app
   docker build -t my-vue-app .
   ```

5. **اجرای Docker Container:**

   کانتینر را اجرا کنید تا برنامه توسعه داده شود:

   ```bash
   docker run -it -p 5173:5173 -v ${PWD}:/app -v /app/node_modules my-vue-app
   ```


این مراحل به شما اجازه می‌دهند تا بدون نیاز به نصب Node.js و Yarn به صورت محلی، پروژه Vue.js خود را با استفاده از Docker ایجاد و توسعه دهید.


-----
هنگامی که از Docker برای توسعه پروژه خود استفاده می‌کنید و نیاز به اضافه کردن بسته‌های جدید دارید، می‌توانید این کار را با به‌روزرسانی `Dockerfile` و یا استفاده از یک کانتینر موقت انجام دهید. در اینجا دو روش برای اضافه کردن بسته‌های جدید به پروژه توضیح داده شده است:

### روش 1: استفاده از کانتینر موقت برای نصب بسته‌ها

این روش به شما اجازه می‌دهد تا بسته‌های جدید را بدون نیاز به بازسازی کامل Docker Image نصب کنید. می‌توانید از یک کانتینر موقت برای نصب بسته‌ها استفاده کنید و سپس تغییرات را در سیستم فایل محلی خود اعمال کنید:

1. **اجرای کانتینر موقت:**

   از یک کانتینر موقت برای نصب بسته جدید استفاده کنید:

   ```bash
   docker run --rm -v ${PWD}:/app -w /app node:lts-alpine sh -c "yarn add <package-name>"
   ```

   - `<package-name>` را با نام بسته‌ای که می‌خواهید نصب کنید جایگزین کنید.
   - این دستور بسته جدید را نصب کرده و تغییرات را در فایل `package.json` و `yarn.lock` اعمال می‌کند.

2. **به‌روزرسانی کانتینر در حال اجرا:**

   پس از نصب بسته، می‌توانید کانتینر در حال اجرای خود را متوقف کرده و دوباره با همان دستور قبلی اجرا کنید تا تغییرات اعمال شوند.

### روش 2: به‌روزرسانی Dockerfile و بازسازی Image

این روش برای زمانی مناسب است که می‌خواهید تغییرات دائمی در Docker Image ایجاد کنید:

1. **به‌روزرسانی Dockerfile:**

   اگر می‌خواهید بسته‌های جدید را به Dockerfile اضافه کنید، می‌توانید `yarn add <package-name>` را به یک مرحله جدید در Dockerfile اضافه کنید، یا فقط `package.json` را به‌روزرسانی کنید و سپس `yarn install` را اجرا کنید.

2. **بازسازی Docker Image:**

   پس از به‌روزرسانی Dockerfile یا فایل‌های `package.json` و `yarn.lock`، Docker Image را دوباره بسازید:

   ```bash
   docker build -t my-vue-app .
   ```

3. **اجرای کانتینر جدید:**

   کانتینر جدید را با Docker Image به‌روز شده اجرا کنید:

   ```bash
   docker run -it -p 5173:5173 -v ${PWD}:/app -v /app/node_modules my-vue-app
   ```

### نکات:

- **حفظ تغییرات:** روش اول برای توسعه سریع و آزمایش بسته‌های جدید مناسب است، در حالی که روش دوم برای زمانی که می‌خواهید تغییرات را به صورت دائمی در Docker Image خود اعمال کنید مناسب است.
- **کنترل نسخه:** همیشه مطمئن شوید که تغییرات در `package.json` و `yarn.lock` به‌روز هستند و در سیستم کنترل نسخه ذخیره شده‌اند تا همکاران شما نیز بتوانند از آن‌ها استفاده کنند.

با این روش‌ها، می‌توانید به راحتی بسته‌های جدید را به پروژه Docker خود اضافه کنید و محیط توسعه خود را به‌روزرسانی کنید.