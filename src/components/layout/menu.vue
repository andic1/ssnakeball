<template>
    <div class="links" ref="links"></div>
    <div class="nav">
        <div class="nav_soundbtn" :class="{'nav_soundbtn_mute':if_mute}" @click="switch_mutemode">
            <p class="nav_soundbtn_tip _font_1">SOUND</p>
            <div class="nav_soundbtn_wave">
                <div style="--s:1;--d:1"></div>
                <div style="--s:1.5;--d:2"></div>
                <div style="--s:2;--d:3"></div>
            </div>
        </div>
        <svg
            class="nav_colorbtn"
            :class="{'nav_colorbtn_light':current_colormode==='light','nav_colorbtn_dark':current_colormode==='dark'}"
            viewBox="0 0 500 500"
            @click="switch_colormode"
        >
            <path
                class="nav_colorbtn_moon"
                d="M143.03,250c0,64.73,31.22,122.16,79.41,158.06v0.05c-87.33,0-158.12-70.79-158.12-158.12
		S135.12,91.88,222.44,91.88v0.05C174.25,127.84,143.03,185.27,143.03,250z"
            />
            <path class="nav_colorbtn_sun" d="M396.79,250c0,87.33-70.79,158.12-158.12,158.12V91.88C326,91.88,396.79,162.67,396.79,250z" />
            <path class="nav_colorbtn_sunline" d="M238.67,53c108.8,0,197,88.2,197,197s-88.2,197-197,197" />
        </svg>
    </div>
    <div class="copyright _font_1" ref="copyright"></div>
</template>

<script setup>
import { global } from "@/stores/global";
import { ref } from "vue";
import gsap from "gsap";
import { Howler } from "howler";
const store = global();
//当前的颜色模式：默认为light
const current_colormode = ref("light");
// 切换页面颜色模式
function switch_colormode() {
    document.querySelector("html").classList.toggle("_darkmode");
    current_colormode.value = current_colormode.value === "light" ? "dark" : "light";
}
//是否静音，默认为false
let if_mute = ref(false);
// 切换页面静音状态
function switch_mutemode() {
    if_mute.value = !if_mute.value;
    Howler.mute(if_mute.value);
}
// DOM元素
const links = ref(null);
const copyright = ref(null);
// 显示菜单
function show_menu() {
    links.value.classList.remove("_hidden");
    copyright.value.classList.remove("_hidden");
}
// 隐藏菜单
function hidden_menu() {
    links.value.classList.add("_hidden");
    copyright.value.classList.add("_hidden");
}
// 储存全局功能函数
store.show_menu = show_menu;
store.hidden_menu = hidden_menu;
</script>

<style scoped>
* {
    --scale: 1;
    z-index: 10000;
}
.links {
    position: fixed;
    left: var(--margin_x);
    top: var(--margin_y);
    transform: translateY(-50%);
    transition: opacity 1s var(--ease_out), visibility 1s var(--ease_out);
}
.links svg {
    height: calc(var(--scale) * 3rem);
    fill: var(--color_front);
    margin-right: calc(var(--scale) * 3rem);
    overflow: visible;
    transition: fill 0.5s var(--ease_out);
    cursor: pointer;
}
@media (hover: hover) {
    .links svg:hover {
        fill: var(--color_gray);
    }
}
.nav {
    flex-direction: row-reverse;
    position: fixed;
    align-items: center;
    right: var(--margin_x);
    top: var(--margin_y);
    transform: translateY(-50%);
}
.nav_soundbtn {
    justify-content: center;
    align-items: center;
    background-color: var(--color_front);
    border-radius: calc(var(--scale) * 2rem);
    padding: calc(var(--scale) * 1rem) calc(var(--scale) * 1.5rem);
    transition: background-color 0.5s var(--ease_out);
    cursor: pointer;
}
.nav_soundbtn_tip {
    color: var(--color_back);
    line-height: calc(var(--scale) * 1.5rem);
    margin-right: calc(var(--scale) * 0.6rem);
}
.nav_soundbtn_wave {
    align-items: center;
    height: calc(var(--scale) * 1.5rem);
    overflow: hidden;
    transition: height 0.5s var(--ease_out);
}
.nav_soundbtn_wave div {
    width: calc(var(--scale) * 0.5rem * var(--s));
    height: calc(var(--scale) * 0.5rem * var(--s));
    background-color: var(--color_back);
    border-radius: 100%;
    margin: 0 calc(var(--scale) * 0.4rem);
    animation: nav_soundbtn_wave 2s var(--ease_out) infinite calc(var(--d) * 0.2s);
}
@keyframes nav_soundbtn_wave {
    0% {
        transform: scale(1);
    }
    50% {
        transform: scale(1.5);
    }
    100% {
        transform: scale(1);
    }
}
@media (hover: hover) {
    .nav_soundbtn:hover {
        background-color: var(--color_gray);
    }
}
.nav_soundbtn_mute .nav_soundbtn_wave {
    height: calc(var(--scale) * 0.25rem);
}
.nav_soundbtn_mute .nav_soundbtn_wave div {
    animation-play-state: paused;
}
.nav_colorbtn {
    width: calc(var(--scale) * 3.8rem);
    height: calc(var(--scale) * 3.8rem);
    margin-right: calc(var(--scale) * 3rem);
    cursor: pointer;
}
.nav_colorbtn_moon,
.nav_colorbtn_sun,
.nav_colorbtn_sunline {
    transition: fill 0.5s var(--ease_out), stroke 0.5s var(--ease_out);
}
.nav_colorbtn_moon {
    fill: var(--color_front);
}
.nav_colorbtn_sun {
    fill: var(--color_front);
}
.nav_colorbtn_sunline {
    fill: none;
    stroke: var(--color_front);
    stroke-width: 25;
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-dasharray: 0, 70;
    animation: nav_colorbtn_sunline 0.1s linear infinite;
}
@keyframes nav_colorbtn_sunline {
    0% {
        stroke-dashoffset: 0;
    }
    100% {
        stroke-dashoffset: -70;
    }
}
.nav_colorbtn_light .nav_colorbtn_moon {
    fill: var(--color_gray);
}

@media (hover: hover) {
    .nav_colorbtn_light:hover .nav_colorbtn_moon {
        fill: var(--color_front);
    }
}
.nav_colorbtn_dark .nav_colorbtn_sun {
    fill: var(--color_gray);
}
.nav_colorbtn_dark .nav_colorbtn_sunline {
    stroke: var(--color_gray);
}

@media (hover: hover) {
    .nav_colorbtn_dark:hover .nav_colorbtn_sun {
        fill: var(--color_front);
    }
    .nav_colorbtn_dark:hover .nav_colorbtn_sunline {
        stroke: var(--color_front);
    }
}

.copyright {
    position: fixed;
    left: var(--margin_x);
    bottom: var(--margin_y);
    line-height: calc(var(--scale) * 2rem);
    color: var(--color_front);
    text-decoration: none;
    transition: opacity 1s var(--ease_out), visibility 1s var(--ease_out),
        color 0.5s var(--ease_out);
    transform: translateY(50%);
}
@media (hover: hover) {
    .copyright:hover {
        color: var(--color_gray);
    }
}
</style>