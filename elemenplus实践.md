```
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
