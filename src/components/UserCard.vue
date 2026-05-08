<template>
  <div class="user-card glass-card">
    <div class="user-avatar">👵</div>
    <div class="user-name">{{ userInfo.name }}</div>
    <div class="user-detail">
      <p>🎂 年龄：{{ userInfo.age }}岁</p>
      <p>🩺 身体状况：{{ userInfo.healthCondition }}</p>
      <p>📌 注意事项：跌倒高风险时段 00:00-06:00</p>
    </div>
    <div class="setting-buttons">
      <div class="btn-outline" @click="showUserDialog = true">👤 更改使用者信息</div>
      <div class="btn-outline" @click="showECGDialog = true">❤️ 心电图设置</div>
      <div class="btn-outline" @click="showRouteDialog = true">🗺️ 路径图设置</div>
    </div>

    <!-- 使用者信息修改弹窗 -->
    <div class="dialog-overlay" v-if="showUserDialog" @click="showUserDialog = false">
      <div class="dialog-content" @click.stop>
        <div class="dialog-header">
          <h3>👤 更改使用者信息</h3>
          <button class="close-btn" @click="showUserDialog = false">×</button>
        </div>
        <div class="dialog-body">
          <div class="form-group">
            <label>姓名：</label>
            <input v-model="userInfo.name" type="text" class="form-input" />
          </div>
          <div class="form-group">
            <label>年龄：</label>
            <input v-model.number="userInfo.age" type="number" class="form-input" min="1" max="150" />
          </div>
          <div class="form-group">
            <label>身体状况：</label>
            <textarea v-model="userInfo.healthCondition" class="form-textarea" rows="3"></textarea>
          </div>
        </div>
        <div class="dialog-footer">
          <button class="btn-cancel" @click="showUserDialog = false">取消</button>
          <button class="btn-confirm" @click="saveUserInfo">保存</button>
        </div>
      </div>
    </div>

    <!-- 心电图设置弹窗 -->
    <div class="dialog-overlay" v-if="showECGDialog" @click="showECGDialog = false">
      <div class="dialog-content" @click.stop>
        <div class="dialog-header">
          <h3>❤️ 心电图设置</h3>
          <button class="close-btn" @click="showECGDialog = false">×</button>
        </div>
        <div class="dialog-body">
          <div class="form-group">
            <label>背景颜色：</label>
            <select v-model="ecgSettings.bgColor" class="form-select">
              <option value="dark">深蓝色</option>
              <option value="black">黑色</option>
              <option value="white">白色</option>
              <option value="green">绿色（夜视模式）</option>
            </select>
          </div>
          <div class="form-group checkbox-group">
            <label>
              <input type="checkbox" v-model="ecgSettings.showData" />
              <span>显示数值数据</span>
            </label>
          </div>
          <div class="form-group">
            <label>波形颜色：</label>
            <select v-model="ecgSettings.waveColor" class="form-select">
              <option value="lightblue">浅蓝色</option>
              <option value="green">绿色</option>
              <option value="red">红色</option>
              <option value="white">白色</option>
            </select>
          </div>
        </div>
        <div class="dialog-footer">
          <button class="btn-cancel" @click="showECGDialog = false">取消</button>
          <button class="btn-confirm" @click="saveECGSettings">保存</button>
          <button class="btn-export" @click="exportECGData">导出心电图数据</button>
        </div>
      </div>
    </div>

    <!-- 路线图设置弹窗 -->
    <div class="dialog-overlay" v-if="showRouteDialog" @click="showRouteDialog = false">
      <div class="dialog-content" @click.stop>
        <div class="dialog-header">
          <h3>🗺️ 路径图设置</h3>
          <button class="close-btn" @click="showRouteDialog = false">×</button>
        </div>
        <div class="dialog-body">
          <div class="form-group checkbox-group">
            <label>
              <input type="checkbox" v-model="routeSettings.showLabels" />
              <span>显示具体地名</span>
            </label>
          </div>
          <div class="form-group checkbox-group">
            <label>
              <input type="checkbox" v-model="routeSettings.showRivers" />
              <span>显示河流</span>
            </label>
          </div>
          <div class="form-group checkbox-group">
            <label>
              <input type="checkbox" v-model="routeSettings.showRoads" />
              <span>显示道路网格</span>
            </label>
          </div>
          <div class="form-group">
            <label>路线颜色：</label>
            <select v-model="routeSettings.routeColor" class="form-select">
              <option value="cyan">青绿色</option>
              <option value="blue">蓝色</option>
              <option value="red">红色</option>
              <option value="yellow">黄色</option>
            </select>
          </div>
        </div>
        <div class="dialog-footer">
          <button class="btn-cancel" @click="showRouteDialog = false">取消</button>
          <button class="btn-confirm" @click="saveRouteSettings">保存</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserCard',
  data() {
    return {
      showUserDialog: false,
      showECGDialog: false,
      showRouteDialog: false,
      userInfo: {
        name: '张德海',
        age: 78,
        healthCondition: '高血压、轻度认知障碍、夜间需关注如厕'
      },
      ecgSettings: {
        bgColor: 'dark',
        showData: false,
        waveColor: 'lightblue'
      },
      routeSettings: {
        showLabels: true,
        showRivers: true,
        showRoads: true,
        routeColor: 'cyan'
      }
    }
  },
  methods: {
    saveUserInfo() {
      this.$emit('update-user-info', this.userInfo)
      this.showUserDialog = false
    },
    saveECGSettings() {
      this.$emit('update-ecg-settings', this.ecgSettings)
      this.showECGDialog = false
    },
    saveRouteSettings() {
      this.$emit('update-route-settings', this.routeSettings)
      this.showRouteDialog = false
    },
    exportECGData() {
      this.$emit('export-ecg-data')
    }
  }
}
</script>

<style scoped>
.setting-buttons { display:flex; flex-direction:column; gap:12px; margin-top:8px; }
.btn-outline { background: rgba(255,255,255,0.05); border:1px solid rgba(72,120,255,0.4); border-radius:40px; padding:10px 0; text-align:center; font-size:0.85rem; font-weight:500; cursor:pointer; transition:0.2s; color:#cddcff; }
.btn-outline:hover { background: rgba(72,120,255,0.25); border-color: #3b6eff }

/* 弹窗样式 */
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  backdrop-filter: blur(8px);
}

.dialog-content {
  background: linear-gradient(145deg, #0f1528 0%, #0a0f1e 100%);
  border: 1px solid rgba(72, 120, 255, 0.4);
  border-radius: 24px;
  padding: 32px;
  width: 90%;
  max-width: 480px;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.6), 0 0 30px rgba(59, 110, 255, 0.1);
  animation: dialogSlideIn 0.3s ease-out;
}

@keyframes dialogSlideIn {
  from {
    opacity: 0;
    transform: scale(0.9) translateY(-20px);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}

.dialog-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
  padding-bottom: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.15);
}

.dialog-header h3 {
  color: #dfe8ff;
  font-size: 1.3rem;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 8px;
}

.close-btn {
  background: rgba(255, 255, 255, 0.05);
  border: none;
  color: #9fb4ff;
  font-size: 1.8rem;
  cursor: pointer;
  padding: 8px;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  transform: rotate(90deg);
}

.dialog-body {
  margin-bottom: 24px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  color: #9fb4ff;
  font-size: 0.95rem;
  margin-bottom: 10px;
  font-weight: 600;
}

.form-input,
.form-textarea,
.form-select {
  width: 100%;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(72, 120, 255, 0.3);
  border-radius: 12px;
  color: #eef2ff;
  font-size: 0.95rem;
  outline: none;
  transition: all 0.3s ease;
  font-family: inherit;
}

.form-input:focus,
.form-textarea:focus,
.form-select:focus {
  border-color: #3b6eff;
  background: rgba(255, 255, 255, 0.08);
  box-shadow: 0 0 0 3px rgba(59, 110, 255, 0.1);
}

.form-textarea {
  resize: vertical;
  min-height: 80px;
}

.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  color: #9fb4ff;
  font-size: 0.9rem;
}

.checkbox-group input[type="checkbox"] {
  width: 20px;
  height: 20px;
  cursor: pointer;
  accent-color: #3b6eff;
}

.dialog-footer {
  display: flex;
  gap: 16px;
  justify-content: flex-end;
  padding-top: 20px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.btn-cancel,
.btn-confirm,
.btn-export {
  padding: 12px 24px;
  border-radius: 12px;
  font-size: 0.95rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  border: none;
  min-width: 80px;
}

.btn-cancel {
  background: rgba(255, 255, 255, 0.05);
  color: #9fb4ff;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.btn-cancel:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-1px);
}

.btn-confirm {
  background: linear-gradient(135deg, #3b6eff, #5b8eff);
  color: white;
  box-shadow: 0 4px 12px rgba(59, 110, 255, 0.3);
}

.btn-confirm:hover {
  background: linear-gradient(135deg, #4b7eff, #6b9eff);
  transform: translateY(-1px);
  box-shadow: 0 6px 16px rgba(59, 110, 255, 0.4);
}

.btn-export {
  background: linear-gradient(135deg, #4ecdc4, #44a08d);
  color: #fff;
  border: 1px solid rgba(78, 205, 196, 0.4);
  box-shadow: 0 4px 12px rgba(78, 205, 196, 0.2);
}

.btn-export:hover {
  background: linear-gradient(135deg, #5ed5cc, #54b0a0);
  transform: translateY(-1px);
  box-shadow: 0 6px 16px rgba(78, 205, 196, 0.3);
}
</style>