[视频地址]( https://v.douyin.com/iPxXoUbF/ )

### 难点和亮点总结
#### **难点**：
1. **滚动监听的性能问题**：
    - 传统的滚动监听（`scroll` 事件）在页面滚动时会频繁触发，导致性能开销较大，尤其是在图片较多的情况下容易引发页面卡顿。
    - 需要解决如何减少频繁触发渲染的问题，同时提升性能。
2. **兼容性与降级处理**：
    - `IntersectionObserver` 的浏览器兼容性（尤其是低版本浏览器）需要处理，确保功能在所有目标用户的设备上正常运行。
    - 需要实现降级策略，在不支持 `IntersectionObserver` 的环境中使用 `scroll` 事件作为备用方案。
3. **图片懒加载的时机优化**：
    - 确保图片懒加载的时机恰到好处：既要在图片接近可视区域时提前加载，又要避免过早加载浪费流量。
    - 对高分辨率图片、慢网速环境或移动端流量有限的场景需要特别优化。
4. **指令封装的通用性与灵活性**：
    - 自定义指令需要足够通用，支持多种场景（如动态图片列表、瀑布流布局等）。
    - 同时需要对外提供灵活的配置项（如预加载阈值、加载动画等），以适应不同需求。

---

#### **亮点**：
1. **使用 IntersectionObserver 替代传统方法**：
    - 使用 `IntersectionObserver` 可以高效监测图片是否进入可视区域，不再依赖 `scroll` 事件，从根本上解决滚动监听的性能瓶颈。
    - 避免了手动计算图片位置和滚动距离，提高了代码的可维护性。
2. **封装自定义指令的模块化设计**：
    - 通过 Vue 3 的自定义指令，将图片懒加载功能抽象为一个指令（如 `v-lazy`），简化了组件的调用方式。
    - 只需在模板中绑定指令即可，使用简单直观，降低了开发复杂度。

示例：

```vue

<img v-lazy="imageSrc" alt="Lazy loaded image" />
```

3. **支持动态加载和热插拔**：
    - 指令封装支持动态 DOM 元素（如通过 `v-for` 渲染的图片列表），无需额外处理。
    - Vue 3 的生命周期钩子结合 `IntersectionObserver` 的监听解绑机制，避免了资源泄漏。
4. **性能提升显著**：
    - 对比传统方案，`IntersectionObserver` 的性能更优，尤其在有大量图片的长列表页面中，滚动流畅度得到明显改善。
    - 减少了对主线程的占用，有效提升页面渲染性能。
5. **可配置化与扩展性**：
    - 支持懒加载图片的占位图（加载前显示默认图片，加载完成后替换成目标图片）。
    - 可扩展加载动画或错误处理（如加载失败显示替代图）。

示例配置：

```javascript
 <template>
  <div>
      <div v-for="item in arr">
          <img height="500" :data-index="item" v-lazy="item" width="360" alt="">
      </div>
  </div>
</template>

<script setup lang='ts'>
import type { Directive } from 'vue'
const images: Record<string, { default: string }> = import.meta.glob('./assets/images/*.*', {eager:true})
let arr = Object.values(images).map(v => v.default)

let vLazy: Directive<HTMLImageElement, string> = async (el, binding) => {
  let url = await import('./assets/vue.svg')
  el.src = url.default;
  let observer = new IntersectionObserver((entries) => {
      console.log(entries[0], el)
      if (entries[0].intersectionRatio > 0 && entries[0].isIntersecting) {
          setTimeout(() => {
              el.src = binding.value;
          }, 2000)
          observer.unobserve(el)
      }
  })
  observer.observe(el)
}

</script>

<style scoped lang='less'></style>
```

6. **前端工程化实践**：
    - 将指令与业务逻辑分离，形成独立模块，可在多个项目中复用。
    - 提供单元测试和兼容性测试用例，保障代码质量。

---

#### 总结一句话：
通过 Vue 3 的自定义指令结合 `IntersectionObserver` 实现图片懒加载，不仅提升了页面性能，还简化了代码逻辑，解决了传统滚动监听方式的缺陷，是一个兼顾性能和开发体验的优雅解决方案。

