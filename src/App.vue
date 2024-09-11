<template>
	<div class="#keypad-enter">test</div>
	<div id="#keypad-enter">test</div>
	<v-app>
		<v-main>
			<v-container>
				<!-- 電話番号表示 -->
				<v-row>
					<v-col v-for="i in 11" :key="i" cols="1">
						<div class=".o-input--num-display">
							{{ phoneNumber[i - 1] }}
						</div>
					</v-col>
				</v-row>
				<!-- 入力ボタン -->
				<v-row>
					<v-col v-for="i in 9" :key="i" cols="4">
						<v-btn color="primary" @click="addPhoneNumber(i)" :class="`#keypad-${i}`"> {{ i }}</v-btn>
					</v-col>
					<v-col cols="4">
						<v-btn color="primary" @click="deletePhoneNumber" :class="'#keypad-delete'">D</v-btn>
					</v-col>
					<v-col cols="4">
						<v-btn color="primary" @click="addPhoneNumber(0)">0</v-btn>
					</v-col>
					<v-col cols="4">
						<v-btn color="primary" @click="sendPhoneNumber" :disabled="enterDisabled"
							:class="'#keypad-enter'">E</v-btn>
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
	// 送信処理
	alert(phoneNumber);
}
</script>

<style scoped>
.o-input--num-display {
	font-size: 20px;
}
</style>