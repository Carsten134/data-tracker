<template>
    <div class="flex justify-center items-center flex-col">
        <div class="font-bold text-xl mb-2">Mood / activity tracker</div>
        <!-- <input type="text" name="test" class="border-[1px] rounded-md hover:shadow-md my-2" id="" :text="mood"> -->
        <hr class="w-full">
        <div class="flex justify-center items-center flex-col" v-if="!dataSent">
            <label for="mood_tracker">Todays mood (1-10):</label>
            <select name="Mood" id="mood_tracker" v-model="mood">
                <option v-for="mood in moodStates" :value="mood">{{ mood }}</option>
            </select>
            <label for="weight-tracker">Todays weight in Kg:</label>
            <input type="number" name="weight" id="weight-tracker" v-model="weight">
        </div>
        <span v-if="dataSent">Daten erfolgreich gesendet!</span>
        <button class="bg-black hover:shadow-md rounded-md text-white px-2 py-1" @click="postData">Send</button>
    </div>
</template>

<script setup>
    let supabase = useSupabaseClient()
    const moodStates = [1,2,3,4,5,6,7,8, 9, 10]
    const mood = ref(1)
    const weight = ref(0)
    const dataSent = ref(false)
    async function postData() {
        const {data, error} = await supabase
            .from("tracker_test")
            .insert({
                "mood": mood.value,
                "weight": weight.value
            })

        if (error) {
            console.log(error)
        }
        else {
            dataSent.value = true
        }
    }
</script>