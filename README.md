<!DOCTYPE html>
<html lang="es" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GlowSwitch - Planificador Académico v4.0</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        darkBg: '#050505',
                        cardBg: 'rgba(255, 255, 255, 0.03)',
                        cardBorder: 'rgba(255, 255, 255, 0.08)',
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #050505;
            color: #f3f4f6;
            overflow-x: hidden;
        }
        .glass-panel {
            background: rgba(14, 14, 14, 0.75);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 10px 40px 0 rgba(0, 0, 0, 0.5);
        }
        .glass-panel-sub {
            background: rgba(255, 255, 255, 0.025);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.06);
        }
        .glass-input {
            background: rgba(255, 255, 255, 0.04);
            border: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(8px);
        }
        .glass-input:focus {
            border-color: rgba(255, 255, 255, 0.5);
            outline: none;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.08);
        }
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #050505;
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.18);
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(255, 255, 255, 0.35);
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(6px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade {
            animation: fadeIn 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        }
        @keyframes glowPulse {
            0%, 100% { filter: drop-shadow(0 0 8px rgba(255, 255, 255, 0.6)); }
            50% { filter: drop-shadow(0 0 18px rgba(255, 255, 255, 0.9)); }
        }
        .logo-glow {
            animation: glowPulse 3s infinite ease-in-out;
        }
    </style>
</head>
<body class="min-h-screen text-slate-100 flex flex-col selection:bg-white selection:text-black">

    <header class="w-full border-b border-white/10 glass-panel sticky top-0 z-50 px-6 py-4 flex items-center justify-between">
        <div class="flex items-center space-x-3.5">
            <div class="w-11 h-11 rounded-2xl bg-gradient-to-br from-white via-neutral-200 to-neutral-500 text-black flex items-center justify-center font-black text-xl shadow-[0_0_30px_rgba(255,255,255,0.35)] relative overflow-hidden group">
                <div class="absolute inset-0 bg-white/20 opacity-0 group-hover:opacity-100 transition-opacity"></div>
                <i class="fa-solid fa-bolt text-black logo-glow text-lg"></i>
            </div>
            <div>
                <h1 class="font-black text-xl tracking-wider text-white flex items-center gap-2">
                    GLOWSWITCH <span class="text-[10px] px-2 py-0.5 rounded-full bg-white/10 text-white/80 font-semibold tracking-widest border border-white/15">v4.0</span>
                </h1>
                <p class="text-xs text-neutral-400 font-medium tracking-wide">Planificador Académico Inteligente</p>
            </div>
        </div>

        <div class="flex items-center space-x-3">
            <button onclick="openTaskModal()" class="px-4 py-2.5 bg-white text-black hover:bg-neutral-200 text-xs font-bold rounded-xl transition-all shadow-[0_0_20px_rgba(255,255,255,0.25)] flex items-center gap-2 active:scale-95">
                <i class="fa-solid fa-plus text-[10px]"></i> Nueva Tarea
            </button>
            <button onclick="openSubjectModal()" class="px-4 py-2.5 glass-panel-sub hover:bg-white/10 text-xs font-semibold rounded-xl transition-all border border-white/10 flex items-center gap-2 active:scale-95">
                <i class="fa-solid fa-book-bookmark text-[10px]"></i> Asignaturas y Horarios
            </button>
        </div>
    </header>

    <div class="flex-1 flex flex-col md:flex-row w-full max-w-[1650px] mx-auto p-4 md:p-6 gap-6">
        
        <aside class="w-full md:w-80 flex flex-col gap-6 shrink-0">
            <div class="glass-panel rounded-2xl p-3 flex flex-col gap-1.5">
                <button onclick="switchTab('calendar')" id="nav-calendar" class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-xs font-semibold tracking-wide transition-all bg-white text-black shadow-lg">
                    <i class="fa-regular fa-calendar-days w-5 text-center text-sm"></i> Calendario
                </button>
                <button onclick="switchTab('tasks')" id="nav-tasks" class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-xs font-semibold tracking-wide transition-all text-neutral-400 hover:text-white hover:bg-white/5">
                    <i class="fa-solid fa-list-check w-5 text-center text-sm"></i> Tareas Pendientes
                </button>
            </div>

            <div class="glass-panel rounded-2xl p-5 flex flex-col gap-4 flex-1">
                <div class="flex items-center justify-between border-b border-white/10 pb-3">
                    <h2 class="font-bold text-xs tracking-wider text-white uppercase flex items-center gap-2">
                        <i class="fa-solid fa-layer-group text-[10px]"></i> Asignaturas Activas
                    </h2>
                    <span id="subject-count" class="text-[11px] px-2.5 py-0.5 rounded-full bg-white/10 text-neutral-300 font-semibold">0</span>
                </div>
                
                <div id="sidebar-subjects-list" class="flex flex-col gap-2.5 overflow-y-auto max-h-[340px] pr-1"></div>

                <button onclick="openSubjectModal()" class="w-full py-2.5 border border-dashed border-white/20 rounded-xl text-xs font-medium text-neutral-400 hover:text-white hover:border-white/50 transition-all flex items-center justify-center gap-2">
                    <i class="fa-solid fa-gear text-[10px]"></i> Administrar Asignaturas
                </button>
            </div>

            <div class="glass-panel rounded-2xl p-5 flex flex-col gap-3">
                <h3 class="text-[11px] font-bold uppercase tracking-wider text-neutral-400">Resumen Académico</h3>
                <div class="grid grid-cols-2 gap-3">
                    <div class="glass-panel-sub p-3 rounded-xl flex flex-col">
                        <span class="text-2xl font-black text-white" id="stat-pending">0</span>
                        <span class="text-[11px] text-neutral-400 font-medium">Pendientes</span>
                    </div>
                    <div class="glass-panel-sub p-3 rounded-xl flex flex-col">
                        <span class="text-2xl font-black text-white" id="stat-completed">0</span>
                        <span class="text-[11px] text-neutral-400 font-medium">Completadas</span>
                    </div>
                </div>
            </div>
        </aside>

        <main class="flex-1 flex flex-col gap-6">
            
            <section id="view-calendar" class="flex flex-col gap-5 animate-fade">
                <div class="glass-panel rounded-2xl p-4 flex flex-wrap items-center justify-between gap-4">
                    <div class="flex items-center space-x-4">
                        <h2 id="calendar-month-year" class="text-xl font-bold tracking-wide text-white min-h-[28px]">Cargando...</h2>
                        <div class="flex items-center space-x-1 glass-panel-sub rounded-xl p-1 border border-white/10">
                            <button onclick="changeMonth(-1)" class="w-8 h-8 rounded-lg flex items-center justify-center hover:bg-white/10 text-neutral-300 transition-all">
                                <i class="fa-solid fa-chevron-left text-xs"></i>
                            </button>
                            <button onclick="goToCurrentMonth()" class="px-3 py-1 rounded-lg text-xs font-medium hover:bg-white/10 text-neutral-300 transition-all">
                                Hoy
                            </button>
                            <button onclick="changeMonth(1)" class="w-8 h-8 rounded-lg flex items-center justify-center hover:bg-white/10 text-neutral-300 transition-all">
                                <i class="fa-solid fa-chevron-right text-xs"></i>
                            </button>
                        </div>
                    </div>

                    <div class="flex items-center space-x-2">
                        <select id="calendar-filter-subject" onchange="renderCalendar()" class="glass-input text-xs rounded-xl px-3.5 py-2 text-neutral-200 outline-none cursor-pointer">
                            <option value="all" class="bg-neutral-900">Todas las asignaturas</option>
                        </select>
                    </div>
                </div>

                <div class="glass-panel rounded-2xl p-5 flex flex-col">
                    <div class="grid grid-cols-7 gap-2 mb-3 text-center text-xs font-bold text-neutral-400 uppercase tracking-wider">
                        <div>Lunes</div><div>Martes</div><div>Miércoles</div><div>Jueves</div><div>Viernes</div><div>Sábado</div><div>Domingo</div>
                    </div>
                    <div id="calendar-grid" class="grid grid-cols-7 gap-2 auto-rows-fr"></div>
                </div>
            </section>

            <section id="view-tasks" class="hidden flex flex-col gap-5 animate-fade">
                <div class="glass-panel rounded-2xl p-5 flex flex-wrap items-center justify-between gap-4">
                    <div>
                        <h2 class="text-xl font-bold tracking-wide text-white">Gestión de Tareas</h2>
                        <p class="text-xs text-neutral-400">Filtra, busca y administra todas tus asignaciones académicas</p>
                    </div>
                    <div class="flex flex-wrap items-center gap-3">
                        <div class="relative">
                            <i class="fa-solid fa-search absolute left-3.5 top-1/2 -translate-y-1/2 text-neutral-400 text-xs"></i>
                            <input type="text" id="task-search-input" oninput="renderTasksView()" placeholder="Buscar tarea..." class="glass-input text-xs rounded-xl pl-9 pr-4 py-2.5 text-neutral-200 w-48 sm:w-60">
                        </div>
                        <select id="task-filter-status" onchange="renderTasksView()" class="glass-input text-xs rounded-xl px-3.5 py-2.5 text-neutral-200 outline-none cursor-pointer">
                            <option value="all" class="bg-neutral-900">Todas las tareas</option>
                            <option value="pending" class="bg-neutral-900">Pendientes</option>
                            <option value="completed" class="bg-neutral-900">Completadas</option>
                        </select>
                        <select id="task-filter-subject" onchange="renderTasksView()" class="glass-input text-xs rounded-xl px-3.5 py-2.5 text-neutral-200 outline-none cursor-pointer">
                            <option value="all" class="bg-neutral-900">Todas las asignaturas</option>
                        </select>
                    </div>
                </div>

                <div class="glass-panel rounded-2xl p-5 flex flex-col gap-3 min-h-[400px]">
                    <div id="tasks-list-container" class="flex flex-col gap-3"></div>
                </div>
            </section>

        </main>
    </div>

    <div id="day-modal" class="fixed inset-0 z-50 bg-black/80 backdrop-blur-md hidden items-center justify-center p-4">
        <div class="glass-panel rounded-2xl w-full max-w-xl p-6 flex flex-col gap-5 border border-white/20 shadow-2xl animate-fade">
            <div class="flex items-center justify-between border-b border-white/10 pb-4">
                <div>
                    <h3 id="day-modal-title" class="text-lg font-bold text-white tracking-wide">Detalle del Día</h3>
                    <p id="day-modal-subtitle" class="text-xs text-neutral-400 mt-0.5">Asignaturas programadas y tareas para esta fecha</p>
                </div>
                <button onclick="closeDayModal()" class="w-8 h-8 rounded-full glass-panel-sub flex items-center justify-center text-neutral-400 hover:text-white transition-all">
                    <i class="fa-solid fa-xmark text-sm"></i>
                </button>
            </div>

            <div class="flex flex-col gap-4 max-h-[400px] overflow-y-auto pr-1">
                <div class="flex flex-col gap-2">
                    <h4 class="text-xs font-semibold text-neutral-400 uppercase tracking-wider">Asignaturas este día de la semana</h4>
                    <div id="day-modal-subjects" class="flex flex-col gap-2"></div>
                </div>

                <div class="flex flex-col gap-2 pt-2 border-t border-white/10">
                    <div class="flex items-center justify-between">
                        <h4 class="text-xs font-semibold text-neutral-400 uppercase tracking-wider">Tareas para esta fecha</h4>
                        <button onclick="openTaskModalForCurrentDay()" class="text-xs text-white hover:underline flex items-center gap-1 font-medium">
                            <i class="fa-solid fa-plus text-[10px]"></i> Añadir aquí
                        </button>
                    </div>
                    <div id="day-modal-tasks" class="flex flex-col gap-2"></div>
                </div>
            </div>

            <div class="flex items-center justify-end pt-2 border-t border-white/10">
                <button type="button" onclick="closeDayModal()" class="px-5 py-2.5 rounded-xl text-xs font-medium glass-panel-sub hover:bg-white/10 text-neutral-300 transition-all">
                    Cerrar
                </button>
            </div>
        </div>
    </div>

    <div id="task-modal" class="fixed inset-0 z-50 bg-black/80 backdrop-blur-md hidden items-center justify-center p-4">
        <div class="glass-panel rounded-2xl w-full max-w-lg p-6 flex flex-col gap-5 border border-white/20 shadow-2xl animate-fade">
            <div class="flex items-center justify-between border-b border-white/10 pb-4">
                <h3 id="task-modal-title" class="text-lg font-bold text-white tracking-wide">Nueva Tarea</h3>
                <button onclick="closeTaskModal()" class="w-8 h-8 rounded-full glass-panel-sub flex items-center justify-center text-neutral-400 hover:text-white transition-all">
                    <i class="fa-solid fa-xmark text-sm"></i>
                </button>
            </div>
            
            <form id="task-form" onsubmit="handleTaskSubmit(event)" class="flex flex-col gap-4">
                <input type="hidden" id="task-id">
                
                <div class="flex flex-col gap-1.5">
                    <label class="text-xs font-medium text-neutral-300">Título de la Tarea *</label>
                    <input type="text" id="task-title" required placeholder="Ej: Práctica de Cálculo Vectorial" class="glass-input rounded-xl px-4 py-2.5 text-sm text-white placeholder-neutral-500">
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div class="flex flex-col gap-1.5">
                        <label class="text-xs font-medium text-neutral-300">Asignatura *</label>
                        <select id="task-subject" required class="glass-input rounded-xl px-4 py-2.5 text-sm text-white outline-none cursor-pointer"></select>
                    </div>
                    <div class="flex flex-col gap-1.5">
                        <label class="text-xs font-medium text-neutral-300">Fecha de Vencimiento *</label>
                        <input type="date" id="task-date" required class="glass-input rounded-xl px-4 py-2.5 text-sm text-white outline-none">
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div class="flex flex-col gap-1.5">
                        <label class="text-xs font-medium text-neutral-300">Prioridad</label>
                        <select id="task-priority" class="glass-input rounded-xl px-4 py-2.5 text-sm text-white outline-none cursor-pointer">
                            <option value="baja" class="bg-neutral-900">Baja</option>
                            <option value="media" selected class="bg-neutral-900">Media</option>
                            <option value="alta" class="bg-neutral-900">Alta</option>
                        </select>
                    </div>
                    <div class="flex flex-col gap-1.5">
                        <label class="text-xs font-medium text-neutral-300">Tipo</label>
                        <select id="task-type" class="glass-input rounded-xl px-4 py-2.5 text-sm text-white outline-none cursor-pointer">
                            <option value="deberes" selected class="bg-neutral-900">Deberes / Trabajo</option>
                            <option value="examen" class="bg-neutral-900">Examen / Prueba</option>
                            <option value="proyecto" class="bg-neutral-900">Proyecto</option>
                        </select>
                    </div>
                </div>

                <div class="flex flex-col gap-1.5">
                    <label class="text-xs font-medium text-neutral-300">Descripción / Notas (Opcional)</label>
                    <textarea id="task-desc" rows="3" placeholder="Detalles de la asignación..." class="glass-input rounded-xl px-4 py-2.5 text-sm text-white placeholder-neutral-500 resize-none"></textarea>
                </div>

                <div class="flex items-center justify-end space-x-3 pt-2">
                    <button type="button" onclick="closeTaskModal()" class="px-5 py-2.5 rounded-xl text-xs font-medium glass-panel-sub hover:bg-white/10 text-neutral-300 transition-all">
                        Cancelar
                    </button>
                    <button type="submit" class="px-6 py-2.5 rounded-xl text-xs font-semibold bg-white text-black hover:bg-neutral-200 transition-all shadow-[0_0_15px_rgba(255,255,255,0.2)]">
                        Guardar Tarea
                    </button>
                </div>
            </form>
        </div>
    </div>

    <div id="subject-modal" class="fixed inset-0 z-50 bg-black/80 backdrop-blur-md hidden items-center justify-center p-4">
        <div class="glass-panel rounded-2xl w-full max-w-xl p-6 flex flex-col gap-5 border border-white/20 shadow-2xl animate-fade max-h-[90vh] overflow-y-auto">
            <div class="flex items-center justify-between border-b border-white/10 pb-4">
                <div>
                    <h3 class="text-lg font-bold text-white tracking-wide">Gestión de Asignaturas</h3>
                    <p class="text-xs text-neutral-400 mt-0.5">Añade o borra asignaturas y configura en qué días te tocan</p>
                </div>
                <button onclick="closeSubjectModal()" class="w-8 h-8 rounded-full glass-panel-sub flex items-center justify-center text-neutral-400 hover:text-white transition-all">
                    <i class="fa-solid fa-xmark text-sm"></i>
                </button>
            </div>

            <form id="subject-form" onsubmit="handleSubjectSubmit(event)" class="flex flex-col gap-4">
                <div class="flex gap-2.5 items-center">
                    <input type="text" id="subject-name" required placeholder="Nombre de asignatura (ej: Matemáticas)" class="glass-input rounded-xl px-4 py-2.5 text-sm text-white placeholder-neutral-500 flex-1">
                    <div class="flex items-center gap-1 glass-input rounded-xl px-2 py-1.5">
                        <input type="color" id="subject-color" value="#ffffff" class="w-8 h-7 bg-transparent border-0 cursor-pointer rounded-lg">
                    </div>
                </div>

                <div class="flex flex-col gap-2">
                    <label class="text-xs font-medium text-neutral-300">¿Qué días de la semana te toca?</label>
                    <div class="grid grid-cols-4 sm:grid-cols-7 gap-1.5" id="subject-days-checkboxes">
                        <label class="glass-panel-sub p-2 rounded-xl text-center text-[11px] font-medium cursor-pointer hover:bg-white/10 select-none flex flex-col items-center gap-1">
                            <input type="checkbox" value="1" class="accent-white"> Lun
                        </label>
                        <label class="glass-panel-sub p-2 rounded-xl text-center text-[11px] font-medium cursor-pointer hover:bg-white/10 select-none flex flex-col items-center gap-1">
                            <input type="checkbox" value="2" class="accent-white"> Mar
                        </label>
                        <label class="glass-panel-sub p-2 rounded-xl text-center text-[11px] font-medium cursor-pointer hover:bg-white/10 select-none flex flex-col items-center gap-1">
                            <input type="checkbox" value="3" class="accent-white"> Mié
                        </label>
                        <label class="glass-panel-sub p-2 rounded-xl text-center text-[11px] font-medium cursor-pointer hover:bg-white/10 select-none flex flex-col items-center gap-1">
                            <input type="checkbox" value="4" class="accent-white"> Jue
                        </label>
                        <label class="glass-panel-sub p-2 rounded-xl text-center text-[11px] font-medium cursor-pointer hover:bg-white/10 select-none flex flex-col items-center gap-1">
                            <input type="checkbox" value="5" class="accent-white"> Vie
                        </label>
                        <label class="glass-panel-sub p-2 rounded-xl text-center text-[11px] font-medium cursor-pointer hover:bg-white/10 select-none flex flex-col items-center gap-1">
                            <input type="checkbox" value="6" class="accent-white"> Sáb
                        </label>
                        <label class="glass-panel-sub p-2 rounded-xl text-center text-[11px] font-medium cursor-pointer hover:bg-white/10 select-none flex flex-col items-center gap-1">
                            <input type="checkbox" value="0" class="accent-white"> Dom
                        </label>
                    </div>
                </div>

                <button type="submit" class="w-full py-2.5 rounded-xl text-xs font-semibold bg-white text-black hover:bg-neutral-200 transition-all shadow-[0_0_15px_rgba(255,255,255,0.2)]">
                    Añadir Asignatura al Plan
                </button>
            </form>

            <div class="flex flex-col gap-2.5 pt-3 border-t border-white/10">
                <h4 class="text-xs font-semibold text-neutral-400 uppercase tracking-wider">Tus Asignaturas Guardadas</h4>
                <div class="flex flex-col gap-2 max-h-[220px] overflow-y-auto pr-1" id="modal-subjects-list"></div>
            </div>

            <div class="flex items-center justify-end pt-2">
                <button type="button" onclick="closeSubjectModal()" class="px-5 py-2.5 rounded-xl text-xs font-medium glass-panel-sub hover:bg-white/10 text-neutral-300 transition-all">
                    Cerrar y Actualizar
                </button>
            </div>
        </div>
    </div>

    <div id="toast" class="fixed bottom-6 right-6 z-50 glass-panel px-4 py-3 rounded-xl border border-white/20 text-xs text-white shadow-2xl translate-y-20 opacity-0 transition-all duration-300 flex items-center gap-2">
        <i id="toast-icon" class="fa-solid fa-circle-check text-green-400"></i>
        <span id="toast-message">Operación realizada con éxito</span>
    </div>

    <script>
        const defaultSubjects = [
            { id: 'sub-1', name: 'Matemáticas', color: '#ffffff', days: [1, 3, 5] },
            { id: 'sub-2', name: 'Física', color: '#a3a3a3', days: [2, 4] },
            { id: 'sub-3', name: 'Programación', color: '#d4d4d4', days: [1, 2, 3, 4, 5] },
            { id: 'sub-4', name: 'Historia', color: '#737373', days: [3, 5] }
        ];

        const defaultTasks = [
            {
                id: 'task-1',
                title: 'Ejercicios de Matrices',
                subjectId: 'sub-1',
                date: new Date(Date.now() + 86400000 * 1).toISOString().split('T')[0],
                priority: 'alta',
                type: 'deberes',
                desc: 'Resolver problemas de la pág 84.',
                completed: false
            },
            {
                id: 'task-2',
                title: 'Proyecto de Programación Web',
                subjectId: 'sub-3',
                date: new Date(Date.now() + 86400000 * 3).toISOString().split('T')[0],
                priority: 'media',
                type: 'proyecto',
                desc: 'Implementar diseño en interfaz.',
                completed: false
            }
        ];

        let subjects = JSON.parse(localStorage.getItem('glowswitch_subjects_v4')) || defaultSubjects;
        let tasks = JSON.parse(localStorage.getItem('glowswitch_tasks_v4')) || defaultTasks;
        
        let currentYear = new Date().getFullYear();
        let currentMonth = new Date().getMonth();
        let activeTab = 'calendar';
        let selectedDateForModal = null;

        window.addEventListener('DOMContentLoaded', () => {
            saveData();
            updateSubjectDropdowns();
            renderSidebarSubjects();
            renderCalendar();
            renderTasksView();
            updateStats();
        });

        function saveData() {
            localStorage.setItem('glowswitch_subjects_v4', JSON.stringify(subjects));
            localStorage.setItem('glowswitch_tasks_v4', JSON.stringify(tasks));
            updateStats();
        }

        function showToast(message, isError = false) {
            const toast = document.getElementById('toast');
            const icon = document.getElementById('toast-icon');
            const msg = document.getElementById('toast-message');
            
            msg.textContent = message;
            icon.className = isError ? 'fa-solid fa-circle-exclamation text-red-400' : 'fa-solid fa-circle-check text-green-400';
            
            toast.classList.remove('translate-y-20', 'opacity-0');
            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }

        function switchTab(tab) {
            activeTab = tab;
            const calView = document.getElementById('view-calendar');
            const taskView = document.getElementById('view-tasks');
            const navCal = document.getElementById('nav-calendar');
            const navTask = document.getElementById('nav-tasks');

            if (tab === 'calendar') {
                calView.classList.remove('hidden');
                taskView.classList.add('hidden');
                navCal.className = "w-full flex items-center gap-3 px-4 py-3 rounded-xl text-xs font-semibold tracking-wide transition-all bg-white text-black shadow-lg";
                navTask.className = "w-full flex items-center gap-3 px-4 py-3 rounded-xl text-xs font-semibold tracking-wide transition-all text-neutral-400 hover:text-white hover:bg-white/5";
                renderCalendar();
            } else {
                calView.classList.add('hidden');
                taskView.classList.remove('hidden');
                navTask.className = "w-full flex items-center gap-3 px-4 py-3 rounded-xl text-xs font-semibold tracking-wide transition-all bg-white text-black shadow-lg";
                navCal.className = "w-full flex items-center gap-3 px-4 py-3 rounded-xl text-xs font-semibold tracking-wide transition-all text-neutral-400 hover:text-white hover:bg-white/5";
                renderTasksView();
            }
        }

        function updateStats() {
            const pending = tasks.filter(t => !t.completed).length;
            const completed = tasks.filter(t => t.completed).length;
            document.getElementById('stat-pending').textContent = pending;
            document.getElementById('stat-completed').textContent = completed;
            document.getElementById('subject-count').textContent = subjects.length;
        }

        const monthNames = [
            "Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio",
            "Julio", "Agosto", "Septiembre", "Octubre", "Noviembre", "Diciembre"
        ];

        function changeMonth(direction) {
            currentMonth += direction;
            if (currentMonth > 11) {
                currentMonth = 0;
                currentYear++;
            } else if (currentMonth < 0) {
                currentMonth = 11;
                currentYear--;
            }
            renderCalendar();
        }

        function goToCurrentMonth() {
            const now = new Date();
            currentYear = now.getFullYear();
            currentMonth = now.getMonth();
            renderCalendar();
        }

        function renderCalendar() {
            const monthYearEl = document.getElementById('calendar-month-year');
            const gridEl = document.getElementById('calendar-grid');
            const filterSubjectId = document.getElementById('calendar-filter-subject').value;

            monthYearEl.textContent = `${monthNames[currentMonth]} ${currentYear}`;
            gridEl.innerHTML = '';

            const firstDayIndex = (new Date(currentYear, currentMonth, 1).getDay() + 6) % 7;
            const totalDays = new Date(currentYear, currentMonth + 1, 0).getDate();
            const prevTotalDays = new Date(currentYear, currentMonth, 0).getDate();
            const todayStr = new Date().toISOString().split('T')[0];

            let html = '';

            for (let i = firstDayIndex - 1; i >= 0; i--) {
                const dayNum = prevTotalDays - i;
                html += `
                    <div class="min-h-[110px] glass-panel-sub rounded-2xl p-2.5 opacity-25 flex flex-col justify-between select-none">
                        <span class="text-xs font-medium text-neutral-500">${dayNum}</span>
                    </div>
                `;
            }

            for (let day = 1; day <= totalDays; day++) {
                const monthStr = String(currentMonth + 1).padStart(2, '0');
                const dayStr = String(day).padStart(2, '0');
                const dateKey = `${currentYear}-${monthStr}-${dayStr}`;
                const isToday = dateKey === todayStr;

                const dateObj = new Date(currentYear, currentMonth, day);
                const dayOfWeek = dateObj.getDay();

                const scheduledSubjects = subjects.filter(s => s.days && s.days.includes(dayOfWeek));

                const dayTasks = tasks.filter(t => {
                    if (t.date !== dateKey) return false;
                    if (filterSubjectId !== 'all' && t.subjectId !== filterSubjectId) return false;
                    return true;
                });

                let badgesHtml = '';
                scheduledSubjects.forEach(s => {
                    badgesHtml += `
                        <div class="text-[10px] px-1.5 py-0.5 rounded truncate flex items-center gap-1 font-medium bg-white/5 border border-white/10" style="color: ${s.color};">
                            <span class="w-1.5 h-1.5 rounded-full shrink-0" style="background-color: ${s.color}"></span>
                            <span class="truncate">${s.name}</span>
                        </div>
                    `;
                });

                dayTasks.forEach(t => {
                    const subj = subjects.find(s => s.id === t.subjectId) || { name: 'General', color: '#ffffff' };
                    const completedClass = t.completed ? 'line-through opacity-40' : '';
                    badgesHtml += `
                        <div title="Tarea: ${t.title}" class="text-[10px] px-1.5 py-0.5 rounded truncate flex items-center gap-1 font-semibold ${completedClass}" style="background-color: ${subj.color}33; border-left: 2px solid ${subj.color}; color: #ffffff;">
                            <i class="fa-solid fa-thumbtack text-[8px]"></i>
                            <span class="truncate">${t.title}</span>
                        </div>
                    `;
                });

                html += `
                    <div onclick="openDayDetailModal('${dateKey}')" class="min-h-[120px] glass-panel-sub rounded-2xl p-2.5 flex flex-col justify-between transition-all hover:bg-white/10 hover:border-white/30 cursor-pointer group ${isToday ? 'border-white/50 shadow-[0_0_20px_rgba(255,255,255,0.15)] bg-white/[0.04]' : ''}">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-bold ${isToday ? 'bg-white text-black px-2 py-0.5 rounded-lg shadow-sm' : 'text-neutral-300'}">${day}</span>
                            ${(scheduledSubjects.length > 0 || dayTasks.length > 0) ? `<span class="text-[10px] text-neutral-400 group-hover:text-white font-mono">${scheduledSubjects.length + dayTasks.length} act</span>` : ''}
                        </div>
                        <div class="flex flex-col gap-1 overflow-y-auto max-h-[75px] mt-1 pr-0.5">
                            ${badgesHtml}
                        </div>
                    </div>
                `;
            }

            const totalCellsRendered = firstDayIndex + totalDays;
            const remainingCells = totalCellsRendered % 7 === 0 ? 0 : 7 - (totalCellsRendered % 7);
            for (let i = 1; i <= remainingCells; i++) {
                html += `
                    <div class="min-h-[110px] glass-panel-sub rounded-2xl p-2.5 opacity-25 flex flex-col justify-between select-none">
                        <span class="text-xs font-medium text-neutral-500">${i}</span>
                    </div>
                `;
            }

            gridEl.innerHTML = html;
        }

        function openDayDetailModal(dateStr) {
            selectedDateForModal = dateStr;
            const [y, m, d] = dateStr.split('-');
            const dateObj = new Date(y, m - 1, d);
            const dayOfWeek = dateObj.getDay();

            document.getElementById('day-modal-title').textContent = `Día ${d} de ${monthNames[m - 1]} de ${y}`;
            document.getElementById('day-modal-subtitle').textContent = `Asignaturas programadas y tareas para esta fecha`;

            const daySubjects = subjects.filter(s => s.days && s.days.includes(dayOfWeek));
            const subContainer = document.getElementById('day-modal-subjects');
            if (daySubjects.length === 0) {
                subContainer.innerHTML = `<p class="text-xs text-neutral-500 italic">No hay asignaturas configuradas para este día de la semana.</p>`;
            } else {
                let subHtml = '';
                daySubjects.forEach(s => {
                    subHtml += `
                        <div class="glass-panel-sub p-3 rounded-xl flex items-center justify-between border border-white/5">
                            <div class="flex items-center gap-2.5">
                                <span class="w-3 h-3 rounded-full" style="background-color: ${s.color}"></span>
                                <span class="text-xs font-semibold text-white">${s.name}</span>
                            </div>
                            <span class="text-[10px] px-2 py-0.5 rounded bg-white/10 text-neutral-300">Clase programada</span>
                        </div>
                    `;
                });
                subContainer.innerHTML = subHtml;
            }

            const dayTasks = tasks.filter(t => t.date === dateStr);
            const taskContainer = document.getElementById('day-modal-tasks');
            if (dayTasks.length === 0) {
                taskContainer.innerHTML = `<p class="text-xs text-neutral-500 italic">No hay tareas pendientes para este día.</p>`;
            } else {
                let taskHtml = '';
                dayTasks.forEach(t => {
                    const subj = subjects.find(s => s.id === t.subjectId) || { name: 'General', color: '#ffffff' };
                    taskHtml += `
                        <div class="glass-panel-sub p-3 rounded-xl flex items-center justify-between border border-white/5 gap-3">
                            <div class="flex items-center gap-3">
                                <button onclick="toggleTaskCompletion('${t.id}')" class="w-5 h-5 rounded glass-input flex items-center justify-center shrink-0 ${t.completed ? 'bg-white text-black' : ''}">
                                    ${t.completed ? '<i class="fa-solid fa-check text-[10px]"></i>' : ''}
                                </button>
                                <div class="flex flex-col">
                                    <span class="text-xs font-semibold ${t.completed ? 'line-through text-neutral-500' : 'text-white'}">${t.title}</span>
                                    <span class="text-[10px]" style="color: ${subj.color}">${subj.name} • ${t.priority}</span>
                                </div>
                            </div>
                            <div class="flex items-center gap-1">
                                <button onclick="openEditTaskModal('${t.id}'); closeDayModal();" class="w-7 h-7 rounded-lg glass-panel-sub flex items-center justify-center text-neutral-300 hover:text-white">
                                    <i class="fa-solid fa-pen text-[10px]"></i>
                                </button>
                                <button onclick="deleteTaskFromModal('${t.id}', '${dateStr}')" class="w-7 h-7 rounded-lg glass-panel-sub flex items-center justify-center text-neutral-400 hover:text-red-400">
                                    <i class="fa-solid fa-trash text-[10px]"></i>
                                </button>
                            </div>
                        </div>
                    `;
                });
                taskContainer.innerHTML = taskHtml;
            }

            const modal = document.getElementById('day-modal');
            modal.classList.remove('hidden');
            modal.classList.add('flex');
        }

        function closeDayModal() {
            const modal = document.getElementById('day-modal');
            modal.classList.remove('flex');
            modal.classList.add('hidden');
        }

        function openTaskModalForCurrentDay() {
            closeDayModal();
            openTaskModalForDate(selectedDateForModal);
        }

        function deleteTaskFromModal(taskId, dateStr) {
            deleteTask(taskId);
            openDayDetailModal(dateStr);
        }

        function renderTasksView() {
            const container = document.getElementById('tasks-list-container');
            const searchQuery = document.getElementById('task-search-input').value.toLowerCase();
            const statusFilter = document.getElementById('task-filter-status').value;
            const subjectFilter = document.getElementById('task-filter-subject').value;

            let filtered = tasks.filter(t => {
                if (searchQuery && !t.title.toLowerCase().includes(searchQuery) && !(t.desc && t.desc.toLowerCase().includes(searchQuery))) {
                    return false;
                }
                if (statusFilter === 'pending' && t.completed) return false;
                if (statusFilter === 'completed' && !t.completed) return false;
                if (subjectFilter !== 'all' && t.subjectId !== subjectFilter) return false;
                return true;
            });

            filtered.sort((a, b) => new Date(a.date) - new Date(b.date));

            if (filtered.length === 0) {
                container.innerHTML = `
                    <div class="flex flex-col items-center justify-center py-16 text-center text-neutral-400 gap-3">
                        <i class="fa-regular fa-folder-open text-3xl opacity-40"></i>
                        <p class="text-xs">No se encontraron tareas con los filtros seleccionados.</p>
                    </div>
                `;
                return;
            }

            let html = '';
            filtered.forEach(t => {
                const subj = subjects.find(s => s.id === t.subjectId) || { name: 'General', color: '#ffffff' };
                const priorityBadgeColor = t.priority === 'alta' ? 'bg-red-500/10 text-red-400 border-red-500/20' : (t.priority === 'media' ? 'bg-amber-500/10 text-amber-400 border-amber-500/20' : 'bg-blue-500/10 text-blue-400 border-blue-500/20');

                html += `
                    <div class="glass-panel-sub rounded-2xl p-4 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 transition-all hover:bg-white/5 border border-white/5">
                        <div class="flex items-start sm:items-center gap-3.5">
                            <button onclick="toggleTaskCompletion('${t.id}')" class="w-6 h-6 rounded-lg glass-input flex items-center justify-center shrink-0 mt-0.5 sm:mt-0 transition-all ${t.completed ? 'bg-white text-black border-white' : 'hover:border-white/40'}">
                                ${t.completed ? '<i class="fa-solid fa-check text-xs font-bold"></i>' : ''}
                            </button>
                            <div class="flex flex-col gap-1">
                                <div class="flex flex-wrap items-center gap-2">
                                    <span class="font-semibold text-xs ${t.completed ? 'line-through text-neutral-500' : 'text-white'}">${t.title}</span>
                                    <span class="text-[10px] px-2 py-0.5 rounded-full border border-white/10" style="color: ${subj.color}; border-color: ${subj.color}40;">
                                        ${subj.name}
                                    </span>
                                </div>
                                <p class="text-[11px] text-neutral-400 line-clamp-1">${t.desc || 'Sin notas adicionales.'}</p>
                            </div>
                        </div>

                        <div class="flex items-center gap-3 self-end sm:self-center shrink-0">
                            <div class="flex items-center gap-1.5 text-xs text-neutral-300 glass-panel-sub px-3 py-1.5 rounded-xl border border-white/10">
                                <i class="fa-regular fa-calendar text-[11px] text-neutral-400"></i>
                                <span>${formatDateStr(t.date)}</span>
                            </div>
                            <span class="text-[10px] px-2.5 py-1 rounded-xl uppercase tracking-wider font-semibold border ${priorityBadgeColor}">
                                ${t.priority}
                            </span>
                            <div class="flex items-center gap-1">
                                <button onclick="openEditTaskModal('${t.id}')" title="Editar" class="w-8 h-8 rounded-xl glass-panel-sub flex items-center justify-center text-neutral-300 hover:text-white hover:bg-white/10 transition-all">
                                    <i class="fa-solid fa-pen text-xs"></i>
                                </button>
                                <button onclick="deleteTask('${t.id}')" title="Eliminar" class="w-8 h-8 rounded-xl glass-panel-sub flex items-center justify-center text-neutral-400 hover:text-red-400 hover:bg-white/10 transition-all">
                                    <i class="fa-solid fa-trash text-xs"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
            });

            container.innerHTML = html;
        }

        function formatDateStr(dateStr) {
            if (!dateStr) return '';
            const [y, m, d] = dateStr.split('-');
            return `${d}/${m}/${y}`;
        }

        function toggleTaskCompletion(id) {
            const task = tasks.find(t => t.id === id);
            if (task) {
                task.completed = !task.completed;
                saveData();
                renderCalendar();
                renderTasksView();
                showToast(task.completed ? 'Tarea marcada como completada' : 'Tarea marcada como pendiente');
            }
        }

        function deleteTask(id) {
            tasks = tasks.filter(t => t.id !== id);
            saveData();
            renderCalendar();
            renderTasksView();
            showToast('Tarea eliminada correctamente');
        }

        function updateSubjectDropdowns() {
            const selects = ['task-subject', 'calendar-filter-subject', 'task-filter-subject'];
            selects.forEach(selId => {
                const el = document.getElementById(selId);
                if (!el) return;
                
                const currentVal = el.value;
                let optionsHtml = '';
                
                if (selId !== 'task-subject') {
                    optionsHtml += `<option value="all" class="bg-neutral-900">Todas las asignaturas</option>`;
                }
                
                subjects.forEach(s => {
                    optionsHtml += `<option value="${s.id}" class="bg-neutral-900" style="color: ${s.color}">${s.name}</option>`;
                });
                
                el.innerHTML = optionsHtml;
                el.value = currentVal || 'all';
            });
        }

        function renderSidebarSubjects() {
            const container = document.getElementById('sidebar-subjects-list');
            let html = '';
            
            subjects.forEach(s => {
                const count = tasks.filter(t => t.subjectId === s.id && !t.completed).length;
                html += `
                    <div class="glass-panel-sub rounded-xl p-2.5 flex items-center justify-between border border-white/5 hover:border-white/20 transition-all">
                        <div class="flex items-center gap-2.5">
                            <span class="w-3 h-3 rounded-full shrink-0 shadow-[0_0_8px]" style="background-color: ${s.color}; box-shadow: 0 0 10px ${s.color}66"></span>
                            <span class="text-xs font-medium text-white truncate max-w-[140px]">${s.name}</span>
                        </div>
                        <span class="text-[10px] px-2 py-0.5 rounded-full bg-white/10 text-neutral-300 font-semibold">${count} pend.</span>
                    </div>
                `;
            });
            container.innerHTML = html;
        }

        const dayNamesMap = { '1': 'Lun', '2': 'Mar', '3': 'Mié', '4': 'Jue', '5': 'Vie', '6': 'Sáb', '0': 'Dom' };

        function renderModalSubjectsList() {
            const container = document.getElementById('modal-subjects-list');
            let html = '';
            
            subjects.forEach(s => {
                let daysStr = '';
                if (s.days && s.days.length > 0) {
                    daysStr = s.days.map(d => dayNamesMap[String(d)]).join(', ');
                } else {
                    daysStr = 'Ningún día configurado';
                }

                html += `
                    <div class="glass-panel-sub rounded-xl p-3 flex items-center justify-between border border-white/5 gap-3">
                        <div class="flex items-center gap-3">
                            <span class="w-4 h-4 rounded-full shrink-0" style="background-color: ${s.color}"></span>
                            <div class="flex flex-col">
                                <span class="text-xs font-bold text-white">${s.name}</span>
                                <span class="text-[10px] text-neutral-400">Días: ${daysStr}</span>
                            </div>
                        </div>
                        <button onclick="deleteSubject('${s.id}')" title="Borrar asignatura" class="w-8 h-8 rounded-xl glass-panel-sub flex items-center justify-center text-neutral-400 hover:text-red-400 hover:bg-white/10 transition-all">
                            <i class="fa-solid fa-trash text-xs"></i>
                        </button>
                    </div>
                `;
            });
            container.innerHTML = html;
        }

        function openSubjectModal() {
            renderModalSubjectsList();
            document.getElementById('subject-modal').classList.remove('hidden');
            document.getElementById('subject-modal').classList.add('flex');
        }

        function closeSubjectModal() {
            document.getElementById('subject-modal').classList.remove('flex');
            document.getElementById('subject-modal').classList.add('hidden');
            updateSubjectDropdowns();
            renderSidebarSubjects();
            renderCalendar();
            renderTasksView();
        }

        function handleSubjectSubmit(e) {
            e.preventDefault();
            const name = document.getElementById('subject-name').value.trim();
            const color = document.getElementById('subject-color').value;

            const checkboxes = document.querySelectorAll('#subject-days-checkboxes input[type="checkbox"]');
            let selectedDays = [];
            checkboxes.forEach(cb => {
                if (cb.checked) {
                    selectedDays.push(parseInt(cb.value));
                }
            });

            if (!name) return;

            const newSubj = {
                id: 'sub-' + Date.now(),
                name,
                color,
                days: selectedDays
            };

            subjects.push(newSubj);
            saveData();
            renderModalSubjectsList();
            document.getElementById('subject-name').value = '';
            checkboxes.forEach(cb => cb.checked = false);
            showToast('Asignatura creada y programada');
        }

        function deleteSubject(id) {
            if (subjects.length <= 1) {
                showToast('Debes mantener al menos una asignatura activa', true);
                return;
            }
            subjects = subjects.filter(s => s.id !== id);
            tasks = tasks.filter(t => t.subjectId !== id);
            saveData();
            renderModalSubjectsList();
            updateSubjectDropdowns();
            renderSidebarSubjects();
            renderCalendar();
            renderTasksView();
            showToast('Asignatura eliminada con éxito');
        }

        function openTaskModal() {
            document.getElementById('task-id').value = '';
            document.getElementById('task-modal-title').textContent = 'Nueva Tarea';
            document.getElementById('task-form').reset();
            
            const todayStr = new Date().toISOString().split('T')[0];
            document.getElementById('task-date').value = todayStr;

            document.getElementById('task-modal').classList.remove('hidden');
            document.getElementById('task-modal').classList.add('flex');
        }

        function openTaskModalForDate(dateStr) {
            openTaskModal();
            document.getElementById('task-date').value = dateStr;
        }

        function openEditTaskModal(id) {
            const task = tasks.find(t => t.id === id);
            if (!task) return;

            document.getElementById('task-id').value = task.id;
            document.getElementById('task-modal-title').textContent = 'Editar Tarea';
            document.getElementById('task-title').value = task.title;
            document.getElementById('task-subject').value = task.subjectId;
            document.getElementById('task-date').value = task.date;
            document.getElementById('task-priority').value = task.priority;
            document.getElementById('task-type').value = task.type;
            document.getElementById('task-desc').value = task.desc || '';

            document.getElementById('task-modal').classList.remove('hidden');
            document.getElementById('task-modal').classList.add('flex');
        }

        function closeTaskModal() {
            document.getElementById('task-modal').classList.remove('flex');
            document.getElementById('task-modal').classList.add('hidden');
        }

        function handleTaskSubmit(e) {
            e.preventDefault();
            const id = document.getElementById('task-id').value;
            const title = document.getElementById('task-title').value.trim();
            const subjectId = document.getElementById('task-subject').value;
            const date = document.getElementById('task-date').value;
            const priority = document.getElementById('task-priority').value;
            const type = document.getElementById('task-type').value;
            const desc = document.getElementById('task-desc').value.trim();

            if (!title || !subjectId || !date) return;

            if (id) {
                const task = tasks.find(t => t.id === id);
                if (task) {
                    task.title = title;
                    task.subjectId = subjectId;
                    task.date = date;
                    task.priority = priority;
                    task.type = type;
                    task.desc = desc;
                    showToast('Tarea actualizada');
                }
            } else {
                const newTask = {
                    id: 'task-' + Date.now(),
                    title,
                    subjectId,
                    date,
                    priority,
                    type,
                    desc,
                    completed: false
                };
                tasks.push(newTask);
                showToast('Tarea creada correctamente');
            }

            saveData();
            closeTaskModal();
            renderCalendar();
            renderTasksView();
            renderSidebarSubjects();
        }
    </script>
</body>
</html>
