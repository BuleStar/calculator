<template>
  <div class="calculator-container">
    <el-row>
      <el-col :span="12" offset="6">
        <el-card class="box-card">
          <template v-slot:header>
            <div class="clearfix">
              <span class="card-title">期货计算器</span>
            </div>
          </template>

          <el-form :model="form">
            <!-- 杠杆 -->
            <el-form-item label="杠杆" prop="leverage">
              <el-input-number v-model="form.leverage" :min="1" :max="100" label="杠杆"/>
            </el-form-item>

            <!-- 开仓价格 -->
            <el-form-item label="开仓价格" prop="entryPrice">
              <el-input v-model="form.entryPrice" type="number" placeholder="请输入开仓价格"/>
            </el-form-item>

            <!-- 仓位 -->
            <el-form-item label="仓位" prop="position">
              <el-input-number v-model="form.position" :min="1" label="仓位"/>
            </el-form-item>

            <!-- 当前价格 -->
            <el-form-item label="当前价格" prop="currentPrice">
              <el-input v-model="form.currentPrice" type="number" placeholder="请输入当前价格"/>
            </el-form-item>

            <!-- 盈亏计算 -->
            <el-form-item label="盈亏" prop="profit">
              <el-input :value="profit" readonly/>
            </el-form-item>

            <!-- 目标价格 -->
            <el-form-item label="目标价格" prop="targetPrice">
              <el-input :value="targetPrice" readonly/>
            </el-form-item>

            <!-- 清算价格 -->
            <el-form-item label="清算价格" prop="liquidationPrice">
              <el-input :value="liquidationPrice" readonly/>
            </el-form-item>

            <el-button type="primary" @click="calculate">计算</el-button>
          </el-form>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script setup>
import {ref, computed} from 'vue';
import {ElRow, ElCol, ElCard, ElForm, ElFormItem, ElInput, ElInputNumber, ElButton} from 'element-plus';

// 表单数据
const form = ref({
  leverage: 10,
  entryPrice: 50000,
  position: 1,
  currentPrice: 52000,
});

// 计算盈亏
const profit = computed(() => {
  return (form.value.currentPrice - form.value.entryPrice) * form.value.position * form.value.leverage;
});

// 计算目标价格
const targetPrice = computed(() => {
  return form.value.entryPrice + (form.value.entryPrice * 0.02); // 假设目标价格为开仓价格的2%上涨
});

// 计算清算价格
const liquidationPrice = computed(() => {
  const liquidation = form.value.entryPrice * (1 - 0.01 / form.value.leverage); // 简单清算公式
  return liquidation.toFixed(2);
});

// 计算按钮的点击事件
const calculate = () => {
  // 重新计算所有的值
};
</script>

<style scoped>
.calculator-container {
  margin-top: 20px;
}

.card-title {
  font-size: 18px;
  font-weight: 600;
}
</style>
