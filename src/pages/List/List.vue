<template>
    <ul>
    <li @click="() => numbers[3] += 10">Here is a list of {{ numbers }}</li>
    <li>Here is those numbers doubled: {{ doubledNumbers }}</li>
    <li>Here is those numbers tripled: {{ tripledNumbers }}</li>
    <li>Here is those numbers quadrupled: {{ quadrupledNumbers }}</li>
    <li>Here is those numbers tripled and then squared: {{ squaredTripledNumbers }}</li>
    <li>Here is those numbers double added to those numbers tripled: {{ doubledAddedToTripled }}</li>
    <li>Here is those numbers double added to those numbers quadrupled: {{ doubledAddQuadrupled }}</li>
    <li>Here is those numbers tripled and multiplied by <span @click="() => factor += 1">{{ factor }}</span>: {{ tripledTimesFactor }}</li>
    <li>Here is those numbers doubled and multiplied by <span @click="() => factor += 1">{{ factor }}</span>: {{ doubledMultipledByFactor }}</li>
    </ul>
</template>

<script>

export default {
    name: 'List',
    data() {
        return {
            numbers: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
            tripledNumbers: [],
            doubledAddQuadrupled: [],
            factor: 10,
            tripledTimesFactor: [],
        }
    },
    // mounted() {
    //     this.tripledNumbers = this.tripleNumbers(this.numbers)
    // },
    computed: {
        doubledNumbers() { //can use computed instead of data, which would also involve using watch and methods (where would also need either mounted to call the function or the property immediate: true under watch)
            const doubledNumbers = this.numbers.map( (number) => {
                return number * 2
            })
            return doubledNumbers
        },
        quadrupledNumbers() { //simpler syntax for when the function is one line and just returns a value
            return this.numbers.map((number) => number * 4)  
        },
        squaredTripledNumbers() { //can use data in computed
            return this.tripledNumbers.map((number) => number * number)
        },
        doubledAddedToTripled() { //can use data and computed properties in computed
            return this.doubledNumbers.map((doubledNumber, index) => doubledNumber + this.tripledNumbers[index])
        },
        doubledMultipledByFactor() { //can easily react to 2 dependencies in computed, whereas in watch would need to create 2 watchers
            return this.multiplyList(this.doubledNumbers, this.factor)
        }
    },
    watch: {
        numbers: {
            deep: true,
            immediate: true,
            handler(newValue, oldValue) {
                this.tripledNumbers = this.tripleNumbers(this.numbers)
                this.doubledAddQuadrupled = this.doubledNumbers.map((doubledNumber, index) => doubledNumber + this.quadrupledNumbers[index])
                this.tripledTimesFactor = this.multiplyList(this.tripledNumbers, this.factor)
            }
        },
        factor() {
            this.tripledTimesFactor = this.multiplyList(this.tripledNumbers, this.factor)
        }
    },
    methods: {
        tripleNumbers(numbers) {
            return numbers.map((number) => number * 3)
        },
        multiplyList(list, factor) {
            return list.map((number) => number * factor)
        }
    }
}
</script>

<style module src="./List.css"></style>