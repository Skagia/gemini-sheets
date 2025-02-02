# gemini-sheets
Código para integrar API do Gemini com o Google Sheets


```
// Coloque aqui a API do Gemini
const GEMINI_API_KEY = 'SUA-API-AQUI';
const GEMINI_API_URL = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent';

function gemini(prompt, maxTokens=2500, temperature=0.7){


// Verifica se o prompt possui algum valor
  if(!prompt){
    throw new Error('Please provide a valid text prompt');
  }
  
  // Objeto cria a requisição informando o prompt, token e temperatura
  const requestBody = {
    contents:[{
      parts: [{
        text: prompt
      }]
    }],
    generationConfig:{
        maxOutputTokens: maxTokens,
        temperature: temperature
    }
  }

  // Objeto cria a requisição informando o método POST, o tipo JSON e carregando as informações do objeto requestBody
  const options = {
    method: 'POST',
    headers: {
      'Content-Type' : 'application/json'
    },
    payload: JSON.stringify(requestBody),
    muteHttpExcpetions: true
  }

  // Try Catch para tratamento de erros 
  try{
    // Realiza a conexão com a API para gerar a resposta
    const response = UrlFetchApp.fetch(`${GEMINI_API_URL}?key=${GEMINI_API_KEY}`, options);
    const responseCode = response.getResponseCode();
    const responseText = response.getContentText();
    const responseData = JSON.parse(responseText);
    // Verifica se o acesso a API foi realizado com sucesso
    if (responseCode !== 200) {
      throw new Error('API Error');
    }

    // generatedText recebe a resposta da API
    const generatedText = responseData.candidates?.[0]?.content?.parts?.[0]?.text;
    // Se não receber nada, vai retornar um erro
    if (!generatedText) {
      throw new Error('Unexpected API response format');
    }

  return generatedText;

  } catch(error){
    // Log do erro que retornou
    console.error('Gemini API Error:', error);
    return `Error: ${error.message}`;
  }
  
}
```
