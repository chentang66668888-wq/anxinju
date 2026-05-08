    draw() {
      const ctx = this.ctx
      const w = this.width
      const h = this.height
      
      // 清除整个画布（使用CSS像素坐标）
      ctx.save()
      ctx.setTransform(1, 0, 0, 1, 0, 0) // 重置变换矩阵
      ctx.clearRect(0, 0, w * this.dpr, h * this.dpr)
      ctx.restore()
      
      // 绘制网格背景
      if (this.gridCanvas) {
        ctx.save()
        ctx.setTransform(1, 0, 0, 1, 0, 0)
        ctx.drawImage(this.gridCanvas, 0, 0, w * this.dpr, h * this.dpr, 0, 0, w * this.dpr, h * this.dpr)
        ctx.restore()
      }
      
      ctx.save()
      // 应用设备像素比变换
      ctx.setTransform(this.dpr, 0, 0, this.dpr, 0, 0)
      
      // 垂直居中
      ctx.translate(0, h / 2)
      const scaleY = h * 0.14
      const len = this.pattern.length
      const scaleX = w > 0 ? len / w : 1
      
      const waveColor = this.getWaveColor()
      
      // 绘制主波形（带发光效果）
      ctx.beginPath()
      for (let x = 0; x < w; x++) {
        const idx = Math.floor((this.offset + x * scaleX) % len)
        const v = this.pattern[idx]
        const y = -v * scaleY
        if (x === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      }
      
      ctx.shadowColor = waveColor.glow
      ctx.shadowBlur = 4
      ctx.lineWidth = 2.5
      ctx.strokeStyle = waveColor.main
      ctx.lineJoin = 'round'
      ctx.lineCap = 'round'
      ctx.stroke()
      
      // 绘制高亮波形（无发光）
      ctx.shadowBlur = 0
      ctx.beginPath()
      for (let x = 0; x < w; x++) {
        const idx = Math.floor((this.offset + x * scaleX) % len)
        const v = this.pattern[idx]
        const y = -v * scaleY
        if (x === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      }
      ctx.lineWidth = 1.2
      ctx.strokeStyle = waveColor.bright
      ctx.stroke()
      
      ctx.restore()
      
      // 更新显示的数值
      if (this.ecgSettings.showData) {
        const idx = Math.floor(this.offset % len)
        this.currentVoltage = (this.pattern[idx] * 1.2).toFixed(2)
      }
    },