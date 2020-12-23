<template>
	<div class="container">
		<my-table :columns="columnData" :tableData="tableData" :rowKey="'id'" :height="400" :isLazy="true" :load="load"></my-table>
	</div>
</template>

<script>
	import myTable from './Table/myTable.vue'
	export default {
		components: {
			myTable
		},
		data() {
			return {
				columnData: [{
						prop: 'name',
						label: '姓名',
						width: '180',
						isFixed: true,
						isSort: true
					},
					{
						prop: 'date',
						label: '日期',
						width: '180',
						isFixed: false,
						isSort: true
					},
					{
						prop: 'address',
						label: '地址',
						isFixed: false,
						isSort: true
					},
					{
						prop: 'age',
						label: '年龄',
						isFixed: false,
						isSort: true
					}
				],
				tableData: [{
					id: 1,
					date: '2016-05-02',
					name: '王小虎',
					address: '上海市普陀区金沙江路 1518 弄',
					age: '12'
				}, {
					id: 2,
					date: '2016-05-04',
					name: '王小虎',
					address: '上海市普陀区金沙江路 1517 弄',
					age: '12'
				}, {
					id: 3,
					date: '2016-05-01',
					name: '王小虎',
					address: '上海市普陀区金沙江路 1519 弄',
					age: '12',
					hasChildren: true
				}, {
					id: 4,
					date: '2016-05-03',
					name: '王小虎',
					address: '上海市普陀区金沙江路 1516 弄',
					age: '12'
				}]
			}
		},
		methods: {
			load(tree, treeNode, resolve) {
			       setTimeout(() => {
			         resolve([
			           {
			             id: 31,
			             date: '2016-05-01',
			             name: '王小虎',
			             address: '上海市普陀区金沙江路 1519 弄'
			           }, {
			             id: 32,
			             date: '2016-05-01',
			             name: '王小虎',
			             address: '上海市普陀区金沙江路 1519 弄'
			           }
			         ])
			       }, 1000)
			     },
		}
	}
</script>

<style>
</style>
