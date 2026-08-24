<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RAMPAGE v1.21.4 </title>
    <style>
        :root {
            --bg-main: #07020a;         /* Глубокий темный фиолетово-черный */
            --bg-card: #0f0514;         /* Фиолетовые карточки */
            --border-color: #2d0c3d;    /* Темно-фиолетовые границы */
            --neon-magenta: #e000ff;    /* Яркий неоновый маджента/пурпурный */
            --neon-purple: #8a00ff;     /* Неоновый фиолетовый */
            --text-main: #ffffff;       /* Чистый белый как пиксельные контуры */
            --text-muted: #805c93;      /* Приглушенный фиолетовый */
        }

        body { 
            font-family: 'Segoe UI', Arial, sans-serif; 
            margin: 0; 
            padding: 30px 20px; 
            background-color: var(--bg-main); 
            color: var(--text-main); 
            user-select: none; 
            overflow-x: hidden;
            position: relative;
            /* Фиолетовая неоновая сетка точь-в-точь как на твоем скриншоте */
            background-image: linear-gradient(rgba(224, 0, 255, 0.04) 1px, transparent 1px),
                              linear-gradient(90deg, rgba(224, 0, 255, 0.04) 1px, transparent 1px);
            background-size: 25px 20px;
        }

        /* Контейнер для летающих неоновых частиц */
        .neon-background {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
            overflow: hidden;
        }

        .floating-particle {
            position: absolute;
            bottom: -50px;
            animation: floatUp 10s linear infinite;
            opacity: 0.3;
            filter: drop-shadow(0 0 5px var(--neon-magenta));
        }

        @keyframes floatUp {
            0% {
                transform: translateY(0) translateX(0) rotate(0deg);
                opacity: 0;
            }
            15% { opacity: 0.5; }
            85% { opacity: 0.5; }
            100% {
                transform: translateY(-110vh) translateX(40px) rotate(360deg);
                opacity: 0;
            }
        }

        .container { max-width: 900px; margin: 0 auto; position: relative; z-index: 1; }

        header { 
            background: rgba(15, 5, 20, 0.9); 
            border: 2px solid var(--neon-magenta); 
            padding: 30px; 
            border-radius: 12px; 
            text-align: center; 
            margin-bottom: 25px; 
            box-shadow: 0 0 25px rgba(224, 0, 255, 0.25);
        }

        h1 { 
            margin: 0; 
            color: var(--text-main); 
            font-size: 32px; 
            text-transform: uppercase; 
            letter-spacing: 4px;
            text-shadow: 0 0 15px var(--neon-magenta), 0 0 5px #fff;
        }

        .version { 
            background: rgba(138, 0, 255, 0.2); 
            color: #fff; 
            padding: 5px 14px; 
            border-radius: 4px; 
            font-size: 13px; 
            margin-top: 10px; 
            display: inline-block; 
            border: 1px solid var(--neon-purple);
            font-weight: bold;
            text-shadow: 0 0 5px var(--neon-purple);
        }

        .developers {
            margin-top: 18px;
            display: flex;
            justify-content: center;
            gap: 15px;
            font-size: 14px;
        }
        .dev-badge {
            background: #050108;
            border: 1px solid var(--border-color);
            padding: 6px 16px;
            border-radius: 6px;
            color: #fff;
            display: flex;
            align-items: center;
            gap: 6px;
            box-shadow: 0 0 10px rgba(138, 0, 255, 0.1);
        }
        .dev-badge span { color: var(--neon-magenta); font-weight: bold; text-shadow: 0 0 5px var(--neon-magenta); }

        .stage-card { 
            background: rgba(15, 5, 20, 0.95); 
            border: 1px solid var(--border-color); 
            border-radius: 12px; 
            padding: 24px; 
            margin-bottom: 20px; 
            transition: all 0.3s ease;
            box-shadow: 0 4px 20px rgba(0,0,0,0.6);
            backdrop-filter: blur(3px);
        }

        .stage-card:hover {
            transform: translateY(-2px);
            border-color: var(--neon-magenta);
            box-shadow: 0 0 20px rgba(224, 0, 255, 0.2);
        }

        .stage-header { 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            border-bottom: 1px solid var(--border-color); 
            padding-bottom: 12px; 
            margin-bottom: 18px; 
        }
        .stage-title { font-size: 18px; font-weight: bold; color: #fff; text-shadow: 0 0 5px rgba(224, 0, 255, 0.3); }
        
        .status { 
            padding: 5px 14px; 
            border-radius: 4px; 
            font-size: 11px; 
            font-weight: bold; 
            text-transform: uppercase; 
            letter-spacing: 0.5px;
        }
        .status.todo { background: rgba(224, 0, 255, 0.1); color: var(--text-muted); border: 1px solid var(--border-color); }
        .status.progress { background: rgba(138, 0, 255, 0.2); color: #b98fff; border: 1px solid var(--neon-purple); }
        .status.done { background: rgba(224, 0, 255, 0.2); color: #fff; border: 1px solid var(--neon-magenta); box-shadow: 0 0 10px rgba(224, 0, 255, 0.3); }

        ul { list-style: none; padding: 0; margin: 0; }
        li { padding: 14px; border-bottom: 1px solid #160720; display: flex; align-items: center; gap: 15px; border-radius: 6px; }
        
        .checkbox { 
            width: 18px; 
            height: 18px; 
            border-radius: 4px; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            font-size: 12px; 
            font-weight: bold; 
            border: 2px solid var(--border-color); 
            color: transparent; 
        }
        
        /* Стили для выполненных шагов */
        .completed .checkbox { 
            background: #fff; 
            border-color: #fff; 
            color: #07020a; 
            box-shadow: 0 0 10px #fff; 
        }
        .completed .task-text { text-decoration: line-through; color: var(--text-muted); opacity: 0.6; }
        
        .progress-container { background: #050108; height: 8px; border-radius: 4px; overflow: hidden; margin-top: 20px; border: 1px solid var(--border-color); }
        .progress-bar { height: 100%; background: linear-gradient(90deg, var(--neon-purple), var(--neon-magenta)); width: 0%; transition: width 0.3s ease; box-shadow: 0 0 10px var(--neon-magenta); }
    </style>
</head>
<body>

    <!-- ФОНОВЫЕ ЧАСТИЦЫ -->
    <div class="neon-background" id="neonBg"></div>

    <div class="container">
        <header>
            <h1>RAMPAGE CLIENT</h1>
            <div class="version">Minecraft 1.21.4</div>
            <div class="developers">
                <div class="dev-badge">👾 Owner: <span>@kuki_v_popke</span></div>
                <div class="dev-badge">👾 Owner: <span>@wyxoon</span></div>
            </div>
        </header>

        <!-- ЭТАП 1 -->
        <div class="stage-card">
            <div class="stage-header">
                <div class="stage-title">Этап 1: Создание визуальной части (UI)</div>
                <span class="status done">Готово</span>
            </div>
            <ul>
                <li class="completed"><span class="checkbox">✓</span><span class="task-text">Настройка главного меню игры в стиле клиента</span></li>
                <li class="completed"><span class="checkbox">✓</span><span class="task-text">Кастомный HUD (отображение эффектов, брони на экране)</span></li>
                <li class="completed"><span class="checkbox">✓</span><span class="task-text">Шрифты (подключение красивых сторонних шрифтов)</span></li>
            </ul>
            <div class="progress-container">
                <div class="progress-bar" style="width: 100%;"></div>
            </div>
        </div>

        <!-- ЭТАП 2 -->
        <div class="stage-card">
            <div class="stage-header">
                <div class="stage-title">Этап 2: Функциональная часть (Меню и функции)</div>
                <span class="status progress"> готово</span>
            </div>
            <ul>
                <li class="completed"><span class="checkbox">✓</span><span class="task-text">Основа ClickGUI (каркас окна меню на RSHIFT)</span></li>
                <li class="completed"><span class="checkbox">✓</span><span class="task-text">Бинды клавиш для включения/выключения функций</span></li>
                <li class="completed"><span class="checkbox">✓</span><span class="task-text">Менеджер конфигов (сохранение настроек в .json)</span></li>
            </ul>
            <div class="progress-container">
                <div class="progress-bar" style="width: 100%;"></div>
            </div>
        </div>

        <!-- ЭТАП 3 -->
        <div class="stage-card">
            <div class="stage-header">
                <div class="stage-title">Этап 3: Модульная часть (Combat / Бой)</div>
                <span class="status todo">В разработке</span>
            </div>
            <ul>
                <li><span class="checkbox">✓</span><span class="task-text">Модуль KillAura (авто-атака под замах 1.21.4)</span></li>
                <li><span class="checkbox">✓</span><span class="task-text">Модуль AutoTotem / Gapple (авто-ресурсы)</span></li>
                <li><span class="checkbox">✓</span><span class="task-text">Модуль Velocity (анти-откидывание)</span></li>
                <li><span class="checkbox">✓</span><span class="task-text">Модуль Criticals (криты при прыжках)</span></li>
            </ul>
            <div class="progress-container">
                <div class="progress-bar" style="width: 10%;"></div>
            </div>
        </div>
