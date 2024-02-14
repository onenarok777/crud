<template>
	<div class="caontainer">
		<div style="margin: 4rem 2rem">
			<div style="display: flex; justify-content: end; margin: 2rem 0">
				<UButton @click="onOpenDialog">เพิ่มข้อมูล</UButton>
			</div>
			<UTable :columns="column" :rows="listData" />
		</div>
		<UModal v-model="openDialog">
			<div class="p-4">
				<UForm
					:validate="validate"
					:state="state"
					class="space-y-4"
					prevent-close
					@submit="onSubmit"
				>
					<UFormGroup label="Name" name="name">
						<UInput v-model="state.name" />
					</UFormGroup>

					<UFormGroup label="Amount" name="amount">
						<UInput v-model="state.amount" />
					</UFormGroup>

					<UFormGroup label="Date" name="date">
						<UInput v-model="state.date" />
					</UFormGroup>

					<UFormGroup label="Status" name="status">
						<UInput v-model="state.status" />
					</UFormGroup>

					<UButton class="flex justify-end ml-auto" type="submit">
						Submit
					</UButton>
				</UForm>
			</div>
		</UModal>
	</div>
</template>
<script setup>
const column = ref([
	{
		key: "name",
		label: "ชื่อ",
	},
	{
		key: "amount",
		label: "จำนวนเงิน",
	},
	{
		key: "date",
		label: "วันที่ครบกำหนด",
	},
	{
		key: "status",
		label: "สถานะ",
	},
]);
const listData = ref([]);
const openDialog = ref(false);

const initState = {
	name: undefined,
	amount: undefined,
	date: undefined,
	status: "ยังไม่จ่าย",
};

const state = reactive({ ...initState });

const onOpenDialog = () => {
	resetForm();
	openDialog.value = true;
};

const validate = (field) => {
	const errors = [];
	if (!field.name) errors.push({ path: "name", message: "Required" });
	if (!field.amount) errors.push({ path: "amount", message: "Required" });
	if (!field.date) errors.push({ path: "date", message: "Required" });
	if (!field.status) errors.push({ path: "status", message: "Required" });
	return errors;
};

const onSubmit = async (e) => {
	await listData.value.push(useCloneDeep(state));
	openDialog.value = false;
};

function resetForm() {
	Object.assign(state, initState);
}
</script>
<style>
.caontainer {
	max-width: 1280px;
	margin: 0 auto;
	height: 100vb;
}
</style>
