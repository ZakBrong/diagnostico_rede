<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>💘 Conselheiro do Amor</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #fff0f5;
      text-align: center;
      padding: 30px;
    }
    button {
      padding: 15px;
      font-size: 18px;
      margin: 10px;
      background-color: #ff69b4;
      color: white;
      border: none;
      border-radius: 8px;
    }
    #resposta {
      margin-top: 20px;
      font-weight: bold;
      color: #c71585;
    }
  </style>
</head>
<body>
  <h1>💘 Conselheiro do Amor</h1>
  <button onclick="iniciarReconhecimento()">🎤 Perguntar falando</button>
  <div id="resposta"></div>
  <audio id="audio" controls style="margin-top:20px;"></audio>

  <script>
    const chavePixnova = "pixnova-raw_c1d4a61da3d6c217b0ae76facfd7ca7e";

    function iniciarReconhecimento() {
      if (!('webkitSpeechRecognition' in window)) {
        alert("Seu navegador não suporta reconhecimento de voz.");
        return;
      }

      const recognition = new webkitSpeechRecognition();
      recognition.lang = "pt-BR";
      recognition.interimResults = false;
      recognition.maxAlternatives = 1;

      recognition.onstart = () => {
        document.getElementById("resposta").innerText = "🎙️ Ouvindo...";
      };

      recognition.onresult = (event) => {
        const pergunta = event.results[0][0].transcript;
        document.getElementById("resposta").innerText = `Você perguntou: "${pergunta}"`;
        const conselho = gerarConselho(pergunta);
        document.getElementById("resposta").innerText += `\n💡 Conselho: "${conselho}"`;
        gerarAudio(conselho);
      };

      recognition.onerror = (event) => {
        alert("Erro no reconhecimento de voz: " + event.error);
      };

      recognition.start();
    }

    function gerarConselho(pergunta) {
      pergunta = pergunta.toLowerCase();
      if (pergunta.includes("terminar")) {
        return "Antes de terminar, pense se ainda há respeito e diálogo entre vocês.";
      } else if (pergunta.includes("ficar com alguém")) {
        return "Siga seu coração, mas observe se há reciprocidade e carinho.";
      } else if (pergunta.includes("traição")) {
        return "Confiança quebrada é difícil de reconstruir. Pense no que você merece.";
      } else {
        return "O amor é feito de escolhas diárias. Cuide, escute e esteja presente.";
      }
    }

    function gerarAudio(texto) {
      fetch("https://api.pixnova.ai/voice-clone", {
        method: "POST",
        headers: {
          "Authorization": `Bearer ${chavePixnova}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          text: texto,
          voice: "pt-br-feminina",
          output_format: "mp3"
        })
      })
      .then(res => res.json())
      .then(data => {
        const audioUrl = data.audio_url;
        const audioPlayer = document.getElementById("audio");
        audioPlayer.src = audioUrl;
        audioPlayer.play();
      })
      .catch(err => {
        alert("Erro ao gerar áudio. Verifique a chave da API.");
        console.error(err);
      });
    }
  </script>
</body>
</html>
