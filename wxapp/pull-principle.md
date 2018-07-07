# 微信小程序下拉刷新，上拉加载实现原理（H5同意适用）

```javascript
data () {
	return {
		scrollTop: true,
		scrollBottom: false,
		startY: 0,
		distanceY: 0
	}
}

methods: {
	upper () { // scroll-view 的事件函数
		this.scrollTop = true
	},
	bottom () { // scroll-view 的事件函数
		this.scrollBottom = true
	},

	// 内层view 的事件函数
	touchstart (e) {
		this.startY = e.touches[0].clinetY
	},
	touchmove (e) {
		let distanceY = e.touches[0].clinetY - this.startY
		this.distanceY = distanceY
		if (distanceY > 0 && scrollTop) {
			// 此时在执行下拉刷新界面动画
			this.animationRefresh()
		}else if (distanceY < 0 && scrollBottom) {
			// 此时在执行上拉加载界面动画
			this.animationLoadMore()
		}else if (distanceY > 0 && !scrollTop) {
			// 普通的下划
			this.scrollBottom = false
		}else if (distanceY < 0 && !scrollBottom) {
			// 普通的上划
			this.scrollTop = false
		}
	},
	touchend (e) {
		if (this.distanceY > 80 && this.scrollTop) {
			// 执行数据请求更新 
			this.getRefresh(function(data) {
				// 数据请求成功后, 恢复distanceY和startY的默认值
				this.distanceY = 0
				this.startY = 0
				// TODO 处理你的data... ...
			})
		} else if (this.distanceY < 50 && this.scrollBottom) {
			// 执行数据请求加载更多 该处为模拟的
			this.getMore(function(data) {
				// 数据请求成功后, 恢复distanceY和startY的默认值
				this.distanceY = 0
				this.startY = 0
				// TODO 处理你的data... ...
			})
		}else {
			this.distanceY = 0
			this.startY = 0
		}
	},
        
	// 动画执行相关函数
	animationRefresh () {
		// 刷新操作的动画函数
	},
	animationLoadMore () {
		// 上拉加载更多的动画函数
	},
	// 数据请求相关函数
	getRefresh (fn) {
		// 请求刷新数据函数
		setTimeout(function() {
			fn('the data refreshed')
		}, 2000)
	}
	getMore (fn) {
		// 请求加载更多数据函数
		setTimeout(function() {
			fn('the data loadmore')
		}, 2000)
	}
}
```

