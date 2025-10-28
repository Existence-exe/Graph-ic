<template>

    <div class="flex flex-row justify-center align-center p-5">
        <div class="graph-container" ref="calculator" style="width: 500px; height: 500px;"></div>
    </div>
    <form class="flex flex-row justify-center align-center p-5 gap-x-10" @submit.prevent="addEquation">
        <label for="newEquation">Add New Equation</label>
        <input type="text" id="newEquation" name="newEquation">
        <button type="submit">ADD NEW EQUATION</button>
    </form>
    <div class="flex flex-row justify-center align-center p-5">
        <button @click="saveGraph">Save Graph as PNG</button>
    </div>

</template>

<script>

export default {
    name: "DesmosGraph",
    mounted() {

        if (window.Desmos) {
            this.initDesmos();
        } else {
            console.error("Desmos library not loaded");
        }
    },
    methods: {
        
        initDesmos() {
            const elt = this.$refs.calculator;
            this.calculator = Desmos.GraphingCalculator(elt, {
                expressions: false,
                keypad: true,
                settingsMenu: false,
                zoomButtons: false,
                showGrid: false,
                xAxisNumbers: false,
                yAxisNumbers: false
            });
            
        },
        updateEquation(latex) {
            this.calculator.setExpression({ id: 'graph1', latex });
        }, 
        addEquation() { 
            if (newEquation.value.trim() === '') return;
            this.calculator.setExpression({ id: 'graph1', latex: newEquation.value });
        },
        saveGraph() {
            if (!this.calculator) return;

           
            const rect = this.$refs.calculator.getBoundingClientRect();
            const width = rect.width;
            const height = rect.height;

        
            const imageData = this.calculator.screenshot({
                width,
                height,
                targetPixelRatio: 2, 
                showLabels: true,
                transparent: false,
            });

           
            const link = document.createElement("a");
            link.href = imageData;
            link.download = "desmos-graph.png";
            link.click();
        }
    },
    beforeUnmount() {
        if (this.calculator) {
            this.calculator.destroy();
        }
    }
};
</script>

<style scoped>
.graph-container {
    border: 1px solid #ccc;
    border-radius: 10px;
}
</style>
