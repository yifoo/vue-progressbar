<style>
.__cov-progress {
  opacity: 1;
  z-index: 999999;
}
</style>
<template>
  <div class="__cov-progress" :style="style"></div>
</template>
<script>
const inBrowser = typeof window !== 'undefined'
export default {
    name: 'VueProgress',
    serverCacheKey: () => 'Progress',
    computed: {
        style () {
            const progress = this.progress
            const options = progress.options
            const isShow = !!options.show
            const location = options.location
            const top = options.top ? options.top:'0px';
            const left = options.left ? options.left:'0px';
            const bottom = options.bottom ? options.bottom:'0px';
            const right = options.right ? options.right:'0px';
            const style = {
                'background-color': options.canSuccess ? options.color : options.failedColor,
                'opacity': options.show ? 1 : 0,
                'position': options.position
            }
            if (location === 'top' || location === 'bottom') {
                location === 'top' ? style.top = top : style.bottom = bottom
                if (!options.inverse) {
                    style.left = left
                } else {
                    style.right = right
                }
                style.width = progress.percent + '%'
                style.height = options.thickness
                style.transition = (isShow ? "width " + options.transition.speed + ", " : "") + "opacity " + options.transition.opacity
            } else if (location === 'left' || location === 'right') {
                location === 'left' ? style.left = left : style.right = right
                if (!options.inverse) {
                    style.bottom = bottom
                } else {
                    style.top = top
                }
                style.height = progress.percent + '%'
                style.width = options.thickness
                style.transition = (isShow ? "height " + options.transition.speed + ", " : "") + "opacity " + options.transition.opacity
            }
            return style
        },
        progress() {
            if (inBrowser) {
                return window.VueProgressBarEventBus.RADON_LOADING_BAR
            } else {
                return {
                    percent: 0,
                    options: {
                        canSuccess: true,
                        show: false,
                        color: 'rgb(19, 91, 55)',
                        failedColor: 'red',
                        thickness: '2px',
                        transition: {
                            speed: '0.2s',
                            opacity: '0.6s',
                            termination: 300
                        },
                        location: 'top',
                        autoRevert: true,
                        inverse: false
                    }
                }
            }
        }
    }
}

</script>
