<template>
  <article>
    <PasswordDisplay :password="generatedPassword" />
    <form>
      <fieldset>
        <legend class="visually-hidden">Password Settings</legend>
        <div class="slider">
          <label for="character-length"
            >Character Length <output>{{ sliderValue }}</output></label
          >
          <div class="custom-slider">
            <input
              ref="slider"
              type="range"
              class="custom-range"
              v-model="sliderValue"
              :min="min"
              :max="max"
              :step="step"
              @input="({ target }) => (sliderValue = parseFloat(target.value))"
            />
          </div>
        </div>
        <div class="settings">
          <label>
            <input
              type="checkbox"
              class="custom-checkbox"
              v-model="uppercase"
            />
            include uppercase letters
          </label>
          <label>
            <input
              type="checkbox"
              class="custom-checkbox"
              v-model="lowercase"
              checked
            />
            include lowercase letters
          </label>
          <label>
            <input type="checkbox" class="custom-checkbox" v-model="numbers" />
            include numbers
          </label>
          <label>
            <input type="checkbox" class="custom-checkbox" v-model="symbols" />
            include symbols
          </label>
        </div>
        <PasswordStrengthIndicator
          :length="sliderValue"
          :uppercase="uppercase"
          :lowercase="lowercase"
          :numbers="numbers"
          :symbols="symbols"
        />
      </fieldset>
      <GenerateButton
        :length="sliderValue"
        :uppercase="uppercase"
        :lowercase="lowercase"
        :numbers="numbers"
        :symbols="symbols"
        :disabled="!isAnyCheckboxChecked"
        @passwordGenerated="updatePassword"
      />
    </form>
  </article>
</template>

<script setup>
import { ref, computed, watch, onMounted } from "vue";
import PasswordStrengthIndicator from "./PasswordStrengthIndicator.vue";
import PasswordDisplay from "./PasswordDisplay.vue";
import GenerateButton from "./GenerateButton.vue";

const min = 0;
const max = 20;
const step = 1;

const sliderValue = ref(10);
const slider = ref(null);

const uppercase = ref(false);
const lowercase = ref(true);
const numbers = ref(false);
const symbols = ref(false);

const generatedPassword = ref("");

function updatePassword(newPassword) {
  generatedPassword.value = newPassword;
}

// Computed property to check if at least one checkbox is selected
const isAnyCheckboxChecked = computed(() => {
  return uppercase.value || lowercase.value || numbers.value || symbols.value;
});

const getProgress = (value, min, max) => {
  return ((value - min) / (max - min)) * 100;
};

const setCSSProgress = (progress) => {
  if (slider.value) {
    slider.value.style.setProperty("--ProgressPercent", `${progress}%`);
  }
};

const updateSliderColor = () => {
  const progress = getProgress(sliderValue.value, min, max);
  setCSSProgress(progress);
};

onMounted(() => {
  updateSliderColor();
});

watch(sliderValue, updateSliderColor);
</script>

<style scoped>
form {
  background-color: var(--primary-500);
  padding: 1rem;
}

fieldset {
  all: unset;
  min-width: 100%;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

output {
  color: var(--secondary);
  font-size: clamp(1.5rem, 2.036vw + 1.023rem, 2rem);
}

.visually-hidden {
  position: absolute !important;
  width: 1px !important;
  height: 1px !important;
  padding: 0 !important;
  margin: -1px !important;
  overflow: hidden !important;
  clip: rect(0, 0, 0, 0) !important;
  white-space: nowrap !important;
  border: 0 !important;
}

label {
  display: flex;
  gap: clamp(1.25rem, 1.018vw + 1.011rem, 1.5rem);
  color: var(--primary-100);
  font-size: clamp(1rem, 0.509vw + 0.881rem, 1.125rem);
}

.slider label {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: clamp(0.5rem, 2.036vw + 0.023rem, 1rem);
}

.settings {
  text-transform: capitalize;
  display: flex;
  flex-direction: column;
  gap: clamp(1rem, 1.018vw + 0.761rem, 1.25rem);
}

.custom-checkbox {
  appearance: none;
  -webkit-appearance: none;
  width: 20px;
  height: 20px;
  border: 2px solid var(--primary-100);
  background-color: transparent;
  position: relative;
  cursor: pointer;
}

.custom-checkbox:hover {
  background-color: transparent;
  border: 2px var(--secondary) solid;
}

.custom-checkbox:checked {
  background-color: var(--secondary);
  background-image: url(../assets/images/icon-check.svg);
  background-repeat: no-repeat;
  background-position: center;
  border: none;
}

/* style the input element with type "range" */
.custom-slider input[type="range"] {
  width: 100%;
  position: relative;
  appearance: none;
  z-index: 0;
  background-color: transparent;
}

/* ::before element to replace the slider track for non-Firefox browsers */
.custom-slider input[type="range"]::before {
  content: "";
  position: absolute;
  width: var(--ProgressPercent);
  height: 100%;
  background-color: var(--secondary);
  pointer-events: none;
}

/* Targets the track (background) of a range slider in WebKit browsers (Chrome, Safari) */
.custom-slider input[type="range"]::-webkit-slider-runnable-track {
  appearance: none;
  background: var(--primary-900);
  height: 0.5rem;
}

/* Targets the progress of the range slider in WebKit browsers */
.custom-slider input[type="range"]::-webkit-slider-thumb {
  position: relative;
  width: 1.75rem;
  height: 1.75rem;
  margin-top: calc((0.5rem - 1.75rem) / 2);
  background: var(--primary-100);
  border-radius: 999px;
  pointer-events: all;
  appearance: none;
  z-index: 1;
  cursor: pointer;
}

.custom-slider input[type="range"]::-webkit-slider-thumb:hover {
  background-color: var(--primary-900);
  border: var(--secondary) 2px solid;
}

/* Targets the track (background) of a range slider in Mozilla Firefox */
.custom-slider input[type="range"]::-moz-range-track {
  background: var(--primary-900);
  height: 0.5rem;
}

/* Targets the progress of a range slider in Mozilla Firefox */
.custom-slider input[type="range"]::-moz-range-progress {
  background: var(--secondary);
  height: 0.5rem;
}

/* Style for the thumb of the range slider in Mozilla Firefox */
.custom-slider input[type="range"]::-moz-range-thumb {
  position: relative;
  width: 1.75rem;
  height: 1.75rem;
  background: var(--primary-100);
  border-radius: 999px;
  pointer-events: all;
  cursor: pointer;
  z-index: 1;
}

.custom-slider input[type="range"]::-moz-range-thumb:hover {
  background-color: var(--primary-900);
  border: var(--secondary) 2px solid;
}

@media (min-width: 37.5rem) {
  form {
    padding: 1.5rem 2rem 2rem 2rem;
  }
}
</style>
