<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de EAN-13</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; }
        .container { max-width: 600px; margin: 0 auto; }
        input, button { padding: 10px; margin: 10px; }
        #barcode { margin-top: 20px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Gerador de EAN-13</h1>
        <input type="text" id="eanInput" placeholder="Digite 12 dígitos" maxlength="12">
        <button onclick="generateEAN()">Gerar EAN-13</button>
        <div id="barcode"></div>
        <button onclick="downloadBarcode()">Download PNG</button>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
    <script>
        function generateEAN() {
            const input = document.getElementById('eanInput').value;
            if (input.length !== 12 || !/^\d+$/.test(input)) {
                alert("Por favor, insira 12 dígitos numéricos!");
                return;
            }
            // Cálculo do dígito verificador (EAN-13)
            let sum = 0;
            for (let i = 0; i < 12; i++) {
                sum += parseInt(input[i]) * (i % 2 === 0 ? 1 : 3);
            }
            const checkDigit = (10 - (sum % 10)) % 10;
            const ean13 = input + checkDigit;
            
            // Gerar código de barras
            JsBarcode("#barcode", ean13, { format: "EAN13", displayValue: true });
        }

        function downloadBarcode() {
            const canvas = document.querySelector("#barcode canvas");
            if (!canvas) {
                alert("Gere um código primeiro!");
                return;
            }
            const link = document.createElement('a');
            link.download = 'ean13_barcode.png';
            link.href = canvas.toDataURL('image/png');
            link.click();
        }
    </script>
</body>
</html>
