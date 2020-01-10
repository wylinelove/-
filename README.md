# -
学习
	//渲染
				var addid = '';
				var inst1 = tree.render({
					elem: '#test1' //绑定元素
						,
					edit: ['update', 'del', 'pri'] //操作节点的图标
						,
					data: res,
					operate: function(obj) {
						var type = obj.type; //得到操作类型：add、edit、del
						var data = obj.data; //得到当前节点的数据
						var elem = obj.elem; //得到当前节点元   
						// console.log('父节点',elem.parentElement)
						console.log(obj.data.id); //得到当前点击的节点数据
						// console.log(obj.elem.id); //得到当前节点元素
						var id = obj.data.id; //得到节点索引  
						if (type === 'add') { //增加节点
							addid = id
						} else if (type == 'pri') {
							console.log("修改操作", data)
							layer.alert("因数据量过大,此处打印成功后会刷新页面.请勿当错误信息处理", {
								time: 2000,
								icon: 6
							}, function() {
								location.reload();
							});
							$.ajax({
								type: 'get',
								url: getPath() + '/Tsc/tscs',
								data: {
									id: id
								},
								dataType: "json",
								success: function(data) { 
								},
							});
							//window.history.go(0)
						} else if (type === 'update') { //修改节点
							// console.log(elem.find('.layui-tree-txt').html()); //得到修改后的内容
							console.log('addid', addid)
							//增加
							if (data.id == '' || data.id == null) {
								$.ajax({
									type: 'get',
									url: getPath() + '/house/reg',
									data: {
										title: elem.find('.layui-tree-txt').html(),
										field: addid
									},
									dataType: "json",
									success: function(data) {
										console.log('data', data)
										if (data.code == 200) {
											layer.msg("增加成功", {
												icon: 6
											}, function() {
												var index = parent.layer.getFrameIndex(window.name);
												parent.layer.close(index);
												addid = ''
												location.reload();
											});
										} else {
											layer.msg(data.msg)
										}
									},
									error: function(data) {
										console.log(data)
									},
								});
							} else { //修改
								console.log("修改的ID：" + id)
								console.log("修改的值：" + elem.find('.layui-tree-txt').html())
								$.ajax({
									type: 'get',
									url: getPath() + '/house/update',
									data: {
										title: elem.find('.layui-tree-txt').html(),
										id: data.id
									},
									dataType: "json",
									success: function(data) {
										console.log('data', data)
										if (data.code == 200) {
											layer.msg("修改成功", {
												icon: 6
											}, function() {
												var index = parent.layer.getFrameIndex(window.name);
												parent.layer.close(index);
												location.reload();
											});
										} else {
											layer.msg(data.msg)
										}
									},
									error: function(data) {
										console.log(data)
									},
								});
							}
						} else if (type === 'del') { //删除节点						
							console.log("删除的ID：" + id)
							$.ajax({
								type: 'get',
								url: getPath() + '/house/delete',
								data: {
									id: data.id
									// id 分类编号
									// title 分类名称
									// field 上级分类编号
									// rank 级别
								},
								dataType: "json",
								success: function(data) {
									console.log('data', data)
									if (data.code == 200) {
										layer.msg("删除成功", {
											icon: 6
										}, function() {
											var index = parent.layer.getFrameIndex(window.name);
											parent.layer.close(index);
										});
									} else {
										layer.msg(data.msg)
									}
								},
								error: function(data) {
									console.log(data)
								},
							});
						};
					}
