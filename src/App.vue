<script setup>
import WelcomeItem from './components/WelcomeItem.vue'
import { ref, onMounted, computed, toRaw, watch } from 'vue'

const data1 = ref({})
const data2 = ref([])
const data3 = ref({})
const img = ref([])
const itemData = ref()
const merageData = ref([])
const endTime = ref(0)
const startTime = ref(0)

console.log(merageData)

onMounted(async () => {
	itemData.value = await fetch('./data/header-item.json').then((res) => res.json())
	data1.value = await fetch('./data/data1.json').then((res) => res.json())
	data2.value = await fetch('./data/data2.json').then((res) => res.json())
	data3.value = await fetch('./data/data3.json').then((res) => res.json())
})

// data1 and data2
const mergeData_2 = computed(() => {
	const merageData1 = []
	for (let i = 0; i < data1.value.length; i++) {
		for (let j = 0; j < data2.value.length; j++) {
			if (data1.value[i].key === data2.value[j].key) {
				merageData1.push({
					data: toRaw(data1.value[i]),
					cell8: toRaw(data2.value[j])
				})
			}
		}
	}
	return merageData1
})

// all data
const merageData_3 = computed(() => {

	mergeData_2.value.forEach((element, index) => {
		Object.values(toRaw(data3.value)).forEach((item) => {
			if (element.data.cell4 === item.cell4) {
				merageData.value.push({
					id: index,
					key: element,
					cell8: element.cell8.cell8,
					cell9: item.cell9,
					isStar: false
				})
			}
		})
	})
	return merageData.value
})

const duration = computed(() => {
	endTime.value = performance.now()
	return Math.round(endTime.value - startTime.value) / 10
})

watch(merageData_3, (newVal, oldVal) => {
	console.log(`${JSON.stringify(oldVal)} ${JSON.stringify(newVal)}`);
})

function onOpenImg(index) {
	merageData_3.value[index].isStar = !merageData_3.value[index].isStar
}

function addEvent() {
	event.stopPropagation();
}



</script>

<template>
	<WelcomeItem>
		<template #icon>

		</template>
		<template #details>
		</template>
		<template #heading>
			<div>
				<p>取得資料 至 表格建立完成(含事件綁定)期間，共耗費 {{ duration }} ms</p>
			</div>
			<div class="header">
				<div class="header-item-data" v-for="item in itemData">
					{{ item.key }}
				</div>
			</div>

			<div class="item-data1" tabindex="-1" v-for="(link, index) in merageData_3" :key="link.id" @click="addEvent()">
				<div class="item-data1-text">
					<div class="item-icon">
						<div class="item-icon-img" @click="onOpenImg(index)">
							<img v-if="link.isStar" class="item-star-icon" src="../src/assets/star_icon.png" alt="" />
							<img v-else class="item-star-icon" src="../src/assets/star.png" alt="" />
						</div>
						<p>{{ link.key.data.key }}</p>
					</div>
				</div>
				<div class="item-data1-text">
					<p>{{ link.key.data.cell1 }}</p>
				</div>
				<div class="item-data1-text">
					<p>{{ link.key.data.cell2 }}</p>
				</div>
				<div class="item-data1-text">
					<p>{{ link.key.data.cell3 }}</p>
				</div>
				<div class="item-data1-text">
					<p>{{ link.key.data.cell4 }}</p>
				</div>
				<div class="item-data1-text">
					<p>{{ link.key.data.cell5 }}</p>
				</div>
				<div class="item-data1-text">
					<p>{{ link.key.data.cell6 }}</p>
				</div>
				<div class="item-data1-text">
					<p>{{ link.key.data.cell7 }}</p>
				</div>
				<div class="item-data1-text">
					<p>{{ link.key.cell8.cell8 }}</p>
				</div>
				<div class="item-data1-text">
					<p>{{ link.cell9 }}</p>
				</div>
			</div>



		</template>
	</WelcomeItem>
</template>


<style scoped>
.header {
	width: 100%;
	display: flex;
	background: #98b7da;
}

.header-item-data {
	width: 100%;
	display: flex;
	justify-content: center;
	border: 1px solid;
	margin: -1px 0 0 -1px;
	padding: 10px 0px 10px 0px;
	font-size: 1.2rem;
	font-weight: 800;
}

.item-data1 {
	width: 100%;
	font-size: 1.2rem;
	font-weight: 800;
	display: flex;
	justify-content: space-around;
}

.item-data1:hover {
	background-color: #faf;
}

.item-data1:focus {
	background-color: #d8e69e;
}

.item-data1-text {
	width: 100%;
	text-align: center;
	border: 1px solid;
	margin: -1px 0 0 -1px;
	padding: 10px;
}

.item-star {
	width: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
}

.item-star-icon {
	width: 30px;
	height: 30px;
}

.item-icon {
	display: flex;
	justify-content: center;
}

.item-icon-img {
	display: flex;
	align-items: center;
	justify-content: center;
}

.star-icon-dark {
	background-image: url('../src/assets/star.png');
}

.star-icon-bright {
	background-image: url('../src/assets/star_icon.png');
}
</style>
