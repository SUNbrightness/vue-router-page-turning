# vue-router-page-turning

## Project setup
```
npm install vue-router-page-turning -S
```

## demo

```
<template>
    <div id="app">
        <vue-router-page-turning>
            <!-- 动画如果出现卡顿建议使用 keep-alive -->
            <keep-alive>
                <router-view />
            </keep-alive>
        </vue-router-page-turning>
    </div>
</template>
<script>
    import VueRouterPageTurning from 'vue-router-page-turning';
    export default {
        name: "app",
        components: {
            VueRouterPageTurning
        }
    };
</script>

<style>


</style>

```
## prop

Attribute|type| default |explain
---|---|---|---
transitionTime|number|500|过渡时间
transitionRange|string|100%|过渡幅度
exclude|Array|[]|不需要动画的路由

## show
![show](https://sunbrightness.github.io/my-img/1591771235893.gif)
