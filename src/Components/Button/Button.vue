<template>
	<button
		class="c-application c-button"
		:class="[
			computedSize,
			computedColor,
			computedFull,
			computedType,
			computedLoading,
			computedFixed,
			computedAbsolute,
			computedShadow,
		]"
		v-bind="$attrs"
		:disabled="disabled"
		v-on="$listeners"
	>
		<template v-if="loading">
			<div class="c-button--loading">
				<Loader :size="computedLoaderSize" :reversed="isFillType" :color="color" />
			</div>
		</template>
		<div class="c-button--icon" :class="setIconSpacing('left')">
			<slot name="left-icon" />
		</div>
		<slot />
		<div class="c-button--icon" :class="setIconSpacing('right')">
			<slot name="right-icon" />
		</div>
	</button>
</template>

<script>
import Loader from '@/src/Components/Other/Loader';

export const buttonSizes = ['small', 'medium', 'large', 'xlarge'];
export const buttonColors = ['primary', 'success', 'secondary', 'error'];
export const buttonTypes = ['fill', 'outlined', 'text', 'icon'];

export default {
	name: 'Button',
	inheritAttrs: false,
	props: {
		size: {
			type: String,
			default: 'medium',
			validator(value) {
				return buttonSizes.indexOf(value) !== -1;
			},
		},
		color: {
			type: String,
			default: 'primary',
			validator(value) {
				const isValid = buttonColors.indexOf(value) !== -1;
				if (!isValid) {
					console.error(`${value} is not a valid value of Button color`);
				}
				return isValid;
			},
		},
		type: {
			type: String,
			default: 'fill',
			validator(value) {
				return buttonTypes.indexOf(value) !== -1;
			},
		},
		full: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		disabled: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		loading: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		fixed: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		absolute: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
		shadow: {
			type: Boolean,
			default: false,
			validator(value) {
				return typeof value === 'boolean';
			},
		},
	},
	computed: {
		computedSize() {
			return this.size;
		},
		computedColor() {
			return this.color;
		},
		computedType() {
			return this.type;
		},
		computedFull() {
			return { full: this.full };
		},
		computedLoading() {
			return { loading: this.loading };
		},
		computedShadow() {
			return { shadow: this.shadow };
		},
		computedFixed() {
			return { 'p-fixed': this.fixed };
		},
		computedAbsolute() {
			return { 'p-absolute': this.absolute };
		},
		computedLoaderSize() {
			return this.size === 'xlarge' ? 'large' : this.size;
		},
		computedIconMargin() {
			const isXLarge = this.size.indexOf('xlarge') !== -1;
			return isXLarge ? 4 : 2;
		},
		isFillType() {
			return this.type === 'fill';
		},
	},
	methods: {
		setIconSpacing(position) {
			const oppositePosition = position === 'left' ? 'r' : 'l';
			return this.$slots[`${position}-icon`] && `m${oppositePosition}-${this.computedIconMargin}`;
		},
	},
	components: { Loader },
};
</script>

<style lang="scss" scoped>
$primary-background-color: $primary;
$primary-hover-background-color: $green800;
$primary-disabled-background-color: $green100;
$primary-text-color: $green800;

$success-background-color: $success;
$success-hover-background-color: $blue800;
$success-disabled-background-color: $blue100;
$success-text-color: $blue600;

$secondary-background-color: $gray600;
$secondary-hover-background-color: $gray800;
$secondary-disabled-background-color: $gray100;
$secondary-text-color: $gray500;

$error-background-color: $red600;
$error-hover-background-color: $red800;
$error-disabled-background-color: $red100;
$error-text-color: $red600;
.c-button {
	color: $white;
	background-color: $primary;
	border: none;
	@include border-radius(4px);
	@include flexbox();
	@include align-items(center);
	@include justify-content(center);
	position: relative;
	cursor: pointer;

	&:disabled {
		cursor: not-allowed !important;
		&:active {
			pointer-events: none;
		}
		.c-button--icon::v-deep .c-icon {
			@include disabled();
		}
	}
	&--loading {
		position: absolute;
		width: 100%;
		height: 100%;
		@include border-radius(4px);
		@include flexbox();
		@include align-items(center);
		@include justify-content(center);
	}
	&--icon {
		@include flexbox();
	}

	&.small {
		height: 24px;
		padding: 0 8px;
		min-width: 36px;
		@include caption2;
		.loading {
			padding: 0 9.5px;
			line-height: 90%;
		}
	}
	&.medium {
		height: 36px;
		padding: 0 12px;
		min-width: 60px;
		@include body2;
		font-weight: bold;
		.loading {
			padding: 0 28.5px;
			line-height: 90%;
		}
	}
	&.large {
		height: 48px;
		padding: 0 14px;
		min-width: 76px;
		@include body1;
		font-weight: bold;
		.loading {
			padding: 0 14.5px;
			line-height: 90%;
		}
	}
	&.xlarge {
		height: 60px;
		padding: 0 26px;
		min-width: 130px;
		@include headline6;
		font-weight: bold;
		.loading {
			padding: 0 9.5px;
			line-height: 90%;
		}
	}
	&.full {
		width: 100%;
	}
	&.text {
		@include f-normal();
		background: transparent;
		border: none;
		color: $gray500;

		.c-button--loading {
			background-color: $white;
		}
	}
}

.primary {
	background-color: $primary-background-color;
	&:hover,
	&:focus,
	&:active {
		background-color: $primary-hover-background-color;
	}
	&:disabled {
		background-color: $primary-disabled-background-color;
		.c-button--icon::v-deep .c-icon {
			fill: $white !important;
		}
		.c-button--loading {
			background-color: $primary-disabled-background-color;
		}
	}
	.c-button--loading {
		background-color: $primary-background-color;
	}

	&.text {
		color: $primary-text-color;
		&:hover,
		&:focus,
		&:active {
			background-color: $primary-disabled-background-color;
		}
		&:disabled {
			.c-button--icon::v-deep .c-icon {
				fill: $primary-disabled-background-color !important;
			}
			color: $primary-disabled-background-color;
			background: none;
		}
	}
	&.outlined {
		background-color: $white;
		border: 1px solid $primary-text-color;
		color: $primary-text-color;
		&:hover,
		&:focus,
		&:active {
			background-color: $primary-disabled-background-color;
		}
		&:disabled {
			color: $primary-disabled-background-color;
			border: 1px solid $primary-disabled-background-color;
			background-color: $white;
			.c-button--icon::v-deep .c-icon {
				fill: $primary-disabled-background-color !important;
			}
		}
		.c-button--loading {
			background-color: $white;
		}
	}
}

.success {
	background-color: $success-background-color;
	&:hover,
	&:focus,
	&:active {
		background-color: $success-hover-background-color;
	}
	&:disabled {
		background-color: $success-disabled-background-color;
		.c-button--icon::v-deep .c-icon {
			fill: $white !important;
		}
		.c-button--loading {
			background-color: $success-disabled-background-color;
		}
	}
	.c-button--loading {
		background-color: $success-background-color;
	}
	&.text {
		color: $success-text-color;
		&:hover,
		&:focus,
		&:active {
			background-color: $success-disabled-background-color;
		}
		&:disabled {
			color: $success-disabled-background-color;
			background: none;
			.c-button--icon::v-deep .c-icon {
				fill: $success-disabled-background-color !important;
			}
		}
	}
	&.outlined {
		background-color: $white;
		border: 1px solid $success-text-color;
		color: $success-text-color;
		&:hover,
		&:focus,
		&:active {
			background-color: $success-disabled-background-color;
		}
		&:disabled {
			color: $success-disabled-background-color;
			border: 1px solid $success-disabled-background-color;
			background-color: $white;
			.c-button--icon::v-deep .c-icon {
				fill: $success-disabled-background-color !important;
			}
		}
		.c-button--loading {
			background-color: $white;
		}
	}
}

.secondary {
	background-color: $secondary-background-color;
	&:hover,
	&:focus,
	&:active {
		background-color: $secondary-hover-background-color;
	}
	&:disabled {
		background-color: $secondary-disabled-background-color;
		.c-button--icon::v-deep .c-icon {
			fill: $white !important;
		}
		.c-button--loading {
			background-color: $secondary-disabled-background-color;
		}
	}
	.c-button--loading {
		background-color: $secondary-background-color;
	}
	&.text {
		color: $secondary-text-color;
		&:hover,
		&:focus,
		&:active {
			background-color: $secondary-disabled-background-color;
		}
		&:disabled {
			color: $secondary-disabled-background-color;
			background: none;
			.c-button--icon::v-deep .c-icon {
				fill: $secondary-disabled-background-color !important;
			}
		}
	}
	&.outlined {
		background-color: $white;
		border: 1px solid $gray400;
		color: $secondary-text-color;
		&:hover,
		&:focus,
		&:active {
			background-color: $secondary-disabled-background-color;
		}
		&:disabled {
			color: $secondary-disabled-background-color;
			border: 1px solid $secondary-disabled-background-color;
			background-color: $white;
			.c-button--icon::v-deep .c-icon {
				fill: $secondary-disabled-background-color !important;
			}
		}
		.c-button--loading {
			background-color: $white;
		}
	}
}
.error {
	background-color: $error-background-color;
	&:hover,
	&:focus,
	&:active {
		background-color: $error-hover-background-color;
	}
	&:disabled {
		background-color: $error-disabled-background-color;
		.c-button--icon::v-deep .c-icon {
			fill: $white !important;
		}
		.c-button--loading {
			background-color: $error-disabled-background-color;
		}
	}
	.c-button--loading {
		background-color: $error-background-color;
	}
	&.text {
		color: $error-text-color;
		&:hover,
		&:focus,
		&:active {
			background-color: $red000;
		}
		&:disabled {
			color: $error-disabled-background-color;
			background: none;
			.c-button--icon::v-deep .c-icon {
				fill: $red100 !important;
			}
		}
	}
	&.outlined {
		background-color: $white;
		border: 1px solid $red600;
		color: $red600;
		&:hover,
		&:focus,
		&:active {
			background-color: $red100;
		}
		&:disabled {
			color: $red100;
			border: 1px solid $red100;
			background-color: $white;
			.c-button--icon::v-deep .c-icon {
				fill: $error-disabled-background-color !important;
			}
		}
		.c-button--loading {
			background-color: $white;
		}
	}
}

.loading {
	@include disabled();
}

.shadow {
	@include shadow4();
}
</style>
