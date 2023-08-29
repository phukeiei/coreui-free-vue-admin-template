<template>
    <div>
        <v-main>
            <HelloWorld />
            <!-- <DWT /> -->
        </v-main>
    </div>
</template>
<script>
import HelloWorld from '@/components/HelloWorld';

// import DWT from '@/components/DWT';

export default {
    name: 'App',

    components: {
        HelloWorld,
        // DWT
    },

    data: () => ({
        //
    }),
};
</script> 