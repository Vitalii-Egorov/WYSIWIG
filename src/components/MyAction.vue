<template>
    <div class="main__action action">
        <div class="action__link" @click="undo"> <img src="@/img/back.svg" alt="back">
        </div>
        <div class="action__link" @click="redo"> <img src="@/img/forward.svg" alt="back"> </div>
        <div class="action__link" @click="applyHeading"> <img src="@/img/title.svg" alt="back"> </div>
        <div class="action__link" @click="applyParagraph"> <img src="@/img/paragraph.svg" alt="back"> </div>
        <div class="action__link" @click="insertImage2"> <img src="@/img/img.svg" alt="back"> </div>
        <div class="action__link" @click="copyHTML">Скопировать HTML</div>
    </div>
</template>

<script>
import Quill from 'quill';
export default {
    name: 'my-action',
    mounted() {
        const container = document.getElementById('editor-container'); // Получаем контейнер по его id
        this.quill = new Quill(container, {
            // Выберите тему редактора (snow, bubble)
            // Другие настройки Quill.js
        });
        const savedContent = localStorage.getItem('quillContent');
        if (savedContent) {
            this.quill.clipboard.dangerouslyPasteHTML(savedContent);
        }

        this.quill.on('text-change', () => {
            const content = this.quill.root.innerHTML;
            localStorage.setItem('quillContent', content);
        });
    },
    methods: {
        // Вперед по истории изменений
        redo() {
            this.quill.history.redo();
        },
        // Назад по истории изменений
        undo() {
            this.quill.history.undo();
        },
        // Превращение выделенного текста в заголовок
        applyHeading() {
            const range = this.quill.getSelection();
            if (range) {
                this.quill.format('header', '1', 'user');
            }
        },
        // Превращение выделенного текста в абзац
        applyParagraph() {
            const range = this.quill.getSelection();
            if (range) {
                this.quill.format('header', false, 'user');
            }
        },
        // Загрузка изображения с компьютера
        insertImage() {
            const fileInput = document.createElement("input");
            fileInput.type = "file";
            fileInput.accept = "image/*";
            fileInput.onchange = (event) => {
                const file = event.target.files[0];
                if (!file) {
                    return;
                }

                const range = this.quill.getSelection(true);
                const reader = new FileReader();
                reader.onload = () => {
                    this.quill.insertEmbed(range.index, "image", reader.result, "user");
                    this.quill.setSelection(range.index + 1, "silent");
                };
                reader.readAsDataURL(file);
            };
            fileInput.click();
        },
        // Загрузка изображения по URL
        insertImage2() {
            const url = prompt('Введите URL изображения:');
            if (!url) {
                return;
            }

            const range = this.quill.getSelection(true);
            this.quill.insertEmbed(range.index, 'image', url, 'user');
            this.quill.setSelection(range.index + 1, 'silent');
        },
        // Копирование HTML в буфер обмена
        copyHTML() {
            const html = this.quill.root.innerHTML;
            navigator.clipboard.writeText(html).then(() => {
                alert('HTML скопирован в буфер обмена');
            });
        },
    },
}

</script>

<style></style>