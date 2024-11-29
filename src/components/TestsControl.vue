<template>
    <div class="main-container">
        <div class="container" v-if="currentTestIndex !== null">
            <component :is="currentTestComponent" :test-data="testData[currentTestIndex]" />
        </div>
        <div class="container" v-else>
            <h1>Выберите тест</h1>
        </div>
        <div class="select-test-container">
            <button v-for="(test, index) in testData" :key="index" @click="selectTest(index)"
                :class="{ selected: index === currentTestIndex }" class="select-button">{{ test.name }}</button>
        </div>
    </div>
</template>

<script>
import Test1 from './Test1.vue';
import Test2 from './Test2.vue';
import Test3 from './Test3.vue';
import Test4 from './Test4.vue';
import Test5 from './Test5.vue';

export default {
    components: {
        Test1,
        Test2,
        Test3,
        Test4,
        Test5,
    },
    data() {
        return {
            testData: [
                { name: 'Тест на внимание', component: 'Test1' },
                { name: 'Тест 2', component: 'Test2' },
                { name: 'Тест 3', component: 'Test3' },
                { name: 'Тест 4', component: 'Test4' },
                { name: 'Тест 5', component: 'Test5' },
            ],
            currentTestIndex: null,
            currentTestComponent: null,
        };
    },
    methods: {
        selectTest(index) {
            this.currentTestIndex = index;
            this.currentTestComponent = this.testData[index].component;
        },
    },
};
</script>