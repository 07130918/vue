<template>
    <div class="main">
        <button @click="animationType = 'slide'">Slide</button>
        <button @click="animationType = 'fade'">Fade</button>
        <p>{{animationType}}</p>
        <br>
        <TransitionGroupPractice :animationType="animationType"></TransitionGroupPractice>
        <button @click="show = !show">切り替え</button>
        <br><br>
        <TransitionCircle :show="show"></TransitionCircle>
        <br>
        <!-- <transition name="v"> <-デフォルト -->
        <transition
            enter-active-class="animate__animated animate__bounce" leave-active-class="animate__animated animate__shakeX" appear>
            <h4 v-if="show">Hello</h4>
        </transition>
        <!-- animationとtransitionの効果時間が異なる時どちらに合わせるかをtypeで指定 -->
        <transition :name="animationType" appear>
            <p v-if="show">bye</p>
        </transition>
        <!-- mode="out-in"で要素が完全に出きってから切り替わる -->
        <transition :name="animationType" mode="out-in">
            <p v-if="show" key="bye">さよなら</p>
            <p v-if="!show" key="hello">こんにちは</p>
        </transition>

        <button @click="currentComponent = 'ComponentA'">ComponentA</button>
        <button @click="currentComponent = 'ComponentB'">ComponentB</button>
        <transition :name="animationType" mode="out-in">
            <component :is="currentComponent"></component>
        </transition>
    </div>
</template>

<script>
import TransitionGroupPractice from './transition_material/TransitionGroupPractice.vue'
import TransitionCircle from './transition_material/TransitionCircle.vue'
import ComponentA from './transition_material/ComponentA.vue'
import ComponentB from './transition_material/ComponentB.vue'

export default {
    data() {
        return {
            show: true,
            animationType: "fade",
            currentComponent: "ComponentA",
        };
    },
    components: {
        TransitionGroupPractice,
        TransitionCircle,
        ComponentA,
        ComponentB,
    }
}
</script>

<style>
.fade-move {
    /* transition-groupのみにつけれる */
    transition: transform .5s;
}
.fade-enter { /*現れるときの最初の状態*/
    opacity: 0;
}
.fade-enter-active { /*現れるときのトランジションの状態*/
    transition: opacity .3s;
}
.fade-enter-to { /*現れるときの最後の状態*/
    opacity: 1;
}
.fade-leave { /*消えるときの最初の状態*/
    opacity: 1;
}
.fade-leave-active { /*消えるときのトランジションの状態*/
    transition: opacity .3s;
    position: absolute;
    width: 200px;
}
.fade-leave-to { /*消えるときの最後の状態*/
    opacity: 0;
}

.slide-move {
    transition: transform .5s;
}
.slide-enter, .slide-leave-to {
    opacity: 0;
}
.slide-enter-active{
    animation: slide-in .5s;
    transition: opacity .5s;
}
.slide-leave-active{
    animation: slide-in .5s reverse;
    transition: opacity .5s;
    position: absolute;
    width: 200px;
}

@keyframes slide-in {
    from {
        transform: translateX(100px);
    }
    to {
        transform: translateX(0);
    }
}

.main {
    width: 70%;
    margin: 3vh auto 20vh;
    text-align: center;
}
</style>
