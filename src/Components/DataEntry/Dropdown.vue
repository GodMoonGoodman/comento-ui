<template>
	<div :id="`c-dropdown-${uid}`" class="c-application c-dropdown" :class="[computedDirection, computedFull]">
		<!-- 라벨 영역 -->
		<div v-if="label" class="c-dropdown--label">
			<Label type="dataEntry">{{ label }}</Label>
		</div>

		<div class="c-dropdown--item-group" :style="computedMaxWidth">
			<!-- 아이템 영역 -->
			<div v-if="$slots['item']">
				<slot name="item" />
			</div>

			<!-- 리스트 영역 -->
			<div v-if="$slots['list']" class="c-dropdown--list-wrapper" :style="computedMaxHeight">
				<slot name="list" />
			</div>
		</div>
	</div>
</template>

<script>
import Label from '@/src/Components/DataDisplay/Label';
import uniqueId from '@/utils/unique-id';

export default {
	name: 'Dropdown',
	props: {
		maxWidth: {
			type: String,
			default: '200px',
			validator(value) {
				return typeof value === 'string';
			},
		},
		maxHeight: {
			type: String,
			default: 'auto',
			validator(value) {
				return typeof value === 'string';
			},
		},
		label: {
			type: String,
			default: '',
			validator(value) {
				return typeof value === 'string';
			},
		},
		vertical: {
			type: Boolean,
			default: true,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		full: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
	},
	data() {
		return {
			uid: uniqueId(),
		};
	},
	computed: {
		computedMaxWidth() {
			return { maxWidth: !this.full && this.maxWidth };
		},
		computedMaxHeight() {
			return { maxHeight: this.maxHeight !== 'auto' && this.maxHeight };
		},
		computedDirection() {
			return !this.vertical && 'horizontal';
		},
		computedFull() {
			return { full: this.full };
		},
	},
	components: {
		Label,
	},
};
</script>

<style lang="scss" scoped>
.c-dropdown {
	position: relative;
	@include inline-flex();
	@include flex-direction(column);

	&.full {
		@include flexbox();
		width: 100%;
	}

	&--item-group {
		position: relative;
	}

	&--list-wrapper {
		display: block;
		position: absolute;
		top: 100%;
		width: 100%;
		padding: 8px 12px;
		background: $white;
		@include shadow4();
		overflow-y: auto;
		box-sizing: border-box;
		z-index: 8;
	}

	&--label {
		margin-bottom: 4px;
	}
}

.horizontal {
	&.c-dropdown {
		@include flex-direction(row);
		@include align-items(center);
	}
	.c-dropdown--label {
		margin-right: 8px;
		margin-bottom: 0;
		flex: none;
	}
}
</style>
