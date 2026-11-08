# LangChain Pipelines

Учебный проект поиска товаров по запросам на естественном языке. Qwen извлекает параметры поиска, Pydantic проверяет их, а Python выполняет параметризованный запрос к каталогу SQLite. Ноутбук демонстрирует структурированный вывод, вызов инструмента, повторные попытки с журналом и уточняющие запросы с сохранением состояния.

- [Ноутбук](langchain-pipeline.ipynb)
- [Отчёт](Отчет.md)
- [Учебный каталог](products.db)
- [Зависимости](requirements.txt)

## Инструкция по запуску

Для запуска нужны **Python 3.12** и доступ к модели **Qwen 3.8 (27B)** через OpenAI-совместимый API. Узнайте у провайдера адрес API (обычно заканчивается на `/v1`), ключ и идентификатор модели. Для демонстрации `/tool` API должен поддерживать вызов инструментов, а для сравнения thinking — параметр `chat_template_kwargs.enable_thinking`.

1. Откройте терминал в папке с ноутбуком. Создайте виртуальное окружение, активируйте его и установите зависимости.

   **Windows (PowerShell)**

   ```powershell
   py -3.12 -m venv .venv
   .\.venv\Scripts\Activate.ps1
   python -m pip install -r requirements.txt notebook
   ```

   **macOS или Linux**

   ```bash
   python3.12 -m venv .venv
   source .venv/bin/activate
   python -m pip install -r requirements.txt notebook
   ```

2. Задайте `QWEN_BASE_URL` и `QWEN_API_KEY`. При необходимости задайте `QWEN_MODEL`. По умолчанию используется `qwen3.8-27b`. Замените адрес и идентификатор в примерах значениями от своего провайдера. Введите токен в скрытом поле терминала. Используйте токен без кавычек и префикса `Bearer`.

   **Windows (PowerShell)**

   ```powershell
   $env:QWEN_BASE_URL = "https://your-provider.example/v1"
   $env:QWEN_MODEL = "qwen3.8-27b"
   $env:QWEN_API_KEY = [System.Net.NetworkCredential]::new(
       "", (Read-Host "Ключ API Qwen" -AsSecureString)
   ).Password
   ```

   **macOS или Linux (Bash)**

   ```bash
   export QWEN_BASE_URL="https://your-provider.example/v1"
   export QWEN_MODEL="qwen3.8-27b"
   read -rs -p "Ключ API Qwen: " QWEN_API_KEY
   echo
   export QWEN_API_KEY
   ```

3. Из **того же терминала**, где заданы переменные, запустите Jupyter:

   ```text
   python -m notebook
   ```

   Откройте файл `.ipynb` в браузере. Если Jupyter предложит выбрать ядро, выберите **Python 3 (ipykernel)**. При запуске через VS Code откройте редактор из того же терминала командой `code .` и выберите ядро окружения `.venv`.

4. Выполняйте ячейки сверху вниз. Тестовая ячейка в начале проверяет подключение к настроенной модели. Если в части 4 появится запрос подтверждения поиска без ограничения цены, введите `да` или `нет`.

5. В разделе **«Результат лабораторной»** запустите ячейку с `catalog_chat()` и выберите режим повторных попыток. Введите «Покажи ноутбуки до 50 тысяч», затем «А сначала дорогие?». Под ячейкой появятся журнал и результаты поиска.

   - `/tool <запрос>` — разовый поиск через инструмент `search_products`.
   - `/reset` — сброс параметров диалога.
   - `/exit` — завершение чата.
