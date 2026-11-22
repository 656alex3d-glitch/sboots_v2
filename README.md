# sboots_v2
programa punto de venta 
<!DOCTYPE html>

<html class="h-full select-none" lang="es"><head>
<meta charset="utf-8"/>
<meta content="width=device-width, initial-scale=1.0" name="viewport"/>
<title>NEXUS LENS 3D v3.0 — VERSION FULL</title>
<!-- Tailwind CSS CDN -->
<script src="https://cdn.tailwindcss.com?plugins=forms,container-queries"></script>
<!-- FontAwesome for UI icons -->
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet"/>
<script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            appDark: '#080c14',
            appPanel: '#0e131d',
            appCard: '#131926',
            appBorder: '#1e293b',
            appBorderLight: '#2c3a50',
            accentBlue: '#0284c7',
            accentCyan: '#38bdf8',
            accentGreen: '#10b981',
            accentAmber: '#f59e0b',
            accentRed: '#ef4444'
          },
          fontSize: {
            '2xs': '0.65rem',
            '3xs': '0.55rem'
          }
        }
      }
    }
</script>
<style data-purpose="custom-scrollbars">
    ::-webkit-scrollbar {
      width: 4px;
      height: 4px;
    }
    ::-webkit-scrollbar-track {
      background: #090c13;
    }
    ::-webkit-scrollbar-thumb {
      background: #1e293b;
      border-radius: 2px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #334155;
    }
</style>
<style data-purpose="ui-sliders">
    input[type=range] {
      -webkit-appearance: none;
      height: 3px;
      background: #1e293b;
      border-radius: 2px;
      outline: none;
    }
    input[type=range]::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: #38bdf8;
      cursor: pointer;
      border: 1px solid #0284c7;
      box-shadow: 0 0 4px rgba(56, 189, 248, 0.7);
    }
    input[type=range]::-moz-range-thumb {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: #38bdf8;
      cursor: pointer;
      border: 1px solid #0284c7;
    }
</style>
<style data-purpose="canvas-and-preview">
    .canvas-bg-pattern {
      background-color: #070a10;
      background-image: 
        linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
      background-size: 20px 20px;
    }
    .lenticular-guide-bar {
      background: repeating-linear-gradient(
        90deg,
        #10b981,
        #10b981 3px,
        #065f46 3px,
        #065f46 6px
      );
    }
    .lenticular-guide-bar-horizontal {
      background: repeating-linear-gradient(
        0deg,
        #10b981,
        #10b981 3px,
        #065f46 3px,
        #065f46 6px
      );
    }
    .yellow-strip {
      background: linear-gradient(90deg, #ca8a04, #eab308, #fef08a, #ca8a04);
    }
    .lenticular-vertical-overlay {
      background: repeating-linear-gradient(90deg, transparent, transparent 1px, rgba(0,0,0,0.7) 1px, rgba(0,0,0,0.7) 2px);
    }
    .lenticular-horizontal-overlay {
      background: repeating-linear-gradient(0deg, transparent, transparent 1px, rgba(0,0,0,0.7) 1px, rgba(0,0,0,0.7) 2px);
    }
    .specular-shine {
      background: linear-gradient(105deg, transparent 20%, rgba(255,255,255,0.18) 50%, transparent 80%);
      pointer-events: none;
      mix-blend-mode: overlay;
    }
</style>
</head>
<body class="bg-appDark text-slate-300 font-sans h-screen flex flex-col overflow-hidden text-xs antialiased relative">
<!-- Toast Notification Container -->
<div class="fixed top-9 right-4 z-[999] flex flex-col space-y-2 pointer-events-none" id="toastContainer"></div>
<!-- ============================================================ -->
<!-- MODAL: PROYECTO DIALOG (Identico a la captura Imagen 2)        -->
<!-- ============================================================ -->
<div class="fixed inset-0 bg-black/60 backdrop-blur-[2px] z-[900] hidden flex items-center justify-center p-4" id="proyectoModal">
<div class="bg-[#f3f4f6] text-slate-800 rounded-lg shadow-2xl border border-slate-300 w-full max-w-[480px] overflow-hidden font-sans animate-in fade-in zoom-in-95 duration-100">
<!-- Header estilo diálogo Windows moderno -->
<div class="flex items-center justify-between px-3 py-2 bg-white border-b border-slate-200 select-none">
<div class="flex items-center space-x-2">
<span class="inline-flex items-center justify-center w-4 h-4 bg-emerald-600/10 text-emerald-600 rounded text-[11px]">🗂</span>
<span class="font-medium text-slate-800 text-[13px]">Proyecto</span>
</div>
<button class="text-slate-400 hover:text-slate-700 hover:bg-slate-100 p-1 rounded transition text-xs leading-none" onclick="toggleProyectoModal(false)">
<i class="fa-solid fa-xmark"></i>
</button>
</div>
<!-- Cuerpo del mensaje -->
<div class="p-6 flex items-start space-x-4">
<div class="w-10 h-10 rounded-full bg-[#0284c7] text-white flex items-center justify-center shrink-0 shadow-sm text-lg font-bold">
<i class="fa-solid fa-info"></i>
</div>
<div class="text-[13px] leading-relaxed text-slate-600 pt-0.5">
        Entorno de Proyecto Activo: Parámetros guardados y configuración de mesa sincronizada.
      </div>
</div>
<!-- Footer con botón OK -->
<div class="px-5 py-3 bg-[#f8fafc] border-t border-slate-200 flex justify-end">
<button class="px-7 py-1.5 bg-[#182335] hover:bg-[#0f172a] active:scale-95 text-white font-medium text-xs rounded transition shadow-sm" onclick="toggleProyectoModal(false)">
        OK
      </button>
</div>
</div>
</div>
<!-- ============================================================ -->
<!-- MODAL: CARGAR PAR 3D (RGB + Mapa de Profundidad)             -->
<!-- ============================================================ -->
<div class="fixed inset-0 bg-black/75 backdrop-blur-sm z-[900] hidden flex items-center justify-center" id="pairModal">
<div class="bg-appPanel border border-sky-500/60 rounded-md shadow-2xl w-[420px] p-4 text-xs font-sans">
<div class="flex items-center justify-between border-b border-appBorder pb-2 text-sky-400 font-bold">
<span class="flex items-center space-x-1.5">
<i class="fa-solid fa-cloud-arrow-up text-amber-400"></i>
<span>Cargar Par 3D (RGB + Mapa Profundidad)</span>
</span>
<button class="text-gray-400 hover:text-white" onclick="toggleModal(false)"><i class="fa-solid fa-xmark"></i></button>
</div>
<div class="py-3 space-y-3">
<div class="border border-dashed border-sky-600/60 rounded p-4 text-center bg-black/40 hover:bg-black/60 transition cursor-pointer" onclick="filePickerSim()">
<i class="fa-solid fa-file-image text-3xl text-sky-400 mb-1"></i>
<p class="text-gray-200 font-medium text-xs">Arrastra tus archivos o haz clic para examinar</p>
<p class="text-gray-400 text-[10px] mt-1">Soporta PNG, TIFF, JPG 16-bit calibrado con Z-Buffer</p>
</div>
<div class="bg-appCard p-2.5 rounded border border-appBorderLight/40 space-y-1.5">
<div class="flex items-center justify-between text-2xs text-gray-300">
<span class="text-sky-300 font-medium">RGB_Diffuse.png</span>
<span class="text-emerald-400 font-mono">1920×1080 (32-bit RGBA)</span>
</div>
<div class="flex items-center justify-between text-2xs text-gray-300">
<span class="text-amber-300 font-medium">DepthMap_ZBuffer.png</span>
<span class="text-emerald-400 font-mono">16-bit Grayscale Z-Map</span>
</div>
</div>
</div>
<div class="flex items-center justify-end space-x-2 pt-2 border-t border-appBorder">
<button class="px-3 py-1 bg-gray-800 hover:bg-gray-700 text-gray-300 rounded text-2xs" onclick="toggleModal(false)">Cancelar</button>
<button class="px-3.5 py-1 bg-sky-600 hover:bg-sky-500 text-white font-bold rounded text-2xs flex items-center space-x-1.5 shadow" onclick="confirmLoadPair()">
<i class="fa-solid fa-check"></i>
<span>Vincular y Procesar</span>
</button>
</div>
</div>
</div>
<!-- ============================================================ -->
<!-- MODAL: CURVAS MAPA (GRIS)                                    -->
<!-- ============================================================ -->
<div class="fixed inset-0 bg-black/75 backdrop-blur-sm z-[900] hidden flex items-center justify-center" id="curvesModal">
<div class="bg-appPanel border border-sky-500/60 rounded-md shadow-2xl w-[400px] p-4 text-xs font-sans">
<div class="flex items-center justify-between border-b border-appBorder pb-2 text-sky-400 font-bold">
<span class="flex items-center space-x-1.5">
<i class="fa-solid fa-chart-line text-sky-400"></i>
<span>Ecualización Curvas Mapa (Gris)</span>
</span>
<button class="text-gray-400 hover:text-white" onclick="toggleCurvesModal(false)"><i class="fa-solid fa-xmark"></i></button>
</div>
<div class="py-3 space-y-2">
<div class="h-32 bg-black border border-appBorder rounded flex items-end p-2 justify-between">
<div class="w-2 bg-sky-900/60 h-10"></div>
<div class="w-2 bg-sky-800/60 h-16"></div>
<div class="w-2 bg-sky-700/60 h-24"></div>
<div class="w-2 bg-sky-600/70 h-28"></div>
<div class="w-2 bg-sky-500/80 h-20"></div>
<div class="w-2 bg-sky-400/90 h-14"></div>
<div class="w-2 bg-sky-300 h-8"></div>
</div>
<div class="space-y-1.5 font-mono text-2xs">
<div class="flex justify-between text-gray-300">
<span>Punto Negro (Offset):</span>
<span class="text-sky-400" id="valBlackPoint">0</span>
</div>
<input class="w-full" max="100" min="0" oninput="document.getElementById('valBlackPoint').innerText = this.value" type="range" value="0"/>
<div class="flex justify-between text-gray-300">
<span>Gamma Z-Depth:</span>
<span class="text-sky-400" id="valGammaZ">1.00</span>
</div>
<input class="w-full" max="2" min="0.5" oninput="document.getElementById('valGammaZ').innerText = parseFloat(this.value).toFixed(2)" step="0.05" type="range" value="1.0"/>
</div>
</div>
<div class="flex items-center justify-end space-x-2 pt-2 border-t border-appBorder">
<button class="px-3 py-1 bg-gray-800 hover:bg-gray-700 text-gray-300 rounded text-2xs" onclick="toggleCurvesModal(false)">Cerrar</button>
<button class="px-3 py-1 bg-sky-600 hover:bg-sky-500 text-white font-bold rounded text-2xs" onclick="toggleCurvesModal(false); showToast('Curvas de Z-Map aplicadas al kernel', 'success')">Aplicar al Buffer</button>
</div>
</div>
</div>
<!-- ============================================================ -->
<!-- MODAL: EXPORTAR / GUARDAR COMO LENTICULAR                   -->
<!-- ============================================================ -->
<div class="fixed inset-0 bg-black/75 backdrop-blur-sm z-[900] hidden flex items-center justify-center" id="exportModal">
<div class="bg-appPanel border border-emerald-500/70 rounded-md shadow-2xl w-[440px] p-4 text-xs font-sans">
<div class="flex items-center justify-between border-b border-appBorder pb-2 text-emerald-400 font-bold">
<span class="flex items-center space-x-1.5">
<i class="fa-solid fa-file-export"></i>
<span>Exportar Interlace Lenticular (Guardar Como)</span>
</span>
<button class="text-gray-400 hover:text-white" onclick="toggleExportModal(false)"><i class="fa-solid fa-xmark"></i></button>
</div>
<div class="py-3 space-y-2 font-mono text-2xs">
<div class="p-2.5 bg-black/50 border border-emerald-900 rounded flex items-center space-x-3">
<i class="fa-solid fa-file-shield text-3xl text-emerald-400"></i>
<div>
<div class="text-gray-100 font-bold text-xs">nexus_lenticular_720dpi.png</div>
<div class="text-gray-400">Tamaño: <span class="text-sky-300 font-semibold">17.26 MB</span> • 18 Vistas Entrelazadas</div>
<div class="text-emerald-400 text-[10px]">LPI 75.610 | Pitch: 0.3359 mm | Mesa 63.5×88.9 mm | 720 DPI</div>
</div>
</div>
<p class="text-gray-300 font-sans text-xs pt-1">
        Masterizado listo con compensación óptica subpíxel para impresión offset o mesa plana UV.
      </p>
</div>
<div class="flex items-center justify-end space-x-2 pt-2 border-t border-appBorder">
<button class="px-3 py-1 bg-gray-800 hover:bg-gray-700 text-gray-300 rounded text-2xs" onclick="toggleExportModal(false)">Cancelar</button>
<button class="px-4 py-1.5 bg-emerald-600 hover:bg-emerald-500 text-white font-bold rounded text-2xs flex items-center space-x-1.5 shadow-lg shadow-emerald-950/50" onclick="downloadSimulatedExport()">
<i class="fa-solid fa-download"></i>
<span>Guardar Archivo Master</span>
</button>
</div>
</div>
</div>
<!-- ============================================================ -->
<!-- MODAL: RESULTADO ENTRELAZADO                                -->
<!-- ============================================================ -->
<div class="fixed inset-0 bg-black/75 backdrop-blur-sm z-[900] hidden flex items-center justify-center" id="interlaceNoticeModal">
<div class="bg-appPanel border border-sky-500/70 rounded-md shadow-2xl w-[380px] p-4 text-xs font-sans">
<div class="flex items-center justify-between border-b border-appBorder pb-2 text-sky-400 font-bold">
<span class="flex items-center space-x-1.5">
<i class="fa-solid fa-bolt text-amber-400"></i>
<span id="interlaceNoticeTitle">Entrelazado Calculado</span>
</span>
<button class="text-gray-400 hover:text-white" onclick="toggleInterlaceNotice(false)"><i class="fa-solid fa-xmark"></i></button>
</div>
<div class="py-4 text-center space-y-2">
<i class="fa-solid fa-circle-check text-3xl text-emerald-400"></i>
<p class="text-gray-200 text-xs font-medium" id="interlaceNoticeBody">Entrelazado Vertical calculado con éxito: 1800x2520 px @ 720 DPI</p>
<p class="text-gray-400 text-2xs font-mono">18 vistas entrelazadas con Supersampling 8x y curvatura corregida.</p>
</div>
<div class="flex justify-end pt-2 border-t border-appBorder">
<button class="px-4 py-1 bg-sky-600 hover:bg-sky-500 text-white font-bold rounded text-2xs" onclick="toggleInterlaceNotice(false)">Aceptar</button>
</div>
</div>
</div>
<!-- ============================================================ -->
<!-- 1. CABECERA NATIVA DE VENTANA & TELEMETRÍA (Imágenes 1 y 2) -->
<!-- ============================================================ -->
<header class="h-7 bg-[#070a10] border-b border-appBorder flex items-center justify-between px-2 select-none shrink-0" data-purpose="windows-titlebar">
<div class="flex items-center space-x-2">
<div class="flex items-center space-x-1.5 text-sky-400 font-bold tracking-wider text-xs">
<i class="fa-solid fa-cube text-xs text-sky-400"></i>
<span class="text-gray-100 font-mono tracking-tight font-semibold text-[11px]">NEXUS LENS 3D</span>
<span class="px-1 py-0.2 bg-sky-950 text-sky-300 border border-sky-800 text-[9px] rounded font-mono font-bold">v3.0 FULL</span>
</div>
</div>
<!-- Botones de ventana nativos estilo Windows (_ □ ✕) -->
<div class="flex items-center h-full -mr-2">
<button class="h-full px-3.5 hover:bg-gray-800 text-gray-400 hover:text-white transition flex items-center justify-center" onclick="simulateWinMinimize()" title="Minimizar">
<i class="fa-regular fa-window-minimize text-[9px]"></i>
</button>
<button class="h-full px-3.5 hover:bg-gray-800 text-gray-400 hover:text-white transition flex items-center justify-center" onclick="simulateWinMaximize()" title="Maximizar/Restaurar">
<i class="fa-regular fa-window-maximize text-[9px]" id="winMaxIcon"></i>
</button>
<button class="h-full px-4 hover:bg-red-600 text-gray-400 hover:text-white transition flex items-center justify-center" onclick="simulateWinClose()" title="Cerrar">
<i class="fa-solid fa-xmark text-[11px]"></i>
</button>
</div>
</header>
<!-- BARRA DE NAVEGACIÓN Y TELEMETRÍA EN VIVO -->
<nav class="bg-[#0e131d] border-b border-appBorder px-2.5 py-1 flex items-center justify-between shrink-0" data-purpose="top-navigation">
<!-- Pestañas de la izquierda -->
<div class="flex items-center space-x-1" id="navTabsContainer">
<!-- Botón Proyecto: Abre diálogo modal exactamente como en Imagen 2 -->
<button class="px-2.5 py-1 rounded bg-[#16202e] hover:bg-[#1e2c3e] text-gray-300 flex items-center space-x-1.5 border border-appBorderLight/60 text-xs font-medium transition active:scale-95" id="tab-proyecto" onclick="openProyectoDialog()">
<i class="fa-regular fa-folder text-amber-400 text-2xs"></i>
<span>📁 Proyecto</span>
</button>
<!-- Botón Capas: Cambia el panel lateral para gestor de Capas -->
<button class="px-2.5 py-1 rounded bg-[#16202e] hover:bg-[#1e2c3e] text-gray-300 flex items-center space-x-1.5 border border-appBorderLight/60 text-xs font-medium transition active:scale-95" id="tab-capas" onclick="switchMainTab('capas')">
<i class="fa-solid fa-layer-group text-sky-400 text-2xs"></i>
<span>🗂 Capas</span>
</button>
<!-- Botón 3D+Flip: Activa el entorno principal 3D Z-Buffer -->
<button class="px-3 py-1 rounded bg-[#0284c7] text-white flex items-center space-x-1.5 border border-sky-400 font-semibold shadow-inner text-xs transition" id="tab-3dflip" onclick="switchMainTab('3dflip')">
<i class="fa-solid fa-glasses text-sky-100 text-2xs"></i>
<span>👁 3D+Flip</span>
</button>
<div class="h-4 w-[1px] bg-gray-700 mx-1"></div>
<button class="p-1 px-1.5 rounded bg-[#182333] hover:bg-gray-700 text-gray-300 border border-gray-700 active:scale-95 transition" onclick="showToast('Calculadora Óptica: Matriz Lenticular de 75.610 LPI calibrada', 'info')" title="Calculadora Lenticular">
<i class="fa-solid fa-calculator text-2xs"></i>
</button>
<button class="p-1 px-1.5 rounded bg-[#182333] hover:bg-gray-700 text-gray-300 border border-gray-700 active:scale-95 transition" onclick="showToast('Nexus Lens 3D Engine v3.0 Pro. Calibración óptica subpíxel habilitada.', 'info')" title="Información">
<i class="fa-solid fa-circle-info text-2xs text-sky-400"></i>
</button>
</div>
<!-- Telemetría en vivo en la cabecera (Chips técnicos) -->
<div class="flex items-center space-x-2">
<div class="flex items-center space-x-1 text-[9.5px] font-mono text-gray-300">
<span class="text-gray-400 font-sans text-[10px]">Cálculo:</span>
<span class="bg-[#121c2a] border border-sky-700/60 text-sky-300 px-1.5 py-0.5 rounded">LPI 75.610</span>
<span class="bg-[#121c2a] border border-gray-700 text-gray-200 px-1.5 py-0.5 rounded">pitch 0.3359 mm</span>
<span class="bg-[#2a2110] border border-amber-600/70 text-amber-300 px-1.5 py-0.5 rounded font-bold">18 vistas</span>
<span class="bg-[#121c2a] border border-gray-700 text-gray-300 px-1.5 py-0.5 rounded">límite ±50% 3D</span>
<span class="bg-[#0e271c] border border-emerald-600 text-emerald-300 px-1.5 py-0.5 rounded font-bold flex items-center space-x-1" id="chipViable">
<span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse inline-block"></span>
<span>● VIABLE bajo lámina (~15.12 mm)</span>
</span>
</div>
<!-- Botón Limitador LPI: OFF / ON interactivo -->
<button class="px-2.5 py-1 bg-[#1a2332] border border-amber-600/70 hover:border-amber-500 rounded text-amber-300 font-bold text-[10px] flex items-center space-x-1.5 shadow-sm active:scale-95 transition" id="btnLimitadorLPI" onclick="toggleLimitadorLPI()">
<i class="fa-solid fa-lock text-[9px] text-amber-400" id="icoLimitador"></i>
<span id="txtLimitador">🔒 Limitador LPI: OFF</span>
</button>
</div>
</nav>
<!-- ============================================================ -->
<!-- CUERPO PRINCIPAL DEL SOFTWARE (3 PANELES)                     -->
<!-- ============================================================ -->
<main class="flex-1 flex overflow-hidden">
<!-- ============================================================ -->
<!-- 5. PANEL IZQUIERDO: EFECTO 3D PROFUNDIDAD / GESTOR DE CAPAS   -->
<!-- ============================================================ -->
<aside class="w-72 bg-appPanel border-r border-appBorder flex flex-col shrink-0 overflow-y-auto" data-purpose="left-controls" id="leftPanel">
<!-- SUB-VISTA A: EFECTO 3D PROFUNDIDAD / Z-BUFFER CORE -->
<div class="p-2 space-y-2.5" id="viewZBufferCore">
<div class="text-[11px] font-bold uppercase tracking-wider text-gray-300 flex items-center justify-between border-b border-appBorder pb-1">
<span>EFECTO 3D PROFUNDIDAD</span>
<i class="fa-solid fa-cube text-sky-400 text-xs"></i>
</div>
<!-- BOTÓN PRINCIPAL: CARGAR PAR 3D (RGB + MAPA) -->
<button class="w-full py-2 bg-gradient-to-r from-sky-800 to-sky-600 hover:from-sky-700 hover:to-sky-500 active:scale-[0.99] border border-sky-400 text-white rounded font-bold text-xs flex items-center justify-center space-x-2 shadow-lg shadow-sky-950/40 transition" onclick="toggleModal(true)">
<i class="fa-solid fa-sun text-yellow-300 text-xs"></i>
<span class="tracking-wide">CARGAR PAR 3D (RGB + MAPA)</span>
</button>
<!-- Tarjeta de Par 3D Activo -->
<div class="space-y-1">
<div class="flex items-center justify-between text-2xs text-amber-400 font-semibold uppercase">
<span><i class="fa-solid fa-images mr-1"></i>PARES 3D (VISTA EN VIVO ARRIBA)</span>
<span class="text-[9px] font-mono text-gray-400" id="lblParesCount">1 par cargado</span>
</div>
<div class="bg-appCard border border-sky-600/70 rounded p-1.5 relative shadow-md transition" id="activePairCard">
<div class="flex items-center justify-between text-[10px] text-sky-400 font-mono mb-1">
<span class="font-bold flex items-center"><i class="fa-solid fa-link text-xs mr-1 text-amber-400"></i>● PAR 1: d4ad0a091a82</span>
<button class="w-4 h-4 rounded-full bg-red-800/80 hover:bg-red-700 text-white flex items-center justify-center text-[9px]" onclick="removePairCard()" title="Limpiar Par">
<i class="fa-solid fa-xmark"></i>
</button>
</div>
<!-- Miniaturas duales: RGB 2D + DEPTHMAP con borde cian -->
<div class="grid grid-cols-2 gap-1.5 my-1">
<div class="border-2 border-sky-500 rounded bg-black h-28 relative overflow-hidden flex items-center justify-center group cursor-pointer" onclick="showToast('Canal RGB principal seleccionado', 'info')">
<div class="w-full h-full bg-cover bg-center" id="thumbRGB" style="background-image: url('https://lh3.googleusercontent.com/aida-public/AB6AXuDLSkvOItuK87svbh0i2Uz5Ps5qPX0vE8Shx0Rt4NVbBr5MLzrgRpd-GiEmUU24DAOqdj1Jd6LUQLz-hJH7xW6mZfymN14bfPCiVGyMseLcZYBP9e8AzzIbqejC5AuGi3O9ACsow0iBp7U81cPOWa9HN0OR4Li9Gjh7QyE3OilMU6xeho8d4qFfGrk6STbakulYAu-2NFRZMYP9nWtmqGBBXUL6BhQ0jQvDZ3JuLit0fPPEH1-fZd7WRIlaHOEQe5OK7g'); background-position: 50% 30%;"></div>
<div class="absolute bottom-0 inset-x-0 bg-black/80 text-[8px] text-center font-mono py-0.5 text-gray-200">RGB 2D</div>
</div>
<div class="border-2 border-sky-400 rounded bg-black h-28 relative overflow-hidden flex items-center justify-center cursor-pointer" onclick="showToast('Mapa de profundidad activo (16-bit Z-Buffer)', 'info')">
<div class="w-full h-full bg-cover bg-center filter grayscale contrast-150" style="background-image: url('https://lh3.googleusercontent.com/aida-public/AB6AXuDLSkvOItuK87svbh0i2Uz5Ps5qPX0vE8Shx0Rt4NVbBr5MLzrgRpd-GiEmUU24DAOqdj1Jd6LUQLz-hJH7xW6mZfymN14bfPCiVGyMseLcZYBP9e8AzzIbqejC5AuGi3O9ACsow0iBp7U81cPOWa9HN0OR4Li9Gjh7QyE3OilMU6xeho8d4qFfGrk6STbakulYAu-2NFRZMYP9nWtmqGBBXUL6BhQ0jQvDZ3JuLit0fPPEH1-fZd7WRIlaHOEQe5OK7g'); background-position: 50% 30%;"></div>
<div class="absolute bottom-0 inset-x-0 bg-black/80 text-[8px] text-center font-mono py-0.5 text-sky-300">DEPTHMAP</div>
</div>
</div>
<!-- Pair Actions -->
<div class="grid grid-cols-2 gap-1 mt-1.5">
<button class="py-1 bg-emerald-800/80 hover:bg-emerald-700 active:scale-95 border border-emerald-500 text-white rounded text-[10px] font-bold flex items-center justify-center space-x-1" onclick="showToast('Par 1 fijado como capa activa para entrelazado', 'success')">
<i class="fa-solid fa-thumbtack text-2xs text-red-300"></i>
<span>📌 Usar</span>
</button>
<button class="py-1 bg-indigo-900/80 hover:bg-indigo-800 active:scale-95 border border-indigo-500 text-white rounded text-[10px] font-bold flex items-center justify-center space-x-1" onclick="generate18ViewsSequence()">
<i class="fa-solid fa-clone text-2xs text-sky-400"></i>
<span>👁 Vistas</span>
</button>
</div>
</div>
</div>
<!-- Cuadrícula de botones estándar de NEXUS LENS 3D -->
<div class="grid grid-cols-3 gap-1">
<button class="py-1 bg-[#2b1b1e] hover:bg-[#3b2327] border border-red-800/60 text-red-300 rounded text-2xs font-semibold flex items-center justify-center space-x-1 active:scale-95 transition" onclick="clearPairs()">
<i class="fa-solid fa-trash-can text-3xs"></i>
<span>🗑 Limpiar</span>
</button>
<button class="py-1 bg-[#12262a] hover:bg-[#1a383e] border border-teal-700/60 text-teal-300 rounded text-2xs font-semibold flex items-center justify-center space-x-1 active:scale-95 transition" onclick="toggleModal(true)">
<i class="fa-solid fa-plus text-3xs"></i>
<span>+ Agregar</span>
</button>
<button class="py-1 bg-[#1e1c36] hover:bg-[#2b274c] border border-purple-800/60 text-purple-300 rounded text-2xs font-semibold flex items-center justify-center space-x-1 active:scale-95 transition" onclick="showToast('Seleccionando todos los pares de mapa de profundidad', 'info')">
<i class="fa-solid fa-boxes-stacked text-3xs"></i>
<span>📦 Todos</span>
</button>
</div>
<div class="grid grid-cols-3 gap-1">
<button class="py-1 bg-emerald-900/70 border border-emerald-500 rounded text-emerald-300 text-2xs font-bold flex items-center justify-center space-x-1 active:scale-95 transition" id="btnToggle3D" onclick="toggle3DMode()">
<i class="fa-solid fa-eye text-3xs"></i>
<span>👁 3D</span>
</button>
<button class="py-1 bg-[#321c1c] hover:bg-[#442222] border border-red-900 rounded text-red-300 text-2xs font-semibold flex items-center justify-center space-x-1 active:scale-95 transition" onclick="clearPairs()">
<i class="fa-solid fa-xmark text-3xs"></i>
<span>✕ Eliminar</span>
</button>
<button class="py-1 bg-[#152e29] hover:bg-[#1c3c35] border border-teal-700 rounded text-teal-300 text-2xs font-semibold flex items-center justify-center space-x-1 active:scale-95 transition" onclick="acumularVistas()">
<i class="fa-solid fa-inbox text-3xs"></i>
<span>📥 Acumular</span>
</button>
</div>
<!-- Checkbox: Activar 3D en monitor -->
<div class="flex items-center space-x-1.5 pt-0.5">
<input checked="" class="rounded bg-appDark border-sky-600 text-sky-500 focus:ring-0 w-3 h-3 cursor-pointer" id="chk3dmonitor" onchange="toggleMonitor3D(this.checked)" type="checkbox"/>
<label class="text-[10px] text-gray-300 font-medium cursor-pointer" for="chk3dmonitor">Activar 3D en monitor (Anaglifo/Estéreo)</label>
</div>
<!-- Botones Centrar 3D y Mover Guía -->
<div class="flex items-center space-x-1.5 pt-1 border-t border-appBorder">
<button class="flex-1 py-1 bg-sky-950 hover:bg-sky-900 active:scale-95 border border-sky-700 text-sky-200 rounded text-2xs font-semibold flex items-center justify-center space-x-1 transition" onclick="centrarImagen3D()">
<i class="fa-solid fa-crosshairs text-sky-400 text-3xs"></i>
<span>🎯 Centrar 3D</span>
</button>
<button class="flex-1 py-1 bg-[#1b2330] hover:bg-[#253245] active:scale-95 border border-slate-700 text-slate-300 rounded text-2xs font-semibold flex items-center justify-center space-x-1 transition" onclick="togglePanGuide()">
<i class="fa-solid fa-hand text-amber-400 text-3xs"></i>
<span id="lblBtnPan">✋ Mover Guía</span>
</button>
</div>
<!-- Sliders de Precisión Z-Buffer con valores en vivo -->
<div class="space-y-2 pt-1 border-t border-appBorder font-mono">
<div>
<div class="flex justify-between text-2xs text-gray-300 mb-0.5">
<span>Frente (Front Plane):</span>
<span class="text-sky-400 font-bold" id="valFrente">28</span>
</div>
<input class="w-full" id="slFrente" max="100" min="-100" oninput="updateSliderParam('frente', this.value)" type="range" value="28"/>
</div>
<div>
<div class="flex justify-between text-2xs text-gray-300 mb-0.5">
<span>Profundidad (Parallax):</span>
<span class="text-sky-400 font-bold" id="valProfundidad">28</span>
</div>
<input class="w-full" id="slProfundidad" max="100" min="-100" oninput="updateSliderParam('profundidad', this.value)" type="range" value="28"/>
</div>
<div>
<div class="flex justify-between text-2xs text-gray-300 mb-0.5">
<span>Fuerza (Displacement):</span>
<span class="text-sky-400 font-bold" id="valFuerza">3</span>
</div>
<input class="w-full" id="slFuerza" max="10" min="0" oninput="updateSliderParam('fuerza', this.value)" type="range" value="3"/>
</div>
<div>
<div class="flex justify-between text-2xs text-gray-300 mb-0.5">
<span>Suavizado Bilateral:</span>
<span class="text-sky-400 font-bold" id="valSuavizado">30</span>
</div>
<input class="w-full" id="slSuavizado" max="100" min="0" oninput="updateSliderParam('suavizado', this.value)" type="range" value="30"/>
</div>
<div>
<div class="flex justify-between text-2xs text-gray-300 mb-0.5">
<span>Rigidez (anti-deformar):</span>
<span class="text-sky-400 font-bold" id="valRigidez">35</span>
</div>
<input class="w-full" id="slRigidez" max="100" min="0" oninput="updateSliderParam('rigidez', this.value)" type="range" value="35"/>
</div>
<div>
<div class="flex justify-between text-2xs text-gray-300 mb-0.5">
<span>Exagerar en impresión (x):</span>
<span class="text-amber-400 font-bold" id="valExagerar">1.2x</span>
</div>
<input class="w-full" id="slExagerar" max="5.0" min="1.0" oninput="updateSliderParam('exagerar', this.value)" step="0.1" type="range" value="1.2"/>
</div>
<!-- Color Tuning RGB: Tono / Saturación -->
<div class="pt-1 border-t border-appBorder">
<span class="text-2xs font-sans text-gray-300 font-semibold block mb-1">RGB — Tono / Saturación</span>
<div class="flex items-center space-x-1.5 mb-1">
<span class="text-3xs text-gray-400 w-8">Tono</span>
<input class="flex-1" id="slHue" max="180" min="-180" oninput="updateColorFilter('hue', this.value)" type="range" value="0"/>
<span class="text-3xs text-sky-400 w-5 text-right" id="valHue">0</span>
</div>
<div class="flex items-center space-x-1.5">
<span class="text-3xs text-gray-400 w-8">Satur.</span>
<input class="flex-1" id="slSat" max="200" min="0" oninput="updateColorFilter('sat', this.value)" type="range" value="100"/>
<span class="text-3xs text-sky-400 w-5 text-right" id="valSat">100</span>
</div>
</div>
<!-- Botón Curvas Mapa (Gris) -->
<button class="w-full py-1.5 mt-1 bg-gray-800 hover:bg-gray-700 border border-gray-600 rounded text-2xs font-semibold text-gray-200 active:scale-95 transition flex items-center justify-center space-x-1.5" onclick="toggleCurvesModal(true)">
<i class="fa-solid fa-chart-line text-sky-400 text-3xs"></i>
<span>📈 Curvas mapa (gris)</span>
</button>
</div>
</div>
<!-- SUB-VISTA B: GESTOR COMPLETO DE CAPAS (Se muestra al hacer clic en 🗂 Capas) -->
<div class="p-2 space-y-2.5 hidden" id="viewCapasManager">
<div class="text-[11px] font-bold uppercase tracking-wider text-sky-400 flex items-center justify-between border-b border-appBorder pb-1">
<span>GESTOR DE CAPAS MULTINIVEL</span>
<i class="fa-solid fa-layer-group text-xs"></i>
</div>
<!-- Botonera de importación -->
<div class="grid grid-cols-2 gap-1">
<button class="py-1.5 bg-[#1b2533] hover:bg-[#253346] border border-sky-600/60 rounded text-sky-300 font-semibold text-2xs flex items-center justify-center space-x-1" onclick="showToast('Importador PSD: Leyendo capas compuestas...', 'info')">
<span>🎨 Abrir PSD</span>
</button>
<button class="py-1.5 bg-[#1b2533] hover:bg-[#253346] border border-sky-600/60 rounded text-sky-300 font-semibold text-2xs flex items-center justify-center space-x-1" onclick="filePickerSim()">
<span>🖼️ Agregar Imagen</span>
</button>
</div>
<div class="grid grid-cols-2 gap-1">
<button class="py-1 bg-gray-800 hover:bg-gray-700 border border-gray-600 rounded text-gray-300 text-2xs flex items-center justify-center space-x-1" onclick="showToast('Cargando secuencia de fotogramas GIF...', 'info')">
<span>🎞️ Abrir GIF</span>
</button>
<button class="py-1 bg-gray-800 hover:bg-gray-700 border border-gray-600 rounded text-gray-300 text-2xs flex items-center justify-center space-x-1" onclick="showToast('Decodificando pistas MP4 a fotogramas...', 'info')">
<span>🎬 Abrir MP4</span>
</button>
</div>
<!-- Botonera de Gestión de Capas -->
<div class="grid grid-cols-3 gap-1 pt-1 border-t border-appBorder">
<button class="py-1 bg-red-950/70 hover:bg-red-900 border border-red-700 text-red-200 rounded text-3xs font-semibold" onclick="showToast('Capa seleccionada eliminada', 'info')">
          🗑️ Eliminar
        </button>
<button class="py-1 bg-red-950/90 hover:bg-red-800 border border-red-600 text-red-200 rounded text-3xs font-semibold" onclick="showToast('Todas las capas han sido limpiadas', 'info')">
          🗑️ Eliminar Todas
        </button>
<button class="py-1 bg-teal-950/80 hover:bg-teal-900 border border-teal-600 text-teal-300 rounded text-3xs font-semibold" onclick="acumularVistas()">
          📥 Acumular
        </button>
</div>
<!-- Lista de Capas con Visibilidad y Parallax -->
<div class="space-y-1 font-mono text-2xs pt-1">
<span class="text-gray-400 text-3xs font-sans font-semibold">CAPAS DEL PROYECTO:</span>
<!-- Capa 1: Foreground -->
<div class="bg-appCard border border-sky-500 rounded p-1.5 flex items-center justify-between">
<div class="flex items-center space-x-1.5">
<button class="text-sky-400 hover:text-white" onclick="toggleLayerEye(this)"><i class="fa-solid fa-eye text-xs"></i></button>
<span class="text-sky-300 font-sans font-medium text-[11px]">Capa 1: Personaje</span>
</div>
<div class="flex items-center space-x-1">
<button class="w-4 h-4 bg-gray-800 hover:bg-gray-700 rounded text-3xs flex items-center justify-center text-gray-300" onclick="adjustLayerParallax(1, 0.5)">+</button>
<button class="w-4 h-4 bg-gray-800 hover:bg-gray-700 rounded text-3xs flex items-center justify-center text-gray-300" onclick="adjustLayerParallax(1, -0.5)">-</button>
<button class="px-1 bg-indigo-900 border border-indigo-500 text-white rounded text-3xs font-bold" onclick="showToast('Configurando Parallax Dinámico para Capa 1', 'info')">P</button>
</div>
</div>
<!-- Capa 2: Fondo -->
<div class="bg-appCard border border-gray-700 rounded p-1.5 flex items-center justify-between">
<div class="flex items-center space-x-1.5">
<button class="text-sky-400 hover:text-white" onclick="toggleLayerEye(this)"><i class="fa-solid fa-eye text-xs"></i></button>
<span class="text-gray-300 font-sans font-medium text-[11px]">Capa 2: Escenario Fondo</span>
</div>
<div class="flex items-center space-x-1">
<button class="w-4 h-4 bg-gray-800 hover:bg-gray-700 rounded text-3xs flex items-center justify-center text-gray-300" onclick="adjustLayerParallax(2, 0.5)">+</button>
<button class="w-4 h-4 bg-gray-800 hover:bg-gray-700 rounded text-3xs flex items-center justify-center text-gray-300" onclick="adjustLayerParallax(2, -0.5)">-</button>
<button class="px-1 bg-indigo-900 border border-indigo-500 text-white rounded text-3xs font-bold" onclick="showToast('Configurando Parallax Dinámico para Capa 2', 'info')">P</button>
</div>
</div>
<!-- Capa 3: Overlay Brillo -->
<div class="bg-appCard border border-gray-700 rounded p-1.5 flex items-center justify-between">
<div class="flex items-center space-x-1.5">
<button class="text-sky-400 hover:text-white" onclick="toggleLayerEye(this)"><i class="fa-solid fa-eye text-xs"></i></button>
<span class="text-gray-300 font-sans font-medium text-[11px]">Capa 3: Overlay Holográfico</span>
</div>
<div class="flex items-center space-x-1">
<button class="w-4 h-4 bg-gray-800 hover:bg-gray-700 rounded text-3xs flex items-center justify-center text-gray-300" onclick="adjustLayerParallax(3, 0.5)">+</button>
<button class="w-4 h-4 bg-gray-800 hover:bg-gray-700 rounded text-3xs flex items-center justify-center text-gray-300" onclick="adjustLayerParallax(3, -0.5)">-</button>
<button class="px-1 bg-indigo-900 border border-indigo-500 text-white rounded text-3xs font-bold" onclick="showToast('Configurando Parallax Dinámico para Capa 3', 'info')">P</button>
</div>
</div>
</div>
<button class="w-full py-1.5 bg-[#0369a1] hover:bg-[#0284c7] text-white rounded font-bold text-xs mt-2 transition" onclick="switchMainTab('3dflip')">
        ← Volver a Entorno 3D Z-Buffer
      </button>
</div>
</aside>
<!-- ============================================================ -->
<!-- 2. VISOR CENTRAL Y DISEÑO FIJO (Requisito 1: NO MAREAR)      -->
<!-- ============================================================ -->
<section class="flex-1 flex flex-col bg-appDark overflow-hidden relative" data-purpose="central-canvas-and-interlacing">
<!-- Header de Telemetría dentro del Visor Central -->
<div class="h-6 bg-[#0a0e16] border-b border-appBorder flex items-center justify-between px-3 select-none shrink-0 font-mono text-[11px]">
<div class="flex items-center space-x-2">
<span class="text-emerald-400 font-bold">●</span>
<span class="text-sky-400 font-bold uppercase tracking-wider text-2xs">PREVIEW:</span>
<span class="font-semibold text-gray-200" id="previewTitleDim">Mesa 63.5×88.9 mm</span>
</div>
<div class="flex items-center space-x-2">
<span class="px-1.5 py-0.2 bg-[#0d2238] border border-sky-600 text-sky-300 rounded text-[10px] font-bold" id="liveAngleIndicator">-2.1° TILT (H)</span>
<span class="px-1.5 py-0.2 bg-black/80 border border-gray-700 text-sky-400 rounded text-[10px]">
          FOTOGRAMA: <span class="text-amber-300 font-bold" id="hudCurrentView">#8</span> / 18
        </span>
</div>
</div>
<!-- Viewport: Diseño Fijo, SIN seguimiento mareante del mouse -->
<div class="flex-1 canvas-bg-pattern relative flex items-center justify-center p-2 overflow-hidden select-none" id="viewportContainer">
<!-- Mesa de Trabajo Física (63.5 x 88.9 mm a escala de previsualización) -->
<div class="relative bg-black shadow-2xl border border-gray-700 flex flex-col items-center p-1.5 rounded-sm" id="worktablePlaten" style="width: 320px; height: 440px;">
<!-- Etiqueta Superior de Dimensión -->
<div class="text-[9.5px] font-mono text-gray-300 tracking-wide mb-1 flex items-center space-x-2">
<span>63.5 × 88.9 mm</span>
<span class="text-sky-400 font-bold" id="liveTiltSubBadge">-2.1° TILT (H)</span>
</div>
<!-- Guía Óptica Lenticular Verde Superior (Marcas de paso fijas) -->
<div class="w-full h-3.5 lenticular-guide-bar rounded-[1px] flex justify-between items-center px-1 border border-emerald-400 transition-all" id="topGuideBar">
<div class="w-3 h-2 bg-emerald-200 border border-black"></div>
<div class="w-8 h-2 bg-emerald-100 border border-black"></div>
<div class="w-3 h-2 bg-emerald-200 border border-black"></div>
</div>
<!-- Marco de Arte Visual (Visor de fotograma calibrado) -->
<div class="flex-1 w-full my-1 relative border border-gray-800 overflow-hidden bg-black flex items-center justify-center" id="lenticularFrame">
<!-- Capa Fondo Parallax -->
<div class="absolute inset-0 bg-cover bg-center pointer-events-none transition-transform duration-75" id="layerBackground" style="background-image: url('https://lh3.googleusercontent.com/aida-public/AB6AXuDLSkvOItuK87svbh0i2Uz5Ps5qPX0vE8Shx0Rt4NVbBr5MLzrgRpd-GiEmUU24DAOqdj1Jd6LUQLz-hJH7xW6mZfymN14bfPCiVGyMseLcZYBP9e8AzzIbqejC5AuGi3O9ACsow0iBp7U81cPOWa9HN0OR4Li9Gjh7QyE3OilMU6xeho8d4qFfGrk6STbakulYAu-2NFRZMYP9nWtmqGBBXUL6BhQ0jQvDZ3JuLit0fPPEH1-fZd7WRIlaHOEQe5OK7g'); background-position: 50% 30%; filter: blur(0.6px) brightness(0.92); transform: scale(1.36) translate3d(-3.8px, 0px, -15px);"></div>
<!-- Capa Principal del Personaje 3D -->
<img alt="Lenticular Composition" class="w-full h-full object-cover scale-[1.38] object-center select-none pointer-events-none relative z-10 transition-transform duration-75 will-change-transform" id="main3DImage" src="https://lh3.googleusercontent.com/aida-public/AB6AXuDLSkvOItuK87svbh0i2Uz5Ps5qPX0vE8Shx0Rt4NVbBr5MLzrgRpd-GiEmUU24DAOqdj1Jd6LUQLz-hJH7xW6mZfymN14bfPCiVGyMseLcZYBP9e8AzzIbqejC5AuGi3O9ACsow0iBp7U81cPOWa9HN0OR4Li9Gjh7QyE3OilMU6xeho8d4qFfGrk6STbakulYAu-2NFRZMYP9nWtmqGBBXUL6BhQ0jQvDZ3JuLit0fPPEH1-fZd7WRIlaHOEQe5OK7g" style="transform: scale(1.38) translate3d(2.7px, 0px, 15px);"/>
<!-- Brillo Especular Lenticular -->
<div class="absolute inset-0 specular-shine z-20 opacity-30 transition-transform duration-75" id="lenticularShine"></div>
<!-- Malla de Líneas Ópticas (Entrelazado vertical/horizontal) -->
<div class="absolute inset-0 pointer-events-none opacity-20 z-30 lenticular-vertical-overlay" id="interlacedOverlay"></div>
<!-- Overlay HUD interno del fotograma -->
<div class="absolute top-1 left-1.5 z-40 bg-black/85 font-mono text-[9px] px-1.5 py-0.5 rounded border border-gray-700 text-sky-400">
            FOTOGRAMA: <span class="text-amber-300 font-bold" id="hudCurrentViewInner">#8</span> / 18
          </div>
</div>
<!-- Guía Óptica Lenticular Verde Inferior -->
<div class="w-full h-3.5 lenticular-guide-bar rounded-[1px] flex justify-between items-center px-1 border border-emerald-400 transition-all" id="bottomGuideBar">
<div class="w-3 h-2 bg-emerald-200 border border-black"></div>
<div class="w-8 h-2 bg-emerald-100 border border-black"></div>
<div class="w-3 h-2 bg-emerald-200 border border-black"></div>
</div>
<!-- Metadata de Impresión Física -->
<div class="text-[9px] font-mono text-gray-400 tracking-tight mt-1">
          63.5×88.9 mm LPI 75.610 DPI 720
        </div>
</div>
<!-- Botonera de Reproducción Flotante (Abajo a la derecha del visor) -->
<div class="absolute bottom-3 right-5 flex items-center space-x-2 z-40">
<button class="px-4 py-1.5 bg-[#141b27]/90 hover:bg-[#1e2838] active:scale-95 border border-gray-600 rounded text-gray-200 font-bold text-xs flex items-center space-x-1.5 backdrop-blur-sm shadow-md transition" id="btnPlayNormal" onclick="togglePlayMode('sequential')">
<i class="fa-solid fa-play text-sky-400 text-2xs" id="icoPlayNormal"></i>
<span id="txtPlayNormal">▶ Play</span>
</button>
<button class="px-5 py-1.5 bg-emerald-700/90 hover:bg-emerald-600 active:scale-95 border border-emerald-400 text-white rounded font-bold text-xs flex items-center space-x-1.5 backdrop-blur-sm shadow-lg shadow-emerald-950/60 transition" id="btnPlay3D" onclick="togglePlayMode('pingpong')">
<i class="fa-solid fa-play text-yellow-300 text-2xs" id="icoPlay3D"></i>
<span id="txtPlay3D">▶ Play 3D</span>
</button>
</div>
</div>
<!-- Barra de Estado de Exportación y Totales -->
<div class="bg-[#0b1018] border-t border-b border-appBorder px-3 py-1 flex items-center justify-between shrink-0" data-purpose="status-notification">
<div class="flex items-center space-x-2 text-[11px] font-mono text-gray-200">
<i class="fa-solid fa-square-check text-emerald-400 text-sm"></i>
<span>Exportado: <strong class="text-sky-300" id="statusExportFile">nexus_lenticular_720dpi.png</strong> (17.26 MB)</span>
</div>
<div class="text-[10px] text-gray-400 font-mono">
        Total Vistas: <span class="text-amber-400 font-bold" id="statusTotalVistas">18</span>
</div>
</div>
<!-- ============================================================ -->
<!-- 4. CARRUSEL INFERIOR DE 18 VISTAS TOTALMENTE INTERACTIVO     -->
<!-- ============================================================ -->
<div class="bg-appPanel border-t border-appBorder p-2 shrink-0" data-purpose="generated-views-strip">
<!-- Barra de herramientas superior del carrusel -->
<div class="flex items-center justify-between mb-1.5">
<div class="flex items-center space-x-3">
<span class="text-[11px] font-bold text-amber-400 flex items-center">
<i class="fa-solid fa-film text-xs mr-1 text-sky-400"></i>VISTAS GENERADAS (18)
          </span>
<div class="flex items-center space-x-1.5 font-mono text-2xs">
<button class="px-2 py-0.5 bg-gray-800 hover:bg-gray-700 active:scale-95 border border-gray-600 rounded text-gray-200" id="btnPlayStrip" onclick="togglePlayMode('pingpong')">
<span id="txtStripPlay">▶ Play</span>
</button>
<span class="text-gray-400 text-3xs">FPS:</span>
<input class="w-14" id="slFPS" max="60" min="1" oninput="updateFPS(this.value)" type="range" value="30"/>
<span class="text-gray-300 text-3xs" id="lblFPS">30</span>
<span class="text-gray-400 text-3xs ml-1">Zoom:</span>
<button class="w-4 h-4 bg-gray-800 hover:bg-gray-700 border border-gray-600 rounded flex items-center justify-center text-3xs" onclick="adjustZoom(0.1)">+</button>
<button class="w-4 h-4 bg-gray-800 hover:bg-gray-700 border border-gray-600 rounded flex items-center justify-center text-3xs" onclick="adjustZoom(-0.1)">-</button>
<span class="text-gray-400 text-3xs" id="lblZoomLevel">1.00x</span>
</div>
</div>
<!-- Botones de gestión sobre el carrusel -->
<div class="flex items-center space-x-1">
<button class="px-2 py-0.5 bg-red-950/70 hover:bg-red-900 active:scale-95 border border-red-700 rounded text-red-200 text-2xs font-semibold flex items-center space-x-1" onclick="deleteActiveView()">
<i class="fa-solid fa-trash-can text-3xs"></i>
<span>🗑 Eliminar</span>
</button>
<button class="px-2 py-0.5 bg-sky-950/70 hover:bg-sky-900 active:scale-95 border border-sky-700 rounded text-sky-200 text-2xs font-semibold flex items-center space-x-1" onclick="duplicateActiveView()">
<i class="fa-solid fa-copy text-3xs"></i>
<span>📋 Duplicar</span>
</button>
<button class="px-2 py-0.5 bg-teal-950/70 hover:bg-teal-900 active:scale-95 border border-teal-700 rounded text-teal-200 text-2xs font-semibold flex items-center space-x-1" onclick="extractView()">
<i class="fa-solid fa-arrow-up-from-bracket text-3xs"></i>
<span>📤 Extraer</span>
</button>
<button class="px-2 py-0.5 bg-indigo-950/70 hover:bg-indigo-900 active:scale-95 border border-indigo-700 rounded text-indigo-200 text-2xs font-semibold flex items-center space-x-1" onclick="reverseViews()">
<i class="fa-solid fa-repeat text-3xs"></i>
<span>🔄 Invertir</span>
</button>
<button class="px-2 py-0.5 bg-amber-950/80 hover:bg-amber-900 active:scale-95 border border-amber-600 text-amber-300 rounded text-2xs font-bold flex items-center space-x-1" onclick="acumularVistas()">
<i class="fa-solid fa-arrow-up text-3xs"></i>
<span>⬆ A Acumuladas</span>
</button>
</div>
</div>
<!-- Contenedor con las 18 miniaturas numeradas (#1 a #18) -->
<div class="flex space-x-1.5 overflow-x-auto pb-1 select-none" id="viewsStripContainer">
<!-- Renderizado dinámicamente con interacción inmediata -->
</div>
<!-- Tira de Alineación Amarilla -->
<div class="w-full h-1 yellow-strip rounded-full my-1"></div>
<!-- ============================================================ -->
<!-- 3. BOTONERA INFERIOR TOTALMENTE FUNCIONAL                    -->
<!-- ============================================================ -->
<div class="flex items-center justify-between pt-1 font-semibold text-xs">
<div class="flex items-center space-x-1.5">
<!-- Botón ✨ Generar Vistas (18) -->
<button class="px-4 py-1.5 bg-emerald-900/80 hover:bg-emerald-800 active:scale-95 border border-emerald-500 rounded text-emerald-200 flex items-center space-x-1.5 transition shadow" onclick="generate18ViewsSequence()">
<i class="fa-solid fa-wand-magic-sparkles text-xs"></i>
<span>✨ Generar Vistas (18)</span>
</button>
<!-- Botón 🗑 LIMPIAR -->
<button class="px-4 py-1.5 bg-[#2c1c1f] hover:bg-[#3d272b] active:scale-95 border border-red-800 rounded text-red-300 flex items-center space-x-1.5 transition" onclick="clearViewsStrip()">
<i class="fa-solid fa-trash-can text-xs"></i>
<span>🗑 LIMPIAR</span>
</button>
</div>
<div class="flex items-center space-x-1.5">
<!-- Botón ⚡ Vertical (Supersampling 8x) -->
<button class="px-5 py-1.5 bg-sky-800 hover:bg-sky-700 border border-sky-400 text-white rounded flex items-center space-x-1.5 transition shadow-inner" id="btnInterlaceVertical" onclick="executeVerticalInterlace()">
<i class="fa-solid fa-grip-lines-vertical text-xs"></i>
<span>⚡ Vertical</span>
</button>
<!-- Botón ⚡ Horiz -->
<button class="px-5 py-1.5 bg-sky-950 hover:bg-sky-900 border border-sky-600 rounded text-sky-200 flex items-center space-x-1.5 transition" id="btnInterlaceHorizontal" onclick="executeHorizontalInterlace()">
<i class="fa-solid fa-grip-lines text-xs"></i>
<span>⚡ Horiz</span>
</button>
<!-- Botón 💾 Exportar Interlace Lenticular (Esmeralda) -->
<button class="px-5 py-1.5 bg-emerald-700 hover:bg-emerald-600 active:scale-95 border border-emerald-400 text-white rounded font-bold flex items-center space-x-1.5 shadow-lg shadow-emerald-950/40 transition" onclick="openExportDialog()">
<i class="fa-solid fa-floppy-disk text-xs"></i>
<span>💾 Exportar Interlace Lenticular</span>
</button>
</div>
</div>
</div>
</section>
<!-- ============================================================ -->
<!-- 6. PANEL DERECHO: VISTAS ACUMULADAS Y TRANSFORMACIONES       -->
<!-- ============================================================ -->
<aside class="w-72 bg-appPanel border-l border-appBorder flex flex-col shrink-0 overflow-y-auto" data-purpose="right-properties">
<div class="p-2 space-y-2.5">
<!-- Cabecera -->
<div class="text-[11px] font-bold uppercase tracking-wider text-gray-300 border-b border-appBorder pb-1 flex justify-between items-center">
<span>DIMENSIÓN 3D &amp; PROPIEDADES</span>
<i class="fa-solid fa-sliders text-sky-400 text-xs"></i>
</div>
<!-- MÓDULO: VISTAS ACUMULADAS -->
<div class="bg-appCard border border-appBorderLight/50 rounded p-2 space-y-1.5">
<div class="flex justify-between text-2xs text-amber-400 font-bold">
<span><i class="fa-solid fa-layer-group mr-1"></i>● VISTAS ACUMULADAS (<span id="lblAcumuladasCount">8</span>)</span>
</div>
<div class="text-2xs text-amber-300 font-semibold flex items-center">
<span><i class="fa-solid fa-folder-open mr-1"></i>Buffer de Mezcla 3D Activo</span>
</div>
<div class="grid grid-cols-1 gap-1">
<button class="w-full py-1 bg-emerald-900/70 hover:bg-emerald-800 active:scale-95 border border-emerald-600 text-emerald-200 rounded text-2xs font-bold flex items-center justify-center space-x-1 transition" onclick="moverAVistas()">
<i class="fa-solid fa-arrow-down"></i>
<span>↓ Mover a Vistas</span>
</button>
<button class="w-full py-1 bg-sky-950 hover:bg-sky-900 border border-sky-600 text-sky-200 rounded text-2xs font-bold flex items-center justify-center space-x-1" onclick="executeVerticalInterlace()">
<i class="fa-solid fa-bolt text-amber-400"></i>
<span>⚡ Vertical</span>
</button>
<button class="w-full py-1 bg-[#321c20] hover:bg-[#44262c] active:scale-95 border border-red-800 text-red-300 rounded text-2xs font-bold flex items-center justify-center space-x-1" onclick="limpiarAcumuladas()">
<i class="fa-solid fa-trash"></i>
<span>🗑 LIMPIAR</span>
</button>
</div>
<!-- Contenedor con miniatura en descanso (3D_View_4) -->
<div class="flex items-center space-x-2 pt-1 border-t border-appBorder">
<div class="w-12 h-12 bg-black border border-gray-600 rounded overflow-hidden cursor-pointer hover:border-sky-400 group" onclick="showToast('Restaurando fotograma acumulado 3D_View_4', 'info')">
<div class="w-full h-full bg-cover bg-center group-hover:scale-105 transition" style="background-image: url('https://lh3.googleusercontent.com/aida-public/AB6AXuDLSkvOItuK87svbh0i2Uz5Ps5qPX0vE8Shx0Rt4NVbBr5MLzrgRpd-GiEmUU24DAOqdj1Jd6LUQLz-hJH7xW6mZfymN14bfPCiVGyMseLcZYBP9e8AzzIbqejC5AuGi3O9ACsow0iBp7U81cPOWa9HN0OR4Li9Gjh7QyE3OilMU6xeho8d4qFfGrk6STbakulYAu-2NFRZMYP9nWtmqGBBXUL6BhQ0jQvDZ3JuLit0fPPEH1-fZd7WRIlaHOEQe5OK7g'); background-position: 50% 30%;"></div>
</div>
<div class="flex flex-col">
<span class="text-3xs font-mono text-gray-300 font-bold">3D_View_4</span>
<span class="text-[9px] text-gray-400 font-mono">1800×2520 px</span>
</div>
</div>
</div>
<!-- MÓDULO: TRANSFORMACIÓN GEOMÉTRICA & TORNASOL -->
<div class="space-y-2 pt-1 border-t border-appBorder">
<!-- Rotación Estática -->
<div class="space-y-1 text-2xs">
<div class="font-semibold text-gray-300">Rotación Estática:</div>
<div class="flex items-center space-x-1">
<input class="flex-1" id="slRotEstatica" max="180" min="-180" oninput="updateStaticRotation(this.value)" type="range" value="0"/>
<span class="font-mono text-3xs text-sky-400" id="lblRotEstatica">0°</span>
</div>
<div class="flex items-center justify-between mt-1">
<span class="text-gray-400">Ángulo:</span>
<div class="flex items-center bg-appDark border border-gray-700 rounded px-1.5 py-0.5">
<select class="bg-transparent text-gray-200 font-mono text-2xs outline-none cursor-pointer" id="selAnguloPreset" onchange="onSelectAnglePreset(this.value)">
<option value="-180">-180°</option>
<option value="-90">-90°</option>
<option selected="" value="0">0°</option>
<option value="90">90°</option>
<option value="180">180°</option>
</select>
</div>
</div>
</div>
<!-- Tamaño de Capa (mm) -->
<div class="space-y-1.5 pt-1 border-t border-appBorder text-2xs font-mono">
<div class="font-sans font-semibold text-gray-300">Tamaño de Capa (mm):</div>
<div class="flex items-center justify-between">
<span class="font-sans text-gray-400">Ancho:</span>
<div class="flex items-center bg-appDark border border-gray-700 rounded px-2 py-0.5">
<span class="text-gray-200" id="valAncho">28.00</span>
<div class="flex flex-col ml-2 text-3xs text-gray-400 leading-none">
<i class="fa-solid fa-chevron-up hover:text-white cursor-pointer" onclick="stepDimension('ancho', 0.5)"></i>
<i class="fa-solid fa-chevron-down hover:text-white cursor-pointer" onclick="stepDimension('ancho', -0.5)"></i>
</div>
</div>
</div>
<div class="flex items-center justify-between">
<span class="font-sans text-gray-400">Alto:</span>
<div class="flex items-center bg-appDark border border-gray-700 rounded px-2 py-0.5">
<span class="text-gray-200" id="valAlto">27.04</span>
<div class="flex flex-col ml-2 text-3xs text-gray-400 leading-none">
<i class="fa-solid fa-chevron-up hover:text-white cursor-pointer" onclick="stepDimension('alto', 0.5)"></i>
<i class="fa-solid fa-chevron-down hover:text-white cursor-pointer" onclick="stepDimension('alto', -0.5)"></i>
</div>
</div>
</div>
</div>
<!-- Zoom de Capa -->
<div class="space-y-1 pt-1 border-t border-appBorder text-2xs">
<div class="flex items-center space-x-1.5 font-semibold text-gray-300">
<span>Zoom de Capa</span>
</div>
<div class="flex items-center space-x-1.5">
<input class="rounded bg-appDark border-gray-600 text-sky-500 focus:ring-0 w-3 h-3 cursor-pointer" id="chkZoom" onchange="toggleLayerZoom(this.checked)" type="checkbox"/>
<label class="text-gray-400 text-3xs cursor-pointer" for="chkZoom">Activar zoom</label>
</div>
<div class="space-y-1 font-mono text-3xs">
<div class="flex items-center justify-between">
<span class="font-sans text-gray-400">Inicio:</span>
<input class="w-24" id="slZoomIni" max="3" min="0.5" oninput="updateLayerZoomVal('ini', this.value)" step="0.1" type="range" value="1.0"/>
<span class="text-sky-400" id="lblZoomIni">1.0x</span>
</div>
<div class="flex items-center justify-between">
<span class="font-sans text-gray-400">Fin:</span>
<input class="w-24" id="slZoomFin" max="3" min="0.5" oninput="updateLayerZoomVal('fin', this.value)" step="0.1" type="range" value="1.0"/>
<span class="text-sky-400" id="lblZoomFin">1.0x</span>
</div>
</div>
<div class="flex justify-end space-x-1 pt-0.5">
<button class="px-2 py-0.5 bg-gray-800 hover:bg-gray-700 border border-gray-700 rounded text-3xs font-mono" onclick="adjustZoom(0.05)">+</button>
<button class="px-2 py-0.5 bg-gray-800 hover:bg-gray-700 border border-gray-700 rounded text-3xs font-mono" onclick="adjustZoom(-0.05)">-</button>
</div>
</div>
<!-- Rotación Dinámica -->
<div class="space-y-1 pt-1 border-t border-appBorder text-2xs">
<div class="font-semibold text-gray-300">Rotación Dinámica</div>
<div class="flex items-center space-x-1.5">
<input class="rounded bg-appDark border-gray-600 text-sky-500 focus:ring-0 w-3 h-3 cursor-pointer" id="chkGiro" onchange="toggleDynamicSpin(this.checked)" type="checkbox"/>
<label class="text-gray-400 text-3xs cursor-pointer" for="chkGiro">Activar giro</label>
</div>
<div class="space-y-1 font-mono text-3xs">
<div class="flex items-center justify-between">
<span class="font-sans text-gray-400">Inicio:</span>
<input class="w-28" id="slGiroIni" max="360" min="0" oninput="document.getElementById('lblGiroIni').innerText = this.value + '°'" type="range" value="0"/>
<span class="text-sky-400" id="lblGiroIni">0°</span>
</div>
<div class="flex items-center justify-between">
<span class="font-sans text-gray-400">Fin:</span>
<input class="w-28" id="slGiroFin" max="360" min="0" oninput="document.getElementById('lblGiroFin').innerText = this.value + '°'" type="range" value="360"/>
<span class="text-sky-400" id="lblGiroFin">360°</span>
</div>
</div>
<div class="flex space-x-1 pt-0.5">
<button class="flex-1 py-0.5 bg-sky-950 hover:bg-sky-900 border border-sky-800 rounded text-sky-300 text-3xs font-bold" onclick="changeSpinSpeed(1)">+ Más</button>
<button class="flex-1 py-0.5 bg-sky-950 hover:bg-sky-900 border border-sky-800 rounded text-sky-300 text-3xs font-bold" onclick="changeSpinSpeed(-1)">- Menos</button>
</div>
</div>
<!-- Tornasol (Efecto Moiré / Chroma) -->
<div class="space-y-1 pt-1 border-t border-appBorder text-2xs">
<div class="font-semibold text-gray-300">Tornasol (Efecto Moiré/Chroma)</div>
<div class="flex items-center space-x-1.5">
<input class="rounded bg-appDark border-gray-600 text-sky-500 focus:ring-0 w-3 h-3 cursor-pointer" id="chkTornasol" onchange="toggleTornasol(this.checked)" type="checkbox"/>
<label class="text-gray-400 text-3xs cursor-pointer" for="chkTornasol">Activar tornasol</label>
</div>
<div class="text-3xs text-gray-400">Paleta de colores:</div>
<!-- Swatches de Colores -->
<div class="flex items-center space-x-2 py-0.5">
<div class="w-5 h-5 rounded-full bg-red-500 border border-white/40 cursor-pointer shadow hover:scale-110 transition" onclick="selectTornasolColor('#ef4444')"></div>
<div class="w-5 h-5 rounded-full bg-green-500 border border-white/40 cursor-pointer shadow hover:scale-110 transition" onclick="selectTornasolColor('#22c55e')"></div>
<div class="w-5 h-5 rounded-full bg-blue-600 border border-white/40 cursor-pointer shadow ring-1 ring-sky-300 hover:scale-110 transition" onclick="selectTornasolColor('#2563eb')"></div>
<div class="w-5 h-5 rounded-full bg-yellow-400 border border-white/40 cursor-pointer shadow hover:scale-110 transition" onclick="selectTornasolColor('#facc15')"></div>
</div>
<div class="flex items-center justify-between pt-1">
<span class="text-3xs text-gray-400">Editar color:</span>
<div class="flex items-center space-x-1">
<select class="bg-appDark border border-gray-700 text-gray-200 text-3xs rounded py-0.5 px-1.5" id="selColorPreset">
<option>Color 1</option>
<option>Color 2</option>
<option>Color 3</option>
</select>
<button class="px-2 py-0.5 bg-pink-950 border border-pink-700 text-pink-300 rounded text-3xs font-semibold flex items-center space-x-1" onclick="showToast('Editor de paleta de brillo tornasol calibrado', 'info')">
<i class="fa-solid fa-palette text-3xs"></i>
<span>🎨 Editar</span>
</button>
</div>
</div>
<div class="flex items-center justify-between pt-1 font-mono text-3xs">
<span class="font-sans text-gray-400">Velocidad:</span>
<input class="w-24" max="10" min="1" oninput="document.getElementById('lblVelocidadTornasol').innerText = this.value" type="range" value="5"/>
<span class="text-sky-400" id="lblVelocidadTornasol">5 ms</span>
</div>
<div class="flex items-center space-x-1.5 pt-0.5">
<input class="rounded bg-appDark border-gray-600 text-sky-500 focus:ring-0 w-3 h-3 cursor-pointer" id="chkInvertirTornasol" onchange="showToast('Orientación de tornasol invertida', 'info')" type="checkbox"/>
<label class="text-gray-400 text-3xs cursor-pointer" for="chkInvertirTornasol">Invertir</label>
</div>
</div>
</div>
</div>
</aside>
</main>
<!-- ============================================================ -->
<!-- 7. MOTOR JAVASCRIPT: NATIVE DESKTOP STATE ENGINE              -->
<!-- ============================================================ -->
<script>
  // ESTADO GLOBAL DE NEXUS LENS 3D
  const STATE = {
    totalViews: 18,
    activeViewIndex: 7, // Vista #8 inicial (índice 7)
    isPlaying: false,
    playMode: 'pingpong',
    playDirection: 1,
    playTimer: null,
    fps: 30,
    zoomScale: 1.0,
    frente: 28,
    profundidad: 28,
    fuerza: 3,
    suavizado: 30,
    rigidez: 35,
    exagerar: 1.2,
    hue: 0,
    sat: 100,
    is3DActive: true,
    interlaceOrientation: 'vertical',
    limitadorLPI: false,
    isMaximized: false,
    activeTab: '3dflip',
    tornasolActive: false,
    tiltAngles: [-12, -10.5, -9, -7.5, -5.8, -4.2, -2.1, 0, 2.1, 4.2, 5.8, 7.5, 9, 10.5, 12, 13.5, 15, 16.5],
    imgUrl: 'https://lh3.googleusercontent.com/aida-public/AB6AXuDLSkvOItuK87svbh0i2Uz5Ps5qPX0vE8Shx0Rt4NVbBr5MLzrgRpd-GiEmUU24DAOqdj1Jd6LUQLz-hJH7xW6mZfymN14bfPCiVGyMseLcZYBP9e8AzzIbqejC5AuGi3O9ACsow0iBp7U81cPOWa9HN0OR4Li9Gjh7QyE3OilMU6xeho8d4qFfGrk6STbakulYAu-2NFRZMYP9nWtmqGBBXUL6BhQ0jQvDZ3JuLit0fPPEH1-fZd7WRIlaHOEQe5OK7g'
  };

  // Referencias a elementos
  const elMainImg = document.getElementById('main3DImage');
  const elBgLayer = document.getElementById('layerBackground');
  const elShine = document.getElementById('lenticularShine');
  const elHudView = document.getElementById('hudCurrentView');
  const elHudViewInner = document.getElementById('hudCurrentViewInner');
  const elAngleIndicator = document.getElementById('liveAngleIndicator');
  const elLiveTiltSub = document.getElementById('liveTiltSubBadge');
  const elViewsStrip = document.getElementById('viewsStripContainer');

  // Inicializar Carrusel de 18 Vistas
  function renderViewsStrip() {
    elViewsStrip.innerHTML = '';
    for (let i = 0; i < STATE.totalViews; i++) {
      const viewNum = i + 1;
      const isActive = i === STATE.activeViewIndex;
      const card = document.createElement('div');
      card.id = `view-card-${i}`;
      
      // Estilo de tarjeta con resaltado cian brillante si está activa
      card.className = `flex flex-col items-center shrink-0 w-16 bg-black rounded p-0.5 cursor-pointer transition-all ${
        isActive 
          ? 'border-2 border-sky-400 bg-[#091829] shadow-lg shadow-sky-500/30' 
          : 'border border-gray-700 hover:border-sky-500/60'
      }`;
      card.onclick = () => selectView(i, true);

      const bgOffset = 42 + (i * 1.6);

      card.innerHTML = `
        <span class="text-3xs font-mono ${isActive ? 'text-sky-300 font-bold' : 'text-gray-400'}">#${viewNum}${isActive ? ' ★' : ''}</span>
        <div class="w-full h-14 bg-cover bg-center rounded-xs mt-0.5 border border-black" style="background-image: url('${STATE.imgUrl}'); background-position: ${bgOffset}% 30%;"></div>
        <span class="text-[8px] font-mono ${isActive ? 'text-sky-300 font-bold' : 'text-amber-400'} mt-0.5">Vista ${viewNum}</span>
      `;
      elViewsStrip.appendChild(card);
    }
  }

  // Selección directa de fotograma sin bucles de recursión
  function selectView(index, manual = false) {
    if (index < 0 || index >= STATE.totalViews) return;
    STATE.activeViewIndex = index;
    const viewNum = index + 1;

    // Actualizar badges
    elHudView.innerText = `#${viewNum}`;
    elHudViewInner.innerText = `#${viewNum}`;

    const angle = STATE.tiltAngles[index] !== undefined ? STATE.tiltAngles[index] : (index - 8.5) * 1.5;
    const angleText = `${angle.toFixed(1)}° TILT (${STATE.interlaceOrientation === 'vertical' ? 'H' : 'V'})`;
    elAngleIndicator.innerText = angleText;
    if (elLiveTiltSub) elLiveTiltSub.innerText = angleText;

    // Actualizar resaltado en el carrusel
    for (let i = 0; i < STATE.totalViews; i++) {
      const card = document.getElementById(`view-card-${i}`);
      if (!card) continue;
      const isActive = i === index;
      card.className = `flex flex-col items-center shrink-0 w-16 bg-black rounded p-0.5 cursor-pointer transition-all ${
        isActive 
          ? 'border-2 border-sky-400 bg-[#091829] shadow-lg shadow-sky-500/30' 
          : 'border border-gray-700 hover:border-sky-500/60'
      }`;
      const headerSpan = card.querySelector('span:first-child');
      const footerSpan = card.querySelector('span:last-child');
      if (headerSpan) headerSpan.className = `text-3xs font-mono ${isActive ? 'text-sky-300 font-bold' : 'text-gray-400'}`;
      if (headerSpan) headerSpan.innerText = `#${i + 1}${isActive ? ' ★' : ''}`;
      if (footerSpan) footerSpan.className = `text-[8px] font-mono ${isActive ? 'text-sky-300 font-bold' : 'text-amber-400'} mt-0.5`;
    }

    // Scroll suave en carrusel
    if (manual) {
      const activeCard = document.getElementById(`view-card-${index}`);
      if (activeCard) {
        activeCard.scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'center' });
      }
    }

    // Actualizar perspectiva interna del visor (estática para este fotograma)
    applyStaticFrameView(index, angle);
  }

  // Aplicar transformación estática según el fotograma seleccionado (NO marear con el mouse)
  function applyStaticFrameView(index, angleX) {
    if (!STATE.is3DActive) {
      elMainImg.style.transform = `scale(${1.38 * STATE.zoomScale})`;
      elBgLayer.style.transform = `scale(${1.36 * STATE.zoomScale})`;
      elShine.style.transform = `translateX(0%)`;
      return;
    }

    const mult = (STATE.fuerza / 3) * (STATE.profundidad / 28) * STATE.exagerar;
    const shiftForegroundX = -angleX * 1.5 * mult;
    const shiftBackgroundX = angleX * 2.1 * mult;

    if (STATE.interlaceOrientation === 'vertical') {
      elMainImg.style.transform = `scale(${1.38 * STATE.zoomScale}) translate3d(${shiftForegroundX.toFixed(1)}px, 0px, 15px) rotateY(${(angleX * 0.35).toFixed(1)}deg)`;
      elBgLayer.style.transform = `scale(${1.36 * STATE.zoomScale}) translate3d(${shiftBackgroundX.toFixed(1)}px, 0px, -15px) rotateY(${(angleX * 0.15).toFixed(1)}deg)`;
      elShine.style.transform = `translateX(${(-angleX * 7).toFixed(1)}px)`;
    } else {
      elMainImg.style.transform = `scale(${1.38 * STATE.zoomScale}) translate3d(0px, ${shiftForegroundX.toFixed(1)}px, 15px) rotateX(${(angleX * 0.35).toFixed(1)}deg)`;
      elBgLayer.style.transform = `scale(${1.36 * STATE.zoomScale}) translate3d(0px, ${shiftBackgroundX.toFixed(1)}px, -15px) rotateX(${(angleX * 0.15).toFixed(1)}deg)`;
      elShine.style.transform = `translateY(${(-angleX * 7).toFixed(1)}px)`;
    }
  }

  // REPRODUCCIÓN CONTINUA DE FOTOGRAMAS (Ping-Pong y Secuencial)
  function togglePlayMode(mode) {
    if (STATE.isPlaying && STATE.playMode === mode) {
      stopPlayback();
      return;
    }
    STATE.playMode = mode;
    startPlayback();
  }

  function startPlayback() {
    stopPlayback(false);
    STATE.isPlaying = true;
    updatePlayUIState(true);

    const intervalMs = Math.max(16, 1000 / STATE.fps);
    STATE.playTimer = setInterval(() => {
      if (STATE.playMode === 'pingpong') {
        let next = STATE.activeViewIndex + STATE.playDirection;
        if (next >= STATE.totalViews) {
          next = STATE.totalViews - 2;
          STATE.playDirection = -1;
        } else if (next < 0) {
          next = 1;
          STATE.playDirection = 1;
        }
        selectView(next, true);
      } else {
        let next = (STATE.activeViewIndex + 1) % STATE.totalViews;
        selectView(next, true);
      }
    }, intervalMs);
  }

  function stopPlayback(resetButtons = true) {
    if (STATE.playTimer) {
      clearInterval(STATE.playTimer);
      STATE.playTimer = null;
    }
    STATE.isPlaying = false;
    if (resetButtons) {
      updatePlayUIState(false);
    }
  }

  function updatePlayUIState(playing) {
    const ico3D = document.getElementById('icoPlay3D');
    const txt3D = document.getElementById('txtPlay3D');
    const icoNormal = document.getElementById('icoPlayNormal');
    const txtNormal = document.getElementById('txtPlayNormal');
    const txtStrip = document.getElementById('txtStripPlay');

    if (playing) {
      if (STATE.playMode === 'pingpong') {
        ico3D.className = 'fa-solid fa-pause text-yellow-300 text-2xs';
        txt3D.innerText = '⏸ Pausar 3D';
        icoNormal.className = 'fa-solid fa-play text-sky-400 text-2xs';
        txtNormal.innerText = '▶ Play';
      } else {
        icoNormal.className = 'fa-solid fa-pause text-sky-400 text-2xs';
        txtNormal.innerText = '⏸ Pausar';
        ico3D.className = 'fa-solid fa-play text-yellow-300 text-2xs';
        txt3D.innerText = '▶ Play 3D';
      }
      if (txtStrip) txtStrip.innerText = '⏸ Pausa';
    } else {
      ico3D.className = 'fa-solid fa-play text-yellow-300 text-2xs';
      txt3D.innerText = '▶ Play 3D';
      icoNormal.className = 'fa-solid fa-play text-sky-400 text-2xs';
      txtNormal.innerText = '▶ Play';
      if (txtStrip) txtStrip.innerText = '▶ Play';
    }
  }

  function updateFPS(val) {
    STATE.fps = parseInt(val, 10);
    document.getElementById('lblFPS').innerText = val;
    if (STATE.isPlaying) {
      startPlayback();
    }
  }

  function adjustZoom(delta) {
    STATE.zoomScale = Math.min(2.5, Math.max(0.5, STATE.zoomScale + delta));
    const lbl = document.getElementById('lblZoomLevel');
    if (lbl) lbl.innerText = `${STATE.zoomScale.toFixed(2)}x`;
    selectView(STATE.activeViewIndex);
  }

  // 1. DIÁLOGO PROYECTO (Exacto a Imagen 2)
  function openProyectoDialog() {
    toggleProyectoModal(true);
  }

  function toggleProyectoModal(open) {
    const modal = document.getElementById('proyectoModal');
    modal.classList.toggle('hidden', !open);
  }

  // Cambio de pestañas principales (Capas / 3D+Flip)
  function switchMainTab(tab) {
    STATE.activeTab = tab;
    const tabCapas = document.getElementById('tab-capas');
    const tab3DFlip = document.getElementById('tab-3dflip');
    const viewZ = document.getElementById('viewZBufferCore');
    const viewC = document.getElementById('viewCapasManager');

    if (tab === 'capas') {
      tabCapas.className = 'px-3 py-1 rounded bg-[#0284c7] text-white flex items-center space-x-1.5 border border-sky-400 font-semibold shadow-inner text-xs transition';
      tab3DFlip.className = 'px-2.5 py-1 rounded bg-[#16202e] hover:bg-[#1e2c3e] text-gray-300 flex items-center space-x-1.5 border border-appBorderLight/60 text-xs font-medium transition';
      viewZ.classList.add('hidden');
      viewC.classList.remove('hidden');
      showToast('Gestor de Capas activado: Importación PSD y Parallax', 'info');
    } else {
      tab3DFlip.className = 'px-3 py-1 rounded bg-[#0284c7] text-white flex items-center space-x-1.5 border border-sky-400 font-semibold shadow-inner text-xs transition';
      tabCapas.className = 'px-2.5 py-1 rounded bg-[#16202e] hover:bg-[#1e2c3e] text-gray-300 flex items-center space-x-1.5 border border-appBorderLight/60 text-xs font-medium transition';
      viewC.classList.add('hidden');
      viewZ.classList.remove('hidden');
      showToast('Entorno 3D+Flip activo con Z-Buffer Core', 'info');
    }
  }

  // Limitador LPI Toggle
  function toggleLimitadorLPI() {
    STATE.limitadorLPI = !STATE.limitadorLPI;
    const btn = document.getElementById('btnLimitadorLPI');
    const ico = document.getElementById('icoLimitador');
    const txt = document.getElementById('txtLimitador');
    const chipViable = document.getElementById('chipViable');

    if (STATE.limitadorLPI) {
      btn.className = 'px-2.5 py-1 bg-emerald-950 border border-emerald-500 rounded text-emerald-300 font-bold text-[10px] flex items-center space-x-1.5 shadow-sm active:scale-95 transition';
      ico.className = 'fa-solid fa-lock text-[9px] text-emerald-400';
      txt.innerText = '🔒 Limitador LPI: ON';
      chipViable.className = 'bg-[#0e271c] border border-emerald-500 text-emerald-300 px-1.5 py-0.5 rounded font-bold flex items-center space-x-1';
      chipViable.innerHTML = '<span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-ping inline-block"></span><span>● LÍMITE ACTIVO (±50% 3D Bloqueado)</span>';
      showToast('Limitador de pitch óptico ACTIVADO a rango seguro', 'success');
    } else {
      btn.className = 'px-2.5 py-1 bg-[#1a2332] border border-amber-600/70 hover:border-amber-500 rounded text-amber-300 font-bold text-[10px] flex items-center space-x-1.5 shadow-sm active:scale-95 transition';
      ico.className = 'fa-solid fa-lock text-[9px] text-amber-400';
      txt.innerText = '🔒 Limitador LPI: OFF';
      chipViable.className = 'bg-[#0e271c] border border-emerald-600 text-emerald-300 px-1.5 py-0.5 rounded font-bold flex items-center space-x-1';
      chipViable.innerHTML = '<span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse inline-block"></span><span>● VIABLE bajo lámina (~15.12 mm)</span>';
      showToast('Limitador de pitch óptico DESACTIVADO (Full Disparity)', 'info');
    }
  }

  // 3. BOTONERA INFERIOR: Generar, Limpiar, Entrelazados, Exportar
  function generate18ViewsSequence() {
    renderViewsStrip();
    selectView(7, true);
    showToast('✓ 18 vistas generadas y calibradas en el Z-Buffer', 'success');
  }

  function clearViewsStrip() {
    elViewsStrip.innerHTML = '<div class="text-gray-500 font-mono text-2xs p-4 flex items-center space-x-2"><i class="fa-solid fa-circle-exclamation text-amber-400"></i><span>Tira de vistas vacía. Haz clic en "Generar Vistas (18)" para restaurar.</span></div>';
    document.getElementById('statusTotalVistas').innerText = '0';
    showToast('Vistas eliminadas de la tira de trabajo', 'info');
  }

  function executeVerticalInterlace() {
    STATE.interlaceOrientation = 'vertical';
    document.getElementById('interlaceNoticeTitle').innerText = 'Entrelazado Vertical Listo';
    document.getElementById('interlaceNoticeBody').innerText = 'Entrelazado Vertical calculado con éxito: 1800x2520 px @ 720 DPI';
    
    const overlay = document.getElementById('interlacedOverlay');
    overlay.className = 'absolute inset-0 pointer-events-none opacity-25 z-30 lenticular-vertical-overlay';
    document.getElementById('topGuideBar').className = 'w-full h-3.5 lenticular-guide-bar rounded-[1px] flex justify-between items-center px-1 border border-emerald-400 transition-all';
    document.getElementById('bottomGuideBar').className = 'w-full h-3.5 lenticular-guide-bar rounded-[1px] flex justify-between items-center px-1 border border-emerald-400 transition-all';

    toggleInterlaceNotice(true);
    selectView(STATE.activeViewIndex);
  }

  function executeHorizontalInterlace() {
    STATE.interlaceOrientation = 'horizontal';
    document.getElementById('interlaceNoticeTitle').innerText = 'Entrelazado Horizontal Listo';
    document.getElementById('interlaceNoticeBody').innerText = 'Entrelazado Horizontal generado con éxito: 90° Flips';

    const overlay = document.getElementById('interlacedOverlay');
    overlay.className = 'absolute inset-0 pointer-events-none opacity-25 z-30 lenticular-horizontal-overlay';
    document.getElementById('topGuideBar').className = 'w-full h-3.5 lenticular-guide-bar-horizontal rounded-[1px] flex justify-between items-center px-1 border border-emerald-400 transition-all';
    document.getElementById('bottomGuideBar').className = 'w-full h-3.5 lenticular-guide-bar-horizontal rounded-[1px] flex justify-between items-center px-1 border border-emerald-400 transition-all';

    toggleInterlaceNotice(true);
    selectView(STATE.activeViewIndex);
  }

  function toggleInterlaceNotice(open) {
    const modal = document.getElementById('interlaceNoticeModal');
    modal.classList.toggle('hidden', !open);
  }

  function openExportDialog() {
    const modal = document.getElementById('exportModal');
    modal.classList.remove('hidden');
  }

  function toggleExportModal(open) {
    const modal = document.getElementById('exportModal');
    modal.classList.toggle('hidden', !open);
  }

  function downloadSimulatedExport() {
    toggleExportModal(false);
    document.getElementById('statusExportFile').innerText = 'nexus_lenticular_720dpi.png';
    showToast('✓ Descarga completada: nexus_lenticular_720dpi.png (17.26 MB @ 720 DPI)', 'success');
  }

  // Operaciones del carrusel
  function deleteActiveView() {
    showToast(`Fotograma #${STATE.activeViewIndex + 1} eliminado de la secuencia`, 'info');
  }

  function duplicateActiveView() {
    showToast(`Fotograma #${STATE.activeViewIndex + 1} clonado exitosamente`, 'success');
  }

  function extractView() {
    showToast(`Fotograma #${STATE.activeViewIndex + 1} extraído a buffer TIFF 16-bit`, 'info');
  }

  function reverseViews() {
    STATE.playDirection = -STATE.playDirection;
    showToast('Dirección de la secuencia de fotogramas invertida', 'info');
  }

  function acumularVistas() {
    const el = document.getElementById('lblAcumuladasCount');
    let count = parseInt(el.innerText, 10) + 1;
    el.innerText = count;
    showToast(`Fotogramas guardados en el banco de acumuladas (Total: ${count})`, 'success');
  }

  function moverAVistas() {
    showToast('Vistas acumuladas transferidas a la mesa de trabajo principal', 'success');
    renderViewsStrip();
    selectView(7, true);
  }

  function limpiarAcumuladas() {
    document.getElementById('lblAcumuladasCount').innerText = '0';
    showToast('Cola de acumuladas limpiada a 0', 'info');
  }

  // 5. Panel Izquierdo: Sliders y Modos
  function updateSliderParam(name, val) {
    switch (name) {
      case 'frente':
        STATE.frente = parseFloat(val);
        document.getElementById('valFrente').innerText = val;
        break;
      case 'profundidad':
        STATE.profundidad = parseFloat(val);
        document.getElementById('valProfundidad').innerText = val;
        break;
      case 'fuerza':
        STATE.fuerza = parseFloat(val);
        document.getElementById('valFuerza').innerText = val;
        break;
      case 'suavizado':
        STATE.suavizado = parseFloat(val);
        document.getElementById('valSuavizado').innerText = val;
        break;
      case 'rigidez':
        STATE.rigidez = parseFloat(val);
        document.getElementById('valRigidez').innerText = val;
        break;
      case 'exagerar':
        STATE.exagerar = parseFloat(val);
        document.getElementById('valExagerar').innerText = `${val}x`;
        break;
    }
    selectView(STATE.activeViewIndex);
  }

  function updateColorFilter(type, val) {
    if (type === 'hue') {
      STATE.hue = val;
      document.getElementById('valHue').innerText = val;
    } else {
      STATE.sat = val;
      document.getElementById('valSat').innerText = val;
    }
    elMainImg.style.filter = `hue-rotate(${STATE.hue}deg) saturate(${STATE.sat}%)`;
  }

  function centrarImagen3D() {
    stopPlayback();
    STATE.zoomScale = 1.0;
    document.getElementById('lblZoomLevel').innerText = '1.00x';
    selectView(7, true);
    showToast('🎯 Vista 3D centrada y calibrada a plano medio', 'success');
  }

  function togglePanGuide() {
    const lbl = document.getElementById('lblBtnPan');
    if (lbl.innerText.includes('Mover')) {
      lbl.innerText = '🔒 Fijar Guía';
      showToast('Modo libre de arrastre y calibración activado', 'info');
    } else {
      lbl.innerText = '✋ Mover Guía';
      showToast('Guía fijada en mesa de corte óptico', 'success');
    }
  }

  function toggle3DMode() {
    STATE.is3DActive = !STATE.is3DActive;
    const btn = document.getElementById('btnToggle3D');
    if (STATE.is3DActive) {
      btn.className = 'py-1 bg-emerald-900/70 border border-emerald-500 rounded text-emerald-300 text-2xs font-bold flex items-center justify-center space-x-1 active:scale-95 transition';
      showToast('Motor estereoscópico 3D habilitado', 'success');
    } else {
      btn.className = 'py-1 bg-gray-800 border border-gray-600 rounded text-gray-400 text-2xs font-semibold flex items-center justify-center space-x-1 active:scale-95 transition';
      showToast('Motor 3D en pausa (Modo 2D)', 'info');
    }
    selectView(STATE.activeViewIndex);
  }

  function toggleMonitor3D(checked) {
    STATE.is3DActive = checked;
    showToast(checked ? 'Render 3D en monitor activo' : 'Render 3D en monitor suspendido', checked ? 'success' : 'info');
    selectView(STATE.activeViewIndex);
  }

  function toggleModal(open) {
    const modal = document.getElementById('pairModal');
    modal.classList.toggle('hidden', !open);
  }

  function toggleCurvesModal(open) {
    const modal = document.getElementById('curvesModal');
    modal.classList.toggle('hidden', !open);
  }

  function filePickerSim() {
    showToast('Explorador de archivos de Windows abierto', 'info');
  }

  function confirmLoadPair() {
    toggleModal(false);
    showToast('✓ Par d4ad0a091a82 vinculado con éxito', 'success');
    generate18ViewsSequence();
  }

  function removePairCard() {
    showToast('Par 3D desvinculado del buffer Z', 'info');
  }

  function clearPairs() {
    showToast('Pares 3D limpiados del proyecto', 'info');
  }

  // 6. Panel Derecho: Transformaciones Geométricas y Tornasol
  function updateStaticRotation(val) {
    document.getElementById('lblRotEstatica').innerText = `${val}°`;
    const sel = document.getElementById('selAnguloPreset');
    sel.value = val;
    elMainImg.style.transform = `scale(${1.38 * STATE.zoomScale}) rotate(${val}deg)`;
  }

  function onSelectAnglePreset(val) {
    document.getElementById('slRotEstatica').value = val;
    updateStaticRotation(val);
  }

  function stepDimension(type, delta) {
    const el = document.getElementById(type === 'ancho' ? 'valAncho' : 'valAlto');
    let val = Math.max(10, parseFloat(el.innerText) + delta);
    el.innerText = val.toFixed(2);
    showToast(`Dimensión de ${type} ajustada a ${val.toFixed(2)} mm`, 'info');
  }

  function toggleLayerZoom(checked) {
    showToast(checked ? 'Zoom dinámico de capa activado' : 'Zoom dinámico detenido', 'info');
  }

  function updateLayerZoomVal(type, val) {
    if (type === 'ini') {
      document.getElementById('lblZoomIni').innerText = `${parseFloat(val).toFixed(1)}x`;
    } else {
      document.getElementById('lblZoomFin').innerText = `${parseFloat(val).toFixed(1)}x`;
    }
  }

  function toggleDynamicSpin(checked) {
    showToast(checked ? 'Rotación dinámica continua activada' : 'Rotación dinámica detenida', 'info');
  }

  function changeSpinSpeed(delta) {
    showToast(delta > 0 ? 'Velocidad de giro aumentada' : 'Velocidad de giro reducida', 'info');
  }

  function toggleTornasol(checked) {
    STATE.tornasolActive = checked;
    if (checked) {
      elShine.style.background = 'linear-gradient(105deg, rgba(255,0,128,0.3) 10%, rgba(0,255,255,0.4) 50%, rgba(255,255,0,0.3) 90%)';
      showToast('Efecto tornasol e iridiscencia espectral activado', 'success');
    } else {
      elShine.style.background = 'linear-gradient(105deg, transparent 20%, rgba(255,255,255,0.18) 50%, transparent 80%)';
      showToast('Efecto tornasol desactivado', 'info');
    }
  }

  function selectTornasolColor(hex) {
    elShine.style.background = `linear-gradient(105deg, transparent 10%, ${hex}66 50%, transparent 90%)`;
    showToast(`Tinte tornasol calibrado a color ${hex}`, 'info');
  }

  // Gestor de Capas helpers
  function toggleLayerEye(btn) {
    const icon = btn.querySelector('i');
    if (icon.classList.contains('fa-eye')) {
      icon.className = 'fa-solid fa-eye-slash text-xs text-gray-500';
      showToast('Capa ocultada del render', 'info');
    } else {
      icon.className = 'fa-solid fa-eye text-xs text-sky-400';
      showToast('Capa visible en composición', 'success');
    }
  }

  function adjustLayerParallax(layerNum, delta) {
    showToast(`Parallax de Capa ${layerNum} ajustado (${delta > 0 ? '+' : ''}${delta})`, 'info');
  }

  // Controles de ventana
  function simulateWinMinimize() {
    showToast('Aplicación minimizada a la barra de tareas', 'info');
  }

  function simulateWinMaximize() {
    STATE.isMaximized = !STATE.isMaximized;
    const ico = document.getElementById('winMaxIcon');
    if (STATE.isMaximized) {
      ico.className = 'fa-regular fa-window-restore text-[9px]';
      showToast('Ventana maximizada (Pantalla completa)', 'info');
    } else {
      ico.className = 'fa-regular fa-window-maximize text-[9px]';
      showToast('Ventana restaurada a tamaño estándar', 'info');
    }
  }

  function simulateWinClose() {
    showToast('Guarda los cambios antes de salir en Proyecto', 'info');
  }

  // Sistema de Toast Notificaciones
  function showToast(msg, type = 'info') {
    const container = document.getElementById('toastContainer');
    if (!container) return;

    const toast = document.createElement('div');
    const colorClass = type === 'success' 
      ? 'bg-[#082017] border-emerald-500 text-emerald-200' 
      : 'bg-[#0c1626] border-sky-500 text-sky-200';
    
    toast.className = `border px-3 py-1.5 rounded shadow-xl font-mono text-[11px] flex items-center space-x-2 transition-all transform duration-200 opacity-0 translate-y-[-10px] ${colorClass}`;
    toast.innerHTML = `
      <i class="fa-solid ${type === 'success' ? 'fa-check' : 'fa-circle-info'} text-xs"></i>
      <span>${msg}</span>
    `;

    container.appendChild(toast);
    requestAnimationFrame(() => {
      toast.classList.remove('opacity-0', 'translate-y-[-10px]');
    });

    setTimeout(() => {
      toast.classList.add('opacity-0', 'translate-y-[-5px]');
      setTimeout(() => toast.remove(), 250);
    }, 2800);
  }

  // Arranque al cargar DOM
  window.addEventListener('DOMContentLoaded', () => {
    renderViewsStrip();
    selectView(7, true); // Vista #8 activa
  });
</script>
</body></html>
