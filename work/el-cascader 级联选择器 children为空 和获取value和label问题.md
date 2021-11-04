# el-cascader 级联选择器 children为空 和获取value和label问题

1. 出现问题bug: el-cascader控件 最后一级出现空白 暂无数据

   ![img](C:\Users\guoge\Desktop\MyProject\note-gg\work\images\20210322142831330.png)


   在后端处理完树形数据之后最后一个children数组为空数组，这样就会产生bug

   解决方法:  （和后端处理数据一样就行递归判断数组是否为空）

      getClassificationTree() {
    		getClassificationTree({
    			'name': 'name'
    		}).then(response => {
    			this.options = this.getTreeData(response.data);
    		})
    	},
    	getTreeData(data) {
    		for (var i = 0; i < data.length; i++) {
    			if (data[i].children.length < 1) {
    				// children若为空数组，则将children设为undefined
    				data[i].children = undefined;
    			} else {
    				// children若不为空数组，则继续 递归调用 本方法
    				this.getTreeData(data[i].children);
    			}
    		}
    		return data;
    	},
2. 获取value和label问题
  先给级联选择器ref=""重命一个新组件名为xxxxLabel

  ```
   <el-cascader ref="xxxxLabel"
     		:options="options"
     		:props="{ value: 'Id',label: 'Name',children: 'children'}"
     		@change="handleChange2">
     </el-cascader>
  
  handleChange2: function(e) {
  	const checkedNodes = this.$refs['xxxxLabel'].getCheckedNodes()
  	console.log(checkedNodes) // 获取当前点击的节点
      console.log(checkedNodes[0].data.label) // 获取当前点击的节点的label
      console.log(checkedNodes[0].pathLabels) // 获取由 label 组成的数组
  }
  ```

   
