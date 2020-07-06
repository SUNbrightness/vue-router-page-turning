<template>
    <div>
        <transition :name="transitionName">
            <slot></slot>
        </transition>
    </div>
</template>

<script>
    export default {
        name: "vue-router-page-truning",
        data() {
            return {
                transitionName: 'vue-router-page-truning-left',
                historyQueue: [],
            }
        },
        props: {
            exclude: {
                type: Array,
                default: function () {
                    return ['/']
                },
            },
            transitionTime: {
                type: Number,
                default: 500
            },
            transitionRange: {
                type: String,
                default: '100%'
            }
        },
        created() {
            this.appendStyle(
                [
                    `.vue-router-page-truning-right-enter-active,
      .vue-router-page-truning-right-leave-active,
      .vue-router-page-truning-left-enter-active,
      .vue-router-page-truning-left-leave-active {
        will-change: transform;
        transition: all ${this.transitionTime}ms;
        position: absolute;
      }`,
                    `.vue-router-page-truning-right-enter {
        opacity: 0;
        transform: translate3d(-${this.transitionRange}, 0, 0);
      }`,
                    `.vue-router-page-truning-right-leave-active {
        opacity: 0;
        transform: translate3d(${this.transitionRange}, 0, 0);
      }`,
                    `.vue-router-page-truning-left-enter {
        opacity: 0;
        transform: translate3d(${this.transitionRange}, 0, 0);
      }`,
                    `.vue-router-page-truning-left-leave-active {
        opacity: 0;
        transform: translate3d(-${this.transitionRange}, 0, 0);
      }  `
                ]
            )
        },
        watch: {
            //使用watch 监听$router的变化切换不同方向动画
            $route(to, from) {
                let toPath = to.path;
                //维护结构
                //历史记录最多50
                if (this.historyQueue.length > 50) {
                    this.historyQueue.shift();
                }
                //是否是返回？
                if (this.historyQueue.length > 1) {
                    let popPath = this.historyQueue[this.historyQueue.length - 2];
                    if (popPath == toPath) {
                        this.transitionName = 'vue-router-page-truning-right';
                        //出栈
                        this.historyQueue.pop();
                        return;
                    }
                }

                //判断是否是排除路由
                if (_.indexOf(this.exclude, toPath) != -1){
                this.transitionName = '';
                }else{
                  //非返回
                  this.transitionName = 'vue-router-page-truning-left';
                }

                //浏览记录入栈
                this.historyQueue.push(toPath);
            }
        },
        methods: {
            appendStyle(styles) {
                var style = document.createElement("style");
                style.type = "text/css";
                try {
                    for (let s of styles) {
                        style.appendChild(document.createTextNode(s));
                    }
                } catch (ex) {
                    for (let s of styles) {
                        style.styleSheet.cssText += s; //针对IE
                    }
                }
                var head = document.getElementsByTagName("head")[0];
                head.appendChild(style);
            }
        }


    };
</script>

<style>
</style>
