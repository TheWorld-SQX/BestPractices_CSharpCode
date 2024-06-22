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


##
flase不要一下改完；页面会重新渲染；



##
为了实现点击“修改”按钮后显示表格并动态渲染列，可以按以下步骤进行修改：

1. **定义表格的显示状态**：使用一个响应式变量控制表格的显示和隐藏。
2. **在点击“修改”按钮时更新表格数据并显示表格**。

下面是完整的示例代码：

### 示例代码

```html
<template>
  <div>
    <!-- 表单部分 -->
    <el-dialog :title="title" :visible.sync="open">
      <el-form :model="form" ref="formRef">
        <!-- 表单项 -->
        <!-- 示例表单项 -->
        <el-form-item label="物料名称" prop="materialName">
          <el-input v-model="form.materialName"></el-input>
        </el-form-item>
        <!-- 更多表单项 -->
      </el-form>
      <div slot="footer">
        <el-button @click="open = false">取消</el-button>
        <el-button type="primary" @click="handleSubmit">提交</el-button>
      </div>
    </el-dialog>

    <!-- 表格部分 -->
    <el-table :data="dataList" v-loading="loading" ref="table" border header-cell-class-name="el-table-header-cell"
              highlight-current-row @sort-change="sortChange" style="width: 100%;" v-if="tableVisible">
      <!-- 动态渲染表格列 -->
      <template v-for="column in columns" :key="column.prop">
        <el-table-column v-if="column.visible" :prop="column.prop" :label="column.label" align="center"
                         :show-overflow-tooltip="true" />
      </template>
    </el-table>
    <el-button type="success" size="small" v-hasPermi="['prodmatlistmanage:mesprodmatlistmanage:edit']"
               @click="handleUpdate(scope.row)">修改</el-button>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import { getmesprodmatlistmanage } from 'path-to-your-api'; // 请根据你的实际路径修改

const dataList = ref([]);
const loading = ref(false);
const tableVisible = ref(false); // 控制表格显示状态
const formRef = ref();
const form = ref({}); // 存储表单数据
const title = ref('');
const open = ref(false);
const opertype = ref(0);

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

const reset = () => {
  form.value = {};
};

const handleUpdate = (row) => {
  reset();
  const id = row.id || ids.value;
  getmesprodmatlistmanage(id).then((res) => {
    const { code, data } = res;
    if (code === 200) {
      tableVisible.value = true; // 显示表格
      title.value = '修改生产物料清单管理';
      opertype.value = 2;

      dataList.value = [data]; // 更新表格数据，只显示当前修改的行
    }
  });
};

const sortChange = () => {
  // 你的排序逻辑
};

const handleSubmit = () => {
  formRef.value.validate((valid) => {
    if (valid) {
      // 提交表单
      console.log('Form submitted:', form.value);
      open.value = false;
    } else {
      console.log('Form validation failed.');
    }
  });
};
</script>

<style scoped>
/* 样式 */
</style>
```

### 解释

1. **模板部分 (`<template>`)**：
   - 使用 `v-if="tableVisible"` 来控制表格的显示和隐藏。
   - 在表格之外添加一个按钮，点击按钮时调用 `handleUpdate` 方法。
   - 表单部分通过 `el-dialog` 和 `el-form` 进行展示和编辑数据。

2. **脚本部分 (`<script setup>`)**：
   - `tableVisible`：用于控制表格的显示和隐藏状态。
   - `dataList`：存储表格数据，初始为空数组。
   - `reset` 方法：用于重置表单数据。
   - `handleUpdate` 方法：点击“修改”按钮时调用，获取数据并更新 `dataList`，同时显示表格。
   - `handleSubmit` 方法：用于提交表单数据。

3. **样式部分 (`<style scoped>`)**：
   - 可根据需要添加自定义样式。

通过这种方式，当点击“修改”按钮时，会获取相应的数据并更新表格的显示内容，同时将表格显示出来。这样可以动态地展示和修改表格中的数据。
