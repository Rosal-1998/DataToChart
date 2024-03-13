<template>
   <el-row :gutter="20">
    <el-col :span="8" :offset="2"> 
      <div class="grid-content ep-bg-purple">
        <h2>DataToChart</h2>
        <el-upload class="upload-demo" drag multiple :on-change="handleChange" :auto-upload="false">
          <el-icon class="el-icon--upload"><upload-filled /></el-icon>
          <div class="el-upload__text">
            拖动文件上传<em>或点击上传</em>
          </div>
        </el-upload>
      </div>
    </el-col>
    <el-col :span="12" :offset="2">
      <div class="grid-content ep-bg-purple-light">
        <v-chart class="chart" :option="option" autoresize />
      </div>
    </el-col>
  </el-row>

</template>

<script setup>
import { UploadFilled } from '@element-plus/icons-vue'
import * as XLSX from 'xlsx'
import { use } from 'echarts/core';
import { CanvasRenderer } from 'echarts/renderers';
import { PieChart, LineChart } from 'echarts/charts';
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent
} from 'echarts/components';
import VChart, { THEME_KEY } from 'vue-echarts';
import { ref, provide } from 'vue';

use([
  CanvasRenderer,
  PieChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent,
  LineChart
]);

provide(THEME_KEY, 'dark');
const X = ref([])
const S = ref([{}])
const option = ref({
  tooltip: {
    trigger: 'item',
  },
  xAxis: {
    data: ['A', 'B', 'C', 'D', 'E']
  },
  yAxis: {},
  series: [
    {
      data: [10, 22, 28, 43, 49],
      type: 'line',
      stack: 'x'
    },
    {
      data: [5, 4, 3, 5, 10],
      type: 'line',
      stack: 'y'
    }
  ]
});
const handleChange = (file) => {
  const reader = new FileReader()
  reader.readAsArrayBuffer(file.raw)
  reader.onload = () => {
    const buffer = reader.result;
    const bytes = new Uint8Array(buffer);
    const length = bytes.byteLength;
    let binary = "";
    for (let i = 0; i < length; i++) {
      binary += String.fromCharCode(bytes[i]);
    }
    const wb = XLSX.read(binary, {
      type: "binary"
    });
    const outdata = XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]); // 默认第一行下为空也能解析出第一四行
    console.log(outdata)
    analysisData(outdata)
  }
}
const analysisData = (outdata) => {
  const keys = Object.keys(outdata[0]); // 获取所有键名
  for (let i = 1; i < keys.length; i++) { // 从第二个键名开始遍历
    const key = keys[i];
    X.value.push(keys[i])
    console.log(`${key}: ${outdata[0][key]}`);
  }
  console.log(X.value)
  console.log(outdata.length)
  for (let k = 0; k < outdata.length; k++) {
    if (!S.value[k]) {
      S.value[k] = { data: [] };
    } else if (!S.value[k].data) {
      S.value[k].data = [];
    }

    for (let j = 0; j < X.value.length; j++) {
      S.value[k].data.push(outdata[k][X.value[j]]);
    }
    S.value[k].stack = k;
    S.value[k].type = 'line';
  }
  option.value.xAxis.data = X
  option.value.series = S


}
</script>

<style scoped>
.chart {
  height: 100vh;
}

.el-row {
  margin-bottom: 20px;
}
.el-row:last-child {
  margin-bottom: 0;
}
.el-col {
  border-radius: 4px;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
</style>
