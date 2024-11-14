<template>
	<input ref="maskedInput" v-bind="$attrs" @input="handleInput" v-model="maskedValue" :class="class" :id="id" :placeholder="placeholder" :readonly="readonly"/>
</template>

<script>
	import {maskDefinitions} from "./maskDefinitions";

	export default {
		inheritAttrs: false,
		props: {
			mask: {
				type: String,
				required: true
			},
			class: {
				type: String,
				required: false
			},
			id: {
				type: String,
				required: false,
				default: ''
			},
			placeholder: {
				type: String,
				required: false
			},
			readonly: {
				type: Boolean,
				required: false,
				default: false
			},
			modelValue: {
				type: String,
				default: '',
			},
			textmode: {
				type: String,
				default: ''
			}
		},
		emits: ["update:modelValue"],
		data() {
			return {
				maskedValue: this.applyMask(this.modelValue)
			}
		},
		methods: {
			handleInput(event) {
				const maskedValue = this.applyMask(event.target.value);
				event.target.value = maskedValue;
				this.$emit("update:modelValue", maskedValue);
			},
			applyMask(value) {
				const _maskedValue = [];
				let unmaskedIndex = 0;
				let maskIndex = 0;

				while (unmaskedIndex < value.length && maskIndex < this.mask.length) {
					const maskChar = this.mask[maskIndex];
					const unmaskedChar = value[unmaskedIndex];
					const maskDefinition = maskDefinitions[maskChar];

					if (maskDefinition) {
						if (maskDefinition.escape) {
							maskIndex++;
							_maskedValue.push(value[unmaskedIndex]);
							unmaskedIndex++;
						} else if (maskDefinition.pattern.test(unmaskedChar)) {
							const transformedChar = maskDefinition.transform ? maskDefinition.transform(unmaskedChar) : unmaskedChar;
							_maskedValue.push(transformedChar);
							unmaskedIndex++;
						} else {
							break;
						}
					} else {
						_maskedValue.push(maskChar);
						if (maskChar === unmaskedChar) {
							unmaskedIndex++;
						}
					}
					maskIndex++;
				}
				switch (this.textmode) {
					case 'uppercase':
						return _maskedValue.join("").toUpperCase();
						break;
					case 'lowercase':
						return _maskedValue.join("").toLowerCase();
						break;
					case 'sentencecase':
						let _value = _maskedValue.join("");
						return _value.substring(0,1).toUpperCase()+_value.substring(1).toLowerCase();
						break;
				}
			}
		},
		watch: {
			modelValue: {
				immediate: true,
				handler(newValue, oldValue) {
					const newMaskedValue = this.applyMask(newValue);
					if (newMaskedValue !== this.maskedValue) {
						this.maskedValue = newMaskedValue;
					}
				}
			}
		}
	};
</script>
