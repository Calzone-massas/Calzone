
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pedidos Calzone</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            color: #333;
            margin: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .form-container {
            background: #fff;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            max-width: 400px;
            width: 100%;
        }
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        .form-group input, .form-group textarea, .form-group select {
            width: 100%;
            padding: 10px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 14px;
        }
        button {
            background-color: #007bff;
            color: #fff;
            border: none;
            padding: 10px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
        }
        button:hover {
            background-color: #0056b3;
        }
        .comanda {
            width: 8cm;
            border: 1px solid #000;
            padding: 10px;
            margin-top: 20px;
            font-size: 12px;
        }
        .comanda h2 {
            text-align: center;
            margin-bottom: 10px;
            font-size: 16px;
        }
        .comanda p {
            margin: 5px 0;
        }
        @media print {
            body * {
                visibility: hidden;
            }
            .comanda, .comanda * {
                visibility: visible;
            }
            .comanda {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                margin: 0;
                padding: 0;
            }
        }
    </style>
</head>
<body>
    <div class="form-container">
        <h1>Monte seu macarrão</h1>
        <div class="form-group">
            <label for="cliente">Nome do Cliente</label>
            <input type="text" id="cliente">
        </div>
        <div class="form-group">
            <label for="pagamento">Forma de Pagamento</label>
            <select id="pagamento">
                <option value="cartao">Cartão</option>
                <option value="dinheiro">Dinheiro</option>
                <option value="pix">Pix</option>
            </select>
        </div>
        <div class="form-group">
            <label for="endereco">Endereço de Entrega</label>
            <input type="text" id="endereco">
        </div>
        <div class="form-group">
            <label for="detalhes">Detalhes do Pedido</label>
            <textarea id="detalhes" rows="10"></textarea>
        </div>
        <div class="form-group">
            <label for="outrosProdutos">Outros Produtos (opcional)</label>
            <textarea id="outrosProdutos" rows="3"></textarea>
        </div>
        <div class="form-group">
            <label for="valorPedido">Valor do Pedido (opcional)</label>
            <input type="number" id="valorPedido" step="0.01" onchange="calcularTotal()">
        </div>
        <div class="form-group">
            <label for="valorFrete">Valor do Frete (opcional)</label>
            <input type="number" id="valorFrete" step="0.01" onchange="calcularTotal()">
        </div>
        <div class="form-group">
            <label for="valorTotal">Valor Total (opcional)</label>
            <input type="number" id="valorTotal" readonly>
        </div>
        <button onclick="gerarComanda()">Imprimir</button>

        <div id="comanda" class="comanda" style="display: none;">
            <h2>Comanda</h2>
            <div id="comandaConteudo"></div>
        </div>
    </div>

    <script>
        function calcularTotal() {
            const valorPedido = parseFloat(document.getElementById('valorPedido').value) || 0;
            const valorFrete = parseFloat(document.getElementById('valorFrete').value) || 0;
            const valorTotal = valorPedido + valorFrete;
            document.getElementById('valorTotal').value = valorTotal.toFixed(2);
        }

        function gerarComanda() {
            const cliente = document.getElementById('cliente').value;
            const pagamento = document.getElementById('pagamento').value;
            const endereco = document.getElementById('endereco').value;
            const detalhes = document.getElementById('detalhes').value;
            const outrosProdutos = document.getElementById('outrosProdutos').value;
            const valorPedido = document.getElementById('valorPedido').value;
            const valorFrete = document.getElementById('valorFrete').value;
            const valorTotal = document.getElementById('valorTotal').value;

            const topicos = ['Massa', 'Tempero', 'Refogar', 'Molho', 'Finalização', 'Acompanhamentos', 'Adicional'];
            const detalhesArray = detalhes.split('\n');

            let comandaHTML = `<p><strong>Nome do Cliente:</strong> ${cliente}</p>`;
            comandaHTML += `<p><strong>Forma de Pagamento:</strong> ${pagamento}</p>`;
            comandaHTML += `<p><strong>Endereço de Entrega:</strong> ${endereco}</p>`;
            comandaHTML += '<hr>';

            for (let i = 0; i < topicos.length; i++) {
                if (detalhesArray[i]) {
                    comandaHTML += `<p><strong>${topicos[i]}:</strong> ${detalhesArray[i]}</p>`;
                }
            }

            if (outrosProdutos) {
                comandaHTML += `<p><strong>Outros Produtos:</strong> ${outrosProdutos}</p>`;
            }

            if (valorPedido) {
                comandaHTML += `<p><strong>Valor do Pedido:</strong> R$ ${valorPedido}</p>`;
            }

            if (valorFrete) {
                comandaHTML += `<p><strong>Valor do Frete:</strong> R$ ${valorFrete}</p>`;
            }

            comandaHTML += `<p><strong>Valor Total:</strong> R$ ${valorTotal}</p>`;

            document.getElementById('comandaConteudo').innerHTML = comandaHTML;
            document.getElementById('comanda').style.display = 'block';

            window.print();
        }
    </script>
</body>
</html>
