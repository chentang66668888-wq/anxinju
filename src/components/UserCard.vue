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
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  backdrop-filter: blur(4px);
}

.dialog-content {
  background: linear-gradient(145deg, #0f1528 0%, #0a0f1e 100%);
  border: 1px solid rgba(72, 120, 255, 0.3);
  border-radius: 20px;
  padding: 24px;
  width: 90%;
  max-width: 450px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

.dialog-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding-bottom: 16px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.dialog-header h3 {
  color: #dfe8ff;
  font-size: 1.2rem;
  margin: 0;
}

.close-btn {
  background: none;
  border: none;
  color: #9fb4ff;
  font-size: 1.8rem;
  cursor: pointer;
  padding: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: 0.2s;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.dialog-body {
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  color: #9fb4ff;
  font-size: 0.9rem;
  margin-bottom: 8px;
}

.form-input,
.form-textarea,
.form-select {
  width: 100%;
  padding: 10px 12px;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(72, 120, 255, 0.3);
  border-radius: 8px;
  color: #eef2ff;
  font-size: 0.9rem;
  outline: none;
  transition: 0.2s;
}

.form-input:focus,
.form-textarea:focus,
.form-select:focus {
  border-color: #3b6eff;
  background: rgba(255, 255, 255, 0.08);
}

.form-textarea {
  resize: vertical;
  font-family: inherit;
}

.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.checkbox-group input[type="checkbox"] {
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.dialog-footer {
  display: flex;
  gap: 12px;
  justify-content: flex-end;
  padding-top: 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.btn-cancel,
.btn-confirm,
.btn-export {
  padding: 10px 20px;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  transition: 0.2s;
  border: none;
}

.btn-cancel {
  background: rgba(255, 255, 255, 0.05);
  color: #9fb4ff;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.btn-cancel:hover {
  background: rgba(255, 255, 255, 0.1);
}

.btn-confirm {
  background: linear-gradient(135deg, #3b6eff, #5b8eff);
  color: white;
}

.btn-confirm:hover {
  background: linear-gradient(135deg, #4b7eff, #6b9eff);
}

.btn-export {
  background: rgba(78, 205, 196, 0.2);
  color: #4ecdc4;
  border: 1px solid rgba(78, 205, 196, 0.4);
}

.btn-export:hover {
  background: rgba(78, 205, 196, 0.3);
}
</style>