# Weather Report

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Atmosphere - Modern Weather Forecast</title>
    <!-- Google Fonts & Tailwind CDN -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons CDN -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['"Plus Jakarta Sans"', 'sans-serif'],
                    },
                    boxShadow: {
                        'glass-card': '0 8px 32px 0 rgba(0, 0, 0, 0.37)',
                        'glow': '0 0 25px rgba(56, 189, 248, 0.35)',
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background: linear-gradient(135deg, #0b1329 0%, #172554 45%, #0f172a 100%);
            min-height: 100vh;
        }

        .glass-panel {
            background: rgba(17, 24, 39, 0.65);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.12);
        }

        .glass-inner {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        .ambient-glow {
            position: fixed;
            width: 450px;
            height: 450px;
            border-radius: 50%;
            filter: blur(120px);
            opacity: 0.28;
            pointer-events: none;
            z-index: 0;
            transition: all 1s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .animate-fade-in {
            animation: fadeIn 0.45s ease-out forwards;
        }
    </style>
</head>
<body class="text-slate-100 flex flex-col justify-between antialiased selection:bg-cyan-500 selection:text-white min-h-screen">

    <!-- Ambient glowing backgrounds -->
    <div id="ambientOne" class="ambient-glow bg-cyan-500 top-[-80px] left-[-80px]"></div>
    <div id="ambientTwo" class="ambient-glow bg-blue-600 bottom-[-100px] right-[-80px]"></div>

    <!-- Main Container -->
    <div class="relative z-10 w-full max-w-4xl mx-auto px-4 py-8 md:py-12 flex-1 flex flex-col items-center justify-center">

        <!-- Header Brand -->
        <header class="w-full text-center mb-6">
            <div class="inline-flex items-center gap-2 px-3 py-1.5 rounded-full glass-inner text-cyan-300 text-xs font-semibold tracking-wide uppercase mb-3">
                <i data-lucide="cloud-sun" class="w-4 h-4"></i>
                <span>Weather Intelligence</span>
            </div>
            <h1 class="text-3xl sm:text-4xl font-extrabold tracking-tight bg-clip-text text-transparent bg-gradient-to-r from-white via-slate-200 to-cyan-300">
                Atmosphere Radar
            </h1>
        </header>

        <!-- Search Bar Section -->
        <section class="w-full max-w-2xl mb-4">
            <form id="searchForm" class="relative flex items-center">
                <div class="relative w-full">
                    <span class="absolute inset-y-0 left-0 flex items-center pl-4 pointer-events-none text-slate-400">
                        <i data-lucide="search" class="w-5 h-5"></i>
                    </span>
                    <input 
                        type="text" 
                        id="locationInput" 
                        placeholder="Search city, state or country..." 
                        autocomplete="off"
                        required
                        class="w-full pl-12 pr-28 py-3.5 rounded-2xl glass-panel text-white placeholder-slate-400 text-sm md:text-base focus:outline-none focus:ring-2 focus:ring-cyan-400 focus:border-transparent transition-all shadow-glass-card"
                    >
                    <button 
                        type="button" 
                        id="geoBtn" 
                        title="Use Current Location"
                        class="absolute right-14 inset-y-0 my-auto h-8 w-8 flex items-center justify-center text-slate-400 hover:text-cyan-400 rounded-lg hover:bg-white/10 transition-colors"
                    >
                        <i data-lucide="crosshair" class="w-4 h-4"></i>
                    </button>
                    <button 
                        type="submit" 
                        id="submitBtn"
                        class="absolute right-1.5 inset-y-0 my-auto h-9 px-4 rounded-xl bg-cyan-500 hover:bg-cyan-400 text-slate-950 font-bold text-xs uppercase tracking-wider flex items-center gap-1.5 transition-all active:scale-95 shadow-md hover:shadow-glow"
                    >
                        <span>Find</span>
                        <i data-lucide="arrow-right" class="w-3.5 h-3.5"></i>
                    </button>
                </div>
            </form>

            <!-- Quick City Chips -->
            <div class="flex items-center gap-2 mt-3 flex-wrap justify-center text-xs font-medium text-slate-300">
                <span class="text-slate-400 flex items-center gap-1"><i data-lucide="sparkles" class="w-3 h-3"></i> Quick pick:</span>
                <button type="button" class="quick-chip px-3 py-1 rounded-full glass-inner hover:bg-white/15 transition-all text-slate-200" data-city="London">London</button>
                <button type="button" class="quick-chip px-3 py-1 rounded-full glass-inner hover:bg-white/15 transition-all text-slate-200" data-city="New York">New York</button>
                <button type="button" class="quick-chip px-3 py-1 rounded-full glass-inner hover:bg-white/15 transition-all text-slate-200" data-city="Tokyo">Tokyo</button>
                <button type="button" class="quick-chip px-3 py-1 rounded-full glass-inner hover:bg-white/15 transition-all text-slate-200" data-city="Delhi">Delhi</button>
                <button type="button" class="quick-chip px-3 py-1 rounded-full glass-inner hover:bg-white/15 transition-all text-slate-200" data-city="Paris">Paris</button>
            </div>
        </section>

        <!-- Status & Error Alert Container -->
        <div id="statusAlert" class="hidden w-full max-w-2xl mb-4 px-4 py-3 rounded-xl border border-red-500/30 bg-red-500/10 text-red-200 text-sm flex items-center gap-2.5 animate-fade-in">
            <i data-lucide="alert-circle" class="w-5 h-5 flex-shrink-0 text-red-400"></i>
            <span id="statusMessage">Location not found. Please try again.</span>
        </div>

        <!-- Skeleton Loading View -->
        <div id="loadingSkeleton" class="hidden w-full max-w-2xl glass-panel p-8 rounded-3xl animate-pulse space-y-6">
            <div class="flex justify-between items-center">
                <div class="space-y-2">
                    <div class="h-7 w-48 bg-white/10 rounded-lg"></div>
                    <div class="h-4 w-32 bg-white/10 rounded"></div>
                </div>
                <div class="h-16 w-16 bg-white/10 rounded-full"></div>
            </div>
            <div class="h-14 w-40 bg-white/10 rounded-xl my-4"></div>
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
                <div class="h-20 bg-white/5 rounded-2xl"></div>
                <div class="h-20 bg-white/5 rounded-2xl"></div>
                <div class="h-20 bg-white/5 rounded-2xl"></div>
                <div class="h-20 bg-white/5 rounded-2xl"></div>
            </div>
        </div>

        <!-- Primary Weather Card -->
        <main id="weatherDisplay" class="w-full max-w-2xl glass-panel rounded-3xl p-6 sm:p-8 shadow-glass-card transition-all">
            <!-- Top Bar: Location & Unit Switcher -->
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 border-b border-white/10 pb-5">
                <div>
                    <div class="flex items-center gap-2">
                        <i data-lucide="map-pin" class="w-5 h-5 text-cyan-400"></i>
                        <h2 id="cityName" class="text-2xl sm:text-3xl font-bold tracking-tight text-white">--</h2>
                    </div>
                    <p id="cityRegion" class="text-xs sm:text-sm text-slate-400 mt-1 pl-7">--</p>
                    <p id="localTime" class="text-xs text-slate-400 pl-7 mt-0.5 flex items-center gap-1.5">
                        <i data-lucide="clock" class="w-3.5 h-3.5"></i>
                        <span id="timeText">--</span>
                    </p>
                </div>

                <!-- Unit Switcher Button -->
                <div class="flex items-center self-end sm:self-auto bg-slate-900/60 p-1 rounded-xl border border-white/10">
                    <button id="btnCelsius" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all bg-cyan-500 text-slate-950 shadow">°C</button>
                    <button id="btnFahrenheit" class="px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-white">°F</button>
                </div>
            </div>

            <!-- Middle Section: Main Hero Temperature & Visual -->
            <div class="my-8 flex flex-col sm:flex-row items-center justify-between gap-6">
                <div class="flex items-center gap-6">
                    <div class="relative">
                        <img id="weatherIcon" src="https://placehold.co/100x100/172554/38bdf8?text=Weather" alt="Weather condition" class="w-24 h-24 sm:w-28 sm:h-28 object-contain filter drop-shadow-md">
                    </div>
                    <div>
                        <div class="flex items-baseline">
                            <span id="temperatureVal" class="text-5xl sm:text-6xl font-extrabold tracking-tighter text-white">--</span>
                            <span id="tempUnitDisplay" class="text-2xl sm:text-3xl font-light text-cyan-300 ml-1">°C</span>
                        </div>
                        <p id="conditionText" class="text-base sm:text-lg font-medium text-cyan-200 mt-1">--</p>
                        <p class="text-xs text-slate-400 mt-0.5">Feels like <span id="feelsLikeVal" class="text-slate-200 font-semibold">--</span></p>
                    </div>
                </div>

                <!-- AQI Banner Widget -->
                <div id="aqiBadge" class="w-full sm:w-auto glass-inner px-4 py-3 rounded-2xl flex items-center justify-between sm:justify-start gap-4">
                    <div>
                        <div class="text-[10px] uppercase font-bold tracking-wider text-slate-400">Air Quality Index</div>
                        <div id="aqiStatus" class="text-sm font-bold text-emerald-400">Good</div>
                        <div class="text-[11px] text-slate-400">PM2.5: <span id="pm25Val" class="text-slate-200">--</span> µg/m³</div>
                    </div>
                    <div id="aqiIndicator" class="w-10 h-10 rounded-full flex items-center justify-center bg-emerald-500/20 text-emerald-400 border border-emerald-500/40">
                        <i data-lucide="leaf" class="w-5 h-5"></i>
                    </div>
                </div>
            </div>

            <!-- Bottom Section: Meteorological Details Grid -->
            <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 pt-2">
                <!-- Humidity -->
                <div class="glass-inner p-3.5 rounded-2xl flex flex-col justify-between">
                    <div class="flex items-center justify-between text-slate-400 mb-2">
                        <span class="text-xs font-medium">Humidity</span>
                        <i data-lucide="droplets" class="w-4 h-4 text-sky-400"></i>
                    </div>
                    <div>
                        <span id="humidityVal" class="text-lg font-bold text-white">--</span>
                    </div>
                </div>

                <!-- Wind Speed -->
                <div class="glass-inner p-3.5 rounded-2xl flex flex-col justify-between">
                    <div class="flex items-center justify-between text-slate-400 mb-2">
                        <span class="text-xs font-medium">Wind</span>
                        <i data-lucide="wind" class="w-4 h-4 text-teal-400"></i>
                    </div>
                    <div>
                        <span id="windVal" class="text-lg font-bold text-white">--</span>
                        <span id="windUnit" class="text-xs text-slate-400 ml-0.5">kph</span>
                    </div>
                </div>

                <!-- UV Index -->
                <div class="glass-inner p-3.5 rounded-2xl flex flex-col justify-between">
                    <div class="flex items-center justify-between text-slate-400 mb-2">
                        <span class="text-xs font-medium">UV Index</span>
                        <i data-lucide="sun" class="w-4 h-4 text-amber-400"></i>
                    </div>
                    <div>
                        <span id="uvVal" class="text-lg font-bold text-white">--</span>
                        <span id="uvRisk" class="text-[10px] text-amber-300 ml-1">Moderate</span>
                    </div>
                </div>

                <!-- Pressure -->
                <div class="glass-inner p-3.5 rounded-2xl flex flex-col justify-between">
                    <div class="flex items-center justify-between text-slate-400 mb-2">
                        <span class="text-xs font-medium">Pressure</span>
                        <i data-lucide="gauge" class="w-4 h-4 text-indigo-400"></i>
                    </div>
                    <div>
                        <span id="pressureVal" class="text-lg font-bold text-white">--</span>
                        <span class="text-[10px] text-slate-400 ml-0.5">mb</span>
                    </div>
                </div>

                <!-- Visibility -->
                <div class="glass-inner p-3.5 rounded-2xl flex flex-col justify-between">
                    <div class="flex items-center justify-between text-slate-400 mb-2">
                        <span class="text-xs font-medium">Visibility</span>
                        <i data-lucide="eye" class="w-4 h-4 text-violet-400"></i>
                    </div>
                    <div>
                        <span id="visibilityVal" class="text-lg font-bold text-white">--</span>
                        <span id="visUnit" class="text-xs text-slate-400 ml-0.5">km</span>
                    </div>
                </div>

                <!-- Wind Direction -->
                <div class="glass-inner p-3.5 rounded-2xl flex flex-col justify-between">
                    <div class="flex items-center justify-between text-slate-400 mb-2">
                        <span class="text-xs font-medium">Wind Dir</span>
                        <i data-lucide="compass" class="w-4 h-4 text-emerald-400"></i>
                    </div>
                    <div>
                        <span id="windDirVal" class="text-lg font-bold text-white">--</span>
                    </div>
                </div>

                <!-- Cloud Cover -->
                <div class="glass-inner p-3.5 rounded-2xl flex flex-col justify-between">
                    <div class="flex items-center justify-between text-slate-400 mb-2">
                        <span class="text-xs font-medium">Cloud Cover</span>
                        <i data-lucide="cloud" class="w-4 h-4 text-slate-300"></i>
                    </div>
                    <div>
                        <span id="cloudVal" class="text-lg font-bold text-white">--</span>
                    </div>
                </div>

                <!-- Day/Night Status -->
                <div class="glass-inner p-3.5 rounded-2xl flex flex-col justify-between">
                    <div class="flex items-center justify-between text-slate-400 mb-2">
                        <span class="text-xs font-medium">Cycle</span>
                        <i id="cycleIcon" data-lucide="moon" class="w-4 h-4 text-yellow-300"></i>
                    </div>
                    <div>
                        <span id="isDayVal" class="text-lg font-bold text-white">--</span>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <!-- Footer -->
    <footer class="w-full text-center py-4 text-xs text-slate-500 z-10">
        Data powered by WeatherAPI.com • Real-time atmospheric metrics
    </footer>

    <!-- Application Script -->
    <script>
        // API Configuration (Configurable)
        const API_KEY = "e1c797be981547eb94541442262810";
        const API_BASE_URL = "https://api.weatherapi.com/v1/current.json";

        // Application State
        let currentUnit = "C"; // "C" or "F"
        let currentWeatherData = null;

        // DOM Elements
        const searchForm = document.getElementById('searchForm');
        const locationInput = document.getElementById('locationInput');
        const geoBtn = document.getElementById('geoBtn');
        const submitBtn = document.getElementById('submitBtn');
        const statusAlert = document.getElementById('statusAlert');
        const statusMessage = document.getElementById('statusMessage');
        const loadingSkeleton = document.getElementById('loadingSkeleton');
        const weatherDisplay = document.getElementById('weatherDisplay');

        const cityName = document.getElementById('cityName');
        const cityRegion = document.getElementById('cityRegion');
        const timeText = document.getElementById('timeText');
        const weatherIcon = document.getElementById('weatherIcon');
        const temperatureVal = document.getElementById('temperatureVal');
        const tempUnitDisplay = document.getElementById('tempUnitDisplay');
        const conditionText = document.getElementById('conditionText');
        const feelsLikeVal = document.getElementById('feelsLikeVal');
        const humidityVal = document.getElementById('humidityVal');
        const windVal = document.getElementById('windVal');
        const windUnit = document.getElementById('windUnit');
        const uvVal = document.getElementById('uvVal');
        const uvRisk = document.getElementById('uvRisk');
        const pressureVal = document.getElementById('pressureVal');
        const visibilityVal = document.getElementById('visibilityVal');
        const visUnit = document.getElementById('visUnit');
        const windDirVal = document.getElementById('windDirVal');
        const cloudVal = document.getElementById('cloudVal');
        const isDayVal = document.getElementById('isDayVal');
        const aqiStatus = document.getElementById('aqiStatus');
        const pm25Val = document.getElementById('pm25Val');
        const aqiIndicator = document.getElementById('aqiIndicator');
        const btnCelsius = document.getElementById('btnCelsius');
        const btnFahrenheit = document.getElementById('btnFahrenheit');
        const quickChips = document.querySelectorAll('.quick-chip');

        function refreshIcons() {
            if (window.lucide) {
                lucide.createIcons();
            }
        }

        function showLoading() {
            statusAlert.classList.add('hidden');
            weatherDisplay.classList.add('hidden');
            loadingSkeleton.classList.remove('hidden');
        }

        function hideLoading() {
            loadingSkeleton.classList.add('hidden');
        }

        function showError(msg) {
            hideLoading();
            statusMessage.textContent = msg;
            statusAlert.classList.remove('hidden');
            weatherDisplay.classList.remove('hidden');
        }

        // Calculate EPA Air Quality scale label & colors
        function getAqiDescription(index) {
            switch (index) {
                case 1:
                    return { text: "Good", color: "text-emerald-400", bg: "bg-emerald-500/20", border: "border-emerald-500/40" };
                case 2:
                    return { text: "Moderate", color: "text-yellow-400", bg: "bg-yellow-500/20", border: "border-yellow-500/40" };
                case 3:
                    return { text: "Unhealthy (Sensitive)", color: "text-amber-400", bg: "bg-amber-500/20", border: "border-amber-500/40" };
                case 4:
                    return { text: "Unhealthy", color: "text-red-400", bg: "bg-red-500/20", border: "border-red-500/40" };
                case 5:
                    return { text: "Very Unhealthy", color: "text-purple-400", bg: "bg-purple-500/20", border: "border-purple-500/40" };
                case 6:
                    return { text: "Hazardous", color: "text-rose-600", bg: "bg-rose-600/20", border: "border-rose-600/40" };
                default:
                    return { text: "Moderate", color: "text-cyan-400", bg: "bg-cyan-500/20", border: "border-cyan-500/40" };
            }
        }

        function getUvRisk(uv) {
            if (uv <= 2) return "Low";
            if (uv <= 5) return "Moderate";
            if (uv <= 7) return "High";
            if (uv <= 10) return "Very High";
            return "Extreme";
        }

        // Render currentWeatherData into DOM based on selected unit
        function updateDisplay() {
            if (!currentWeatherData) return;

            const { location, current } = currentWeatherData;

            // Location
            cityName.textContent = location.name;
            cityRegion.textContent = `${location.region ? location.region + ', ' : ''}${location.country}`;
            timeText.textContent = `Local: ${location.localtime}`;

            // Main Temp & Feels
            if (currentUnit === "C") {
                temperatureVal.textContent = Math.round(current.temp_c);
                tempUnitDisplay.textContent = "°C";
                feelsLikeVal.textContent = `${Math.round(current.feelslike_c)}°C`;
                windVal.textContent = current.wind_kph;
                windUnit.textContent = "kph";
                visibilityVal.textContent = current.vis_km;
                visUnit.textContent = "km";
            } else {
                temperatureVal.textContent = Math.round(current.temp_f);
                tempUnitDisplay.textContent = "°F";
                feelsLikeVal.textContent = `${Math.round(current.feelslike_f)}°F`;
                windVal.textContent = current.wind_mph;
                windUnit.textContent = "mph";
                visibilityVal.textContent = current.vis_miles;
                visUnit.textContent = "mi";
            }

            // Weather Condition & Icon
            conditionText.textContent = current.condition.text;
            let iconUrl = current.condition.icon;
            if (iconUrl.startsWith("//")) {
                iconUrl = "https:" + iconUrl;
            }
            weatherIcon.src = iconUrl;
            weatherIcon.alt = current.condition.text;

            // Meteorological Extras
            humidityVal.textContent = `${current.humidity}%`;
            uvVal.textContent = current.uv;
            uvRisk.textContent = getUvRisk(current.uv);
            pressureVal.textContent = current.pressure_mb;
            windDirVal.textContent = current.wind_dir;
            cloudVal.textContent = `${current.cloud}%`;
            isDayVal.textContent = current.is_day ? "Daytime" : "Night";

            // Air Quality (US - EPA standard)
            const airQuality = current.air_quality;
            if (airQuality) {
                const epaIndex = airQuality["us-epa-index"] || 1;
                const aqiInfo = getAqiDescription(epaIndex);
                aqiStatus.textContent = aqiInfo.text;
                aqiStatus.className = `text-sm font-bold ${aqiInfo.color}`;

                aqiIndicator.className = `w-10 h-10 rounded-full flex items-center justify-center ${aqiInfo.bg} ${aqiInfo.color} border ${aqiInfo.border}`;
                pm25Val.textContent = airQuality.pm2_5 ? Math.round(airQuality.pm2_5 * 10) / 10 : '--';
            } else {
                aqiStatus.textContent = "Data N/A";
                pm25Val.textContent = "--";
            }

            // Update ambient lighting mood based on is_day and condition
            const amb1 = document.getElementById('ambientOne');
            const amb2 = document.getElementById('ambientTwo');
            if (!current.is_day) {
                amb1.style.background = '#4338ca';
                amb2.style.background = '#1e1b4b';
            } else if (current.temp_c > 26) {
                amb1.style.background = '#f97316';
                amb2.style.background = '#0284c7';
            } else {
                amb1.style.background = '#06b6d4';
                amb2.style.background = '#2563eb';
            }

            // Show card
            weatherDisplay.classList.remove('hidden');
            weatherDisplay.classList.add('animate-fade-in');
            refreshIcons();
        }

        // Fetch Weather from API
        async function fetchWeather(query) {
            if (!query || !query.trim()) return;

            showLoading();

            try {
                const encodedQuery = encodeURIComponent(query.trim());
                const endpoint = `${API_BASE_URL}?key=${API_KEY}&q=${encodedQuery}&aqi=yes`;
                
                const response = await fetch(endpoint);
                const data = await response.json();

                if (!response.ok || data.error) {
                    const message = data.error ? data.error.message : "Unable to retrieve weather for this location.";
                    throw new Error(message);
                }

                currentWeatherData = data;
                hideLoading();
                statusAlert.classList.add('hidden');
                updateDisplay();

            } catch (err) {
                showError(err.message || "Failed to fetch weather data. Please check connection.");
            }
        }

        // Handlers: Celsius / Fahrenheit toggles
        btnCelsius.addEventListener('click', () => {
            if (currentUnit === "C") return;
            currentUnit = "C";
            btnCelsius.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition-all bg-cyan-500 text-slate-950 shadow";
            btnFahrenheit.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-white";
            updateDisplay();
        });

        btnFahrenheit.addEventListener('click', () => {
            if (currentUnit === "F") return;
            currentUnit = "F";
            btnFahrenheit.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition-all bg-cyan-500 text-slate-950 shadow";
            btnCelsius.className = "px-3 py-1.5 rounded-lg text-xs font-bold transition-all text-slate-400 hover:text-white";
            updateDisplay();
        });

        // Search Form Submit
        searchForm.addEventListener('submit', (e) => {
            e.preventDefault();
            const location = locationInput.value;
            fetchWeather(location);
        });

        // Quick City Chips Click
        quickChips.forEach(chip => {
            chip.addEventListener('click', () => {
                const city = chip.getAttribute('data-city');
                locationInput.value = city;
                fetchWeather(city);
            });
        });

        // Geolocation button handler
        geoBtn.addEventListener('click', () => {
            if (!navigator.geolocation) {
                showError("Geolocation is not supported by your browser.");
                return;
            }

            showLoading();
            navigator.geolocation.getCurrentPosition(
                (pos) => {
                    const coords = `${pos.coords.latitude},${pos.coords.longitude}`;
                    fetchWeather(coords);
                },
                (err) => {
                    hideLoading();
                    showError("Location permission denied. Please type a city name.");
                },
                { timeout: 10000 }
            );
        });

        // Default initial load
        window.addEventListener('DOMContentLoaded', () => {
            refreshIcons();
            locationInput.value = "London";
            fetchWeather("London");
        });
    </script>
</body>
</html>
