
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scalping Blocks - App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #121212;
            color: #ffffff;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .container {
            width: 100%;
            max-width: 400px;
            background: #1e1e1e;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }
        h2 { text-align: center; color: #ffcc00; margin-top: 0; }
        .card {
            background: #2b2b2b;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 15px;
        }
        .linha-central { color: #ffcc00; font-weight: bold; }
        .bloco { color: #4da6ff; }
        #paywall {
            display: none;
            text-align: center;
            background: #2c1a1a;
            border: 2px solid #ff4d4d;
            padding: 20px;
            border-radius: 8px;
            margin-top: 20px;
        }
        .btn-assinar {
            background-color: #00C853;
            color: white;
            border: none;
            padding: 12px 20px;
            font-size: 16px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
            margin-top: 10px;
        }
        .btn-assinar:hover { background-color: #00b047; }
        #timer { font-size: 14px; color: #aaa; text-align: center; margin-bottom: 15px; }
        .preco-destaque { color: #00C853; font-size: 20px; font-weight: bold; }
    </style>
</head>
<body>

    <div class="container">
        <h2>Painel de Blocos</h2>
        <div id="timer">Teste grátis: <span id="countdown">180</span>s restantes</div>

        <!-- Painel de Dados -->
        <div class="card" id="app-content">
            <p><strong>Preço Atual (BTC):</strong> <span id="preco-btc">Carregando...</span></p>
            <hr style="border-color: #444;">
            <p class="linha-central">⭐ Linha Central: <span id="val-central">---</span></p>
            <p class="bloco">📈 Topo (.868): <span id="val-topo">---</span></p>
            <p class="bloco">⚖️ Meio (.556): <span id="val-meio">---</span></p>
            <p class="bloco">🛡️ Suporte (.235): <span id="val-suporte">---</span></p>
            <p class="bloco">📉 Fundo (.055): <span id="val-fundo">---</span></p>
        </div>

        <!-- Tela de Bloqueio (Paywall com o valor de lançamento) -->
        <div id="paywall">
            <h3>🔒 O Teste Gratuito Terminou!</h3>
            <p>Gostou da precisão dos blocos? Garanta seu acesso completo agora mesmo pelo preço especial de lançamento:</p>
            <p class="preco-destaque">Apenas R$ 14,90 / mês</p>
            <p style="font-size: 12px; color: #ccc;">(Preço promocional por tempo limitado, depois sobe para R$ 19,90).</p>
            
            <!-- COLE SEU LINK DO MERCADO PAGO ABAIXO ENTRE AS ASPAS -->
            <button class="btn-assinar" onclick="window.location.href='COLIQUE_SEU_LINK_DO_MERCADO_PAGO_AQUI'">QUERO ASSINAR POR R$ 14,90</button>
        </div>
    </div>

    <script>
        // Tempo de teste grátis em segundos (180 segundos = 3 minutos)
        let tempoRestante = 180; 
        const timerElement = document.getElementById("countdown");
        
        const intervalo = setInterval(() => {
            tempoRestante--;
            timerElement.innerText = tempoRestante;
            if (tempoRestante <= 0) {
                clearInterval(intervalo);
                document.getElementById("app-content").style.display = "none";
                document.getElementById("paywall").style.display = "block";
                document.getElementById("timer").style.display = "none";
            }
        }, 1000);

        // Puxa o preço real do Bitcoin da Binance e calcula os blocos na tela
        async function atualizarPreco() {
            try {
                let resposta = await fetch('https://api.binance.com/api/v3/ticker/price?symbol=BTCUSDT');
                let dados = await resposta.json();
                let preco = parseFloat(dados.price);
                
                document.getElementById("preco-btc").innerText = preco.toFixed(2);

                let milhar = Math.floor(preco / 1000) * 1000;

                document.getElementById("val-central").innerText = (milhar + 420.50).toFixed(2);
                document.getElementById("val-topo").innerText = (milhar + 868.00).toFixed(2);
                document.getElementById("val-meio").innerText = (milhar + 556.40).toFixed(2);
                document.getElementById("val-suporte").innerText = (milhar + 235.90).toFixed(2);
                document.getElementById("val-fundo").innerText = (milhar + 055.60).toFixed(2);

            } catch (erro) {
                console.log("Erro ao buscar preço:", erro);
            }
        }

        setInterval(atualizarPreco, 2000);
        atualizarPreco();
    </script>
</body>
</html>
