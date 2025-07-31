<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simulador de Empréstimo</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 2rem;
      background-color: #f9f9f9;
    }
    .container {
      max-width: 500px;
      margin: auto;
      background: white;
      padding: 2rem;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h1 {
      text-align: center;
      color: #333;
    }
    label {
      display: block;
      margin-top: 1rem;
    }
    input, select, button {
      width: 100%;
      padding: 0.5rem;
      margin-top: 0.3rem;
    }
    .result {
      margin-top: 2rem;
      background: #e6f7e6;
      padding: 1rem;
      border: 1px solid #a0d4a0;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Simulador de Empréstimo</h1>
    <label for="valor">Valor do Empréstimo (R$)</label>
    <input type="number" id="valor" placeholder="Ex: 5000" min="0">

    <label for="parcelas">Nº de Parcelas</label>
    <select id="parcelas">
      <!-- de 3x a 24x -->
      <option value="">Selecione</option>
    </select>

    <button onclick="simular()">Simular</button>

    <div class="result" id="resultado" style="display:none;"></div>
  </div>

  <script>
    // Popular opções de parcelas (3 a 24)
    const selParcelas = document.getElementById("parcelas");
    for (let i = 3; i <= 24; i++) {
      let opt = document.createElement("option");
      opt.value = i;
      opt.textContent = i + "x";
      selParcelas.appendChild(opt);
    }

    function simular() {
      const valor = parseFloat(document.getElementById("valor").value);
      const parcelas = parseInt(document.getElementById("parcelas").value);
      const taxa = 0.01; // 1% ao mês

      if (!valor || !parcelas) {
        alert("Preencha todos os campos.");
        return;
      }

      // Fórmula da Parcela com juros compostos: 
      // PMT = P * i / [1 - (1+i)^-n]
      const i = taxa;
      const n = parcelas;
      const parcela = valor * i / (1 - Math.pow(1 + i, -n));
      const total = parcela * n;
      const jurosTotais = total - valor;

      const res = document.getElementById("resultado");
      res.style.display = "block";
      res.innerHTML = `
        <strong>Resultado da Simulação:</strong><br>
        Valor Solicitado: <strong>R$ ${valor.toFixed(2)}</strong><br>
        Parcelas: <strong>${n}x de R$ ${parcela.toFixed(2)}</strong><br>
        Total a Pagar: <strong>R$ ${total.toFixed(2)}</strong><br>
        Juros Totais: <strong>R$ ${jurosTotais.toFixed(2)}</strong><br>
        CET (Custo Efetivo Total): <strong>${(taxa * 100).toFixed(2)}% ao mês / 12,00% ao ano</strong>
      `;
    }
  </script>
</body>
</html>
