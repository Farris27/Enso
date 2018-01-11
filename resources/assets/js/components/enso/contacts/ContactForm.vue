<template>
    <transition enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
        <div class="modal is-active">
            <div class="modal-background"></div>
            <div class="modal-content" style="width:80%">
                <div class="box">
                    <span class="icon is-pulled-right">
                      <i class="fa fa-times close-button" @click="$emit('form-close')"></i>
                    </span>

                    <vue-form class="box"
                              :data="form">
                    </vue-form>
                </div>
            </div>
            <button class="modal-close is-large" aria-label="close"></button>
        </div>
    </transition>
</template>

<script>

import VueForm from '../vueforms/VueForm.vue';

export default {
    name: 'ContactForm',

    components: { VueForm },

    props: {
        id: {
            default: null,
        },
        type: {
            type: String,
            default: '',
        },
        action: {
            type: String,
            required: true,
        },
    },

    data() {
        return {
            form: {
                type: Object,
                default: null,
            },
        };
    },

    methods: {
        getEditForm(address) {
            axios.get(route('core.contacts.edit', address.id, false)).then(({ data }) => {
                this.$emit('form-loaded', data);
                this.form = data.editForm;
            }).catch((error) => {
                this.handleError(error);
            });
        },
        getCreateForm() {
            const params = { contactable_id: this.id, contactable_type: this.type };
            axios.get(route('core.contacts.create', params, false)).then(({ data }) => {
                this.form = data.createForm;
            }).catch((error) => {
                this.handleError(error);
            });
        },
    },
};

</script>

<style>
    .close-button {
        z-index: 1;
        position: relative;
        cursor: hand;
    }

    .modal.is-active {
        z-index: 1100;
    }

    .address-modal-mask {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .5);
        transition: opacity .3s ease;
    }

    .address-modal-container {
        min-width: 250px;
        min-height: 415px;
        width:80%;
        max-height: 90%;
        overflow-y: auto;
        margin: 0 auto;
        padding: 0;
        color: #3c3a3a;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
    }

    .address-modal-container .box {
        margin-bottom: 0;
        padding-left: 5px;
        padding-right: 5px;
    }
</style>
