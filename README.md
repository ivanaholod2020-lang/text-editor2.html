<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Профессиональный текстовый редактор</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f5f5f5;
        }
        #editor {
            width: 100%;
            height: 400px;
            padding: 15px;
            border: 2px solid #3498db;
            border-radius: 8px;
            font-size: 16px;
            resize: vertical;
            background-color: white;
        }
        button {
            padding: 10px 15px;
            margin: 5px;
            border: none;
            border-radius: 5px;
            background-color: #3498db;
            color: white;
            cursor: pointer;
            font-weight: bold;
        }
        button:hover {
            background-color: #2980b9;
        }
        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .toolbar {
            margin-bottom: 15px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Профессиональный текстовый редактор</h1>
        <p>Редактируйте текст прямо в браузере. Используйте инструменты форматирования и сохраняйте в разных форматах.</p>

        <div class="toolbar">
            <button onclick="formatText('bold')">Жирный</button>
            <button onclick="formatText('italic')">Курсив</button>
            <button onclick="formatText('underline')">Подчёркивание</button>
            <button onclick="insertLink()">Ссылка</button>
            <button onclick="insertImage()">Изображение</button>
        </div>

        <textarea id="editor" placeholder="Начните печатать здесь..."></textarea>

        <div>
            <button onclick="saveText('txt')">Сохранить как TXT</button>
            <button onclick="saveText('html')">Сохранить как HTML</button>
            <button onclick="clearEditor()">Очистить</button>
            <button onclick="copyText()">Копировать текст</button>
            <button onclick="previewText()">Предпросмотр</button>
        </div>

        <div id="preview" style="margin-top: 20px; display: none;">
            <h3>Предпросмотр:</h3>
            <div id="preview-content"></div>
        </div>
    </div>

    <script>
        function formatText(command) {
            document.execCommand(command, false, null);
            document.getElementById('editor').focus();
        }

        function insertLink() {
            const url = prompt('Введите URL:');
            if (url) {
                document.execCommand('createLink', false, url);
                document.getElementById('editor').focus();
            }
        }

        function insertImage() {
            const src = prompt('Введите URL изображения:');
            if (src) {
                document.execCommand('insertImage', false, src);
                document.getElementById('editor').focus();
            }
        }

        function saveText(format) {
            const text = document.getElementById('editor').value;
            if (!text) {
                alert('Текст пуст!');
                return;
            }

            let blob, filename;
            if (format === 'txt') {
                blob = new Blob([text], {type: 'text/plain'});
                filename = 'мой-текст.txt';
            } else if (format === 'html') {
                blob = new Blob([`<html><head><meta charset="UTF-8"></head><body>${text}</body></html>`], {type: 'text/html'});
                filename = 'мой-текст.html';
            }

            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        function clearEditor() {
            document.getElementById('editor').value = '';
        }

        function copyText() {
            const editor = document.getElementById('editor');
            editor.select();
            document.execCommand('copy');
            alert('Текст скопирован в буфер обмена!');
        }

        function previewText() {
            const preview = document.getElementById('preview');
            const content = document.getElementById('preview-content');
            content.innerHTML = document.getElementById('editor').value;
            preview.style.display = preview.style.display === 'none' ? 'block' : 'none';
        }
    </script>
</body>
</html>
