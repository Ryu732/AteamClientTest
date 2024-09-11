<template>
	<v-app>
		<v-main>
			<v-container>
				<!-- 電話番号表示 -->
				<v-row class="numberList">
					<v-col v-for="i in 11" :key="i" cols="1" class="number">
						<div class=".o-input--num-display d-flex align-center justify-center fill-height">
							{{ phoneNumber[i - 1] }}
						</div>
					</v-col>
				</v-row>
				<!-- 入力ボタン -->
				<v-row class="btnList ">
					<v-col v-for="i in 9" :key="i" cols="4">
						<v-btn color="primary" @click="addPhoneNumber(i)" :class="`#keypad-${i}`"
							class="d-flex align-center justify-center fill-height"> {{ i }}</v-btn>
					</v-col>
					<v-col cols="4">
						<v-btn color="primary" @click="deletePhoneNumber" :class="'#keypad-delete'"
							class="d-flex align-center justify-center fill-height">D</v-btn>
					</v-col>
					<v-col cols="4">
						<v-btn color="primary" @click="addPhoneNumber(0)"
							class="d-flex align-center justify-center fill-height">0</v-btn>
					</v-col>
					<v-col cols="4">
						<v-btn color="primary" @click="sendPhoneNumber" :disabled="enterDisabled"
							:class="'#keypad-enter'" class="d-flex align-center justify-center fill-height">E</v-btn>
					</v-col>
				</v-row>
			</v-container>
		</v-main>
	</v-app>
</template>

<script setup>
import { reactive, ref, watch } from 'vue';

// 入力された電話番号
const phoneNumber = reactive([]);

// enterが押せるかどうかのフラグ
const enterDisabled = ref(true);

// 電話番号を追加
function addPhoneNumber(num) {
	if (phoneNumber.length < 11) {
		phoneNumber.push(num);
	}
}

// 電話番号を削除
function deletePhoneNumber() {
	phoneNumber.pop();
}

// 電話番号が送信できるかの判定
watch(phoneNumber, () => {
	// 電話番号が10桁で0から始まる時は送信可能
	if (phoneNumber.length === 10 && phoneNumber[0] === 0) {
		enterDisabled.value = false;
	} else if (phoneNumber.length === 11 && phoneNumber[0] === 0 && phoneNumber[2] === 0) {
		// 電話番号が11桁で0から始まる時は送信可能
		enterDisabled.value = false;
	} else {
		enterDisabled.value = true;
	}
});

function sendPhoneNumber() {
	// 電話番号を一つの文字列に変換
	let sendNumber = '';
	for (const num of phoneNumber) {
		sendNumber += num;
	}
	// 送信処理
	alert(sendNumber);
}
</script>

<style scoped>
.o-input--num-display {
	font-size: 20px;
}

.number {
	background-color: rgb(245, 245, 245);
	height: 4em;
	width: auto;
	margin: auto;
}

.numberList {
	width: 500px;
	height: 75px;
	margin-top: 10px;
	background-color: rgb(209, 208, 208);
}

.btnList {
	width: 500px;
	height: 300px;
	margin-top: 10px;
	background-color: rgb(230, 229, 229);
}
</style>