<template>
  <Hero />
  <div class="flex flex-col items-center bg-gray-50 py-12">
    <div class="w-full flex flex-col items-center px-4 sm:px-6 lg:px-8 gap-8">
      <!-- Show these cards only before payment -->
      <template v-if="!showPayment">
        <DonationCard />
        <DonorNameCard />
        <div class="mt-4 md:w-[50%] lg:w-[48%] w-full">
          <UButton
            class="w-full bg-[#138B96] text-white font-bold py-3 rounded-lg text-center"
            @click="handleDonation"
            color="primary"
            variant="solid"
            block
          >
            ادفع الان
          </UButton>
        </div>
      </template>

      <!-- Show payment card -->
      <template v-else>
        <div class="flex items-center gap-4">
          <span>الدفع عبر ميسر</span>
          <UToggle v-model="useMoyasar" />
          <span>دفع مخصص</span>
        </div>

        <div class="w-full mt-6 flex items-center">
          <!-- <component :is="useMoyasar ? 'MoyasarPayment' : 'CustomPayment'" /> -->
          <component :is="useMoyasar ? MoyasarPayment : PaymentOptionsCard" />
        </div>
      </template>
    </div>
  </div>

  <SuccessModal ref="successModalRef" />
</template>

<script setup>
import { ref } from "vue";
import Hero from "@/components/ui/Hero.vue";
import DonationCard from "@/components/cards/DonationCard.vue";
import DonorNameCard from "@/components/cards/DonorNameCard.vue";
import SuccessModal from "@/components/modals/SuccessModal.vue";
import { useDonationStore } from "@/stores/donation/donationStore";
import MoyasarPayment from "@/components/cards/MoyasarPayment.vue";
//import CustomPayment from "@/components/cards/CustomPayment.vue";
import PaymentOptionsCard from "@/components/cards/PaymentOptionsCard.vue";
definePageMeta({
  layout: "default",
});

const donationStore = useDonationStore();
const successModalRef = ref(null);

const showPayment = ref(false);
const useMoyasar = ref(true);

const handleDonation = async () => {
  showPayment.value = true;
  await donationStore.submitDonation();
  if (!donationStore.submissionError) {
    // successModalRef.value?.openModal();
  }
};
</script>


<style scoped>
button {
  transition: transform 0.2s ease, background-color 0.2s ease;
}

button:hover {
  transform: translateY(-2px);
}
</style>
