<template>
    <div class="landing">
        <div class="landing-box">
        <h1>SEO TOOL: Intención de búsqueda con CHAT GPT</h1>

       
        <div v-if="getExcelContent()"><h2>Palabras Clave:</h2><br/><p> {{ getExcelContent() }}</p>
            <button @click="resetExcelContent()">Reset</button>
            <button @click="sendToChatGpt()">Enviar prompt a Chat GPT</button><br>
            <p>{{ response }}</p>
        </div>
        <div v-else>
            
            <input id="excelfile" type="file" @change="uploadExcel()" />
        
            <p>Selecciona un archivo .xlsx</p>
        </div>
           
    </div>
    </div>
</template>

<script setup>
import readXlsFile from "read-excel-file";
import { ref } from 'vue';
const response = ref({});

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


const sendToChatGpt = () => {
  const promptContent = getExcelContent();
  const systemMessage = [{
    role: "system",
    content:  `Ordena las siguientes keywords por intención de búsqueda y familias semanticas. Seria interesante que lo ordenaras para que quedaran agrupadas las keywords por diferentes paginas.  ${promptContent}`
  }];
  fetch("https://api.openai.com/v1/chat/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: "Bearer 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",
    },
    body: JSON.stringify({
      model: "gpt-3.5-turbo",
      messages: systemMessage,
      max_tokens: 1024,
      temperature: 0.7,
      n: 1,
    }),
  })
    .then((response) => response.json())
    .then((data) => {
        response.value = data.choices[0].message.content;
    })
    .catch((error) => console.error(error));
};


</script>
<style scoped>
.landing {
    min-height: 94vh;
    display: flex;
    justify-content: center;
    align-items: center;

}

h1 {
    text-align: center;
}
.landing-box{
height: 70vh;
border: 2px solid grey;
border-radius: 14px;
box-shadow: 0px 4px 6px rgba(0,0,0,0.4);
padding: 100px;
}
#excelfile{
    margin-top: 4vh;
}
</style>
  
