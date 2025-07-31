<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Rifa Automatizado</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
        }
        
        .ticket-animation {
            animation: ticketPulse 2s ease-in-out infinite;
        }
        
        @keyframes ticketPulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        
        .number-reveal {
            animation: numberReveal 0.5s ease-out;
        }
        
        @keyframes numberReveal {
            0% { opacity: 0; transform: translateY(-20px); }
            100% { opacity: 1; transform: translateY(0); }
        }
        
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        
        .payment-card {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
        }
    </style>
</head>
<body class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="gradient-bg text-white py-8">
        <div class="container mx-auto px-4 text-center">
            <h1 class="text-4xl md:text-5xl font-bold mb-4">🎟️ Rifa Automatizada</h1>
            <p class="text-xl opacity-90">Compra tu boleto y participa por increíbles premios</p>
        </div>
    </header>

    <div class="container mx-auto px-4 py-8">
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
            <!-- Formulario de Compra -->
            <div class="bg-white rounded-2xl shadow-xl p-8">
                <h2 class="text-3xl font-bold text-gray-800 mb-6 text-center">Comprar Boleto</h2>
                
                <form id="raffleForm" class="space-y-6">
                    <!-- Información Personal -->
                    <div class="space-y-4">
                        <h3 class="text-xl font-semibold text-gray-700 border-b pb-2">Información Personal</h3>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Nombre</label>
                                <input type="text" id="nombre" required 
                                       class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all">
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Apellido</label>
                                <input type="text" id="apellido" required 
                                       class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all">
                            </div>
                        </div>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Cédula de Identidad</label>
                            <input type="text" id="cedula" required placeholder="V-12345678"
                                   class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all">
                        </div>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Teléfono</label>
                            <input type="tel" id="telefono" required placeholder="0414-1234567"
                                   class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all">
                        </div>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Correo Electrónico</label>
                            <input type="email" id="email" required 
                                   class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all">
                        </div>
                    </div>

                    <!-- Selección de Boletos -->
                    <div class="space-y-4">
                        <h3 class="text-xl font-semibold text-gray-700 border-b pb-2">Selección de Boletos</h3>
                        
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-2">Cantidad de Boletos</label>
                            <select id="cantidadBoletos" onchange="updateTotal()" 
                                    class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all">
                                <option value="1">1 Boleto - $5.00</option>
                                <option value="2">2 Boletos - $10.00</option>
                                <option value="3">3 Boletos - $15.00</option>
                                <option value="5">5 Boletos - $25.00</option>
                                <option value="10">10 Boletos - $50.00</option>
                            </select>
                        </div>
                        
                        <div class="bg-blue-50 p-4 rounded-lg">
                            <div class="flex justify-between items-center">
                                <span class="text-lg font-semibold text-gray-700">Total a Pagar:</span>
                                <span id="totalAmount" class="text-2xl font-bold text-blue-600">$5.00</span>
                            </div>
                        </div>
                    </div>

                    <button type="submit" 
                            class="w-full bg-gradient-to-r from-blue-500 to-purple-600 hover:from-blue-600 hover:to-purple-700 text-white font-bold py-4 px-8 rounded-lg transition-all duration-300 transform hover:scale-105 shadow-lg">
                        🎲 Generar Números de Rifa
                    </button>
                </form>
            </div>

            <!-- Información de Pago y Resultados -->
            <div class="space-y-8">
                <!-- Información de Pago Móvil -->
                <div class="payment-card text-white rounded-2xl shadow-xl p-8">
                    <h2 class="text-3xl font-bold mb-6 text-center">💳 Información de Pago</h2>
                    
                    <div class="space-y-4">
                        <div class="bg-white/20 backdrop-blur-sm rounded-lg p-4">
                            <h3 class="text-lg font-semibold mb-2">Pago Móvil</h3>
                            <div class="space-y-2 text-sm">
                                <p><strong>Banco:</strong> Banco de Venezuela</p>
                                <p><strong>Teléfono:</strong> 0414-1234567</p>
                                <p><strong>Cédula:</strong> V-12345678</p>
                                <p><strong>Titular:</strong> María González</p>
                            </div>
                        </div>
                        
                        <div class="bg-white/20 backdrop-blur-sm rounded-lg p-4">
                            <h3 class="text-lg font-semibold mb-2">Transferencia Bancaria</h3>
                            <div class="space-y-2 text-sm">
                                <p><strong>Banco:</strong> Banesco</p>
                                <p><strong>Cuenta:</strong> 0134-0123-45-1234567890</p>
                                <p><strong>Titular:</strong> María González</p>
                                <p><strong>Cédula:</strong> V-12345678</p>
                            </div>
                        </div>
                        
                        <div class="bg-yellow-400/20 backdrop-blur-sm rounded-lg p-4 border border-yellow-300">
                            <p class="text-sm"><strong>⚠️ Importante:</strong> Después del pago, envía el comprobante por WhatsApp al <strong>+58 414-1234567</strong> junto con tus números de rifa.</p>
                        </div>
                    </div>
                </div>

                <!-- Resultados de la Rifa -->
                <div id="resultadosSection" class="bg-white rounded-2xl shadow-xl p-8 hidden">
                    <h2 class="text-3xl font-bold text-gray-800 mb-6 text-center">🎉 ¡Tus Números de Rifa!</h2>
                    
                    <div id="participantInfo" class="bg-gray-50 rounded-lg p-4 mb-6">
                        <!-- Información del participante se llenará aquí -->
                    </div>
                    
                    <div id="numerosGenerados" class="grid grid-cols-2 md:grid-cols-3 gap-4 mb-6">
                        <!-- Los números se generarán aquí -->
                    </div>
                    
                    <div class="text-center">
                        <p class="text-lg text-gray-600 mb-4">¡Guarda estos números! Son tu comprobante de participación.</p>
                        <button onclick="downloadTicket()" 
                                class="bg-green-500 hover:bg-green-600 text-white font-bold py-3 px-6 rounded-lg transition-all duration-300">
                            📄 Descargar Comprobante
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Lista de Participantes -->
        <div class="mt-12 bg-white rounded-2xl shadow-xl p-8">
            <h2 class="text-3xl font-bold text-gray-800 mb-6 text-center">👥 Participantes Registrados</h2>
            <div id="listaParticipantes" class="space-y-4">
                <p class="text-gray-500 text-center">No hay participantes registrados aún.</p>
            </div>
        </div>
    </div>

    <script>
        let participantes = [];
        let numerosUsados = new Set();

        function updateTotal() {
            const cantidad = document.getElementById('cantidadBoletos').value;
            const total = cantidad * 5;
            document.getElementById('totalAmount').textContent = `$${total.toFixed(2)}`;
        }

        function generarNumeroAleatorio() {
            let numero;
            do {
                numero = Math.floor(Math.random() * 10000) + 1;
            } while (numerosUsados.has(numero));
            
            numerosUsados.add(numero);
            return numero.toString().padStart(4, '0');
        }

        function mostrarResultados(participante, numeros) {
            const resultadosSection = document.getElementById('resultadosSection');
            const participantInfo = document.getElementById('participantInfo');
            const numerosGenerados = document.getElementById('numerosGenerados');

            // Mostrar información del participante
            participantInfo.innerHTML = `
                <h3 class="text-lg font-semibold text-gray-800 mb-2">Información del Participante</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm text-gray-600">
                    <p><strong>Nombre:</strong> ${participante.nombre} ${participante.apellido}</p>
                    <p><strong>Cédula:</strong> ${participante.cedula}</p>
                    <p><strong>Teléfono:</strong> ${participante.telefono}</p>
                    <p><strong>Email:</strong> ${participante.email}</p>
                    <p><strong>Boletos:</strong> ${participante.cantidadBoletos}</p>
                    <p><strong>Total:</strong> $${(participante.cantidadBoletos * 5).toFixed(2)}</p>
                </div>
            `;

            // Mostrar números generados
            numerosGenerados.innerHTML = '';
            numeros.forEach((numero, index) => {
                setTimeout(() => {
                    const numeroDiv = document.createElement('div');
                    numeroDiv.className = 'ticket-animation number-reveal bg-gradient-to-r from-purple-500 to-pink-500 text-white text-center py-4 px-6 rounded-lg font-bold text-xl shadow-lg';
                    numeroDiv.textContent = numero;
                    numerosGenerados.appendChild(numeroDiv);
                }, index * 200);
            });

            resultadosSection.classList.remove('hidden');
            resultadosSection.scrollIntoView({ behavior: 'smooth' });
        }

        function actualizarListaParticipantes() {
            const lista = document.getElementById('listaParticipantes');
            
            if (participantes.length === 0) {
                lista.innerHTML = '<p class="text-gray-500 text-center">No hay participantes registrados aún.</p>';
                return;
            }

            lista.innerHTML = participantes.map((p, index) => `
                <div class="bg-gray-50 rounded-lg p-4 border-l-4 border-blue-500">
                    <div class="flex justify-between items-start">
                        <div>
                            <h4 class="font-semibold text-gray-800">${p.nombre} ${p.apellido}</h4>
                            <p class="text-sm text-gray-600">Cédula: ${p.cedula} | Teléfono: ${p.telefono}</p>
                            <p class="text-sm text-gray-600">Boletos: ${p.cantidadBoletos} | Total: $${(p.cantidadBoletos * 5).toFixed(2)}</p>
                        </div>
                        <div class="text-right">
                            <p class="text-xs text-gray-500">Números:</p>
                            <div class="flex flex-wrap gap-1 mt-1">
                                ${p.numeros.map(num => `<span class="bg-blue-100 text-blue-800 text-xs px-2 py-1 rounded">${num}</span>`).join('')}
                            </div>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function downloadTicket() {
            const ultimoParticipante = participantes[participantes.length - 1];
            if (!ultimoParticipante) return;

            const contenido = `
COMPROBANTE DE PARTICIPACIÓN - RIFA AUTOMATIZADA
================================================

PARTICIPANTE:
Nombre: ${ultimoParticipante.nombre} ${ultimoParticipante.apellido}
Cédula: ${ultimoParticipante.cedula}
Teléfono: ${ultimoParticipante.telefono}
Email: ${ultimoParticipante.email}

BOLETOS COMPRADOS: ${ultimoParticipante.cantidadBoletos}
TOTAL PAGADO: $${(ultimoParticipante.cantidadBoletos * 5).toFixed(2)}

NÚMEROS ASIGNADOS:
${ultimoParticipante.numeros.join(' - ')}

FECHA: ${new Date().toLocaleString('es-ES')}

INSTRUCCIONES DE PAGO:
- Realiza el pago móvil o transferencia según los datos proporcionados
- Envía el comprobante por WhatsApp al +58 414-1234567
- Incluye este comprobante en tu mensaje

¡Buena suerte!
            `;

            const blob = new Blob([contenido], { type: 'text/plain' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `comprobante-rifa-${ultimoParticipante.cedula}.txt`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            window.URL.revokeObjectURL(url);
        }

        document.getElementById('raffleForm').addEventListener('submit', function(e) {
            e.preventDefault();

            // Recopilar datos del formulario
            const participante = {
                nombre: document.getElementById('nombre').value,
                apellido: document.getElementById('apellido').value,
                cedula: document.getElementById('cedula').value,
                telefono: document.getElementById('telefono').value,
                email: document.getElementById('email').value,
                cantidadBoletos: parseInt(document.getElementById('cantidadBoletos').value),
                fecha: new Date().toISOString()
            };

            // Generar números aleatorios
            const numeros = [];
            for (let i = 0; i < participante.cantidadBoletos; i++) {
                numeros.push(generarNumeroAleatorio());
            }
            participante.numeros = numeros;

            // Agregar a la lista de participantes
            participantes.push(participante);

            // Mostrar resultados
            mostrarResultados(participante, numeros);

            // Actualizar lista de participantes
            actualizarListaParticipantes();

            // Limpiar formulario
            this.reset();
            updateTotal();

            // Mostrar mensaje de éxito
            alert('¡Números generados exitosamente! Recuerda realizar el pago y enviar el comprobante.');
        });

        // Validación de cédula venezolana
        document.getElementById('cedula').addEventListener('input', function(e) {
            let value = e.target.value.toUpperCase();
            if (value && !value.match(/^[VE]-?\d/)) {
                if (value.match(/^\d/)) {
                    value = 'V-' + value;
                }
            }
            e.target.value = value;
        });

        // Validación de teléfono
        document.getElementById('telefono').addEventListener('input', function(e) {
            let value = e.target.value.replace(/\D/g, '');
            if (value.length >= 4) {
                value = value.substring(0, 4) + '-' + value.substring(4, 11);
            }
            e.target.value = value;
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'967d8ab8100f6c18',t:'MTc1Mzk2OTU4Ni4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
