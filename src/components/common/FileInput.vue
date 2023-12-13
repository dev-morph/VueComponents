<template>
    <div class="file__input__wrapper">
        <label
            :for="uuid"
            @dragover.prevent
            @dragenter.prevent
            @drop.prevent="fileUploadHandler"
            class="fi__label"
        >
            <span>{{ buttonText || '파일선택' }}</span>
            <p>
                {{
                    !file || file.name.length <= 0
                        ? placeholder || '파일을 선택하거나 이곳으로 드래그 해주세요.'
                        : file.name
                }}
            </p>
            <input
                :id="uuid"
                type="file"
                title="min is "
                placeholder="asdfasdf"
                @change="(e) => fileUploadHandler(e)"
            />
        </label>
        <div class="init__btn" v-if="fileName" @click.stop.prevent="(e) => fileUploadHandler(e)">
            <div class="btn__content"></div>
        </div>
    </div>
</template>

<script setup lang="ts">
interface FileInputProps {
    file: File | null
    buttonText?: string
    placeholder?: string
}
import { onMounted, ref, toRefs, computed } from 'vue'

const props = defineProps<FileInputProps>()
const { file, buttonText, placeholder } = toRefs(props)
const emit = defineEmits(['update:modelValue', 'test'])
// const uploadedFile = ref('');
const uuid = crypto.randomUUID()

const fileName = computed(() => {
    if (file.value) {
        return file.value.name
    }
    return null
})

function fileUploadHandler(e: Event) {
    let files: FileList | null = null
    if (e instanceof DragEvent) {
        //Drag
        if (e.dataTransfer) {
            files = e.dataTransfer.files
        }
    } else if (e instanceof MouseEvent) {
        //delete
    } else {
        //Input Event
        const target = e.target as HTMLInputElement
        files = target.files
    }
    emit('update:modelValue', files)
}

onMounted(() => {})
</script>

<style lang="scss" scoped>
.file__input__wrapper {
    width: 100%;
    position: relative;
    .fi__label {
        position: relative;
        display: flex;
        width: 100%;
        align-items: center;
        height: 2.5rem;
        border: 1px solid #d1d5db;
        border-radius: 0.375rem;
        cursor: pointer;
        background-color: #f3f4f6;
        @media (prefers-color-scheme: dark) {
            &:hover {
                background-color: #374151;
            }
            background-color: #4b5563;
            border-color: #4b5563;
            &:hover {
                border-color: #6b7280;
            }
        }

        span {
            height: 100%;
            background-color: #1f2937;
            color: #ffffff;
            font-size: 0.875rem;
            padding: 0.5rem 1rem;
            border-top-left-radius: 0.375rem;
            border-bottom-left-radius: 0.375rem;
            display: flex;
            align-items: center;
            font-weight: 500;
        }

        p {
            padding-left: 0.5rem;
            font-size: 0.875rem;
            color: #6b7280;
            @media (prefers-color-scheme: dark) {
                color: #9ca3af;
            }
        }

        input[type='file'] {
            display: none;
        }
    }
    .init__btn {
        position: absolute;
        width: 2.5rem;
        height: 2.5rem;
        top: 0;
        right: 0;
        color: red;
        cursor: pointer;

        .btn__content {
            position: relative;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            &::after {
                content: '\00d7';
                font-size: 1.2rem;
                color: white;
            }
        }
    }
}
</style>
