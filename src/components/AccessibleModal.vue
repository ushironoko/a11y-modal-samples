<template>
	<teleport v-if="visible" to="#app">
		<div
			ref="focusTrapTarget"
			role="dialog"
			aria-modal
			aria-labelledby="modal-title"
			aria-describedby="modal-body"
			v-bind="$attrs"
			class="fixed top-0 left-0 flex items-start justify-center w-screen h-screen overflow-y-scroll backdrop-filter backdrop-blur-md"
			@click.stop="clickBackDrop"
			@keydown.esc="clickBackDrop"
		>
			<section
				class="box-border min-w-60 max-w-screen-sm p-3 mx-2 my-16 bg-white shadow-xl"
				@click.stop
			>
				<h2 id="modal-title" ref="titleRef" class="w-full text-center">
					<slot name="title"></slot>
				</h2>
				<div id="modal-body" class="py-3">
					<slot name="body" />
				</div>
				<footer>
					<slot name="footer"></slot>
				</footer>
			</section>
		</div>
	</teleport>
</template>

<script lang="ts">
import {
	defineComponent,
	nextTick,
	onUnmounted,
	onMounted,
	ref,
	watchEffect,
} from "vue";
import { useFocusTrap } from "@vueuse/integrations";

export default defineComponent({
	name: "AccessibleModal",
	props: {
		visible: {
			type: Boolean,
			required: true,
		},
	},
	emits: ["backdropClicked"],
	setup(props, { emit }) {
		const focusTrapTarget = ref(null);
		useFocusTrap(focusTrapTarget, {
			immediate: true, // immediate: true を渡してコンポーネントマウント時にフォーカストラップを開始する
		});

		const clickBackDrop = () => {
			emit("backdropClicked");
		};

		watchEffect((onInvalidate) => {
			if (!props.visible) return;

			const overflow = document.documentElement.style.overflow;
			document.documentElement.style.overflow = "hidden";

			onInvalidate(() => {
				document.documentElement.style.overflow = overflow;
			});
		});

		const titleRef = ref<HTMLElement | null>(null);

		onMounted(() => {
			nextTick(() => {
				titleRef.value?.focus();
				document.getElementById("app")?.setAttribute("aria-hidden", "true"); // aria-modal非対応UAのためにバックドロップの後ろのコンテンツをaria-hiddenにする
			});
		});

		onUnmounted(() => {
			document.getElementById("app")?.removeAttribute("aria-hidden"); // モーダル解除時に消すのを忘れない
		});

		return {
			clickBackDrop,
			titleRef,
			focusTrapTarget,
		};
	},
});
</script>
