<script>
import { defineComponent, onMounted } from 'vue'
import { Editor } from '@tiptap/core'
import Placeholder from '@tiptap/extension-placeholder'
import StarterKit from '@tiptap/starter-kit'
import Paragraph from '@tiptap/extension-paragraph'
import Bold from '@tiptap/extension-bold'
import Underline from '@tiptap/extension-underline'
import Link from '@tiptap/extension-link'
import BulletList from '@tiptap/extension-bullet-list'
import OrderedList from '@tiptap/extension-ordered-list'
import ListItem from '@tiptap/extension-list-item'
import Blockquote from '@tiptap/extension-blockquote'

export default defineComponent({
  name: 'TipTapEditor',
  setup() {
    onMounted(() => {
      const editor = new Editor({
        element: document.querySelector('#hs-editor-tiptap-modal [data-hs-editor-field]'),
        extensions: [
          Placeholder.configure({
            placeholder: 'Add a message, if you\'d like.',
            emptyNodeClass: 'text-black-800 ',
          }),
          StarterKit,
          Paragraph.configure({
            HTMLAttributes: {
              class: 'text-gray-800 ',
            },
          }),
          Bold.configure({
            HTMLAttributes: {
              class: 'font-bold',
            },
          }),
          Underline,
          Link.configure({
            HTMLAttributes: {
              class: 'inline-flex items-center gap-x-1 text-blue-600 decoration-2 hover:underline font-medium ',
            },
          }),
          BulletList.configure({
            HTMLAttributes: {
              class: 'list-disc list-inside text-gray-800 ',
            },
          }),
          OrderedList.configure({
            HTMLAttributes: {
              class: 'list-decimal list-inside text-gray-800 ',
            },
          }),
          ListItem,
          Blockquote.configure({
            HTMLAttributes: {
              class: 'text-gray-800 sm:text-xl ',
            },
          }),
        ],
      })

      const actions = [
        {
          id: '[data-hs-editor-bold]',
          fn: () => editor.chain().focus().toggleBold().run(),
        },
        {
          id: '[data-hs-editor-italic]',
          fn: () => editor.chain().focus().toggleItalic().run(),
        },
        {
          id: '[data-hs-editor-underline]',
          fn: () => editor.chain().focus().toggleUnderline().run(),
        },
        {
          id: '[data-hs-editor-strike]',
          fn: () => editor.chain().focus().toggleStrike().run(),
        },
        {
          id: '[data-hs-editor-link]',
          fn: () => {
            const url = window.prompt('URL')

            editor.chain().focus().extendMarkRange('link').setLink({ href: url }).run()
          },
        },
        {
          id: '[data-hs-editor-ol]',
          fn: () => editor.chain().focus().toggleOrderedList().run(),
        },
        {
          id: '[data-hs-editor-ul]',
          fn: () => editor.chain().focus().toggleBulletList().run(),
        },
        {
          id: '[data-hs-editor-blockquote]',
          fn: () => editor.chain().focus().toggleBlockquote().run(),
        },
        {
          id: '[data-hs-editor-code]',
          fn: () => editor.chain().focus().toggleCode().run(),
        },
      ]

      actions.forEach(({ id, fn }) => {
        const action = document.querySelector(id)

        if (action !== null)
          action.addEventListener('click', fn)
      })
    })
  },
})
</script>

<template>
  <!-- Modal Button -->
  <button
    type="button"
    class="py-3 px-4 inline-flex items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent bg-blue-600 text-white hover:bg-blue-700 disabled:opacity-50 disabled:pointer-events-none"
    data-hs-overlay="#hs-toggle-password-modal-example"
  >
    Open modal
  </button>
  <!-- End Modal Button -->

  <!-- Modal Content -->
  <div
    id="hs-toggle-password-modal-example"
    class="hs-overlay hidden size-full fixed top-0 start-0 z-[80] overflow-x-hidden overflow-y-auto pointer-events-none"
  >
    <div class="hs-overlay-open:mt-7 hs-overlay-open:opacity-100 hs-overlay-open:duration-500 mt-0 opacity-0 ease-out transition-all sm:max-w-lg sm:w-full m-3 sm:mx-auto">
      <div class="flex flex-col bg-white border shadow-sm rounded-xl pointer-events-auto">
        <div class="flex justify-between items-center py-3 px-4 border-b dark:border-neutral-700">
          <h3 class="font-bold text-gray-800 ">
            Modal example
          </h3>
          <button
            type="button"
            class="hs-dropdown-toggle inline-flex flex-shrink-0 justify-center items-center size-8 rounded-lg text-gray-500 hover:text-gray-400 focus:outline-none focus:ring-2 focus:ring-gray-400 focus:ring-offset-2 focus:ring-offset-white transition-all text-sm dark:focus:ring-neutral-700 dark:focus:ring-offset-gray-800"
            data-hs-overlay="#hs-toggle-password-modal-example"
          >
            <span class="sr-only">Close</span>
            <svg
              class="flex-shrink-0 size-4"
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path d="M18 6 6 18" />
              <path d="m6 6 12 12" />
            </svg>
          </button>
        </div>
        <div class="p-4 overflow-y-auto">
          <!-- Tiptap -->
          <div class="border border-gray-200 rounded-xl overflow-hidden dark:border-neutral-700">
            <div id="hs-editor-tiptap-modal">
              <div class="flex align-middle gap-x-0.5 border-b border-gray-200 p-2 dark:border-neutral-700">
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-bold=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <path d="M14 12a4 4 0 0 0 0-8H6v8" />
                    <path d="M15 20a4 4 0 0 0 0-8H6v8Z" />
                  </svg>
                </button>
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-italic=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <line
                      x1="19"
                      x2="10"
                      y1="4"
                      y2="4"
                    />
                    <line
                      x1="14"
                      x2="5"
                      y1="20"
                      y2="20"
                    />
                    <line
                      x1="15"
                      x2="9"
                      y1="4"
                      y2="20"
                    />
                  </svg>
                </button>
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-underline=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <path d="M6 4v6a6 6 0 0 0 12 0V4" />
                    <line
                      x1="4"
                      x2="20"
                      y1="20"
                      y2="20"
                    />
                  </svg>
                </button>
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-strike=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <path d="M16 4H9a3 3 0 0 0-2.83 4" />
                    <path d="M14 12a4 4 0 0 1 0 8H6" />
                    <line
                      x1="4"
                      x2="20"
                      y1="12"
                      y2="12"
                    />
                  </svg>
                </button>
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-link=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71" />
                    <path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71" />
                  </svg>
                </button>
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-ol=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <line
                      x1="10"
                      x2="21"
                      y1="6"
                      y2="6"
                    />
                    <line
                      x1="10"
                      x2="21"
                      y1="12"
                      y2="12"
                    />
                    <line
                      x1="10"
                      x2="21"
                      y1="18"
                      y2="18"
                    />
                    <path d="M4 6h1v4" />
                    <path d="M4 10h2" />
                    <path d="M6 18H4c0-1 2-2 2-3s-1-1.5-2-1" />
                  </svg>
                </button>
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-ul=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <line
                      x1="8"
                      x2="21"
                      y1="6"
                      y2="6"
                    />
                    <line
                      x1="8"
                      x2="21"
                      y1="12"
                      y2="12"
                    />
                    <line
                      x1="8"
                      x2="21"
                      y1="18"
                      y2="18"
                    />
                    <line
                      x1="3"
                      x2="3.01"
                      y1="6"
                      y2="6"
                    />
                    <line
                      x1="3"
                      x2="3.01"
                      y1="12"
                      y2="12"
                    />
                    <line
                      x1="3"
                      x2="3.01"
                      y1="18"
                      y2="18"
                    />
                  </svg>
                </button>
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-blockquote=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <path d="M17 6H3" />
                    <path d="M21 12H8" />
                    <path d="M21 18H8" />
                    <path d="M3 12v6" />
                  </svg>
                </button>
                <button
                  class="size-8 inline-flex justify-center items-center gap-x-2 text-sm font-semibold rounded-full border border-transparent text-gray-800 hover:bg-gray-100 disabled:opacity-50 disabled:pointer-events-none dark:text-white dark:hover:bg-neutral-700"
                  type="button"
                  data-hs-editor-code=""
                >
                  <svg
                    class="flex-shrink-0 size-4"
                    xmlns="http://www.w3.org/2000/svg"
                    width="24"
                    height="24"
                    viewBox="0 0 24 24"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  >
                    <path d="m18 16 4-4-4-4" />
                    <path d="m6 8-4 4 4 4" />
                    <path d="m14.5 4-5 16" />
                  </svg>
                </button>
              </div>
              <div
                data-hs-editor-field=""
                class="h-20 p-5 text-black-500"
              />
            </div>
          </div>
          <!-- End Tiptap -->
        </div>
        <div class="flex justify-end items-center gap-x-2 py-3 px-4 border-t dark:border-neutral-700">
          <button
            type="button"
            class="py-2 px-3 inline-flex items-center gap-x-2 text-sm font-medium rounded-lg border border-gray-200 bg-white text-gray-800 shadow-sm hover:bg-gray-50 disabled:opacity-50 disabled:pointer-events-none "
            data-hs-overlay="#hs-toggle-password-modal-example"
          >
            Close
          </button>
          <a
            class="py-2 px-3 inline-flex items-center gap-x-2 text-sm font-semibold rounded-lg border border-transparent bg-blue-600 text-white hover:bg-blue-700 disabled:opacity-50 disabled:pointer-events-none"
            href="#"
          >
            Save changes
          </a>
        </div>
      </div>
    </div>
  </div>
  <!-- End Modal Content -->
</template>

  <style>
    .ProseMirror:focus {
      outline: none;
    }

    .tiptap ul p,
    .tiptap ol p {
      display: inline;
    }

    .tiptap p.is-editor-empty:first-child::before {
      block-size: 0;
      content: attr(data-placeholder);
      float: inline-start;
      font-size: 14px;
      pointer-events: none;
    }
  </style>
