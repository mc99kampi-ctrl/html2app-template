<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Panel MvReseller</title>
    <!-- Carga de Tailwind CSS para estilos -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Configuración de la fuente Inter -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap');
        :root {
            /* Colores Base Claro */
            --bg-primary: #f3f4f6;
            --bg-secondary: #ffffff;
            --text-primary: #1f2937;
            --text-secondary: #6b7280;
            --border-color: #e5e7eb;
            --shadow-color: rgba(0, 0, 0, 0.1);
        }
        .dark {
            /* Colores Base Oscuro */
            --bg-primary: #1f2937;
            --bg-secondary: #374151;
            --text-primary: #f9fafb;
            --text-secondary: #9ca3af;
            --border-color: #4b5563;
            --shadow-color: rgba(255, 255, 255, 0.05);
        }
        body { 
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-primary);
            color: var(--text-primary);
        }
        .app-container {
            transition: background-color 0.3s, color 0.3s;
        }
        .app-card {
            background-color: var(--bg-secondary);
            border-color: var(--border-color);
            box-shadow: 0 10px 15px -3px var(--shadow-color);
            transition: background-color 0.3s, border-color 0.3s, box-shadow 0.3s;
        }
        .app-input {
            background-color: var(--bg-secondary);
            border-color: var(--border-color);
            color: var(--text-primary);
        }
        
        /* Estilos para el toggle general */
        .toggle-container {
            position: relative;
            display: inline-block;
            width: 48px;
            height: 24px;
        }
        .toggle-container input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 24px;
        }
        .dark .slider {
            background-color: #4b5563;
        }
        .slider:before {
            position: absolute;
            content: "";
            height: 16px;
            width: 16px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        input:checked + .slider {
            background-color: #2563eb;
        }
        input:checked + .slider:before {
            transform: translateX(24px);
        }

        /* Estilos específicos de Pestañas y Elementos */
        .tab-button {
            color: var(--text-secondary);
            border-bottom: 2px solid transparent;
            transition: color 0.15s, border-color 0.15s, background-color 0.15s;
        }
        .tab-button.active {
            border-color: #3b82f6;
            color: #1d4ed8;
            background-color: var(--bg-primary);
        }
        .dark .tab-button {
             color: #d1d5db; /* Texto claro */
        }
        .dark .tab-button.active {
            border-color: #60a5fa;
            color: #93c5fd;
            background-color: #374151;
        }

        /* Modal Styles */
        #settings-modal {
            background-color: rgba(0, 0, 0, 0.5);
        }
        .dark #settings-modal-content {
            background-color: #374151;
        }
        #settings-modal-content {
            background-color: #ffffff;
        }
        
    </style>
</head>
<body class="app-container">

    <!-- Contenedor Principal -->
    <div id="app" class="max-w-7xl mx-auto p-4 sm:p-8">
        
        <!-- Cabecera y Logo -->
        <header class="py-6 app-card rounded-xl shadow-lg mb-6 flex justify-between items-center px-4 sm:px-8">
            <div class="text-left">
                <h1 class="text-3xl sm:text-4xl font-black text-blue-600 tracking-wider">Mv<span class="text-gray-600 dark:text-gray-100 font-extrabold">Reseller</span></h1>
                <p id="auth-status" class="text-xs sm:text-sm text-gray-500 dark:text-gray-400 mt-1">
                    <!-- Se llenará con el ID de Usuario después de la autenticación -->
                </p>
            </div>
            
            <!-- Botón de Ajustes -->
            <button onclick="openSettingsModal()" class="p-3 rounded-full text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700 transition duration-150">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.59.36 1.25.46 1.9.36zm1.47-4.47a3 3 0 11-4 4 3 3 0 014-4z"></path></svg>
            </button>
        </header>

        <!-- Formulario de Inicio de Sesión (Se mostrará primero) -->
        <div id="login-container" class="max-w-md mx-auto app-card p-8 rounded-xl shadow-2xl transition duration-300">
            <h2 class="text-2xl font-bold mb-6 text-gray-800 dark:text-gray-100 border-b dark:border-gray-600 pb-2">Acceso de Administrador</h2>
            <div class="mb-4">
                <label for="password" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Contraseña</label>
                <input type="password" id="password" placeholder="ADMIN123" class="app-input w-full p-3 border rounded-lg focus:ring-blue-500 focus:border-blue-500 transition duration-150">
            </div>
            <button onclick="login()" class="w-full bg-blue-600 text-white font-semibold py-3 rounded-lg hover:bg-blue-700 transition duration-300 shadow-md">
                Ingresar al Panel
            </button>
            <p id="login-error" class="text-red-500 text-sm mt-4 text-center hidden">Contraseña incorrecta. Inténtalo de nuevo.</p>
        </div>
        
        <!-- Panel Principal (Oculto hasta iniciar sesión) -->
        <div id="admin-panel" class="hidden">
            
            <!-- Selector de Pestañas (Tabs) -->
            <div class="flex border-b border-gray-200 dark:border-gray-700 app-card p-2 rounded-xl shadow-md mb-6">
                <button id="tab-clients-btn" onclick="showTab('clients')" class="tab-button active flex-1 py-3 px-4 text-sm font-semibold text-center rounded-lg border-b-2 border-transparent transition duration-150">
                    <svg class="w-5 h-5 inline mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20h-2m2 0h-2M2 20h5v-2a3 3 0 015.356-1.857M2 20h-2m2 0h-2m2-9a4 4 0 118 0 4 4 0 01-8 0z"></path></svg>
                    Clientes
                </button>
                <button id="tab-messages-btn" onclick="showTab('messages')" class="tab-button flex-1 py-3 px-4 text-sm font-semibold text-center rounded-lg border-b-2 border-transparent transition duration-150">
                    <svg class="w-5 h-5 inline mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 8h10M7 12h10M7 16h10M4 21h16a2 2 0 002-2V5a2 2 0 00-2-2H4a2 2 0 00-2 2v14a2 2 0 002 2z"></path></svg>
                    Mensajes
                </button>
                <button id="tab-finances-btn" onclick="showTab('finances')" class="tab-button flex-1 py-3 px-4 text-sm font-semibold text-center rounded-lg border-b-2 border-transparent transition duration-150">
                    <svg class="w-5 h-5 inline mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 2v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                    Finanzas
                </button>
            </div>

            <!-- Contenido de la Pestaña de Clientes -->
            <div id="content-clients" class="tab-content">
                
                <!-- Alerta de Vencimiento de 2 Días -->
                <div id="two-day-alert" class="hidden bg-red-100 border-l-4 border-red-500 text-red-700 p-4 rounded-lg mb-6 dark:bg-red-900 dark:text-red-300" role="alert">
                    <p class="font-bold">¡Alerta de Vencimiento Urgente!</p>
                    <p id="alert-message">
                        <!-- Mensaje dinámico de clientes por vencer en 2 días -->
                    </p>
                </div>

                <!-- Formulario para Agregar/Editar Cliente -->
                <div class="app-card p-6 sm:p-8 rounded-xl shadow-2xl mb-8">
                    <h2 id="form-title" class="text-2xl font-bold mb-4 text-gray-800 dark:text-gray-100">Agregar Nuevo Cliente</h2>
                    
                    <form id="client-form" onsubmit="event.preventDefault(); saveClient()">
                        <input type="hidden" id="client-id">
                        
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4">
                            <div>
                                <label for="client-name" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Nombre del Cliente</label>
                                <input type="text" id="client-name" required placeholder="Ej: Juan Pérez" class="app-input w-full p-3 border rounded-lg">
                            </div>
                            <div>
                                <label for="client-service" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Nombre del Servicio Contratado</label>
                                <input type="text" id="client-service" required placeholder="Ej: Plan Ultra 10GB" class="app-input w-full p-3 border rounded-lg">
                            </div>
                            <div>
                                <label for="client-price" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Precio del Servicio (MXN/USD)</label>
                                <input type="number" id="client-price" required min="0" step="0.01" placeholder="Ej: 199.50" class="app-input w-full p-3 border rounded-lg">
                            </div>
                            <div>
                                <label for="client-phone" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Teléfono (código de país sin +)</label>
                                <input type="tel" id="client-phone" required pattern="[0-9]+" title="Solo números" placeholder="Ej: 521234567890" class="app-input w-full p-3 border rounded-lg">
                            </div>
                            <div class="md:col-span-2">
                                <label for="client-expiration" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Fecha de Vencimiento</label>
                                <input type="date" id="client-expiration" required class="app-input w-full p-3 border rounded-lg">
                            </div>
                        </div>
                        
                        <div class="flex flex-col sm:flex-row gap-4">
                            <button type="submit" class="flex-grow bg-green-500 text-white font-semibold py-3 px-6 rounded-lg hover:bg-green-600 transition duration-200 shadow-md">
                                Guardar Cliente
                            </button>
                            <button type="button" onclick="clearForm()" class="flex-grow bg-gray-300 text-gray-800 font-semibold py-3 px-6 rounded-lg hover:bg-gray-400 transition duration-200 shadow-md dark:bg-gray-600 dark:text-gray-100 dark:hover:bg-gray-700">
                                Cancelar / Nuevo
                            </button>
                        </div>
                    </form>
                </div>

                <!-- Tabla de Clientes -->
                <div class="app-card p-6 sm:p-8 rounded-xl shadow-2xl overflow-x-auto">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="text-2xl font-bold text-gray-800 dark:text-gray-100">Listado de Clientes</h2>
                        
                        <div class="flex items-center space-x-2">
                            <label for="hide-expired" class="text-sm font-medium text-gray-700 dark:text-gray-100">Ocultar Vencidos</label>
                            <label class="toggle-container">
                                <input type="checkbox" id="hide-expired" onchange="renderClients(window.clientsData || [])">
                                <span class="slider"></span>
                            </label>
                        </div>
                    </div>
                    
                    <table class="min-w-full divide-y divide-gray-200 dark:divide-gray-600">
                        <thead class="bg-gray-50 dark:bg-gray-700">
                            <tr>
                                <th class="px-2 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Cliente / Servicio</th>
                                <th class="px-2 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider hidden sm:table-cell">Precio</th>
                                <th class="px-2 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Vencimiento</th>
                                <th class="px-2 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Días Restantes</th>
                                <th class="px-2 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-300 uppercase tracking-wider">Acciones</th>
                            </tr>
                        </thead>
                        <tbody id="clients-table-body" class="bg-white dark:bg-gray-800 divide-y divide-gray-200 dark:divide-gray-700">
                            <tr><td colspan="5" class="p-4 text-center text-gray-500 dark:text-gray-400">Cargando clientes...</td></tr>
                        </tbody>
                    </table>
                </div>

            </div>

            <!-- Contenido de la Pestaña de Mensajes -->
            <div id="content-messages" class="tab-content hidden max-w-4xl mx-auto">
                <div class="app-card p-6 sm:p-8 rounded-xl shadow-2xl mb-8">
                    <h2 class="text-2xl font-bold mb-6 text-gray-800 dark:text-gray-100 border-b dark:border-gray-600 pb-2">Crear Plantillas de WhatsApp</h2>
                    
                    <form id="template-form" onsubmit="event.preventDefault(); saveTemplate()">
                        <input type="hidden" id="template-id">
                        
                        <div class="mb-4">
                            <label for="template-title" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Título de la Plantilla (Ej: Oferta Navideña)</label>
                            <input type="text" id="template-title" required placeholder="Título para identificar el mensaje" class="app-input w-full p-3 border rounded-lg">
                        </div>
                        <div class="mb-4">
                            <label for="template-type" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">Tipo de Mensaje (Solo los 'Default' se usan en la tabla)</label>
                            <select id="template-type" required class="app-input w-full p-3 border rounded-lg">
                                <option value="custom">Mensaje Personalizado (Oferta)</option>
                                <option value="default_2_day">Recordatorio (Default 2 Días Antes)</option>
                                <option value="default_expired">Vencimiento (Default Vencido)</option>
                            </select>
                        </div>
                        <div class="mb-4">
                            <label for="template-image-url" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">URL de Imagen (Opcional, para adjuntar una imagen)</label>
                            <input type="url" id="template-image-url" placeholder="Ej: https://tudominio.com/oferta.jpg" class="app-input w-full p-3 border rounded-lg">
                        </div>
                        <div class="mb-4">
                            <label for="template-content" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">
                                Contenido del Mensaje
                                <span class="text-xs text-gray-500 italic ml-2">Puedes usar {nombre} y {fecha} para personalización.</span>
                            </label>
                            <textarea id="template-content" rows="6" required placeholder="Ej: Hola {nombre}, te recordamos que tu servicio vence el {fecha}..." class="app-input w-full p-3 border rounded-lg"></textarea>
                        </div>
                        
                        <div class="flex flex-col sm:flex-row gap-4">
                            <button type="submit" class="flex-grow bg-blue-600 text-white font-semibold py-3 px-6 rounded-lg hover:bg-blue-700 transition duration-200 shadow-md">
                                Guardar Plantilla
                            </button>
                            <button type="button" onclick="clearTemplateForm()" class="flex-grow bg-gray-300 text-gray-800 font-semibold py-3 px-6 rounded-lg hover:bg-gray-400 transition duration-200 shadow-md dark:bg-gray-600 dark:text-gray-100 dark:hover:bg-gray-700">
                                Limpiar Formulario
                            </button>
                        </div>
                    </form>
                </div>
                
                <!-- Lista de Plantillas -->
                <div class="app-card p-6 sm:p-8 rounded-xl shadow-2xl">
                    <h3 class="text-xl font-bold mb-4 text-gray-800 dark:text-gray-100">Plantillas Guardadas</h3>
                    <div id="templates-list" class="divide-y divide-gray-200 dark:divide-gray-600">
                        <!-- Plantillas se cargarán aquí -->
                    </div>
                </div>
            </div>

            <!-- Contenido de la Pestaña de Finanzas -->
            <div id="content-finances" class="tab-content hidden max-w-4xl mx-auto">
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                    <!-- Resumen de Finanzas -->
                    <div class="app-card p-6 rounded-xl shadow-lg border-l-4 border-green-500">
                        <p class="text-sm font-medium text-gray-500 dark:text-gray-400">Ingresos Totales (Ventas)</p>
                        <p id="total-income" class="text-3xl font-bold text-green-600 mt-1">$0.00</p>
                    </div>
                    <div class="app-card p-6 rounded-xl shadow-lg border-l-4 border-red-500">
                   ra el HTML para la lista de plantillas.
         */
        function renderTemplates(templates) {
            const list = document.getElementById('templates-list');
            list.innerHTML = '';

            if (templates.length === 0) {
                list.innerHTML = '<p class="text-center text-gray-500 dark:text-gray-400 italic">Aún no hay plantillas guardadas.</p>';
                return;
            }

            templates.forEach(template => {
                list.innerHTML += `
                    <div class="py-4 px-2 hover:bg-gray-50 dark:hover:bg-gray-700 rounded-lg">
                        <div class="flex justify-between items-start">
                            <h4 class="font-bold text-gray-900 dark:text-gray-100">${template.title} (${template.type})</h4>
                            <div class="flex space-x-2">
                                <button onclick="editTemplate(${JSON.stringify(template).replace(/"/g, '&quot;')})" class="text-blue-600 hover:text-blue-900 text-xs font-bold dark:text-blue-400">Editar</button>
                                <button onclick="deleteTemplate('${template.id}')" class="text-red-600 hover:text-red-900 text-xs font-bold dark:text-red-400">Eliminar</button>
                            </div>
                        </div>
                        <p class="mt-1 text-sm text-gray-600 dark:text-gray-300 whitespace-pre-wrap">${template.content}</p>
                        ${template.imageUrl ? `<a href="${template.imageUrl}" target="_blank" class="text-xs text-indigo-500 block mt-1 hover:underline">Ver Imagen</a>` : ''}
                    </div>
                `;
            });
        }
        
        window.editTemplate = function(template) {
            document.getElementById('template-id').value = template.id;
            document.getElementById('template-title').value = template.title;
            document.getElementById('template-type').value = template.type;
            document.getElementById('template-content').value = template.content;
            document.getElementById('template-image-url').value = template.imageUrl || '';
            document.querySelector('#content-messages h2').textContent = 'Editar Plantilla';
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        window.clearTemplateForm = function() {
            document.getElementById('template-form').reset();
            document.getElementById('template-id').value = '';
            document.querySelector('#content-messages h2').textContent = 'Crear Plantillas de WhatsApp';
        }

        window.deleteTemplate = async function(id) {
            const path = getCollectionPath('message_templates');
            if (!path) return;

            if (window.confirm('¿Estás seguro de que quieres eliminar esta plantilla?')) { 
                try {
                    await deleteDoc(doc(window.db, path, id));
                    console.log("Plantilla eliminada:", id);
                } catch (e) {
                    console.error("Error al eliminar plantilla:", e);
                }
            }
        }

        /**
         * Genera un enlace de WhatsApp usando la plantilla por defecto.
         */
        window.sendWhatsAppTemplate = function(phone, name, expirationDate, type) {
            const template = window.templatesData.find(t => t.type === type);
            let message = '';

            if (template) {
                // Usar plantilla guardada y reemplazar placeholders
                message = template.content.replace(/{nombre}/g, name).replace(/{fecha}/g, expirationDate);
            } else {
                // Usar mensaje por defecto si no hay plantilla guardada
                if (type === 'default_2_day') {
                    message = `Hola ${name}, te recordamos que tu servicio vence en 2 días (${expirationDate}). Por favor, contáctanos para renovar o comprar otro de nuestros servicios. ¡Estamos a tus órdenes!`;
                } else if (type === 'default_expired') {
                    message = `Hola ${name}, tu servicio ha vencido. Puedes renovarlo o comprar algún otro de nuestros servicios. ¡Estamos a tus órdenes!`;
                } else {
                    message = `Hola ${name}, te contacto sobre tu servicio que vence el ${expirationDate}.`;
                }
            }

            const encodedMessage = encodeURIComponent(message);
            const whatsappLink = `https://wa.me/${phone}?text=${encodedMessage}`;
            window.open(whatsappLink, '_blank');
        }

        // --- CRUD y Lógica de Finanzas ---

        /**
         * Guarda una nueva transacción (Ingreso/Gasto).
         */
        window.saveTransaction = async function() {
            const description = document.getElementById('transaction-description').value.trim();
            const amount = parseFloat(document.getElementById('transaction-amount').value);
            const type = document.getElementById('transaction-type').value;

            if (!description || isNaN(amount) || amount <= 0) {
                console.error("Descripción y Monto válido son obligatorios.");
                return;
            }

            const path = getCollectionPath('transactions');
            if (!path) return;

            const transactionData = {
                description: description,
                amount: amount,
                type: type, // 'income' o 'expense'
                createdAt: serverTimestamp()
            };

            try {
                await addDoc(collection(window.db, path), transactionData);
                document.getElementById('transaction-form').reset();
                console.log("Transacción registrada.");
            } catch (e) {
                console.error("Error al registrar transacción:", e);
            }
        }

        /**
         * Carga las transacciones y actualiza el resumen financiero.
         */
        function loadTransactions() {
            const path = getCollectionPath('transactions');
            if (!path || !window.db || !window.userId) {
                setTimeout(loadTransactions, 1000); 
                return;
            }

            const q = query(collection(window.db, path));
            
            onSnapshot(q, (snapshot) => {
                window.transactionsData = [];
                let totalIncome = 0;
                let totalExpense = 0;
                
                snapshot.forEach((doc) => {
                    const data = { id: doc.id, ...doc.data() };
                    window.transactionsData.push(data);
                    
                    if (data.type === 'income') {
                        totalIncome += data.amount;
                    } else if (data.type === 'expense') {
                        totalExpense += data.amount;
                    }
                });

                // Actualizar Resumen (Fórmula: Ingreso Total - Gasto Total)
                const netGain = totalIncome - totalExpense;
                
                // Mostrar resultados
                document.getElementById('total-income').textContent = `$${totalIncome.toFixed(2)}`;
                document.getElementById('total-expense').textContent = `$${totalExpense.toFixed(2)}`;
                
                // Aplicar estilo de color a la Ganancia Neta
                const netGainElement = document.getElementById('net-gain');
                netGainElement.textContent = `$${Math.abs(netGain).toFixed(2)}`;
                netGainElement.classList.remove('text-blue-600', 'text-red-600');
                if (netGain >= 0) {
                    netGainElement.classList.add('text-blue-600');
                    // Mostrar el signo + si es positivo, o nada si es cero
                    netGainElement.textContent = `+ $${netGain.toFixed(2)}`;
                } else {
                    netGainElement.classList.add('text-red-600');
                    netGainElement.textContent = `- $${Math.abs(netGain).toFixed(2)}`;
                }


                renderTransactions(window.transactionsData);
            }, (error) => {
                console.error("Error al escuchar transacciones:", error);
            });
        }
        
        /**
         * Genera el HTML para el historial de transacciones.
         */
        function renderTransactions(transactions) {
            const history = document.getElementById('transactions-history');
            history.innerHTML = '';
            
            // Ordenar por fecha descendente
            transactions.sort((a, b) => (b.createdAt?.toMillis() || 0) - (a.createdAt?.toMillis() || 0));

            if (transactions.length === 0) {
                history.innerHTML = '<p class="text-center text-gray-500 dark:text-gray-400 italic">Aún no hay transacciones registradas.</p>';
                return;
            }

            transactions.forEach(tx => {
                const date = tx.createdAt ? new Date(tx.createdAt.seconds * 1000).toLocaleDateString('es-ES') : 'N/A';
                const color = tx.type === 'income' ? 'text-green-600' : 'text-red-600';
                const sign = tx.type === 'income' ? '+' : '-';

                history.innerHTML += `
                    <div class="flex justify-between items-center py-3 border-b dark:border-gray-700 last:border-b-0">
                        <div class="flex-1">
                            <p class="font-medium text-gray-900 dark:text-gray-100">${tx.description}</p>
                            <p class="text-xs text-gray-500 dark:text-gray-400">${date}</p>
                        </div>
                        <div class="text-right">
                            <p class="font-bold ${color}">${sign} $${tx.amount.toFixed(2)}</p>
                        </div>
                    </div>
                `;
            });
        }

        // Iniciar la aplicación de Firebase al cargar la ventana
        window.onload = initFirebase;
    </script>
</bod modifying `AndroidManifest.xml`.

## www/

The www/ folder needs at least a `index.html` file.

## assets/

```
assets/
├── icon-background.png   432x432
├── icon-foreground.png   432x432
├── icon.png              1024x1024
└── splash.png            2732x2732
```
