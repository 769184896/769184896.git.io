<template>
  <div>
    <el-table
    :data="tableData"
    border
    style="width: 100%">
    <el-table-column
      align="center"
      prop="date"
      label="订单编号"
      width="180">
    </el-table-column>
    <el-table-column
      align="center"
      prop="name"
      label="广告主"
      width="180">
    </el-table-column>
    <el-table-column
      prop="address"
      align="center"
      label="下单内容">
      <template>
        <span class='check-out' @click='dialogVisible=true'>查看</span>
      </template>
    </el-table-column>
    <el-table-column
      prop="address"
      align="center"
      label="下单时间">
    </el-table-column>
    <el-table-column
      prop="address"
      align="center"
      label="订单状态">
    </el-table-column>
    <el-table-column
      prop="address"
      align="center"
      label="素材">
      <template>
        <img class="my-img" src="../assets/logo.png" alt="">
      </template>
    </el-table-column>
    <el-table-column label="操作" align="center" width="">
      <template slot-scope="scope">
          <el-button 
            @click.native.prevent="handleEdit(scope.$index, scope.row)" 
            type="text"
              size="small">确认订单
          </el-button>
          <el-button                            	
            @click="putInAD(scope.$index, scope.row)" 
            type="text" 
            size="small">投放广告
          </el-button>
          <el-button                            	
            @click="deleteRow(scope.$index, scope.row)" 
            type="text" 
            size="small">查看监播数据
          </el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- 分页 -->
  <el-pagination
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange"
    :current-page="currentPage4"
    :page-sizes="[100, 200, 300, 400]"
    :page-size="100"
    layout="total, sizes, prev, pager, next, jumper"
    :total="400">
  </el-pagination>
  <!-- dialog -->
    <el-dialog
    title="查看订单内容"
    :visible.sync="dialogVisible"
    width="70%">
      <div class="dialog-content">
        <h1>联系信息</h1>
        <p>姓名：</p>
        <p>手机号码：</p>
        <h1>投放信息</h1>
        <p>项目：  投放时间：  项目均价：</p>
      </div>
      <span slot="footer" class="dialog-footer">
        <!-- <el-button @click="dialogVisible = false">取 消</el-button> -->
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
    <!-- 投放广告 -->
    <el-dialog
      title="投放广告"
      :visible.sync="dialogVisibleAD"
      width="70%">
      <div class="dialog-content">
        <h1>请上传素材</h1>
        <el-upload
          class="upload-demo"
          action="https://jsonplaceholder.typicode.com/posts/"
          :on-preview="handlePreview"
          :on-remove="handleRemove"
          :before-remove="beforeRemove"
          multiple
          :limit="3"
          :on-exceed="handleExceed"
          :file-list="fileList">
          <el-button size="small" type="primary">点击上传</el-button>
          <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>
        <h1>请关联实施订单</h1>
        <el-select v-model="value" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </div>
      <span slot="footer" class="dialog-footer">
        <!-- <el-button @click="dialogVisible = false">取 消</el-button> -->
        <el-button type="primary" @click="dialogVisibleAD = false">确 定</el-button>
      </span>
    </el-dialog>
    <!-- 监播数据 -->
    <div>
      <h1>监播信息</h1>
      <el-tabs v-model="activeName" @tab-click="handleClick">
        <el-tab-pane label="用户管理" name="first"></el-tab-pane>
        <el-tab-pane label="配置管理" name="second"></el-tab-pane>
        <el-tab-pane label="角色管理" name="third"></el-tab-pane>
        <el-tab-pane label="定时任务补偿" name="fourth"></el-tab-pane>
        <el-table
          :data="tableData"
          border
          style="width: 100%">
          <el-table-column
            prop="date"
            label="日期"
            width="180">
          </el-table-column>
          <el-table-column
            prop="name"
            label="姓名"
            width="180">
          </el-table-column>
          <el-table-column
            prop="address"
            label="地址">
          </el-table-column>
        </el-table>
      </el-tabs>
      <h1>验收信息</h1>
      <el-table
        :data="tableData"
        border
        style="width: 100%">
        <el-table-column
          prop="date"
          label="日期"
          width="180">
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          width="180">
        </el-table-column>
        <el-table-column
          prop="address"
          label="地址">
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>

  export default {
    data () {
      return {
        tableData: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }],
        currentPage4: 4,
        dialogVisible:false,
        dialogVisibleAD:false,
        fileList: [{name: 'food.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}, {name: 'food2.jpeg', url: 'https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100'}],
        activeName: 'second',
        options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        value: ''
      }
    },

    mounted() {},

    methods: {
      // 投放广告
      putInAD(){
        this.dialogVisibleAD=true
      },
      // 上传图片
      handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      handlePreview(file) {
        console.log(file);
      },
      handleExceed(files, fileList) {
        this.$message.warning(`当前限制选择 3 个文件，本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
      },
      beforeRemove(file, fileList) {
        return this.$confirm(`确定移除 ${ file.name }？`);
      },
      // 分页
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
      },
      // 
      handleClick(tab, event) {
        console.log(tab, event);
      }
    },

  }

</script>
<style scoped>
.my-img{
  width: 50px;
}
.check-out{
  background: #999999;
  color: #fff;
  border-radius: 50px;
  padding: 5px 20px;
  display: inline-block;
}
.dialog-content h1{
  font-size: 20px;
  font-weight: bold;
  padding: 10px 0;
}
</style>