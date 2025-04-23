<br/>color1
<font style="color:rgb(77, 77, 77);">在开发中，我们经常会发现一些可以重复利用的代码段，于是我们将其封装成函数以供调用。这类函数包括工具函数，但是又不止工具函数，因为我们可能也会封装一些重复的业务逻辑</font>

<br/>

<font style="color:rgb(77, 77, 77);">在 Vue 中使用Hook</font>

<font style="color:rgb(77, 77, 77);">下面我们来看一个简单的自定义 Hook（来自 Vue 官方文档）：</font>

<font style="color:rgb(77, 77, 77);">需求：在页面实时显示鼠标的坐标。 实现：没有使用 Hook。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731461445121-69a71aea-1ab5-4ee3-b401-465c95f89b34.png)

> <font style="color:rgb(77, 77, 77);">在没有封装的情况下，如果我们在另一个页面也需要这个功能，我们需要将代码复制过去。</font>
>
> <font style="color:rgb(77, 77, 77);">它声明了两个变量，并且在生命周期钩子 onMounted 和 onUnmounted 中书写了一些代码，如果这个页面需要更多的功能，那么会出现代码中存在很多变量、生命周期中存在很多逻辑写在一起的现象，使得这些逻辑混杂在一起，而使用 Hook 可以将其分隔开来（这也是为什么会有很多人使用 Hook 的原因，分离代码，提高可维护性！）</font>
>

#### <font style="color:rgb(79, 79, 79);">使用 Hook：</font>
```vue
<script setup>
import { useMouse } from './mouse.js'
 
const { x, y } = useMouse()
</script>
 
<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

<font style="color:rgb(77, 77, 77);">可以发现，比原来的代码更加简洁，这时如果加入其它功能的变量，也不会觉得眼花缭乱了。</font>

<font style="color:rgb(77, 77, 77);">当然，我们需要在外部定义这个 Hook：</font>

```vue
 
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'
 
// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)
 
  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }
 
  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))
 
  // 通过返回值暴露所管理的状态
  return { x, y }
}
```

#### <font style="color:rgb(79, 79, 79);">自定义hook </font>
<font style="color:rgb(77, 77, 77);">首先定义一个表格：</font>

```vue
<template>
  <el-table :data="tableData" style="width: 100%">
    <el-table-column prop="date" label="Date" width="180" />
    <el-table-column prop="name" label="Name" width="180" />
    <el-table-column prop="address" label="Address" />
  </el-table>
  <button @click="refresh">refresh</button>
</template>
```

<font style="color:rgb(77, 77, 77);">表格的数据通过 api 获取（一般写法）：</font>

```vue
<script lang="ts" setup>
import { onMounted, ref } from "vue";
import { getTableDataApi } from "./api.ts";
 
const tableData = ref([]);
const refresh=async () => {
  const data = await getTableDataApi();
  tableData.value = data;
}
 
onMounted(refresh);
</script>
```

<font style="color:rgb(77, 77, 77);">模拟 api：</font>

```json
export const getTableDataApi = () => {
  const data = [
  {
  date: '2016-05-03',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2016-05-02',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2016-05-04',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2016-05-01',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
]
return new Promise(resolve => {
  setTimeout(() => {
  resolve(data)
}, 100);
})
}
```

<font style="color:rgb(77, 77, 77);">如果存在多个表格，我们的 js 代码会变得比较复杂：</font>

```vue
<script lang="ts" setup>
import { onMounted, ref } from "vue";
import { getTableDataApi1, getTableDataApi2, getTableDataApi3 } from "./api.ts";
 
const tableData1 = ref([]);
const refresh1=async () => {
  const data = await getTableDataApi1();
  tableData1.value = data;
}
 
const tableData2 = ref([]);
const refresh2=async () => {
  const data = await getTableDataApi2();
  tableData2.value = data;
}
 
const tableData3 = ref([]);
const refresh3=async () => {
  const data = await getTableDataApi3();
  tableData3.value = data;
}
 
onMounted(refresh1);
</script>
```

<font style="color:rgb(77, 77, 77);">封装我们的 useTable：</font>

```javascript
// useTable.ts
import { ref } from 'vue'
export function useTable(api) {
  const data = ref([])
  const refresh = () => { api().then(res => data.value = res) };
  refresh()
  return [data, refresh]
}
```

<font style="color:rgb(77, 77, 77);">改造代码：</font>

```vue
<script lang="ts" setup>
  import { onMounted, ref } from "vue";
  import { getTableDataApi1, getTableDataApi2, getTableDataApi3 } from "./api.ts";
  import { useTable } from './useTable.ts'

  const [tableData1, refresh1] = useTable(getTableDataApi1);
  const [tableData2, refresh2] = useTable(getTableDataApi2);
  const [tableData3, refresh3] = useTable(getTableDataApi3);

  onMounted(refresh1);
</script>
```

<br/>color1
一般自定义 Hook 有返回数组的，也有返回对象的，上面 useTable 使用了返回数组的写法，useMouse 使用了返回对象的写法。数组是对应位置命名的，可以方便重命名，对象对于类型和语法提示更加友好。两种写法都是可以替换的。

因为 Hook 返回对象或者数组，那么它一定是一个非 async 函数（async 函数一定返回 Promise），所以在 Hook 中，一般使用 then 而不是 await 来处理异步请求。

返回值如果是对象，一般在函数中通过 reactive 创建一个对象，最后通过 toRefs 导出，这样做的原因是可以产生批量的可以解构的 Ref 对象，以免在解构返回值时丢失响应性。

<br/>

上面我们封装了一个简单的 hook，但是实际应用中并不会如此简单，下面我列出一个比较完整的 useTable 在实践中应该具备的功能，并在后续的文章部分完成它。

**封装表格组件逻辑：**

维护 api 的调用和刷新（已完成）

支持分页查询（页数、总条数、每页大小等）

支持 api 参数。

增加辅助功能（loading、立即执行等）

下面我们将对 useTable 进行改造，使其支持分页器。

先改造一些我们的 api，使其支持分页查询：

```json
export const getTableDataApi = (page, limit) => {
  const data = [
  {
  date: '2016-05-03',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2016-05-02',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2016-05-04',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2016-05-01',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2017-05-03',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2017-05-02',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2017-05-04',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
{
  date: '2017-05-01',
  name: 'Tom',
  address: 'No. 189, Grove St, Los Angeles',
},
]
return new Promise(resolve => {
  setTimeout(() => {
  resolve({
  total: data.length,
  data: data.slice((page - 1) * limit, (page - 1) * limit + limit)
})
}, 100);
})
}
```

 <font style="color:rgb(77, 77, 77);">如果没有使用 Hook，我们的 vue 文件应该是这样的：</font>

```vue
<template>
  <el-table :data="tableData" style="width: 100%">
    <el-table-column prop="date" label="Date" width="180" />
    <el-table-column prop="name" label="Name" width="180" />
    <el-table-column prop="address" label="Address" />
  </el-table>
  <button @click="refresh">refresh</button>
  <!-- 分页器 -->
  <el-pagination
    v-model:current-page="current"
    :page-size="size"
    layout="total, prev, pager, next"
    :page-sizes="sizeOption"
    :total="total"
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange"
    />
</template>

<script lang="ts" setup>
  import { onMounted, ref } from "vue";
  import { getTableDataApi } from "./api.ts";

  const tableData = ref([]); // 表格数据
  const current = ref(1); // 当前页数
  const sizeOption = [10, 20, 50, 100, 200]; // 每页大小选项
  const size = ref(sizeOption[0]); //每页大小
  const total = ref(0); // 总条数

  // 每页大小变化
  const handleSizeChange = (size: number) => {
    size.value = size;
    current.value = 1;
    // total.value = 0;
    refresh();
  };

  // 页数变化
  const handleCurrentChange = (page: number) => {
    current.value = page;
    // total.value = 0;
    refresh();
  };

  const refresh = async () => {
    const result = await getTableDataApi({
      page: current.value,
      limit: size.value,
    });
    tableData.value = result.data || [];
    total.value = result.total || 0;
  };

  onMounted(refresh);
</script>
```

<font style="color:rgb(77, 77, 77);">可以看出，如果存在多个表格，会创建很多套变量和重复的代码。</font>

<font style="color:rgb(77, 77, 77);">先写个 usePagination：该钩子接受一个回调函数，当页数改变时就会调用该函数。</font>

```javascript
import { reactive } from "vue";
export function usePagination(
  cb: any,
  sizeOption: Array<number> = [10, 20, 50, 100, 200]
): any {
  const pagination = reactive({
    current: 1,
    total: 0,
    sizeOption,
    size: sizeOption[0],
    // 维护page和size（一般是主动触发）
    onPageChange: (page: number) => {
      pagination.current = page;
      return cb();
    },
    onSizeChange: (size: number) => {
      pagination.current = 1;
      pagination.size = size;
      return cb();
    },
    // 一般调用cb后会还会修改total（一般是被动触发）
    setTotal: (total: number) => {
      pagination.total = total;
    },
    reset() {
      pagination.current = 1;
      pagination.total = 0;
      pagination.size = pagination.sizeOption[0];
    },
  });

  return [
    pagination,
    pagination.onPageChange,
    pagination.onSizeChange,
    pagination.setTotal,
  ];
}
```

<font style="color:rgb(77, 77, 77);">与 useTable 结合：代码非常简单，在调用 api 时传入参数，并在接受返回值时更新 data 和 total。这里我们的 refresh 函数是一个返回 Promise 的函数，能够支持在调用 refresh 处再链接 then 进行下一层处理。</font>

```javascript
export function useTable(api: (params: any) => Promise<T>) {
  const [pagination, , , setTotal] = usePagination(() => refresh());
  const data = ref([]);

  const refresh = () => {
    return api({ page: pagination.current, limit: pagination.size }).then(
      (res) => {
        data.value = res.data;
        setTotal(res.total);
      }
    );
  };
  return [data, refresh, pagination];
}
```

<font style="color:rgb(77, 77, 77);">使用 useTable：</font>

```vue
export function useTable(api: (params: any) => Promise<T>) {
  const [pagination, , , setTotal] = usePagination(() => refresh());
  const data = ref([]);

  const refresh = () => {
    return api({ page: pagination.current, limit: pagination.size }).then(
      (res) => {
        data.value = res.data;
        setTotal(res.total);
      }
    );
  };
  return [data, refresh, pagination];
  }
  // 注：我们新建一个文件 customHooks.js 并将 usePagination 和 useTable 放在里面。

  // 使用 useTable：

    <template>
    <el-table :data="tableData" style="width: 100%">
    <el-table-column prop="date" label="Date" width="180" />
    <el-table-column prop="name" label="Name" width="180" />
    <el-table-column prop="address" label="Address" />
    </el-table>
    <button @click="refresh">refresh</button>
    <!-- 分页器 -->
    <el-pagination
  v-model:current-page="pagination.current"
  :page-size="pagination.size"
  layout="total, prev, pager, next"
  :page-sizes="pagination.sizeOption"
  :total="pagination.total"
  @size-change="pagination.onSizeChange"
  @current-change="pagination.onCurrentChange"
    />
    </template>

    <script lang="ts" setup>
    import { onMounted, ref } from "vue";
  import { getTableDataApi } from "./api.ts";
  import { useTable } from './customHooks.ts'

  const [tableData, refresh, pagination] = useTable(getTableDataApi);

  onMounted(refresh);
</script>
```

