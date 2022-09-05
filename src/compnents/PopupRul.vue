<template>
    <div class="wapper-popup" v-bind:class="getClassPopup">
        <div class="rule">
            <h3>Luật chơi</h3>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit.
                Vero modi expedita consequuntur accusantium culpa reiciendis voluptates. 
                Voluptatibus quo magni vel omnis veniam deleniti eaque vero magnam, 
                mollitia dignissimos, voluptate corporis?
            </p>
            <button 
                v-on:click="confirm"
                class="confirm">Đã hiểu</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'popup-rule',
    props: {
        isOpenPopup: { type: Boolean, default: false }
    },
    data() {
        return {}
    },
    methods: {
        confirm() {
            //truyền event ra app
            // console.log('confirm PopupRule');
            this.$emit('handleConfirm');
        }
    },
    computed: {
        getClassPopup:function() {
            return {
                'open-popup' : this.isOpenPopup
            }
        }
    }
}
</script>

<style>
.wapper-popup {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.4);
    opacity: 0;
    visibility: hidden;
    transition: all .3s ease;
}
.wapper-popup.open-popup {
    opacity: 1;
    visibility: visible;
}
.rule {
    width: 350px;
    padding: 20px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) scale(1.2);
    background-color: #fff;
    position: absolute;
    font-family: Arial, Helvetica, sans-serif;
    transition: all .3s ease;
}
/* scale -> class */
.open-popup .rule {
    transform: translate(-50%, -50%) scale(1);
}
.rule h3 {
    margin-bottom: 10px;
}
.rule .confirm {
    cursor: pointer;
    margin-top: 20px;
    padding: 8px 15px;
    border: 2px solid #333;
    background-color: #fff;
    transition: all .3s ease;
}
.rule .confirm:hover {
    color: #fff;
    background-color: #333;
}
/* Kết hợp với CSS3 để xây dựng hiệu ứng - Zoom in và Zoom out */
</style>
