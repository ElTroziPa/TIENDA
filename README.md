<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generador de Factura - ZitroStyle</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f9fafb;
            color: #333;
            line-height: 1.6;
        }
        header {
            background-color: #3498db;
            color: white;
            padding: 20px;
            text-align: center;
            border-bottom: 5px solid #2980b9;
        }
        header h1 {
            margin: 0;
            font-size: 2rem;
        }
        .container {
            max-width: 1200px;
            margin: 20px auto;
            padding: 30px;
            background: #ffffff;
            box-shadow: 0 6px 10px rgba(0, 0, 0, 0.1);
            border-radius: 12px;
        }
        .controls {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 30px;
        }
        .controls label {
            font-weight: bold;
            margin-right: 10px;
            font-size: 1rem;
        }
        .controls input, .controls button, .controls select {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s ease, box-shadow 0.3s ease;
        }
        .controls input:focus, .controls button:focus, .controls select:focus {
            border-color: #3498db;
            box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
            outline: none;
        }
        .controls button {
            background-color: #3498db;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .controls button:hover {
            background-color: #2980b9;
        }
        .controls button svg {
            margin-right: 8px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            font-size: 1rem;
            border-radius: 8px;
            overflow: hidden;
        }
        table th, table td {
            padding: 12px;
            text-align: left;
            border: 1px solid #ddd;
        }
        table th {
            background-color: #3498db;
            color: white;
            font-weight: bold;
        }
        table tbody tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        table tfoot td {
            font-weight: bold;
            background-color: #f2f2f2;
        }
        .payment-proof img {
            max-width: 100%;
            height: auto;
            border-radius: 8px;
            margin-top: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .hidden {
            display: none;
        }
        .invoice-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 20px;
        }
        .invoice-header img {
            max-width: 100px;
            height: auto;
            border-radius: 8px;
        }
        @media (max-width: 768px) {
            header h1 {
                font-size: 1.5rem;
            }
            .controls {
                grid-template-columns: 1fr;
            }
            table {
                font-size: 0.9rem;
            }
            th, td {
                padding: 8px;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>ZitroStyle - Generador de Facturas</h1>
    </header>
    <div class="container">
        <div class="controls">
            <label for="logoUpload">Subir Logo:</label>
            <input type="file" id="logoUpload">
            <label for="clientName">Nombre del Cliente:</label>
            <input type="text" id="clientName" placeholder="Ejemplo: Juan Pérez">
            <button onclick="addProduct()" class="tooltip">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16">
                    <path d="M8 0a8 8 0 1 0 0 16A8 8 0 0 0 8 0zM7 11a1 1 0 1 1 2 0v3a1 1 0 1 1-2 0v-3zm-4-5a1 1 0 1 1 2 0v6a1 1 0 1 1-2 0V6z"/>
                </svg>
                Añadir Producto
            </button>
            <label for="discount">Descuento ($):</label>
            <input type="number" id="discount" value="0" placeholder="Ejemplo: 10">
            <label for="hideDiscount">Ocultar Descuento:</label>
            <input type="checkbox" id="hideDiscount">
            <label for="paymentMade">Abono Realizado ($):</label>
            <input type="number" id="paymentMade" value="0" placeholder="Ejemplo: 50">
            <label for="currency">Moneda Base:</label>
            <select id="currency">
                <option value="$">Dólares Estadounidenses ($)</option>
                <option value="Bs.D">Bolívares Digitales (Bs.D)</option>
            </select>
            <label for="exchangeRate">Tasa de Cambio (Bs.D/$):</label>
            <input type="number" id="exchangeRate" value="25" placeholder="Ejemplo: 25">
            <label for="paymentProofUpload">Imagen de Pago Referencial:</label>
            <input type="file" id="paymentProofUpload">
            <button onclick="generateInvoice()">
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16">
                    <path d="M8 0a8 8 0 1 0 0 16A8 8 0 0 0 8 0zm0 14A6 6 0 1 1 8 2a6 6 0 0 1 0 12z"/>
                </svg>
                Generar Factura
            </button>
        </div>

        <div id="invoicePreview">
            <div class="invoice-header">
                <img id="invoiceLogo" src="https://via.placeholder.com/100" alt="Logo de la Empresa">
                <h2>Factura</h2>
            </div>
            <p><strong>Empresa:</strong> ZITROSTYLE VNZLA</p>
            <p><strong>Cliente:</strong> <span id="invoiceClientName"></span></p>
            <table>
                <thead>
                    <tr>
                        <th>Producto</th>
                        <th>Cantidad</th>
                        <th>Precio Unitario (<span id="invoiceCurrency">$</span>)</th>
                        <th>Subtotal (<span id="invoiceCurrency">$</span>)</th>
                    </tr>
                </thead>
                <tbody id="invoiceProducts">
                </tbody>
                <tfoot>
                    <tr>
                        <td colspan="3">Total:</td>
                        <td id="subtotal">$0</td>
                    </tr>
                    <tr id="discountRow">
                        <td colspan="3">Descuento:</td>
                        <td id="invoiceDiscount">$0</td>
                    </tr>
                    <tr>
                        <td colspan="3">Abono Realizado:</td>
                        <td id="invoicePaymentMade">$0</td>
                    </tr>
                    <tr>
                        <td colspan="3">Total Final:</td>
                        <td id="finalTotal">$0</td>
                    </tr>
                    <tr id="equivalentRow">
                        <td colspan="3">Equivalente en $:</td>
                        <td id="invoiceEquivalent">$0</td>
                    </tr>
                </tfoot>
            </table>
            <button onclick="downloadPDF()">Descargar PDF</button>
            <div class="payment-proof">
                <h3>Imagen de Pago Referencial</h3>
                <img id="paymentProofImage" src="https://via.placeholder.com/150" alt="Pago Referencial">
            </div>
        </div>
    </div>

    <!-- Contenedor oculto para el PDF -->
    <div id="pdfContainer" class="pdf-container">
        <div class="invoice-header">
            <img id="pdfLogo" src="https://via.placeholder.com/100" alt="Logo de la Empresa">
            <h2>Factura - ZitroStyle</h2>
        </div>
        <p><strong>Empresa:</strong> ZITROSTYLE VNZLA</p>
        <p><strong>Cliente:</strong> <span id="pdfClientName"></span></p>
        <table>
            <thead>
                <tr>
                    <th>Producto</th>
                    <th>Cantidad</th>
                    <th>Precio Unitario (<span id="pdfCurrency">$</span>)</th>
                    <th>Subtotal (<span id="pdfCurrency">$</span>)</th>
                </tr>
            </thead>
            <tbody id="pdfProducts">
            </tbody>
            <tfoot>
                <tr>
                    <td colspan="3">Total:</td>
                    <td id="pdfSubtotal">$0</td>
                </tr>
                <tr id="pdfDiscountRow">
                    <td colspan="3">Descuento:</td>
                    <td id="pdfDiscount">$0</td>
                </tr>
                <tr>
                    <td colspan="3">Abono Realizado:</td>
                    <td id="pdfPaymentMade">$0</td>
                </tr>
                <tr>
                    <td colspan="3">Total Final:</td>
                    <td id="pdfFinalTotal">$0</td>
                </tr>
                <tr id="pdfEquivalentRow">
                    <td colspan="3">Equivalente en $:</td>
                    <td id="pdfEquivalent">$0</td>
                </tr>
            </tfoot>
        </table>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <script>
        let products = [];

        function addProduct() {
            const productName = prompt("Nombre del producto:");
            const quantity = parseInt(prompt("Cantidad:"));
            const unitPrice = parseFloat(prompt("Precio unitario:"));

            if (productName && !isNaN(quantity) && !isNaN(unitPrice)) {
                products.push({ name: productName, quantity, unitPrice });
                updateInvoice();
            } else {
                alert("Por favor, ingresa valores válidos.");
            }
        }

        function updateInvoice() {
            const clientName = document.getElementById('clientName').value;
            const discount = parseFloat(document.getElementById('discount').value);
            const paymentMade = parseFloat(document.getElementById('paymentMade').value);
            const hideDiscount = document.getElementById('hideDiscount').checked;
            const currency = document.getElementById('currency').value;
            const exchangeRate = parseFloat(document.getElementById('exchangeRate').value);

            document.getElementById('invoiceClientName').textContent = clientName;
            document.getElementById('invoiceCurrency').textContent = currency;

            const invoiceProductsTable = document.getElementById('invoiceProducts');
            invoiceProductsTable.innerHTML = '';

            let subtotal = 0;
            products.forEach(product => {
                const productSubtotal = product.quantity * product.unitPrice;
                subtotal += productSubtotal;

                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${product.name}</td>
                    <td>${product.quantity}</td>
                    <td>${currency}${product.unitPrice.toFixed(2)}</td>
                    <td>${currency}${productSubtotal.toFixed(2)}</td>
                `;
                invoiceProductsTable.appendChild(row);
            });

            document.getElementById('subtotal').textContent = `${currency}${subtotal.toFixed(2)}`;
            document.getElementById('invoiceDiscount').textContent = `${currency}${discount.toFixed(2)}`;

            const finalTotal = subtotal - discount - paymentMade;
            document.getElementById('invoicePaymentMade').textContent = `${currency}${paymentMade.toFixed(2)}`;
            document.getElementById('finalTotal').textContent = `${currency}${finalTotal.toFixed(2)}`;

            // Calcular equivalente en $
            const equivalent = currency === "Bs.D" 
                ? finalTotal / exchangeRate // Bolívares a Dólares
                : finalTotal; // Ya está en Dólares
            document.getElementById('invoiceEquivalent').textContent = `$${equivalent.toFixed(2)}`;

            // Toggle discount visibility
            const discountRow = document.getElementById('discountRow');
            if (hideDiscount) {
                discountRow.classList.add('hidden');
            } else {
                discountRow.classList.remove('hidden');
            }
        }

        function generateInvoice() {
            const logoUpload = document.getElementById('logoUpload');
            const invoiceLogo = document.getElementById('invoiceLogo');
            const pdfLogo = document.getElementById('pdfLogo');

            if (logoUpload.files.length > 0) {
                const file = logoUpload.files[0];
                const reader = new FileReader();
                reader.onload = function(e) {
                    invoiceLogo.src = e.target.result;
                    pdfLogo.src = e.target.result;
                };
                reader.readAsDataURL(file);
            }

            const paymentProofUpload = document.getElementById('paymentProofUpload');
            const paymentProofImage = document.getElementById('paymentProofImage');

            if (paymentProofUpload.files.length > 0) {
                const file = paymentProofUpload.files[0];
                const reader = new FileReader();
                reader.onload = function(e) {
                    paymentProofImage.src = e.target.result;
                };
                reader.readAsDataURL(file);
            }

            updateInvoice();
        }

        function downloadPDF() {
            const clientName = document.getElementById('clientName').value;
            const discount = parseFloat(document.getElementById('discount').value);
            const paymentMade = parseFloat(document.getElementById('paymentMade').value);
            const hideDiscount = document.getElementById('hideDiscount').checked;
            const currency = document.getElementById('currency').value;
            const exchangeRate = parseFloat(document.getElementById('exchangeRate').value);

            document.getElementById('pdfClientName').textContent = clientName;
            document.getElementById('pdfCurrency').textContent = currency;

            const pdfProductsTable = document.getElementById('pdfProducts');
            pdfProductsTable.innerHTML = '';

            let subtotal = 0;
            products.forEach(product => {
                const productSubtotal = product.quantity * product.unitPrice;
                subtotal += productSubtotal;

                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${product.name}</td>
                    <td>${product.quantity}</td>
                    <td>${currency}${product.unitPrice.toFixed(2)}</td>
                    <td>${currency}${productSubtotal.toFixed(2)}</td>
                `;
                pdfProductsTable.appendChild(row);
            });

            document.getElementById('pdfSubtotal').textContent = `${currency}${subtotal.toFixed(2)}`;
            document.getElementById('pdfDiscount').textContent = `${currency}${discount.toFixed(2)}`;
            document.getElementById('pdfPaymentMade').textContent = `${currency}${paymentMade.toFixed(2)}`;
            const finalTotal = subtotal - discount - paymentMade;
            document.getElementById('pdfFinalTotal').textContent = `${currency}${finalTotal.toFixed(2)}`;

            // Calcular equivalente en $
            const equivalent = currency === "Bs.D" 
                ? finalTotal / exchangeRate // Bolívares a Dólares
                : finalTotal; // Ya está en Dólares
            document.getElementById('pdfEquivalent').textContent = `$${equivalent.toFixed(2)}`;

            // Toggle discount visibility in PDF
            const pdfDiscountRow = document.getElementById('pdfDiscountRow');
            if (hideDiscount) {
                pdfDiscountRow.classList.add('hidden');
            } else {
                pdfDiscountRow.classList.remove('hidden');
            }

            // Generate PDF
            const pdfContainer = document.getElementById('pdfContainer');
            html2pdf()
                .from(pdfContainer)
                .save('factura_zitrostyle.pdf');
        }
    </script>
</body>
</html>
