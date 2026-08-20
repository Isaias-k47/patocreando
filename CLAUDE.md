==================================================
1. ARCHIVO DE CONFIGURACIÓN REQUERIDO (CLAUDE.md)
==================================================
# Directrices de Diseño y Arquitectura - PatoCreando

Actúa como un Director de Diseño UI/UX Senior y Arquitecto Frontend especializado en interfaces de ultra-alta gama (estilo Apple, Vercel, Linear, Stripe). Tu objetivo es transformar este proyecto en una aplicación web modular, hiper-optimizada y de máxima conversión.

## Estándares Obligatorios
1. **Dirección de Arte**: Fondo negro OLED absoluto (#000000), tipografía de alto contraste (Plus Jakarta Sans), bordes semitransparentes ultra finos, detalles clave en Ice Blue (#38bdf8). Cero colores neón chillones.
2. **Micro-interacciones**: Botones y tarjetas con efectos de brillo deslizante ("sheen"), transiciones fluidas basadas en curvas de Bézier y respuestas avanzadas al cursor.
3. **Componentes Visuales**: Contenedores interactivos modernos, efectos de profundidad tipo Parallax en scroll y soporte para video de fondo cinematográfico sutil.
4. **Rendimiento Extremo**: Optimizado para carga instantánea (<0.6s), móvil-first y fluidez absoluta a 60fps, especialmente en navegadores in-app de redes sociales (Instagram/TikTok).
5. **Estructura Limpia**: Organiza el código modularmente para un mantenimiento impecable.

==================================================
2. CÓDIGO BASE UNIFICADO (index.html limpio)
==================================================
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>PatoCreando | Sistemas Web de Alta Conversión & IA de Élite</title>
  <meta name="description" content="Estrategia humana + Orquestación de Inteligencia Artificial para crear páginas web ultra rápidas (<0.6s) y de máxima conversión.">
  <meta name="theme-color" content="#000000">

  <!-- Preconnect & Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;1,400;1,600&display=swap" rel="stylesheet">

  <style>
    :root {
      --bg-void: #000000;
      --bg-surface-1: #0a0a0a;
      --bg-card: rgba(15, 15, 15, 0.4);
      --text-primary: #ededed;
      --text-secondary: #a1a1aa;
      --text-muted: #71717a;
      --accent-tech: #38bdf8; 
      --accent-success: #10b981;
      --border-subtle: rgba(255, 255, 255, 0.08);
      --font-main: 'Plus Jakarta Sans', -apple-system, BlinkMacSystemFont, sans-serif;
      --radius-full: 9999px;
      --transition-smooth: all 0.35s cubic-bezier(0.16, 1, 0.3, 1);
    }

    *, *::before, *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html {
      scroll-behavior: smooth;
      background-color: var(--bg-void);
      color: var(--text-primary);
      font-family: var(--font-main);
      overflow-x: hidden;
    }

    body {
      min-height: 100vh;
      position: relative;
    }

    .noise-overlay {
      position: fixed;
      inset: 0;
      opacity: 0.02;
      pointer-events: none;
      z-index: 10;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
    }

    .hero-video-container {
      position: absolute;
      inset: 0;
      overflow: hidden;
      z-index: 0;
      opacity: 0.25;
      pointer-events: none;
    }

    .hero-video-container video {
      width: 100%;
      height: 100%;
      object-fit: cover;
      filter: contrast(120%) brightness(80%);
    }

    .hero-video-container::after {
      content: '';
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at center, rgba(0,0,0,0.2) 0%, var(--bg-void) 90%);
    }

    .ambient-glow {
      position: absolute;
      border-radius: 50%;
      filter: blur(140px);
      pointer-events: none;
      z-index: 1;
      opacity: 0.25;
      transition: transform 0.5s ease-out;
    }

    .glow-1 { top: -10vh; left: 40%; width: 600px; height: 600px; background: radial-gradient(circle, rgba(255, 255, 255, 0.08) 0%, transparent 70%); }
    .glow-2 { top: 50vh; right: -10%; width: 500px; height: 500px; background: radial-gradient(circle, rgba(56, 189, 248, 0.05) 0%, transparent 70%); }

    #mouse-glow {
      position: fixed;
      width: 400px;
      height: 400px;
      background: radial-gradient(circle, rgba(255, 255, 255, 0.03) 0%, transparent 60%);
      border-radius: 50%;
      pointer-events: none;
      transform: translate(-50%, -50%);
      z-index: 2;
      opacity: 0;
      transition: opacity 0.5s ease;
    }

    .container {
      width: 100%;
      max-width: 1100px;
      margin: 0 auto;
      padding: 0 1.5rem;
      position: relative;
      z-index: 3;
    }

    .section-padding { padding: clamp(4rem, 8vw, 7rem) 0; }

    h1, h2, h3, h4 { font-weight: 700; line-height: 1.15; letter-spacing: -0.03em; }
    p { color: var(--text-secondary); line-height: 1.7; }
    
    .text-gradient-tech {
      background: linear-gradient(135deg, #7dd3fc 0%, #38bdf8 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    .section-header { text-align: center; max-width: 700px; margin: 0 auto 3.5rem; }
    .section-tag {
      display: inline-flex; align-items: center; font-size: 0.75rem; font-weight: 600;
      text-transform: uppercase; letter-spacing: 0.15em; color: var(--accent-tech);
      border: 1px solid rgba(56, 189, 248, 0.2); background: rgba(56, 189, 248, 0.03);
      padding: 0.35rem 0.85rem; border-radius: var(--radius-full); margin-bottom: 1.25rem;
    }
    .section-title { font-size: clamp(2rem, 3.5vw, 2.8rem); margin-bottom: 1rem; }

    .btn {
      display: inline-flex; align-items: center; justify-content: center; gap: 0.75rem;
      font-weight: 600; font-size: 1rem; padding: 0.9rem 1.8rem; border-radius: var(--radius-full);
      text-decoration: none; transition: var(--transition-smooth); cursor: pointer; border: none;
      position: relative; overflow: hidden;
    }

    .btn-primary {
      background: var(--text-primary); color: var(--bg-void);
      box-shadow: 0 0 0 1px rgba(255, 255, 255, 0.1);
    }

    .btn-primary::after {
      content: ''; position: absolute; top: 0; left: -100%; width: 50%; height: 100%;
      background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.8), transparent);
      transform: skewX(-20deg); transition: 0s;
    }

    .btn-primary:hover {
      background: #ffffff; transform: translateY(-2px);
      box-shadow: 0 8px 25px -8px rgba(255, 255, 255, 0.4);
    }

    .btn-primary:hover::after { left: 200%; transition: left 0.6s ease-in-out; }

    .btn-secondary {
      background: rgba(255, 255, 255, 0.03); border: 1px solid var(--border-subtle);
      color: var(--text-primary); backdrop-filter: blur(10px);
    }
    .btn-secondary:hover { background: rgba(255, 255, 255, 0.08); border-color: rgba(255, 255, 255, 0.2); }
    .btn-sm { padding: 0.5rem 1.2rem; font-size: 0.85rem; }

    .badge-pill {
      display: inline-flex; align-items: center; gap: 0.5rem; padding: 0.35rem 0.85rem;
      background: rgba(255, 255, 255, 0.03); border: 1px solid var(--border-subtle);
      border-radius: var(--radius-full); font-size: 0.8rem; font-weight: 500; color: var(--text-primary);
      backdrop-filter: blur(8px);
    }

    .pulse-dot {
      width: 6px; height: 6px; background-color: var(--accent-success); border-radius: 50%;
      box-shadow: 0 0 8px var(--accent-success); animation: pulse 2s infinite;
    }
    @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.3; } 100% { opacity: 1; } }

    .site-header {
      position: sticky; top: 0; z-index: 100; background: rgba(0, 0, 0, 0.6);
      backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
      border-bottom: 1px solid var(--border-subtle); padding: 1rem 0;
    }
    .nav-wrapper { display: flex; align-items: center; justify-content: space-between; }
    .brand-logo { color: var(--text-primary); font-weight: 700; font-size: 1.2rem; text-decoration: none; }

    .hero-section {
      padding: clamp(6rem, 12vw, 10rem) 0 clamp(4rem, 6vw, 6rem);
      text-align: center;
      position: relative;
    }

    .hero-content { max-width: 860px; margin: 0 auto; position: relative; z-index: 2; }
    .hero-title { font-size: clamp(2.4rem, 5.5vw, 4.4rem); margin: 1.5rem 0; letter-spacing: -0.04em; }
    .hero-subtitle { font-size: 1.15rem; max-width: 680px; margin: 0 auto 2.5rem; }
    .hero-cta-group { display: flex; flex-direction: column; align-items: center; gap: 1rem; }
    .cta-actions { display: flex; gap: 1rem; flex-wrap: wrap; justify-content: center; }

    .auditing-card, .demo-card {
      max-width: 800px; margin: 0 auto; background: var(--bg-surface-1);
      border: 1px solid var(--border-subtle); border-radius: 20px; padding: 2.5rem;
      backdrop-filter: blur(12px);
    }
    .auditing-steps { display: grid; gap: 1.5rem; margin-bottom: 2rem; }
    .auditing-step-item { display: flex; gap: 1rem; align-items: flex-start; }
    .step-number {
      background: rgba(56, 189, 248, 0.1); color: var(--accent-tech);
      padding: 0.5rem 0.75rem; border-radius: 8px; font-weight: 700; font-size: 0.9rem;
    }

    .speed-comparison { display: flex; justify-content: center; align-items: center; gap: 3rem; margin-bottom: 2rem; }
    .speed-block span:first-child { font-size: 0.8rem; color: var(--text-muted); display: block; margin-bottom: 0.25rem; }
    .speed-value-bad { font-size: 1.8rem; font-weight: 700; color: #ef4444; }
    .speed-value-good { font-size: 1.8rem; font-weight: 700; color: var(--accent-success); }
    .divider-vertical { width: 1px; height: 40px; background: var(--border-subtle); }

    .reveal-on-scroll {
      opacity: 0; transform: translateY(30px) scale(0.98);
      transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
    }
    .reveal-on-scroll.is-revealed { opacity: 1; transform: translateY(0) scale(1); }

    @media (max-width: 768px) { .speed-comparison { gap: 1.5rem; } }
  </style>
</head>
<body>

  <div id="mouse-glow"></div>
  <div class="ambient-glow glow-1" id="scroll-glow-1"></div>
  <div class="ambient-glow glow-2" id="scroll-glow-2"></div>
  <div class="noise-overlay"></div>

  <header class="site-header">
    <div class="container nav-wrapper">
      <a href="#" class="brand-logo">PatoCreando</a>
      <div class="badge-pill">
        <span class="pulse-dot"></span>
        2 Cupos de Auditoría (Sin Cargo)
      </div>
    </div>
  </header>

  <main>
    <section class="hero-section">
      <div class="hero-video-container">
        <video autoplay muted loop playsinline>
          <source src="https://assets.mixkit.co/videos/preview/mixkit-digital-animation-of-screens-with-codes-31936-large.mp4" type="video/mp4">
        </video>
      </div>

      <div class="container hero-content">
        <h1 class="hero-title reveal-on-scroll is-revealed">
          No escribo código.<br>
          <span class="text-gradient-tech">Orquesto Inteligencia Artificial.</span>
        </h1>

        <p class="hero-subtitle reveal-on-scroll is-revealed" style="transition-delay: 0.1s;">
          Sistemas web de ultra alta conversión creados en tiempo récord. Velocidad extrema (&lt;0.6s) optimizada para dominar el tráfico de redes sociales.
        </p>

        <div class="hero-cta-group reveal-on-scroll is-revealed" style="transition-delay: 0.2s;">
          <div class="cta-actions">
            <a href="#auditoria" class="btn btn-primary">
              Reclamar Auditoría & Boceto Gratis
            </a>
            <a href="#demo" class="btn btn-secondary">
              Ver Demo Interactiva
            </a>
          </div>
          <span style="font-size: 0.85rem; color: var(--text-muted); margin-top: 0.5rem;">
            Sin tarjeta de crédito • Diagnóstico real en 48hs.
          </span>
        </div>
      </div>
    </section>

    <section id="auditoria" class="section-padding">
      <div class="container">
        <div class="section-header reveal-on-scroll">
          <div class="section-tag">Paso Inicial</div>
          <h2 class="section-title">Auditoría + Boceto Conceptual <span class="text-gradient-tech">Sin Cargo</span></h2>
          <p>Averigua exactamente dónde estás perdiendo dinero en tu embudo actual y recibe un wireframe visual de tu próxima web optimizada.</p>
        </div>

        <div class="auditing-card reveal-on-scroll">
          <div class="auditing-steps">
            <div class="auditing-step-item">
              <div class="step-number">01</div>
              <div>
                <h4 style="color: var(--text-primary); margin-bottom: 0.25rem;">Análisis de Fuga de Tráfico</h4>
                <p style="font-size: 0.95rem;">Reviso tu enlace actual en redes y mido cuántos clientes se van por latencia de carga.</p>
              </div>
            </div>
            <div class="auditing-step-item">
              <div class="step-number">02</div>
              <div>
                <h4 style="color: var(--text-primary); margin-bottom: 0.25rem;">Boceto Visual Personalizado</h4>
                <p style="font-size: 0.95rem;">Te entrego un wireframe esquemático adaptado exactamente a tu infoproducto o servicio.</p>
              </div>
            </div>
          </div>

          <div style="text-align: center;">
            <a href="https://wa.me/?text=Hola%20Pato,%20quiero%20reclamar%20mi%20auditoría%20y%20boceto%20sin%20cargo" target="_blank" class="btn btn-primary" style="width: 100%;">
              Reclamar mi Cupo Sin Cargo (Quedan 2)
            </a>
          </div>
        </div>
      </div>
    </section>

    <section id="demo" class="section-padding" style="border-top: 1px solid var(--border-subtle);">
      <div class="container">
        <div class="section-header reveal-on-scroll">
          <div class="section-tag">Experiencia en Vivo</div>
          <h2 class="section-title">La Diferencia de Velocidad</h2>
          <p>Compara visualmente cómo responde un sistema web nativo de alta gama frente a una estructura pesada tradicional.</p>
        </div>

        <div class="demo-card reveal-on-scroll">
          <div class="speed-comparison">
            <div class="speed-block">
              <span>WordPress Tradicional</span>
              <span class="speed-value-bad">2.8s</span>
            </div>
            <div class="divider-vertical"></div>
            <div class="speed-block">
              <span>Sistema PatoCreando</span>
              <span class="speed-value-good">&lt;0.6s</span>
            </div>
          </div>
          <p style="font-size: 0.95rem; margin-bottom: 1.5rem;">Cero tiempos de espera en el navegador integrado de Instagram o TikTok. Carga instantánea que retiene el 100% de la atención.</p>
          <a href="#auditoria" class="btn btn-secondary btn-sm">Quiero esta velocidad para mi marca</a>
        </div>
      </div>
    </section>
  </main>

  <footer style="border-top: 1px solid var(--border-subtle); padding: 2.5rem 0; text-align: center; font-size: 0.85rem; color: var(--text-muted);">
    <div class="container">
      <p>&copy; 2026 PatoCreando. Diseño Minimalista & Orquestación IA.</p>
    </div>
  </footer>

  <script>
    const mouseGlow = document.getElementById('mouse-glow');
    document.addEventListener('mousemove', (e) => {
      mouseGlow.style.opacity = '1';
      mouseGlow.style.left = e.clientX + 'px';
      mouseGlow.style.top = e.clientY + 'px';
    });
    document.addEventListener('mouseleave', () => mouseGlow.style.opacity = '0');

    const glow1 = document.getElementById('scroll-glow-1');
    const glow2 = document.getElementById('scroll-glow-2');
    window.addEventListener('scroll', () => {
      const scrollY = window.scrollY;
      glow1.style.transform = `translateY(${scrollY * 0.15}px)`;
      glow2.style.transform = `translateY(${scrollY * -0.1}px)`;
    });

    const observer = new IntersectionObserver((entries, observer) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-revealed');
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.1 });

    document.querySelectorAll('.reveal-on-scroll').forEach(el => observer.observe(el));
  </script>
</body>
</html>