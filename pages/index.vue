<template>
    <div class="landing">
        <h1>SEO TOOL: Intención de búsqueda con CHAT GPT</h1>

       
        <div v-if="getExcelContent()"><h2>Palabras Clave:</h2><br/><p> {{ getExcelContent() }}</p>
            <button @click="resetExcelContent()">Reset</button>
        </div>
        <div v-else><input id="excelfile" type="file" @change="uploadExcel()" /></div>
   
    </div>
</template>

<script setup>
import readXlsFile from "read-excel-file";

const data = () => ({
    items: [],
});
const getExcelContent = () => {
  if (typeof window !== 'undefined') {
    return window.localStorage.getItem('excelcontent');
  }
  return '';
};
const resetExcelContent = () => {
  if (typeof window !== "undefined") {
    window.localStorage.removeItem("excelcontent");
    location.reload();
  }
};

//Función que sube un excel y transforma las kw en una cadena de texto
const uploadExcel = () => {
  
    const input = document.getElementById("excelfile");
    readXlsFile(input.files[0]).then((rows) => {
        data.items = rows;
        const valores = data.items.slice(3).map(item => item[0]);
        let excelcontent = valores.join(", ");
        input.style.display="none";
        if (typeof window !== 'undefined') {
      window.localStorage.setItem('excelcontent', excelcontent);
      location.reload();
    }    
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
  
