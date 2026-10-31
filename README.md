# Jarvis Call — APK-обёртка

Минимальное Android-приложение: один экран с WebView, в который загружена
анимация капли (`app/src/main/assets/index.html`). Это промежуточный шаг
перед переписыванием на нативный Kotlin с фоновым оверлей-сервисом.

## Как собрать APK через GitHub Actions

1. Создай новый пустой репозиторий на GitHub (можно приватный).
2. Залей в него всю эту папку целиком:

   ```bash
   cd jarvis-app
   git init
   git add .
   git commit -m "init"
   git branch -M main
   git remote add origin https://github.com/<твой_логин>/<репо>.git
   git push -u origin main
   ```

3. Зайди во вкладку **Actions** репозитория на GitHub — workflow
   «Build APK» запустится автоматически на пуше в `main`.
   Если не запустился сам — открой его и нажми **Run workflow**
   (это работает благодаря `workflow_dispatch` в файле workflow).
4. Когда сборка завершится (зелёная галочка), открой её и внизу страницы
   в разделе **Artifacts** будет `jarvis-debug-apk` — скачай zip,
   внутри лежит `app-debug.apk`.
5. Скинь `app-debug.apk` на телефон и установи (разреши установку
   из неизвестных источников, если попросит).

Это debug-сборка — без релизной подписи, этого достаточно для
собственного телефона. Для публикации в Play Store потребуется
отдельная подпись release-ключом — скажи, когда дойдём до этого.

## Структура

- `app/src/main/assets/index.html` — вся анимация (HTML/CSS/JS),
  та же, что в опубликованном артефакте.
- `app/src/main/java/com/jarvis/call/MainActivity.kt` — просто
  открывает этот HTML в полноэкранном WebView.
- `.github/workflows/build.yml` — CI, который ставит JDK 17,
  Android SDK и Gradle и собирает `assembleDebug`.

## Дальше — Kotlin с фоновым оверлеем

Следующий шаг — не обёртка, а нативный сервис:
`Foreground Service` + `WindowManager` с `TYPE_APPLICATION_OVERLAY`,
чтобы кружок реально всплывал поверх других приложений, а не только
в своём окне. Для этого нужно отдельное разрешение
«поверх других окон» (`SYSTEM_ALERT_WINDOW`), которое пользователь
выдаёт вручную в настройках. Напиши, когда будешь готов — соберём
это отдельным этапом поверх текущего проекта.
