<template>
    <div class="landing">
        <h1>SEO TOOL: Intención de búsqueda con CHAT GPT</h1>

        <input id="excelfile" type="file" @change="uploadExcel()" />

    </div>
</template>
<script setup>
import readXlsFile from "read-excel-file";

const data = () => ({
    items: [],
});

const uploadExcel = () => {
    let excelcontent = "";
    const input = document.getElementById("excelfile");
    readXlsFile(input.files[0]).then((rows) => {
        data.items = rows;
        const valores = data.items.slice(3).map(item => item[0]);
        excelcontent = valores.join(", ");
        console.log(excelcontent);
    });



};
</script>
<style scoped>
.landing {
    min-height: 89vh;
}

h1 {
    text-align: center;
}
</style>
  
