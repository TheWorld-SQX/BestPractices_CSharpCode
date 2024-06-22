查看目录名，tree tree /Full

列里面按钮并排
``` html
<el-table-column label="操作" width="120" align="center">
        <template #default="scope">
          <!-- <el-button type="success" size="small" icon="edit" circle plain title="编辑" v-hasPermi="['prodmatlistmanage:mesprodmatlistmanage:edit']" @click="handleUpdate(scope.row)"></el-button>
          <el-button type="danger" size="small" icon="delete" circle plain title="删除" v-hasPermi="['prodmatlistmanage:mesprodmatlistmanage:delete']" @click="handleDelete(scope.row)"></el-button> -->
          <el-row :gutter="10">
            <el-col :span="12">
              <el-button type="success" size="small" v-hasPermi="['prodmatlistmanage:mesprodmatlistmanage:edit']" @click="handleUpdate(scope.row)">修改</el-button>
            </el-col>
            <el-col :span="12">
               <el-button type="danger" size="small" v-hasPermi="['prodmatlistmanage:mesprodmatlistmanage:delete']" @click="handleDelete(scope.row)">删除</el-button>
            </el-col>
          </el-row>
        </template>
      </el-table-column>
    </el-table>
```
为了使 `columns` 列表中的列动态渲染，并使其自动适应表格宽度，我们可以在模板中使用 `v-for` 循环动态生成表格列。我们还可以根据 `columns` 中的 `visible` 属性来控制列的显示。

以下是修改后的完整代码示例：

动态渲染表格例 示例代码

```html
<template>
  <el-table :data="dataList" v-loading="loading" ref="table" border header-cell-class-name="el-table-header-cell"
            highlight-current-row @sort-change="sortChange" style="width: 100%;">
    
    <!-- 动态渲染表格列 -->
    <template v-for="column in columns" :key="column.prop">
      <el-table-column 
        v-if="column.visible"
        :prop="column.prop" 
        :label="column.label" 
        align="center" 
        :show-overflow-tooltip="true"
      />
    </template>

    <el-table-column label="操作" width="120" align="center">
      <template #default="scope">
        <el-row :gutter="10">
          <el-col :span="12">
            <el-button type="success" size="small" @click="handleUpdate(scope.row)">修改</el-button>
          </el-col>
          <el-col :span="12">
            <el-button type="danger" size="small" @click="handleDelete(scope.row)">删除</el-button>
          </el-col>
        </el-row>
      </template>
    </el-table-column>
  </el-table>
</template>

<script setup>
import { ref } from 'vue';

// 表格数据和加载状态
const dataList = ref([]);
const loading = ref(false);

// 定义列配置
const columns = ref([
  { visible: true, prop: 'workOrderNo', label: '工单编号' },
  { visible: true, prop: 'productNo', label: '产品编号' },
  { visible: true, prop: 'productName', label: '产品名称' },
  { visible: true, prop: 'parentNo', label: '父级物料号' },
  { visible: true, prop: 'childNo', label: '子级物料号' },
  { visible: true, prop: 'materialType', label: '物料类型' },
  { visible: true, prop: 'materialName', label: '物料名称' },
  { visible: false, prop: 'materialSpec', label: '物料规格' },
  { visible: false, prop: 'materialModel', label: '物料型号' },
  { visible: false, prop: 'unit', label: '计量单位' },
  { visible: false, prop: 'materialUsage', label: '物料用量' },
  { visible: false, prop: 'supplier', label: '供应商信息或采购方式' },
  { visible: false, prop: 'material', label: '位置或安装位置' },
  { visible: false, prop: 'process', label: '工艺路线' },
  { visible: false, prop: 'unitCost', label: '单价' },
  { visible: false, prop: 'totalCost', label: '总价' },
  { visible: false, prop: 'remark', label: '备注' },
  { visible: false, prop: 'createTime', label: '创建时间' },
  { visible: false, prop: 'createBy', label: '创建者' },
  { visible: false, prop: 'updateTime', label: '更新时间' },
  { visible: false, prop: 'updateBy', label: '更新者' },
]);

// 排序变化处理函数
const sortChange = () => {
  // 你的排序逻辑
};

// 处理更新
const handleUpdate = (row) => {
  // 处理更新逻辑
};

// 处理删除
const handleDelete = (row) => {
  // 处理删除逻辑
};
</script>

<style scoped>
/* 样式 */
</style>
```

### 解释

1. **模板部分 (`<template>`)**：
   - 使用 `v-for` 指令循环遍历 `columns` 数组，动态生成 `el-table-column`。
   - 通过 `v-if="column.visible"` 来控制列的显示。

2. **脚本部分 (`<script setup>`)**：
   - 使用 `ref` 定义响应式的数据，包括 `dataList`、`loading` 和 `columns`。
   - `columns` 数组定义了表格中要显示的列及其属性。通过 `visible` 属性来控制列是否显示。
   - 定义了 `sortChange`、`handleUpdate` 和 `handleDelete` 方法。

3. **样式部分 (`<style scoped>`)**：
   - 可以在这里添加组件的局部样式。

这种方法确保了所有列都可以根据 `columns` 数组中的 `visible` 属性动态显示，并且表格宽度自动适应。通过调整 `columns` 数组中的 `visible` 属性，可以控制哪些列显示，哪些列隐藏。
