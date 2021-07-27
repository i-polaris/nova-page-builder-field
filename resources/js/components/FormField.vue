<template>
    <div id="editor"></div>
</template>

<script>

import './loader.js'
import 'ckeditor';
import grapesjs from 'grapesjs';
import basicBlocks from 'grapesjs-blocks-basic';
import pluginNavbar from 'grapesjs-navbar';
import pluginCountdown from 'grapesjs-component-countdown';
import pluginForms from 'grapesjs-plugin-forms';
import pluginExport from 'grapesjs-plugin-export';
import pluginNewsletter from 'grapesjs-preset-newsletter';
import pluginsCkEditor from 'grapesjs-plugin-ckeditor';
// import pluginAviary from 'grapesjs-aviary';
// import pluginFilestack from 'grapesjs-plugin-filestack';
import custom from '../plugins/custom';
import { FormField, HandlesValidationErrors } from 'laravel-nova'

export default {

    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    data() {
        return {
            editor: null
        }
    },

    methods: {
        /*
         * Set the initial, internal value for the field.
         */
        setInitialValue() {
            this.value = this.field.value || ''
        },

        /**
         * Fill the given FormData object with the field's internal value.
         */
        fill(formData) {
            let newValue =  '<style>' + this.editor.getCss() + '</style>' + this.editor.getHtml();
            formData.append(this.field.attribute, newValue || '')
        },

        /**
         * Update the field's internal value.
         */
        handleChange(value) {
            this.value = value
        },
    },

    mounted() {

        var editor;
        this.editor = editor = grapesjs.init({
            clearOnRender: true,
            height: '600px',
            storageManager: {
                id: 'gjs-nl-',
            },
            container : '#editor',
            fromElement: true,
            plugins: ['gjs-preset-newsletter', 'gjs-plugin-ckeditor'],
        pluginsOpts: {
          'gjs-preset-newsletter': {
            modalLabelImport: 'Paste all your code here below and click import',
            modalLabelExport: 'Copy the code and use it wherever you want',
            codeViewerTheme: 'material',
            //defaultTemplate: templateImport,
            importPlaceholder: '<table class="table"><tr><td class="cell">Hello world!</td></tr></table>',
            cellStyle: {
              'font-size': '12px',
              'font-weight': 300,
              'vertical-align': 'top',
              color: 'rgb(111, 119, 125)',
              margin: 0,
              padding: 0,
            }
          },'gjs-plugin-ckeditor' : {
            

                position: 'center',
            options: {
              startupFocus: true,
              extraAllowedContent: '*(*);*{*}', // Allows any class and any inline style
              allowedContent: true, // Disable auto-formatting, class removing, etc.
              enterMode: CKEDITOR.ENTER_BR,
              extraPlugins: 'sharedspace,justify,colorbutton,panelbutton,font',
              toolbar: [
                { name: 'styles', items: ['Font', 'FontSize' ] },
                ['Bold', 'Italic', 'Underline', 'Strike'],
                {name: 'paragraph', items : [ 'NumberedList', 'BulletedList']},
                {name: 'links', items: ['Link', 'Unlink']},
                {name: 'colors', items: [ 'TextColor', 'BGColor' ]},
              ],
            }

            }
        }
      });

        window.CKEDITOR.dtd.$editable.span = 1
        window.CKEDITOR.dtd.$editable.a = 1
        this.editor.setComponents(this.field.value);
    }
}
</script>
