<template>
    <div class="modal">
        <div class="modal__background" @click="$emit('close')"></div>
        <div class="modal__wrapper">
            <button 
                @click="$emit('close')"
                class="modal__close"    
            >
                X
            </button>
            <div class="modal__content">
                <form
                    @submit.prevent="submitForm"
                    class="form"		
                >
                    <div class="form__field">
                        <label for="description" class="form__label">Описание</label>
                        <input required type="text" name="description" id="description" v-model="description" class="form__input">
                    </div>
                    <div class="form__field">
                        <label for="priority" class="form__label">Приоритет</label>
                        <select required name="priority" id="priority" class="form__input" v-model="priority">
                            <option value="1" selected>1</option>
                            <option value="2">2</option>
                            <option value="3">3</option>
                        </select>
                    </div>
                    <button class="form__button button">Сохранить</button>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'VModal',
    props: {
        data: {
            description: String,
            priority: Number
        },
        type: String
    },
    data() {
        return {
            description: '',
            priority: 1
        }
    },
    methods: {
        submitForm() {
            if (this.type == 'creating') {
                this.$emit('create', this.description, this.priority);
            } else {
                this.$emit('update', this.description, this.priority);
            }
            this.$emit('close');
        },
        keyPressHandler(event) {
            if (event.key == 'Escape') this.$emit('close');
        }
    },
    created() {
        this.description = this.data.description;
        this.priority = this.data.priority;
    },
    mounted() {
        document.addEventListener('keydown', this.keyPressHandler);
    },
    beforeUnmount() {
        document.removeEventListener('keydown', this.keyPressHandler);
    }
}
</script>