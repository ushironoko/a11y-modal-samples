<template>
	<div id="contents" class="m-4">
		<p class="mb-2 text-2xl">アクセシブルなモーダルのデモ</p>
		<div>
			<div class="w-80">
				<InputText label="input" />
			</div>
			<div class="mb-2">
				<button
					class="px-2 rounded-md h-11 text-white bg-gray-800 box-border focus-visible:outline-none focus-visible:ring-4 focus-visible:ring-blue-300 focus-visible:transition focus-visible:duration-100"
					@click="handleNoAccessibleModalOpen"
				>
					open
				</button>
			</div>
		</div>
		<div>
			<div class="mb-2">
				<button
					ref="accessibleModalOpenButtonRef"
					class="px-2 rounded-md h-11 text-white bg-gray-800 box-border focus:outline-none focus-visible:ring-4 focus-visible:ring-blue-300 focus-visible:transition focus-visible:duration-100"
					@click="handleAccessibleModalOpen"
				>
					open
				</button>
			</div>
		</div>
		<div class="w-80">
			<InputText label="input" />
		</div>
	</div>

	<NoAccessibleModal
		:visible="noAccessibleModalVisible"
		@backdrop-clicked="handleNoAccessibleModalClose"
	>
		<template #title>
			<p class="font-bold">title</p>
		</template>
		<template #body>
			<p>body text</p>
		</template>
		<template #footer>
			<div class="flex justify-end">
				<button
					class="px-2 rounded-md h-11 text-white bg-gray-800 box-border focus:outline-none focus-visible:ring-4 focus-visible:ring-blue-300 focus-visible:transition focus-visible:duration-100"
					@click="handleNoAccessibleModalClose"
				>
					close
				</button>
			</div>
		</template>
	</NoAccessibleModal>
	<AccessibleModal
		v-if="accessibleModalVisible"
		:visible="accessibleModalVisible"
		@backdrop-clicked="handleAccessibleModalClose"
	>
		<template #title>
			<p
				tabindex="0"
				class="font-bold focus:outline-none focus-visible:ring-4 focus-visible:ring-blue-300 focus-visible:transition focus-visible:duration-100"
			>
				title
			</p>
		</template>
		<template #body>
			<p
				tabindex="0"
				class="focus:outline-none focus-visible:ring-4 focus-visible:ring-blue-300 focus-visible:transition focus-visible:duration-100"
			>
				body text
			</p>
		</template>
		<template #footer>
			<div class="flex justify-end">
				<button
					class="px-2 rounded-md h-11 text-white bg-gray-800 box-border focus:outline-none focus-visible:ring-4 focus-visible:ring-blue-300 focus-visible:transition focus-visible:duration-100"
					@click="handleAccessibleModalClose"
				>
					close
				</button>
			</div>
		</template>
	</AccessibleModal>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import AccessibleModal from "./components/AccessibleModal.vue";
import NoAccessibleModal from "./components/NoAccessibleModal.vue";
import InputText from "./components/InputText.vue";

export default defineComponent({
	name: "App",
	components: {
		AccessibleModal,
		NoAccessibleModal,
		InputText,
	},
	setup() {
		const noAccessibleModalVisible = ref(false);
		const handleNoAccessibleModalOpen = () => {
			noAccessibleModalVisible.value = true;
		};

		const handleNoAccessibleModalClose = () => {
			noAccessibleModalVisible.value = false;
		};

		const accessibleModalOpenButtonRef = ref<HTMLElement | null>(null);
		const accessibleModalVisible = ref(false);

		const handleAccessibleModalOpen = () => {
			accessibleModalVisible.value = true;
		};

		const handleAccessibleModalClose = () => {
			accessibleModalVisible.value = false;
			accessibleModalOpenButtonRef.value?.focus(); // モーダルを閉じた時にフォーカスを戻す
		};

		return {
			handleNoAccessibleModalOpen,
			noAccessibleModalVisible,
			accessibleModalVisible,
			accessibleModalOpenButtonRef,
			handleNoAccessibleModalClose,
			handleAccessibleModalOpen,
			handleAccessibleModalClose,
		};
	},
});
</script>
